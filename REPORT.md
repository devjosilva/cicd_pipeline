# Pipeline Execution Report

## Summary

Este documento detalla los pasos realizados para implementar un sistema de integración continua para garantizar la calidad del código y realizar despliegues rápidos y confiables.

## Steps

1. **Git Repository Management**  
     
- Se inicializó un repositorio local con `git init`
- Se realizó el commit inicial
- Se creó un repositorio remoto en GitHub y se vinculó con el repositorio local
- Se instaló json-server para ejecutar la API
   

2. **Jenkins Configuration**  
     
  - Se creó un Jenkinsfile con las etapas del pipeline:
  - Checkout del código
  - Instalación de dependencias
  - Ejecución de pruebas
  - Se configuró Jenkins para ejecutar el pipeline desde el repositorio GitHub

## Issues Encountered

Al ejecutar el pipeline en Jenkins, se encontraron problemas de permisos al intentar construir la imagen Docker.
**Solución**: Se añadió el usuario Jenkins al grupo Docker y se reinició el servicio.

Inicialmente, algunas pruebas fallaban debido a problemas con el entorno de pruebas.
**Solución**: Se modificó la configuración de Jest para usar --forceExit y asegurar que los procesos se cerraran correctamente después de las pruebas..

## Results

Pruebas: 3 pruebas ejecutadas, 3 pasaron
Cobertura: 100% de cobertura de código
Docker: Imagen construida correctamente
Pipeline: Ejecución exitosa del pipeline completo

