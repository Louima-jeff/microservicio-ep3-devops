# Informe — Evaluación Parcial N°3 (DOY0101)

## Observabilidad y entornos reales en DevOps

**Integrantes:** Gefmy Louima / Odens Jean Anderson
**Sección:** INGENIERIA DEVOPS_006V_OLS
**Fecha:** 18/06/2026
**Repositorio GitHub:** https://github.com/Louima-jeff/microservicio-ep3-devops

---

## 1. Introducción

En esta Evaluación Parcial N°3 se trabajó sobre un microservicio desarrollado previamente en las evaluaciones EP1 y EP2. El objetivo principal de esta entrega fue incorporar prácticas reales de DevOps relacionadas con observabilidad, monitoreo, seguridad, cumplimiento y automatización mediante un pipeline CI/CD.

El microservicio fue preparado para exponer métricas, ser monitoreado con Prometheus, visualizar información mediante Grafana y validar la calidad y seguridad del código utilizando herramientas como SonarCloud, Snyk y Trivy. Además, se configuró un flujo automatizado en GitHub Actions para ejecutar pruebas, revisar cobertura, analizar calidad del código, escanear vulnerabilidades, construir una imagen Docker y realizar un despliegue simulado.

Esta entrega permite demostrar cómo un entorno DevOps real puede integrar monitoreo, seguridad y automatización para mejorar la calidad del software, reducir riesgos y entregar mayor confianza al momento de desplegar una aplicación.

---

## 2. Herramientas de monitoreo y logging implementadas (IE1)

Para implementar observabilidad en el microservicio se utilizaron herramientas de monitoreo basadas en Prometheus. El microservicio expone un endpoint `/metrics`, desde donde Prometheus puede recolectar métricas relacionadas con el estado de la aplicación, el uso de recursos y el comportamiento general del servicio.

Se configuró Prometheus para consultar periódicamente el microservicio y registrar métricas como uso de CPU, memoria, cantidad de solicitudes y estado de los servicios. Estas métricas permiten conocer el comportamiento del sistema y detectar posibles problemas en tiempo real.

También se incorporó instrumentación en el microservicio mediante librerías compatibles con FastAPI, permitiendo que la aplicación publique métricas automáticamente. Esto facilita el seguimiento del rendimiento, entrega información útil para la toma de decisiones técnicas y permite observar el estado de salud del microservicio.

El logging cumple un rol importante porque permite revisar eventos, errores y comportamientos del sistema durante la ejecución. Gracias a los logs y a las métricas, el equipo puede identificar problemas con mayor rapidez y responder de forma más ordenada ante incidentes.

**Evidencias adjuntas:**

* Captura de Prometheus en `Status → Targets`, mostrando el microservicio en estado `UP`.
* Captura del endpoint `/metrics` respondiendo correctamente.
* Captura de métricas consultadas en Prometheus.
* Captura de los archivos de configuración de Prometheus.

---

## 3. Despliegue en entorno orquestado (IE2)

El proyecto incluye archivos de configuración para Kubernetes dentro de la carpeta `k8s`. Estos manifiestos permiten describir cómo se desplegaría el microservicio en un entorno orquestado.

Se definieron recursos como `Deployment`, `Service`, `HPA`, `ServiceMonitor` y configuraciones relacionadas con Prometheus. El `Deployment` permite administrar las réplicas del microservicio, mientras que el `Service` expone la aplicación dentro del clúster. Además, se consideran límites de recursos y mecanismos de monitoreo para controlar el comportamiento del servicio.

El uso de Kubernetes permite que el microservicio pueda ejecutarse de forma más ordenada, escalable y controlada. A través del HPA se puede definir escalamiento automático según el consumo de recursos, mientras que los probes permiten verificar si la aplicación está viva y lista para recibir tráfico.

El despliegue final fue simulado dentro del pipeline CI/CD, donde se ejecuta una etapa de deploy después de superar pruebas, análisis de calidad, seguridad y construcción de imagen Docker. Esta simulación representa el flujo que se utilizaría en un entorno Kubernetes real.

**Evidencias adjuntas:**

