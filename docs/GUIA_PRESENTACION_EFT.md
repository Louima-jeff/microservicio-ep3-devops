# Guia de presentacion oral EFT

Duracion recomendada: 6 a 8 minutos.

## 1. Inicio

Buenos dias profesor. Mi nombre es Gefmy Louima y voy a presentar el proyecto final de Ingenieria DevOps. El objetivo fue aplicar un ciclo DevOps completo sobre un microservicio, usando GitHub, GitHub Actions, Docker, pruebas, seguridad y monitoreo.

## 2. Repositorio y ramas

En el repositorio se usa una estrategia de ramas para mantener orden y trazabilidad. `main` representa la version estable. `develop` se usa para integracion. Las ramas `feature` sirven para nuevas funcionalidades y las ramas `hotfix` para correcciones urgentes.

Lo importante es que los cambios no lleguen directo a `main`, sino mediante commits y pull requests.

## 3. Pipeline CI/CD

El pipeline esta configurado con GitHub Actions. Cuando se suben cambios, se ejecutan pruebas, cobertura, calidad, seguridad, construccion Docker y validacion del despliegue simulado.

Si una etapa falla, el pipeline se detiene. Eso evita avanzar con codigo que tiene errores o vulnerabilidades criticas.

## 4. Docker

Docker permite ejecutar la aplicacion dentro de un contenedor. Con Docker Compose se levantan tres servicios: microservicio, Prometheus y Grafana.

Esto facilita que el proyecto pueda ejecutarse en otro computador con el mismo comando.

## 5. Seguridad y calidad

El proyecto considera herramientas como SonarCloud, Snyk, Trivy y Dependabot. Estas herramientas revisan calidad del codigo, dependencias vulnerables e imagen Docker.

Los tokens no van escritos en el codigo. Se configuran como secrets en GitHub.

## 6. Monitoreo

El microservicio expone metricas en `/metrics`. Prometheus las recolecta y Grafana las muestra en un dashboard.

El dashboard ayuda a revisar disponibilidad, errores, latencia, memoria y CPU.

## 7. Cierre

Como conclusion, este trabajo me ayudo a entender que DevOps une desarrollo, automatizacion, seguridad y operacion. El valor principal es detectar problemas antes de entregar y tener evidencia del estado del sistema.

## Preguntas que puede hacer el profesor

| Pregunta | Respuesta corta |
| --- | --- |
| Que es CI/CD? | Integracion y entrega continua para validar y preparar cambios automaticamente. |
| Para que sirve Docker? | Para ejecutar la aplicacion en un ambiente controlado y reproducible. |
| Que pasa si falla una prueba? | El pipeline se detiene y no continua hacia build o deploy. |
| Para que sirve Prometheus? | Para recolectar metricas del microservicio. |
| Para que sirve Grafana? | Para visualizar metricas en un dashboard. |
| Por que usar ramas? | Para ordenar el trabajo y mantener trazabilidad. |

