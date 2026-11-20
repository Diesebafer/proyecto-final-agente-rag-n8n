# proyecto-final-agente-rag-n8n

# Asistente Autónomo de Post-Venta con RAG — NubeLogística S.A.

## 1. Caso de Negocio, ROI y Justificación de RAG

### 1.1 Descripción del Proceso Automatizado
* **Empresa:** NubeLogística S.A. (operador logístico y *fulfillment* para comercio electrónico).
* **Proceso manual previo:** Un analista de atención al cliente recibía reclamos vía formulario web, buscaba manualmente el número de guía en una planilla operativa, verificaba el estado del envío, revisaba un documento estático de políticas comerciales según la categoría del cliente (VIP o Estándar) y redactaba un correo de respuesta o derivaba el caso a un supervisor.
* **Solución implementada:** Flujo autónomo en **n8n** que cruza datos del formulario (**Webhook**) con la base operativa (**Google Sheets**), consulta las políticas vigentes mediante arquitectura **RAG** y utiliza un **AI Agent** (Google Gemini) para resolver, pedir aclaraciones o escalar cada ticket automáticamente.

### 1.2 Cuantificación y Cálculo de ROI Estimado
* **Frecuencia del proceso:** 600 reclamos mensuales (~20 reclamos por día calendario).
* **Tiempo y costo manual actual:** 12 minutos promedio por ticket = **120 horas/mes**. Considerando un costo operativo de $10 USD/hora por analista, el gasto mensual asciende a **$1,200 USD/mes**.
* **Impacto de la automatización:** El sistema resuelve de forma autónoma o filtra para aclaración el **75 % de los casos** (450 tickets/mes) en menos de 10 segundos por ejecución, y entrega los casos escalados (25 %) ya diagnosticados al supervisor.

| Métrica Operativa / Financiera | Proceso Manual | Proceso Automatizado (n8n + Gemini) | Variación / Ahorro |
| :--- | :--- | :--- | :--- |
| **Volumen mensual procesado** | 600 tickets | 600 tickets | 100 % cobertura |
| **Horas humanas requeridas** | 120 horas/mes | 30 horas/mes (solo escalados) | **-90 horas/mes (-75 %)** |
| **Costo operativo mensual** | $1,200 USD | $300 USD (horas) + $35 USD (infraestructura/API) | **+$865 USD/mes netos** |
| **Tiempo de primera respuesta** | 4 a 24 horas | < 15 segundos | **Reducción del 99 %** |

* **Costo de implementación (Setup inicial):** 15 horas de ingeniería de automatización ($300 USD estimados).
* **Retorno de la Inversión (ROI):** El ahorro neto mensual de **$865 USD** amortiza el costo inicial de desarrollo en **menos de 11 días de operación**, generando un ROI superior al **280 % en el primer mes** y superando el **2,400 % mensual** sobre el costo recurrente de infraestructura ($35 USD/mes).

### 1.3 Escalabilidad y Límites del ROI
* **Por qué escala mejor que el proceso manual:** El modelo manual crece de forma lineal (duplicar reclamos exige duplicar personal). El flujo en n8n absorbe picos estacionales (como *Hot Sale* o *Black Friday*) ejecutando solicitudes en paralelo sin degradar el tiempo de respuesta ni aumentar costos fijos.
* **Límites del ROI (cuándo deja de tener sentido automatizar):**
  1. **Bajo volumen operativo:** Con menos de **45 reclamos mensuales**, el ahorro en horas humanas (< $90 USD/mes) no justifica el costo de suscripción de la nube ni el mantenimiento técnico.
  2. **Alta tasa de excepciones físicas:** Si más del **80 % de los reclamos** requirieran peritajes físicos en depósito o negociación legal compleja (casos que el agente debe escalar por política), el sistema actuaría solo como un reenviador de correos sin generar ahorro real de gestión.

### 1.4 Justificación de Arquitectura RAG (Retrieval-Augmented Generation)
* **Dinamismo de reglas comerciales:** Las políticas de compensación logística (porcentajes de cupones, días de tolerancia para clientes VIP vs. Estándar y topes de valor declarado en USD) cambian trimestralmente.
* **Ventaja técnica:** Al almacenar las políticas en un **Vector Store** (almacén vectorial) en lugar de incrustarlas fijas dentro del código o del *prompt* (instrucción), el equipo de negocio puede actualizar el manual operativo sin modificar la lógica del flujo ni reentrenar el modelo de IA, garantizando respuestas auditables y libres de alucinaciones.
