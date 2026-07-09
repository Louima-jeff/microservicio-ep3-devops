# Proyecto Final Transversal - Ingeniería DevOps DOY0101

## Integrantes

- Gefmy Louima
- Jean Odens Anderson

## Repositorio

https://github.com/Louima-jeff/microservicio-ep3-devops

---

## 1. Resumen del proyecto

Este proyecto corresponde a la Evaluación Final Transversal de Ingeniería DevOps.

El objetivo es demostrar un flujo DevOps completo aplicado a un microservicio, usando GitHub, ramas, Pull Requests, GitHub Actions, Docker, pruebas automatizadas, seguridad, monitoreo y dashboard.

---

## 2. Etapas del proyecto

| Etapa | Trabajo realizado |
|---|---|
| EP1 | Repositorio GitHub, ramas, commits y Pull Requests |
| EP2 | Docker, pruebas, GitHub Actions y seguridad |
| EP3 | Prometheus, Grafana, métricas y observabilidad |
| EFT | Presentación y defensa final del proyecto |

---

## 3. Herramientas utilizadas

| Herramienta | Uso |
|---|---|
| GitHub | Repositorio del proyecto |
| Git | Control de versiones |
| GitHub Actions | Pipeline CI/CD |
| FastAPI | Microservicio en Python |
| Pytest | Pruebas automatizadas |
| Docker | Contenedor del microservicio |
| Docker Compose | Despliegue simulado local |
| SonarCloud | Calidad de código |
| Snyk | Seguridad de dependencias |
| Trivy | Seguridad de imagen Docker |
| Prometheus | Recolección de métricas |
| Grafana | Dashboard de monitoreo |

---

## 4. Microservicio

El microservicio está construido con FastAPI, una herramienta de Python para crear APIs.

Endpoints principales:

| Endpoint | Función |
|---|---|
| `/` | Respuesta principal |
| `/health` | Verifica que el servicio está activo |
| `/items` | Endpoint de prueba |
| `/metrics` | Métricas para Prometheus |

---

## 5. Estrategia de ramas

Se utilizó una estrategia simple de ramas:

- `main`: rama principal y estable.
- `develop`: rama para integrar cambios.
- `feature/evidencias-eft`: rama usada para preparar la entrega final.
- `hotfix/...`: rama para correcciones rápidas.

Para la entrega final se creó la rama `feature/evidencias-eft`, se subieron cambios y luego se hizo un Pull Request hacia `main`.

---

## 6. Pipeline CI/CD

El pipeline está en:

```text
.github/workflows/ci-cd.yml
```

El pipeline realiza:

1. Pruebas automatizadas con Pytest.
2. Análisis de calidad con SonarCloud.
3. Revisión de seguridad con Snyk.
4. Construcción de imagen Docker.
5. Escaneo de imagen con Trivy.
6. Validación de despliegue con Docker Compose.

Si una etapa falla, el pipeline se detiene.

---

## 7. Docker Compose

Para levantar el proyecto:

```bash
docker compose up -d --build
```

Servicios disponibles:

| Servicio | URL |
|---|---|
| Microservicio | http://localhost:8000 |
| Health | http://localhost:8000/health |
| Metrics | http://localhost:8000/metrics |
| Prometheus | http://localhost:9090 |
| Grafana | http://localhost:3000 |

Credenciales de Grafana:

```text
Usuario: admin
Contraseña: admin
```

---

## 8. Monitoreo

El microservicio entrega métricas en:

```text
http://localhost:8000/metrics
```

Prometheus lee esas métricas y Grafana las muestra en un dashboard.

Métricas importantes:

- Estado del servicio.
- Cantidad de solicitudes.
- Errores.
- Latencia.
- Memoria.
- CPU.

---

## 9. Evidencias

La entrega considera:

- Repositorio GitHub actualizado.
- Pull Request fusionado a `main`.
- GitHub Actions en verde.
- SonarCloud aprobado.
- Docker Compose funcionando.
- Endpoint `/health`.
- Endpoint `/metrics`.
- Prometheus funcionando.
- Grafana con dashboard.

---

## 10. Presentación individual

La presentación será realizada por **Gefmy Louima**.

Durante la presentación se explicarán los puntos principales del proyecto:

- Repositorio GitHub.
- Estrategia de ramas.
- Pull Request.
- GitHub Actions.
- Pruebas automatizadas.
- Pipeline CI/CD.
- Docker.
- Docker Compose.
- Seguridad con SonarCloud, Snyk y Trivy.
- Prometheus.
- Grafana.
- Dashboard de monitoreo.

Aunque el proyecto fue desarrollado en pareja, la defensa oral será realizada de forma individual, demostrando dominio general del flujo DevOps implementado.

---

## 11. Uso de IA

Se utilizó inteligencia artificial como apoyo para ordenar la documentación y mejorar la redacción.

Referencia:

https://bibliotecas.duoc.cl/ia

---

## 12. Conclusión

Este proyecto demuestra un flujo DevOps completo.

Se usó GitHub para controlar versiones, GitHub Actions para automatizar, Docker para ejecutar la aplicación, herramientas de seguridad para revisar problemas y Prometheus/Grafana para monitorear el microservicio.

La entrega final permite demostrar el trabajo realizado durante el semestre y explicar con palabras propias cada parte del proceso DevOps.