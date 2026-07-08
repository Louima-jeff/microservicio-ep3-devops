# Microservicio EP3 — Observabilidad y entornos reales en DevOps

Extensión del pipeline DevOps de la EP2, incorporando **observabilidad** mediante monitoreo, métricas, dashboard y validación de cumplimiento normativo dentro del ciclo CI/CD.

> Asignatura: Ingeniería DevOps (DOY0101) · Evaluación Parcial N°3 (30%) · Trabajo en parejas

---

## 1. ¿Qué hace este proyecto?

| Componente                          | Herramienta                                                | Indicador |
| ----------------------------------- | ---------------------------------------------------------- | --------- |
| Monitoreo, logs y métricas          | **Prometheus** + instrumentación FastAPI                   | IE1       |
| Despliegue en entorno simulado      | **Docker Compose local**                                   | IE2       |
| Dashboard de métricas clave         | **Grafana**                                                | IE3       |
| Documentación de integración        | Este README + `INFORME.md` + `docs/`                       | IE4       |
| Cumplimiento y auditoría            | **SonarCloud** + **Snyk** + **Trivy** + `scripts/audit.sh` | IE5       |
| Pipeline que se detiene ante fallas | Quality Gate + Snyk/Trivy con bloqueo                      | IE6       |

> Nota: De acuerdo con la indicación del docente, **Kubernetes fue descartado para esta evaluación**. Por lo tanto, el ambiente final se ejecuta mediante **Docker Compose en entorno local**.

---

## 2. Estructura del repositorio

```text
.
├── app/                      # Microservicio FastAPI instrumentado con Prometheus
│   ├── main.py               # Endpoints + /metrics + logging
│   └── metrics.py            # Métricas de negocio personalizadas
├── tests/                    # Pruebas automatizadas con pytest
├── monitoring/
│   ├── prometheus/           # Configuración de scrape + reglas de alerta
│   └── grafana/              # Datasource + dashboard provisionado
├── k8s/                      # Archivos referenciales no utilizados en la entrega final
├── scripts/audit.sh          # Auditoría de cumplimiento local
├── .github/
│   ├── workflows/ci-cd.yml   # Pipeline CI/CD
│   └── dependabot.yml        # Actualización de dependencias
├── docs/                     # Documentación de arquitectura
├── Dockerfile                # Imagen del microservicio
├── docker-compose.yml        # Stack local: app + Prometheus + Grafana
├── sonar-project.properties  # Configuración SonarCloud
├── INFORME.md                # Informe de la evaluación
└── README.md
```

---

## 3. Cómo levantar todo en local

Para ejecutar el ambiente completo se utiliza Docker Compose:

```bash
docker compose up -d --build
```

Luego abre los siguientes servicios:

| Servicio      | URL                           | Notas                              |
| ------------- | ----------------------------- | ---------------------------------- |
| Microservicio | http://localhost:8000         | API principal                      |
| Healthcheck   | http://localhost:8000/health  | Verifica estado del microservicio  |
| Métricas      | http://localhost:8000/metrics | Métricas expuestas para Prometheus |
| Prometheus    | http://localhost:9090         | Monitoreo y consulta de métricas   |
| Grafana       | http://localhost:3000         | Usuario `admin` / clave `admin`    |

En Grafana el dashboard **"Microservicio EP3 - Observabilidad"** ya viene cargado automáticamente.

Para generar tráfico y ver datos en Grafana:

```bash
curl http://localhost:8000/health
curl http://localhost:8000/metrics
curl -X POST http://localhost:8000/items -H "Content-Type: application/json" -d "{\"nombre\":\"Producto\",\"precio\":1990}"
curl http://localhost:8000/items/999
```

---

## 4. Observabilidad implementada

El proyecto incorpora observabilidad usando:

* **Prometheus**, para recolectar métricas del endpoint `/metrics`.
* **Grafana**, para visualizar métricas del microservicio.
* **Métricas HTTP**, para revisar solicitudes, latencia, errores y estado del servicio.
* **Métricas de proceso**, para revisar CPU y memoria.

El dashboard de Grafana muestra:

* Throughput de solicitudes.
* Latencia p95.
* Errores registrados 5xx y errores de negocio.
* Uso de CPU del proceso.
* Uso de memoria.
* Estado del microservicio.
* Solicitudes totales.
* Estado de Prometheus.

La conexión entre Grafana y Prometheus se configura automáticamente mediante:

```text
monitoring/grafana/provisioning/datasources/datasource.yml
```

El dashboard se provisiona desde:

```text
monitoring/grafana/dashboards/microservicio-dashboard.json
```

---

## 5. Integración en el pipeline CI/CD

El pipeline `.github/workflows/ci-cd.yml` ejecuta la siguiente secuencia. Cada etapa que falla detiene las siguientes:

1. **test**
   Ejecuta pruebas automatizadas con `pytest` y genera cobertura.

2. **calidad**
   Ejecuta análisis de calidad con SonarCloud y valida el Quality Gate.

3. **seguridad-dependencias**
   Ejecuta análisis de dependencias con Snyk. Si existen vulnerabilidades altas, el job puede fallar.