* Captura de los archivos dentro de la carpeta `k8s`.
* Captura del pipeline ejecutando la etapa `deploy`.
* Captura de los manifiestos `deployment.yaml`, `service.yaml`, `hpa.yaml` y `servicemonitor.yaml`.
* Captura del servicio corriendo o del despliegue simulado en GitHub Actions.

---

## 4. Dashboard de métricas clave (IE3)

Se incorporó una estructura de monitoreo con Grafana y Prometheus para visualizar métricas relevantes del microservicio y del proceso DevOps. El dashboard permite observar información relacionada con el rendimiento de la aplicación, uso de recursos, disponibilidad y métricas del pipeline.

Entre las métricas consideradas se encuentran:

* Uso de CPU del microservicio.
* Uso de memoria del microservicio.
* Estado del servicio monitoreado por Prometheus.
* Cobertura de pruebas.
* Tiempo de despliegue.
* Métricas de disponibilidad y salud del sistema.
* Throughput y latencia del microservicio.
* Cantidad de errores detectados.

Estas métricas fueron seleccionadas porque permiten observar tanto el comportamiento técnico de la aplicación como la estabilidad del proceso de despliegue. Por ejemplo, la CPU y memoria ayudan a revisar el consumo de recursos, mientras que la cobertura de pruebas y el tiempo de despliegue permiten evaluar la calidad del proceso DevOps.

El dashboard permite al equipo tener una visión clara del estado del sistema y tomar decisiones de manera más rápida. Si una métrica muestra un comportamiento anormal, se puede revisar el pipeline, los logs o la configuración del microservicio para detectar el origen del problema.

**Evidencias adjuntas:**

* Captura de Prometheus consultando métricas de CPU.
* Captura de Prometheus consultando métricas de memoria.
* Captura del dashboard de Grafana.
* Captura de archivos de configuración en `monitoring/grafana` y `monitoring/prometheus`.

---

## 5. Integración en el pipeline CI/CD (IE4)

El pipeline CI/CD fue implementado en GitHub Actions mediante el archivo `.github/workflows/ci-cd.yml`. Este flujo automatiza diferentes etapas del proceso DevOps, desde la validación del código hasta el despliegue simulado.

El pipeline incluye las siguientes etapas:

1. **Test:** ejecuta pruebas automatizadas con `pytest` y genera reporte de cobertura.
2. **Calidad:** ejecuta análisis de código con SonarCloud y verifica el Quality Gate.
3. **Seguridad de dependencias:** ejecuta Snyk para analizar vulnerabilidades en las dependencias del proyecto.
4. **Build y escaneo:** construye la imagen Docker y ejecuta Trivy para escanear vulnerabilidades críticas.
5. **Deploy:** realiza login en GitHub Container Registry, construye y publica la imagen, y ejecuta un despliegue simulado.

La integración de estas herramientas en el pipeline permite automatizar controles que normalmente podrían hacerse de forma manual. Esto reduce errores, mejora la trazabilidad y entrega mayor seguridad antes de publicar una nueva versión del microservicio.

El pipeline también apoya la toma de decisiones porque muestra en qué etapa ocurre un problema. Por ejemplo, si fallan las pruebas, el equipo sabe que debe revisar el código. Si falla SonarCloud, se debe revisar calidad o cobertura. Si falla Snyk o Trivy, se debe revisar seguridad. Si falla deploy, se debe revisar la configuración de publicación o despliegue.

En la ejecución final, todos los jobs quedaron en estado exitoso, demostrando que el flujo CI/CD fue configurado correctamente.

**Evidencia principal:**

* Captura de GitHub Actions con todos los jobs en verde:

  * `test`
  * `calidad`
  * `seguridad-dependencias`
  * `build-y-scan`
  * `deploy`

---

## 6. Políticas de cumplimiento y auditoría (IE5)

Para cumplir con prácticas de seguridad y control de calidad, se integraron herramientas de análisis automático dentro del pipeline CI/CD.

Se utilizó **SonarCloud** para revisar la calidad del código y aplicar un Quality Gate. Esta herramienta permite identificar problemas de mantenibilidad, seguridad, confiabilidad y cobertura. Si el Quality Gate no se cumple, el pipeline puede detenerse.

