# Cambios preparados para la Evaluacion Final Transversal

Estos cambios dejan el repositorio mas claro para la defensa final:

1. El `README.md` ahora esta orientado a la EFT, no solo a la EP3.
2. Se agrego `README_EFT.md` como respaldo del README final.
3. Se agrego `docs/GUIA_PRESENTACION_EFT.md` con un guion oral de 6 a 8 minutos.
4. Se agrego `docs/EVIDENCIAS_EFT.md` con checklist de capturas.
5. Se corrigio el pipeline para hablar de Docker Compose y no de Kubernetes simulado.
6. Se corrigieron comentarios del microservicio relacionados con healthcheck.
7. Se actualizo `.gitignore` para evitar subir archivos comprimidos al repositorio.

## Pendiente que debe hacer el estudiante en GitHub

- Crear una rama `feature/evidencias-eft`.
- Subir estos cambios con commits propios.
- Crear Pull Request hacia `main`.
- Tomar capturas nuevas.
- Agregar carpeta `evidencias-eft/` con capturas reales.
- Revisar que GitHub Actions quede en verde.

## Comandos sugeridos

```bash
git checkout main
git pull
git checkout -b feature/evidencias-eft
git add README.md README_EFT.md CAMBIOS_EFT.md docs/GUIA_PRESENTACION_EFT.md docs/EVIDENCIAS_EFT.md .github/workflows/ci-cd.yml app/main.py .gitignore
git commit -m "docs: prepara entrega final EFT"
git push origin feature/evidencias-eft
```

Despues crear el Pull Request en GitHub.