4. **build-y-scan**
   Construye la imagen Docker y ejecuta análisis de vulnerabilidades con Trivy.

5. **deploy**
   Solo si todo lo anterior pasó y estamos en `main`, se valida el despliegue simulado mediante **Docker Compose**. El ambiente local incluye el microservicio, Prometheus y Grafana, permitiendo evidenciar observabilidad sin utilizar Kubernetes.

---

## 6. Cómo esto apoya la toma de decisiones técnicas

Las métricas y alertas permiten observar el comportamiento real del microservicio.

* Prometheus permite revisar si el servicio está activo.
* Grafana permite visualizar consumo de CPU, memoria, throughput, latencia y errores.
* SonarCloud ayuda a revisar calidad de código.
* Snyk y Trivy permiten detectar vulnerabilidades.
* El pipeline CI/CD bloquea el avance cuando existen fallas relevantes.

Esto permite tomar decisiones técnicas basadas en evidencia, por ejemplo:

* Corregir errores antes de entregar.
* Mejorar rendimiento.
* Revisar consumo de recursos.
* Detectar problemas de seguridad.
* Evitar entregar código con fallas críticas.

---

## 7. Demostración de que el pipeline se detiene

El pipeline puede detenerse por distintos mecanismos:

1. **Quality Gate de SonarCloud**
   Si el proyecto no cumple las reglas de calidad, el job de calidad falla.

2. **Snyk**
   Si se detectan vulnerabilidades relevantes en dependencias, se reporta el problema en el pipeline.

3. **Trivy**
   Si la imagen Docker contiene vulnerabilidades críticas o altas, el escaneo puede detener el flujo.

4. **scripts/audit.sh**
   Permite validar localmente aspectos de cumplimiento, configuración y seguridad.

Estos mecanismos demuestran que el pipeline no solo automatiza tareas, sino que también actúa como control de calidad y seguridad.

---

## 8. Configuración previa

Para ejecutar correctamente el pipeline se requieren secretos en GitHub:

```text
SONAR_TOKEN
SNYK_TOKEN
```

Estos secretos se configuran en:

```text
Settings → Secrets and variables → Actions
```

SonarCloud se configura mediante:

```text
sonar-project.properties
```

No se utiliza Kubernetes en esta entrega, según la indicación del docente. El ambiente se levanta con Docker Compose en entorno local.

---

## 9. Acceso y credenciales para revisión

**Repositorio GitHub:**
https://github.com/Louima-jeff/microservicio-ep3-devops

**Rama utilizada:**
main

**Integrantes:**

* Gefmy Louima
* Jean Odens Anderson

**Forma de ejecución del ambiente:**
Docker Compose en ambiente local.

**Comando de ejecución:**

```bash
docker compose up -d --build
```

**Credenciales Grafana:**

* Usuario: admin
* Contraseña: admin

**URLs locales:**

| Servicio      | URL                           |
| ------------- | ----------------------------- |
| Microservicio | http://localhost:8000         |
| Health        | http://localhost:8000/health  |
| Metrics       | http://localhost:8000/metrics |
| Prometheus    | http://localhost:9090         |
| Grafana       | http://localhost:3000         |

**SonarCloud:**
https://sonarcloud.io/project/overview?id=Louima-jeff_microservicio-ep3-devops

**Snyk:**
Proyecto configurado con GitHub Actions mediante `SNYK_TOKEN`. La evidencia se incluye en `README.md`, `INFORME.md` y capturas del pipeline.

**Acceso para revisión:**

* GitHub: nicosingh
* SonarCloud: [nico@singh.cl](mailto:nico@singh.cl)
* Snyk: [nico@singh.cl](mailto:nico@singh.cl)

---

## 10. Evidencias incluidas

La entrega incluye evidencias de:

* Repositorio GitHub en rama `main`.
* README actualizado.
* INFORME.md.
* GitHub Actions con jobs en verde.
* SonarCloud con Quality Gate Passed.
* Docker Compose funcionando.
* Contenedores Docker activos.
* Microservicio `/health` funcionando.
* Endpoint `/metrics` funcionando.
* Prometheus recolectando métricas.
* Grafana conectado correctamente a Prometheus.
* Dashboard Grafana con métricas reales.
* Repositorio actualizado sin cambios pendientes.

---

## 11. Declaración de uso de IA

Se utilizó apoyo de inteligencia artificial para mejorar redacción, ordenar documentación y apoyar la explicación técnica de la entrega.

El uso de IA fue revisado por los integrantes y adaptado al contexto del proyecto. La responsabilidad final del contenido, configuración y evidencias corresponde al equipo.

---

## 12. Observación final

La entrega se documenta con evidencias en `README.md`, `INFORME.md` y documento Word con capturas. El ambiente se ejecuta con Docker Compose, de acuerdo con la indicación del docente que descarta Kubernetes para esta evaluación.

El pipeline CI/CD finalizó correctamente con los jobs principales en verde: `test`, `calidad`, `seguridad-dependencias`, `build-y-scan` y `deploy`.

El dashboard de Grafana fue corregido y conectado a Prometheus mediante datasource provisionado, permitiendo visualizar métricas reales del microservicio.