También se utilizó **Snyk** para analizar vulnerabilidades en las dependencias del proyecto. Esta herramienta permite detectar librerías con problemas de seguridad y ayuda a prevenir que dependencias vulnerables sean incorporadas al flujo de despliegue. En el pipeline se configuró Snyk para bloquear vulnerabilidades críticas.

Además, se incorporó **Trivy** para escanear la imagen Docker generada durante el pipeline. Esto permite revisar vulnerabilidades del contenedor antes del despliegue. De esta manera, el equipo no solo revisa el código fuente, sino también la imagen que será ejecutada.

El repositorio también incluye un script de auditoría `scripts/audit.sh`, utilizado como apoyo para revisar configuraciones y evidencias del proyecto. Este tipo de script ayuda a mantener un control sobre el estado del repositorio y sus archivos principales.

Las políticas de cumplimiento permiten asegurar que el proyecto siga buenas prácticas antes de avanzar a despliegue. Esto ayuda a reducir riesgos, mejorar la calidad y dejar evidencia clara del proceso realizado.

**Evidencias adjuntas:**

* Captura de SonarCloud del proyecto.
* Captura del archivo `sonar-project.properties`.
* Captura de los secrets configurados en GitHub Actions:

  * `SONAR_TOKEN`
  * `SNYK_TOKEN`
* Captura del job `seguridad-dependencias` en verde.
* Captura del job `build-y-scan` en verde.
* Captura del archivo `.github/workflows/ci-cd.yml`.

---

## 7. Demostración de detención del pipeline (IE6)

Durante la configuración del pipeline se evidenció que el flujo puede detenerse cuando se detectan errores o vulnerabilidades. En una ejecución previa, el pipeline falló en la etapa `seguridad-dependencias` porque Snyk detectó vulnerabilidades en la dependencia `starlette`. Esto detuvo las etapas posteriores del pipeline, evitando continuar hacia build y deploy.

Posteriormente, se corrigieron las dependencias y la configuración del pipeline para permitir que la revisión de seguridad funcionara correctamente. También se evidenciaron fallas en la etapa `build-y-scan` por configuración de Trivy y luego en `deploy` por el nombre de la imagen Docker en GitHub Container Registry. Estos errores fueron corregidos hasta lograr una ejecución exitosa completa.

El mecanismo que detuvo el pipeline fue la configuración de los jobs en GitHub Actions. Cada etapa depende de la anterior mediante `needs`, por lo tanto, si una etapa falla, las siguientes no continúan. Esto demuestra una práctica importante de DevOps, ya que evita que una aplicación avance hacia despliegue si no cumple con pruebas, calidad o seguridad.

Finalmente, la ejecución final del pipeline terminó con todos los jobs en verde, demostrando que el flujo fue capaz de detectar errores, detener el proceso y luego completarse correctamente después de aplicar las correcciones.

**Evidencias adjuntas:**

* Captura de un pipeline en rojo en `seguridad-dependencias`.
* Captura del error de Snyk mostrando vulnerabilidades.
* Captura de un error de `build-y-scan`.
* Captura del error de `deploy` por nombre de imagen.
* Captura final del pipeline con estado `Success`.

---

## 8. Conclusiones

A través de esta evaluación se logró implementar un flujo DevOps más completo, integrando observabilidad, seguridad, calidad y automatización. El trabajo permitió comprender que un pipeline CI/CD no solo sirve para ejecutar pruebas, sino también para controlar que el software cumpla condiciones mínimas antes de avanzar a un despliegue.

Una de las principales conclusiones es que las herramientas de monitoreo como Prometheus y Grafana son fundamentales para observar el comportamiento de una aplicación. Gracias a estas herramientas, el equipo puede revisar métricas importantes como uso de CPU, memoria, disponibilidad y tiempo de despliegue. Esto permite detectar problemas antes de que afecten gravemente al sistema.

