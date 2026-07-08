# Proyecto Final Transversal - Ingenieria DevOps DOY0101

## 1. Resumen del proyecto

Este repositorio corresponde a la Evaluacion Final Transversal de Ingenieria DevOps. El objetivo es demostrar el ciclo DevOps completo aplicado a un microservicio: control de versiones, ramas, pull requests, pipeline CI/CD, contenedores, pruebas, seguridad, despliegue simulado y monitoreo.

El proyecto integra los aprendizajes de las tres evaluaciones parciales:

| Etapa | Trabajo realizado | Evidencia principal |
| --- | --- | --- |
| EP1 | Repositorio Git, ramas, commits, pull requests y convenciones | Historial Git, ramas, README |
| EP2 | Docker, pruebas, GitHub Actions, seguridad y despliegue simulado | `Dockerfile`, `.github/workflows/ci-cd.yml`, `docker-compose.yml` |
| EP3 | Observabilidad, metricas, logs y dashboard | Prometheus, Grafana, `/metrics`, dashboard |

## 2. Microservicio trabajado

La aplicacion es un microservicio construido con FastAPI. Incluye endpoints basicos para validar estado, crear items, listar items y exponer metricas para Prometheus.

Endpoints principales:

| Endpoint | Funcion |
| --- | --- |
| `/` | Respuesta principal del servicio |
| `/health` | Verifica que el microservicio este activo |
| `/items` | Lista o crea items de prueba |
| `/metrics` | Expone metricas para Prometheus |

## 3. Estrategia de ramas

Para la entrega final se documenta una estrategia tipo GitFlow simplificado:

- `main`: version estable y lista para evaluacion.
- `develop`: integracion de cambios antes de pasar a `main`.
- `feature/evidencias-eft`: rama recomendada para agregar evidencias, documentacion y ajustes finales.
- `hotfix/...`: rama para correcciones urgentes.

La idea es evitar cambios directos en `main`. Cada cambio importante debe pasar por una rama temporal, commit claro y pull request.

Comandos sugeridos:

```bash
git checkout main
git pull
git checkout -b feature/evidencias-eft
git add .
git commit -m "docs: agrega documentacion final EFT"
git push origin feature/evidencias-eft
```

Luego se debe crear un Pull Request en GitHub desde `feature/evidencias-eft` hacia `main`.

## 4. Convenciones de commits

Se recomienda usar mensajes simples y claros:

| Tipo | Uso |
| --- | --- |
| `feat:` | nueva funcionalidad |
| `fix:` | correccion de error |
| `docs:` | documentacion |
| `test:` | pruebas |
| `ci:` | pipeline o automatizacion |
| `chore:` | ajustes menores |

Ejemplos:

```bash
git commit -m "ci: ajusta pipeline final EFT"
git commit -m "docs: agrega guia de presentacion"
git commit -m "fix: corrige referencia de despliegue Docker Compose"
```

## 5. Pipeline CI/CD

El pipeline se encuentra en `.github/workflows/ci-cd.yml` y se ejecuta con push a `main` o `develop`, y con pull request hacia `main`.

Etapas del pipeline:

1. Ejecuta pruebas con `pytest`.
2. Genera cobertura de pruebas.
3. Ejecuta analisis de calidad con SonarCloud.
4. Ejecuta analisis de dependencias con Snyk.
5. Construye imagen Docker.
6. Escanea la imagen con Trivy.
7. Valida el despliegue simulado con Docker Compose.

Si una etapa falla, las etapas posteriores no continúan. Esto demuestra control de calidad dentro del flujo DevOps.

## 6. Docker y despliegue simulado

El proyecto usa Docker para ejecutar el microservicio en un contenedor. El stack completo se levanta con Docker Compose:

```bash
docker compose up -d --build
```

Servicios:

| Servicio | Puerto | Funcion |
| --- | --- | --- |
| Microservicio | `8000` | API FastAPI |
| Prometheus | `9090` | Recoleccion de metricas |
| Grafana | `3000` | Dashboard de visualizacion |

Para detener:

```bash
docker compose down
```

## 7. Seguridad y calidad

El proyecto considera controles automatizados:

- SonarCloud o SonarQube para calidad del codigo.
- Snyk para revision de dependencias.
- Trivy para revision de imagen Docker.
- Dependabot para actualizacion de dependencias.
- Secrets de GitHub para no escribir tokens en el codigo.

Secretos necesarios en GitHub Actions:

```text
SONAR_TOKEN
SNYK_TOKEN
```

## 8. Monitoreo y observabilidad

El microservicio expone metricas en `/metrics`. Prometheus consulta esas metricas y Grafana las muestra en un dashboard.

Metricas usadas para la presentacion:

- disponibilidad del servicio;
- cantidad de solicitudes;
- latencia;
- errores;
- memoria;
- CPU;
- estado de Prometheus.

Esto permite detectar problemas y tomar decisiones tecnicas con evidencia.

## 9. Comandos de validacion

Instalar dependencias:

```bash
pip install -r requirements.txt
```

Ejecutar pruebas:

```bash
pytest --cov=app --cov-report=term
```

Levantar ambiente:

```bash
docker compose up -d --build
```

Generar trafico de prueba:

```bash
curl http://localhost:8000/health
curl http://localhost:8000/metrics
curl -X POST http://localhost:8000/items -H "Content-Type: application/json" -d "{\"nombre\":\"Producto EFT\",\"precio\":1990}"
curl http://localhost:8000/items/999
```

Ver logs:

```bash
docker compose logs -f
```

## 10. Evidencias que deben agregarse

Crear una carpeta `evidencias-eft/` y guardar capturas actuales:

- repositorio GitHub;
- ramas del repositorio;
- pull request de `feature/evidencias-eft` a `main`;
- GitHub Actions en verde;
- Docker Compose levantado;
- endpoint `/health`;
- endpoint `/metrics`;
- Prometheus con target `UP`;
- Grafana con dashboard;
- SonarCloud, Snyk, Trivy o Dependabot.

## 11. Uso de inteligencia artificial

Se utilizo IA como apoyo para ordenar redaccion, revisar estructura de documentacion y preparar una guia de presentacion. Las evidencias, validaciones y explicaciones deben ser revisadas por los integrantes antes de entregar.

Referencia institucional: https://bibliotecas.duoc.cl/ia

## 12. Conclusion personal

Este proyecto muestra que DevOps no es solo programar. DevOps permite ordenar el trabajo con Git, automatizar validaciones con CI/CD, ejecutar la aplicacion con Docker, revisar seguridad y observar el sistema con metricas.

Para la presentacion final, lo mas importante es demostrar el repositorio funcionando y explicar con palabras propias cada parte del flujo.

