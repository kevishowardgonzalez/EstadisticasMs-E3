 

Evaluación Parcial N°3 (EP3) — DevOps: Observabilidad, Orquestación y Gobernanza CI/CD

Asignatura: DevOps (DOY0101)

Microservicio: Estadísticas (EstadisticasMs-E3)

Repositorio: kevishowardgonzalez/EstadisticasMs-E3

Infraestructura Cloud: AWS Academy Learner Lab (us-east-1)

1. Descripción General del Proyecto

Este repositorio contiene la implementación de prácticas de monitoreo, observabilidad, despliegue orquestado en la nube, dashboards de métricas y gobernanza automatizada en el pipeline CI/CD para el microservicio Estadísticas. El objetivo es garantizar la disponibilidad operativa, la trazabilidad de solicitudes y el cumplimiento continuo de estándares de calidad y seguridad antes de cualquier promoción a producción.

2. Arquitectura e Implementación por Indicador de Evaluación (Rúbrica EP3)

IE1 (20%) — Monitoreo y Observabilidad en AWS

Se configuró una arquitectura de monitoreo activo sobre infraestructura EC2 en la región us-east-1 (Practica de Monitoreo E3, instancia i-071311a8ad8a6cd4c):

Recolección de Métricas y Logs: Integración mediante AWS Systems Manager (LabInstanceProfile) y el Agente de Amazon CloudWatch para capturar métricas de sistema operativo y disponibilidad en tiempo real.

Pruebas de Estrés y Detección de Anomalías: Ejecución de pruebas de carga controlada con stress-ng, llevando la utilización de CPU al ~99.99% para validar el comportamiento del sistema bajo alta demanda.

Alertas Automatizadas: Configuración de la alarma Alerta-CPU-Alta-E3 en CloudWatch vinculada al tópico de Amazon SNS AlarmaMonitoreoE3, enviando notificaciones automáticas por correo electrónico institucional ante umbrales críticos.

IE2 (20%) — Despliegue Orquestado en la Nube (Amazon EKS + Istio + Kiali)

Se aprovisionó un entorno orquestado en Kubernetes administrado en AWS para el despliegue y trazabilidad del microservicio:

Clúster Kubernetes: Creación del clúster EstadisticasEKS (Kubernetes v1.37, rol IAM LabRole) respaldado por el grupo de nodos administrados nodos-estadisticas (2 nodos t3.medium en Amazon Linux 2023).

Service Mesh y Observabilidad: Instalación de Istio (istioctl, perfil demo optimizado) para la gestión de tráfico y recolección de telemetría distribuida, complementado con Prometheus y Kiali para visualizar la topología de servicios, latencias, tasas de error y trazabilidad en tiempo real.

Manifiestos: Los archivos de despliegue de Kubernetes e Istio se encuentran organizados en el directorio /k8s (o /manifests) de este repositorio.

IE3 (10%) — Dashboards Integrados al Proceso CI/CD

Se implementaron paneles visuales de control para supervisar tanto la salud operativa como el rendimiento del ciclo de entrega:

Métricas de Pipeline CI/CD: Visualización de tiempos de ejecución y despliegue, resultados de pruebas automatizadas y cobertura de código en cada ejecución de GitHub Actions.

Métricas de Infraestructura y Aplicación: Paneles en CloudWatch y Kiali/Prometheus con consumo de CPU/memoria, estado de pods y tasa de errores HTTP.

IE4 (10%) — Documentación e Impacto en la Toma de Decisiones Técnicas

La integración de observabilidad y controles automáticos en el flujo DevOps impacta directamente en la ingeniería y operación del sistema:

Prevención Proactiva de Incidentes: Las alertas tempranas de CloudWatch y las métricas de latencia en Kiali permiten detectar cuellos de botella o degradación de rendimiento antes de afectar a los usuarios finales.

Decisiones Basadas en Evidencia: Los datos históricos de consumo de recursos (t3.medium / t3.small) y tiempos de despliegue fundamentan el dimensionamiento adecuado de nodos y réplicas en EKS.

Mejora Continua y Trazabilidad: Cada cambio en el código queda auditado mediante ejecuciones trazables del pipeline, reduciendo el tiempo medio de recuperación (MTTR) y evitando regresiones en producción.

IE5 (20%) — Políticas de Cumplimiento Automatizadas

Se establecieron reglas estrictas de gobernanza sobre el código fuente en GitHub:

Protección de Rama (Branch Protection): La rama principal main cuenta con políticas activas que bloquean commits directos, exigiendo obligatoriamente la apertura de un Pull Request (PR) y la aprobación de los status checks configurados antes de realizar el merge.

Auditoría Automatizada: Ejecución automática de análisis estático de calidad y revisión de vulnerabilidades en cada PR y push.

IE6 (20%) — Interrupción Automática ante Fallas Críticas

El flujo de integración continua en .github/workflows/ actúa como una compuerta de calidad (Quality Gate):

Si alguna etapa crítica —pruebas unitarias, umbrales de cobertura, análisis de seguridad o construcción de imagen— detecta una falla o vulnerabilidad crítica, el pipeline interrumpe automáticamente su ejecución y bloquea la integración hacia main, protegiendo la estabilidad y seguridad del entorno productivo.

3. Estructura del Repositorio

├── .github/
│   └── workflows/          # Pipelines CI/CD y validaciones automatizadas (IE3, IE5, IE6)
├── k8s/                    # Manifiestos de despliegue Kubernetes e Istio (IE2)
├── evidencias/             # Capturas de respaldo de AWS (CloudWatch, SNS, EKS, Istio/Kiali)
├── src/                    # Código fuente del microservicio Estadísticas
└── README.md               # Documentación técnica, declaración de IA y reflexiones


4. Declaración de Uso de Inteligencia Artificial

 

Propósito y Alcance: La IA fue utilizada exclusivamente como guía de apoyo técnico en AWS (orientación paso a paso para la configuración de servicios en AWS Academy Learner Lab, comandos de aws-cli, kubectl e istioctl, y diagnóstico de estados de aprovisionamiento) y como apoyo en la estructuración y redacción técnica de este documento README.md.

Validación: La utilizacion de comandos

5. Reflexiones Individuales (Redactadas sin apoyo de IA)
 "Durante esta evaluación aprendí a integrar el monitoreo, las alertas y el control del pipeline para cuidar la calidad y estabilidad del microservicio. Trabajar con AWS y GitHub me ayudó a ver cómo las reglas de protección y los bloqueos automáticos evitan errores antes de avanzar, y cómo la observabilidad permite reaccionar a tiempo cuando sube el uso de recursos. En el proyecto me encargué de configurar y validar las pruebas, el flujo de trabajo y los entornos, y me queda claro que documentar bien y revisar cada paso es clave para que todo funcione de forma ordenada."

### Pasos para levantar el stack localmente
1. **Copiar variables de entorno:**
   ```bash
   cp .env.example .env