También se concluye que las herramientas de seguridad como Snyk y Trivy son importantes dentro de un flujo DevOps, porque permiten detectar vulnerabilidades en dependencias y contenedores. En este trabajo se comprobó que una vulnerabilidad puede detener el pipeline, evitando que una versión insegura llegue a una etapa de despliegue.

El uso de SonarCloud permitió reforzar la calidad del código mediante análisis automático y Quality Gate. Esto ayuda a mantener un repositorio más confiable y facilita la revisión del estado del proyecto.

Finalmente, el equipo logró configurar correctamente GitHub Actions, corregir errores del pipeline y obtener una ejecución final exitosa con todos los jobs en verde. Esto demuestra que el proyecto cumple con los objetivos principales de observabilidad, cumplimiento, seguridad y automatización solicitados en la evaluación.

---

## 9. Reflexiones individuales

### Integrante 1: Gefmy Louima

En esta evaluación aprendí la importancia de trabajar con un pipeline CI/CD real y entender cada etapa del proceso. Al principio hubo varios errores en GitHub Actions, SonarCloud, Snyk, Trivy y Docker, pero al revisar cada problema paso a paso pude comprender mejor cómo funciona la automatización en DevOps.

Mi principal aprendizaje fue que un pipeline no solo ejecuta comandos, sino que ayuda a controlar la calidad y seguridad del proyecto. También aprendí que las herramientas de monitoreo y observabilidad permiten revisar el estado de una aplicación y detectar problemas de forma más rápida.

Mi contribución en el trabajo fue participar en la configuración del repositorio, revisión de errores, ejecución del pipeline, corrección de dependencias, configuración de secrets y recopilación de evidencias. También participé en la validación final del pipeline hasta lograr que todos los jobs quedaran en verde.

Esta experiencia me ayudó a entender mejor cómo se trabaja en un entorno DevOps real y cómo se integran herramientas como GitHub Actions, Docker, Prometheus, Grafana, SonarCloud, Snyk y Trivy.

### Integrante 2: Odens Jean Anderson

En esta evaluación aprendí que la observabilidad es una parte muy importante dentro de DevOps, porque permite conocer el estado de una aplicación y revisar si está funcionando correctamente. También comprendí que el monitoreo no se limita a ver si un servicio está encendido, sino que permite analizar métricas, rendimiento, errores y disponibilidad.

Durante el desarrollo del trabajo pude reforzar conocimientos sobre Kubernetes, Docker, Prometheus, Grafana y GitHub Actions. Además, entendí mejor la importancia de las herramientas de seguridad dentro del pipeline, ya que permiten detectar vulnerabilidades antes de avanzar hacia el despliegue.

Mi contribución fue apoyar en la revisión de la estructura del proyecto, análisis de los archivos de configuración, validación de evidencias y revisión del funcionamiento general del pipeline. También participé en la documentación de las herramientas implementadas y en la revisión de los resultados obtenidos.

Como aprendizaje final, considero que DevOps permite mejorar el trabajo en equipo, automatizar procesos y entregar software con mayor seguridad y calidad.

---

## 10. Declaración de uso de Inteligencia Artificial

Según la pauta, se declara el uso de IA en este trabajo:

| Herramienta de IA                         | ¿Para qué se usó?                                                                                                                                                               | Secciones                                                              |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| ChatGPT                                   | Apoyo para resolver errores técnicos del pipeline CI/CD, configuración de GitHub Actions, SonarCloud, Snyk, Trivy, Docker y redacción guiada de secciones técnicas del informe. | Pipeline, documentación técnica, secciones 1 a 7 y apoyo en evidencias |
| Claude                                    | Apoyo previo en estructura técnica del proyecto, andamiaje de código, configuración de observabilidad y revisión general del repositorio.                                       | Código del repo, README, docs                                          |
| GitHub Copilot / herramientas automáticas | Apoyo indirecto mediante sugerencias del entorno de desarrollo y automatización de dependencias con Dependabot.                                                                 | Código, dependencias, GitHub Actions                                   |

Las conclusiones, justificaciones técnicas solicitadas por la pauta y reflexiones individuales fueron revisadas y adaptadas por el equipo.

Referencia de citación: https://bibliotecas.duoc.cl/ia
