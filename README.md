# Apache-Kafka-ARSW

### Actividad 1.  Análisis de comunicación 
Para una tienda en línea, clasifique qué procesos deberían ser síncronos, asíncronos o híbridos: consultar productos, crear pedido, validar pago, enviar notificación, actualizar analítica y registrar auditoría. Justifique brevemente su decisión.


| Proceso | Tipo de Comunicación | Justificación |
|----------|----------------------|---------------|
| Consultar productos | **Síncrono (REST)** | Es una operación que requiere una respuesta inmediata para mostrar la información al usuario. La latencia debe ser mínima. |
| Crear pedido | **Híbrido** | La creación inicial puede ser síncrona para devolver un `orderId` y un estado inicial (`CREATED`) al usuario. Los procesos posteriores (validación de pago, reserva de inventario) se disparan de forma asíncrona. |
| Validar pago | **Asíncrono (Kafka)** | Aunque debe ser rápido, no es necesario que el usuario espere la respuesta. Si falla, se puede notificar por otro medio. Kafka permite desacoplar el servicio de pagos y reintentar en caso de fallos. |
| Enviar notificación | **Asíncrono (Kafka)** | Este proceso no afecta la transacción principal. Si falla, el pedido ya está creado. Kafka permite procesar grandes volúmenes de notificaciones sin ralentizar el flujo principal. |
| Actualizar analítica | **Asíncrono (Kafka)** | Son procesos de fondo para la toma de decisiones. No necesitan ser en tiempo real y pueden consumir grandes cantidades de datos desde Kafka. |
| Registrar auditoría | **Asíncrono (Kafka)** | Es fundamental para la trazabilidad. Kafka, como un log distribuido, es perfecto para almacenar y reprocesar estos eventos históricos. |

