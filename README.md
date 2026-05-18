# Ejemplo 1
## Edicion de HTML con Vi
<img width="581" height="370" alt="UsoDeVi" src="https://github.com/user-attachments/assets/c45c55fd-8b0f-47c2-b3f4-49f629ac9071" />

## HTML Corriendo en LocalHost
<img width="1919" height="1040" alt="CapturaDelResultado" src="https://github.com/user-attachments/assets/fab9b8d5-b67a-4779-a9a5-24b4cd37c863" />

## Evidencia del contenedor funcionando
<img width="1607" height="42" alt="CapturaDeContenedor" src="https://github.com/user-attachments/assets/e36503a7-540c-45c3-9161-fc0875972ccb" />


# Ejemplo 2
## Wordpress Corriendo en LocalHost
<img width="1919" height="1043" alt="CapturaDeWordpressCorriendo" src="https://github.com/user-attachments/assets/99f4055c-e088-4f44-81e8-af094ff92054" />

## Evidencia de contenedores funcionando
<img width="1597" height="82" alt="image" src="https://github.com/user-attachments/assets/f6cceed9-16cd-4d26-83e4-4f63b8740176" />

# Ejemplo 3
## Ejecucion de Run.sh en Bash de GIT (Linux)
<img width="775" height="624" alt="image" src="https://github.com/user-attachments/assets/693b15bf-f4d6-4e96-a424-bab1b750c41d" />

## 📋 Evaluación del Script de S.O. y Problemas de Portabilidad

A continuación se detallan los inconvenientes técnicos identificados al utilizar scripts de sistema operativo (`.sh`, `.bat`, `.ps1`) para la automatización y despliegue de entornos contenedores.

### 🔴 Falta de Portabilidad Absoluta

* **Dependencia del Entorno:** Un archivo `.sh` requiere estrictamente un intérprete de comandos Bash o Sh. En sistemas operativos Windows, este no se ejecuta de forma nativa en la consola clásica (CMD) ni en PowerShell, lo que obliga al usuario a instalar capas de emulación de terceros como *Git Bash*, *WSL (Windows Subsystem for Linux)* o *MINGW64*.
* **El Problema del "Path Conversion":** Las rutas de archivos difieren drásticamente entre sistemas operativos. Mientras que Linux utiliza una estructura limpia como `/var/www/html`, Windows emplea rutas absolutas basadas en unidades (`C:\\Users\\...`). Los emuladores de terminal intentan traducir estas rutas de manera automática, lo que genera fallos críticos de configuración en el daemon de Docker (`invalid mount path`) a menos que se introduzcan variables de entorno paliativas como `MSYS_NO_PATHCONV=1`.

### 🔴 Mantenimiento Tedioso e Ineficiente

* **Manejo de Errores Manual (Scripts Imperativos):** Los scripts de consola tradicionales siguen un flujo secuencial e imperativo. Si un recurso intermedio —como la red virtual `mi-network`— ya fue creado en un intento previo, el comando `docker network create` fallará interrumpiendo la ejecución total del script o continuando con un estado corrupto, a menos que se programe lógica condicional compleja en Bash para la validación previa de recursos.
* **Duplicación de Código para Multiplataforma:** En equipos de desarrollo heterogéneos (donde conviven desarrolladores en Windows nativo, macOS y distribuciones Linux), la automatización mediante scripts de S.O. obliga a mantener y sincronizar múltiples archivos con la misma lógica de negocio pero distinta sintaxis (`run.sh`, `run.ps1` y `run.bat`).

---

### 🛠️ La Solución de la Industria: Docker Compose

Para mitigar la fragilidad de los scripts de consola en la orquestación local, los entornos profesionales adoptan un enfoque **declarativo** mediante **Docker Compose** en lugar de herramientas imperativas.

A través de un archivo único de configuración universal (`docker-compose.yml`), la herramienta abstrae por completo las discrepancias del sistema operativo anfitrión y se encarga de:

1. **Ciclo de Vida Automatizado:** Crear, verificar y enlazar las redes virtuales (`networks`) de forma transparente sin lanzar excepciones si ya existen.
2. **Abstracción de Rutas y Volúmenes:** Gestionar montajes de tipo *bind* o volúmenes de datos con una sintaxis unificada que elimina los errores de traducción de rutas (`$(pwd)` frente a `%cd%`).
3. **Comando Único Universal:** Desplegar, reconstruir y vincular la totalidad de la arquitectura de microservicios con una sola instrucción estándar para cualquier plataforma:

```bash
docker compose up -d
