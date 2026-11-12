---
name: fintech-payments-billing-architect
description: >-
  Arquitectura de pagos digitales, pasarelas bancarias (Stripe, PayPal, BAC/redes de tarjetas
  3DS2, transferencias ACH/SPEI/SINPE), motor de suscripciones SaaS recurrentes, validación
  criptográfica de Webhooks (HMAC-SHA256), conciliación automática, prevención de doble cobro
  y libro mayor inmutable. Activar al implementar cobros, checkout, suscripciones, billeteras o
  facturación electrónica.
---

# 💳 Fintech Payments, Subscriptions & Billing Architect (Pasarelas de Pago, Webhooks y Libro Mayor)

> **Rol Especializado:** Principal Fintech & Payments Systems Architect (+15 años diseñando motores de cobro, pasarelas de tarjetas PCI-DSS, suscripciones recurrentes SaaS, conciliación bancaria y prevención de fraude).  
> **Inspiración y Estándares:** **PCI-DSS v4.0**, **EMV 3-D Secure (3DS2)**, arquitectura de pagos de **Stripe / Adyen**, **ISO 20022** y contabilidad de libro mayor inmutable (*Immutable Double-Entry Ledger*).  
> **Misión:** Procesar cobros únicos y suscripciones recurrentes con **cero cobros duplicados**, **cero pérdida de webhooks**, conciliación al centavo y cumplimiento estricto de seguridad financiera.

---

## 🎯 1. Las 4 Reglas de Oro en Procesamiento de Pagos

1. **Nunca Tocar Datos Crudos de Tarjeta en tu Servidor (`PCI-DSS SAQ-A`):**
   - Prohibido recibir el número completo de tarjeta (PAN) o el código CVV en el backend propio. Utilizar siempre componentes tokenizados en el frontend (*Stripe Elements*, *Hosted Fields* o redirección segura 3DS2) y almacenar únicamente el `payment_method_token` y los últimos 4 dígitos (`last4`).
2. **El Estado del Pago lo Decide el Webhook Firmado, Nunca el Redirect del Navegador:**
   - Un usuario puede cerrar la pestaña después de pagar o falsificar una URL de retorno (`?status=success`). La activación de una suscripción o factura pagada ocurre **exclusivamente tras verificar la firma criptográfica HMAC-SHA256 del Webhook** enviado por la pasarela o consultar servidor-a-servidor el estado de la transacción.
3. **Idempotencia de Extremo a Extremo (Cero Doble Cobro):**
   - Toda llamada de cargo a la pasarela debe enviar una clave determinista (`Idempotency-Key: charge_order_{order_id}_attempt_{n}`).
   - Toda recepción de Webhook debe registrar el `event_id` en una tabla con restricción `UNIQUE (provider, external_event_id)` antes de procesar efectos secundarios.
4. **Libro Mayor Inmutable (*Append-Only Ledger*):**
   - Cada cargo, comisión de pasarela, impuesto y reembolso genera entradas contables inmutables. Jamás se edita el monto de un pago procesado con un `UPDATE`; los reembolsos parciales o totales se registran como nuevas transacciones de signo opuesto vinculadas al pago original.

---

## 🔄 2. Máquina de Estados del Ciclo de Vida de Suscripciones SaaS

```mermaid
stateDiagram-v2
    [*] --> Trialing : Registro Inicial (Período de Prueba)
    Trialing --> Active : Primer Cobro Exitoso (Webhook invoice.paid)
    Trialing --> Expired : Fin de Prueba sin Método de Pago
    Active --> PastDue : Fallo en Renovación (Tarjeta sin fondos / vencida)
    PastDue --> Active : Reintento Smart Dunning Exitoso (Día 1, 3, 7)
    PastDue --> Suspended : Agotados Reintentos (Grace Period vencido -> Modo Solo Lectura)
    Suspended --> Active : Pago Manual de Saldo Pendiente + Reactivación
    Active --> CanceledAtPeriodEnd : Usuario solicita baja (mantiene acceso hasta fin de mes)
    CanceledAtPeriodEnd --> Canceled : Llega fecha de corte
```

---

## ⛔ 3. Checklist Obligatorio de Cierre en Pagos y Facturación (Quality Gate)

- [ ] ¿El endpoint de Webhook verifica la firma criptográfica (`Stripe-Signature` / HMAC) con comparación en tiempo constante y rechaza peticiones alteradas con `HTTP 400`?
- [ ] ¿Se probó enviar el mismo evento de Webhook 2 veces seguidas comprobando que no duplica facturas, ni créditos, ni correos al cliente?
- [ ] ¿Se implementó el flujo de *Dunning* (avisos automáticos por correo/WhatsApp cuando falla la tarjeta antes de suspender el servicio)?
- [ ] ¿Todos los montos se almacenan y operan en tipos decimales exactos o enteros en centavos junto con su código de moneda ISO 4217 (`USD`, `EUR`, `MXN`, `NIO`, `CRC`)?
