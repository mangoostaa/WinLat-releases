# WinLat

**Convertí archivos desde Windows con un menú circular, una cola de trabajo y actualizaciones automáticas.**

[![Windows](https://img.shields.io/badge/Windows-10%20%2F%2011-0078D4)](#requisitos)
[![Arquitectura](https://img.shields.io/badge/arquitectura-x64-555555)](#requisitos)
[![Idioma](https://img.shields.io/badge/interfaz-espa%C3%B1ol%20%2F%20English-orange)](#configuración)

**[Descargar desde Releases](https://github.com/mangoostaa/WinLat-releases/releases)** · **[Última versión publicada](https://github.com/mangoostaa/WinLat-releases/releases/latest)** · **[Reportar un problema](https://github.com/mangoostaa/WinLat-releases/issues)**

Este repositorio distribuye los instaladores y las novedades de WinLat. Los archivos de instalación se publican en **Releases**. Para instalar la aplicación, descargá el archivo `WinLat-Setup-x64.exe` adjunto a una versión; los archivos automáticos **Source code (zip)** y **Source code (tar.gz)** son archivos del repositorio y no contienen el instalador.

> La actualización automática está incorporada desde **WinLat 1.2.0**. Si usás una versión anterior sin actualizador, necesitás instalar una versión con actualizador una vez. Después, WinLat podrá descargar e instalar las próximas versiones automáticamente.

Si todavía no aparece ninguna versión en Releases, el instalador aún no se ha publicado. El enlace a la última versión estará disponible cuando se publique el primer release estable.

## Contenido

- [Qué podés hacer](#qué-podés-hacer)
- [Requisitos](#requisitos)
- [Descarga e instalación](#descarga-e-instalación)
- [Cómo usar WinLat](#cómo-usar-winlat)
- [Formatos y tipos de conversión](#formatos-y-tipos-de-conversión)
- [Actualizaciones automáticas](#actualizaciones-automáticas)
- [Configuración](#configuración)
- [Herramientas adicionales](#herramientas-adicionales)
- [Archivos, preferencias y registros](#archivos-preferencias-y-registros)
- [Solución de problemas](#solución-de-problemas)
- [Desinstalación](#desinstalación)
- [Cómo publicar una nueva versión](#cómo-publicar-una-nueva-versión)
- [Componentes de terceros](#componentes-de-terceros)

## Qué podés hacer

- Convertir imágenes, audio y video con los componentes opcionales incluidos en el instalador.
- Copiar texto con otra extensión, envolver contenido en plantillas y transformar datos en las combinaciones disponibles.
- Abrir un menú circular desde el Explorador con **Espacio + arrastrar**.
- Agregar archivos desde la ventana principal y procesarlos en una cola, con progreso, cancelación y reintentos.
- Elegir entre conservar el original o reemplazarlo cuando la conversión termine correctamente.
- Crear accesos directos de aplicaciones en el Escritorio desde un menú circular.
- Mantener WinLat en la bandeja del sistema mientras seguís trabajando.
- Usar la interfaz en español, inglés o según el idioma de Windows.
- Recibir nuevas versiones automáticamente desde este repositorio.

Las conversiones procesan los archivos en tu computadora. El actualizador necesita conexión a Internet para consultar GitHub y descargar nuevas versiones; las conversiones locales pueden usarse sin conexión si las herramientas necesarias ya están instaladas.

## Requisitos

| Requisito | Detalle |
| --- | --- |
| Sistema operativo | Windows 10 u 11. |
| Arquitectura | Instalador x64. No se distribuye un instalador de 32 bits en este repositorio. |
| Permisos | La instalación normal se realiza para el usuario actual, sin permisos de administrador. |
| .NET | Incluido en la publicación. No necesitás instalar el SDK ni el runtime por separado. |
| Internet | Necesario para descargar el instalador y recibir actualizaciones. |
| Espacio en disco | Depende de los componentes elegidos. Durante una actualización también se guarda temporalmente el instalador descargado. |

## Descarga e instalación

1. Abrí la página de [Releases](https://github.com/mangoostaa/WinLat-releases/releases).
2. Elegí la versión estable más reciente y expandí **Assets** si los archivos no están visibles.
3. Descargá **`WinLat-Setup-x64.exe`**. El archivo **`WinLat-Setup-x64.exe.sha256`** sirve para comprobar la integridad de la descarga; no se ejecuta.
4. Ejecutá el instalador y elegí **Instalación completa** o **Elegir componentes**.
5. Seleccioná si querés un acceso directo en el Escritorio y si WinLat debe iniciar con Windows.
6. Terminá la instalación y abrí WinLat.

La carpeta predeterminada es:

```text
%LOCALAPPDATA%\Programs\WinLat
```

### Componentes del instalador

| Componente | Qué habilita | Herramienta incluida |
| --- | --- | --- |
| WinLat base | Cambios de extensión de texto, plantillas y conversiones de datos disponibles. Es obligatorio. | Aplicación y runtime de .NET. |
| Accesos directos | Menú circular de aplicaciones para crear accesos directos en el Escritorio. | No necesita un conversor externo. |
| Imágenes | Conversiones de imágenes como JPG a PNG o PNG a WEBP. | ImageMagick portable. |
| Audio y video | Conversiones multimedia, extracción de audio y generación de GIF en las combinaciones disponibles. | FFmpeg y ffprobe portables. |

ImageMagick y FFmpeg se guardan junto a WinLat y se ejecutan cuando una conversión los necesita. El instalador no los agrega al `PATH` del sistema. Podés volver a ejecutar el instalador para cambiar los componentes elegidos.

Las conversiones que dependen de Pandoc, LibreOffice o Poppler requieren instalar esas herramientas por separado. Elegir la instalación completa incluye los componentes de la tabla, pero no esas herramientas adicionales.

### Comprobar la descarga con SHA-256

Si querés verificar el instalador antes de ejecutarlo, descargá también su archivo `.sha256`, abrí PowerShell en la carpeta donde guardaste ambos y ejecutá:

```powershell
$actualHash = (Get-FileHash -LiteralPath '.\WinLat-Setup-x64.exe' -Algorithm SHA256).Hash
$expectedHash = (Get-Content -LiteralPath '.\WinLat-Setup-x64.exe.sha256' -Raw).Trim().Split(' ')[0]
if ($actualHash -ne $expectedHash) {
    throw 'La descarga no coincide con el SHA-256 publicado.'
}
Write-Output 'SHA-256 correcto.'
```

La suma verifica que el archivo coincide con el publicado. El instalador de la versión 1.2.0 no tiene firma Authenticode; Windows puede mostrar una advertencia de SmartScreen durante una instalación manual. Descargá los archivos desde los releases de este repositorio.

## Cómo usar WinLat

### Desde el Explorador: menú circular

1. Seleccioná uno o varios archivos en el Explorador de Windows.
2. Mantené presionada la tecla **Espacio** y arrastrá los archivos para abrir el menú circular.
3. Mové el cursor hasta un formato y soltá los archivos sobre esa opción.
4. Elegí si querés **crear un archivo nuevo** o **reemplazar el original al terminar**.
5. Confirmá la operación. WinLat agrega el trabajo a la cola y te avisa cuando termina.

El anillo muestra primero las opciones sugeridas. Para explorar más formatos, mantené el cursor sobre **Otras familias** y luego sobre la familia deseada. Los submenús se abren al mantener el cursor aproximadamente 300 ms. El centro permite volver atrás; también podés escribir para filtrar las opciones mientras el menú está abierto.

En opciones que admiten esos modos, **Ctrl** fuerza copia/cambio de extensión y **Alt** fuerza una plantilla.

### Desde la ventana principal: cola de conversiones

1. Abrí **Conversiones**.
2. Usá **Agregar archivos** o arrastrá archivos al área de selección.
3. Elegí un formato compatible.
4. Pulsá **Añadir a la cola** y confirmá cómo tratar los originales.

Los lotes se procesan en orden, con un archivo activo a la vez. Un error en un archivo no detiene los demás. Podés cancelar trabajos, reintentar archivos fallidos o cancelados, abrir carpetas de resultados y consultar los detalles de cada lote.

FFmpeg puede mostrar porcentaje y tiempo restante cuando conoce la duración del archivo. Otras operaciones muestran **Procesando…** si no proporcionan un porcentaje. **Limpiar terminados** quita los trabajos del historial visible y conserva los archivos generados.

### Qué pasa con el archivo original

- **Crear un archivo nuevo:** conserva el original y guarda el resultado junto a él.
- **Reemplazar al terminar:** conserva el original durante la conversión y lo elimina solo después de generar el resultado correctamente.
- **Nombre de destino ocupado:** usa un nombre como `archivo (1).png` para evitar sobrescribir otro archivo.
- **Error o cancelación antes de completar la conversión:** conserva el original y limpia los archivos temporales de esa operación.

En un lote, la elección se aplica a todos los archivos de ese lote. Para trabajos importantes, revisá los resultados antes de eliminar otras copias del material.

### Accesos directos al Escritorio

Si instalaste el componente **Accesos directos**, mantené **Espacio** y hacé clic derecho en un espacio vacío del Escritorio. Elegí una aplicación del menú para crear su acceso directo. Si el nombre ya existe, WinLat agrega un sufijo numérico.

### Bandeja del sistema

Cerrar la ventana mantiene WinLat activo en la bandeja. Desde su icono podés abrir la aplicación, abrir Configuración, pausar o reanudar el gesto y salir por completo. Si elegís **Salir** con conversiones pendientes, WinLat pide confirmar su cancelación.

La cola se conserva durante la sesión actual: no se restaura después de salir o reiniciar la aplicación.

## Formatos y tipos de conversión

Las opciones dependen del archivo seleccionado, de las reglas disponibles y de las herramientas instaladas. La presencia de un formato dentro de una familia no implica que todas las combinaciones entre formatos estén disponibles.

| Categoría | Ejemplos de opciones | Requisito o alcance |
| --- | --- | --- |
| Texto | TXT a CFG, LOG, JSON o un archivo de código. | Copia, plantilla o transformación según la opción elegida. |
| Datos | JSON a YAML, TOML, XML o CSV; CSV a JSON. | Conversión estructural en las combinaciones implementadas. Los datos anidados se aplanan al exportar JSON a CSV. |
| Imágenes | JPG a PNG, PNG a WEBP, imágenes a BMP o ICO. | Componente Imágenes. Algunas opciones dependen del formato y de los delegados de ImageMagick. |
| Audio | MP3, WAV, OGG y FLAC en las combinaciones disponibles. | Componente Audio y video. |
| Video | AVI o MKV a MP4, MP4 a WEBM, extracción a MP3 o generación de GIF. | Componente Audio y video. El resultado depende también de los códecs del archivo. |
| Documentos | TXT a DOCX o PDF; PDF a PNG. | Herramientas adicionales. Pandoc puede necesitar un motor de PDF instalado. PDF a PNG usa Poppler. |
| Hojas de cálculo | CSV a XLSX. | LibreOffice para la regla disponible. |

WinLat diferencia estas operaciones:

- **Cambio de extensión/copia:** conserva el contenido de texto y cambia el nombre de destino.
- **Plantilla:** envuelve el contenido para generar el archivo de destino.
- **Conversión estructural:** interpreta datos y los escribe en otro formato.
- **Conversión con herramienta:** ejecuta un motor como ImageMagick, FFmpeg, Pandoc o LibreOffice.

Cambiar un archivo `.py` a `.js` no traduce Python a JavaScript: se copia o envuelve el contenido según la opción. Los archivos binarios no se convierten simplemente cambiándoles la extensión. WinLat también inspecciona el contenido para detectar ciertos formatos aunque la extensión no coincida.

## Actualizaciones automáticas

### Funcionamiento

La actualización automática está activada por defecto desde **1.2.0** y funciona con instalaciones realizadas mediante el setup de WinLat.

1. WinLat consulta el último release estable de este repositorio unos **15 segundos después de iniciar**.
2. Mientras permanece abierto, vuelve a comprobarlo **cada 6 horas**.
3. Si la versión publicada es superior a la instalada, descarga el instalador en segundo plano.
4. Verifica la procedencia del enlace, el tamaño del archivo y su **SHA-256**.
5. Espera a que terminen las conversiones y a que se cierre cualquier confirmación de conversión en curso.
6. Cierra WinLat, ejecuta el instalador silenciosamente y vuelve a abrir la aplicación.

Se conservan la configuración del usuario, la carpeta de instalación y los componentes y tareas seleccionados anteriormente. Si WinLat estaba oculto en la bandeja, vuelve a iniciarse en segundo plano.

### Buscar o desactivar actualizaciones

Abrí **Configuración → Actualizaciones** para ver la versión instalada y el estado del actualizador.

- **Buscar actualizaciones** comprueba si hay una versión nueva e inicia su actualización si corresponde.
- Para desactivar la búsqueda e instalación automáticas, desmarcá **Actualizar WinLat automáticamente** y pulsá **Guardar cambios**.

El botón de búsqueda manual sigue disponible si desactivaste la actualización automática.

### Qué versiones se instalan

El actualizador usa el último release estable que devuelve GitHub. Ignora borradores y versiones marcadas como **pre-release**, y no instala versiones iguales o inferiores a la actual.

La aplicación no necesita una cuenta de GitHub ni un token para actualizarse. Necesita que este repositorio y los archivos del release sean públicos. El ejecutable portable y las ejecuciones desde `dotnet run` no realizan la instalación automática.

### Si falla una actualización

Una descarga que no coincide con el tamaño o el SHA-256 publicado no se instala. Los errores aparecen en **Diagnóstico**, y la aplicación puede volver a intentarlo en una comprobación posterior. Si el instalador falla después de cerrar WinLat, el actualizador intenta volver a abrir la versión que haya quedado instalada; no implementa una restauración completa a una versión anterior.

## Configuración

En **Configuración** podés cambiar:

| Opción | Para qué sirve |
| --- | --- |
| Idioma | Español, inglés o selección automática según Windows. |
| Iniciar con Windows | Abre WinLat cuando iniciás sesión. |
| Carpeta de herramientas | Permite indicar dónde están los ejecutables de herramientas adicionales. |
| Modo por defecto de texto hacia código | Elegir copia/cambio de extensión o envoltura en una plantilla. |
| Actualizaciones automáticas | Habilitar o deshabilitar la actualización desde GitHub. |

Pulsá **Guardar cambios** para aplicar las preferencias. En **Herramientas** podés consultar los motores detectados y volver a buscarlos después de instalar uno.

## Herramientas adicionales

Para determinadas conversiones de documentos y hojas de cálculo podés instalar estas herramientas. Los comandos son opcionales y pueden pedir permisos según el instalador de cada proveedor:

```powershell
# Documentos: Pandoc.
winget install -e --id JohnMacFarlane.Pandoc

# Documentos y hojas de cálculo: LibreOffice.
winget install -e --id TheDocumentFoundation.LibreOffice

# PDF a imagen: Poppler, que incluye pdftoppm.
winget install -e --id oschwartz10612.Poppler
```

Después, reiniciá WinLat o usá **Herramientas → Volver a detectar**. Si el ejecutable no se encuentra, indicá su carpeta desde Configuración. Para convertir a PDF con Pandoc puede ser necesario instalar también un motor compatible de generación de PDF.

## Archivos, preferencias y registros

Aunque el nombre visible sea **WinLat**, el ejecutable y algunas carpetas internas conservan el nombre **ShiftConvert**.

| Ubicación | Contenido |
| --- | --- |
| `%LOCALAPPDATA%\Programs\WinLat` | Instalación predeterminada, ejecutable y componentes opcionales. |
| `%APPDATA%\ShiftConvert\settings.json` | Preferencias e historial de elecciones de formato. |
| `%TEMP%\ShiftConvert\diagnostic.log` | Registro de la sesión visible en Diagnóstico. Puede incluir rutas de archivos. |
| `%LOCALAPPDATA%\WinLat\Updates` | Descargas y registros del actualizador. Cada versión tiene su propia carpeta. |
| `setup.log` dentro de una carpeta de actualización | Detalles de la instalación silenciosa. |
| `update.log` dentro de una carpeta de actualización | Resultado de la ejecución del instalador. |

Los resultados de las conversiones se guardan junto a los archivos de entrada. Si compartís un registro para reportar un problema, revisá las rutas y los nombres de archivo que contiene.

## Solución de problemas

| Problema | Qué revisar |
| --- | --- |
| No hay archivos para descargar en Releases. | Verificá que exista una versión publicada y que tenga assets. El código fuente del repositorio no sustituye al instalador. |
| Windows muestra SmartScreen. | Comprobá que descargaste el instalador desde este repositorio y que el SHA-256 coincide. La versión 1.2.0 no está firmada con Authenticode. |
| No aparecen conversiones de imágenes o video. | Revisá si instalaste el componente correspondiente. Podés volver a ejecutar el setup para agregarlo. |
| Una conversión informa que falta una herramienta. | Abrí Herramientas, instalá el motor que indica el error y volvé a detectarlo o configurá su carpeta. |
| Cambié la extensión de código pero el contenido no se tradujo. | Las opciones de texto hacia código copian o envuelven el contenido; no son traductores entre lenguajes. |
| Cerré la ventana y WinLat sigue funcionando. | Es el comportamiento de la bandeja. Usá Salir desde su menú para cerrar completamente. |
| No se actualiza una versión antigua. | Instalá una versión con actualizador, desde 1.2.0, una vez mediante el setup. |
| Hay un release nuevo pero WinLat no lo instala. | Comprobá que sea estable, tenga una versión superior y contenga el EXE y el `.sha256` con sus nombres exactos. Usá Buscar actualizaciones. |
| La actualización está esperando. | Terminá o cancelá los trabajos pendientes y cerrá las confirmaciones de conversión. |
| Falló la descarga o la instalación. | Revisá la conexión, el espacio disponible, Diagnóstico y los registros de actualización. Un archivo que falla la verificación no se instala. |

Para reportar un problema, indicá la versión de WinLat, la versión de Windows, los pasos para reproducirlo, el formato de entrada y salida y el mensaje de error. Usá la sección [Issues de este repositorio](https://github.com/mangoostaa/WinLat-releases/issues). No incluyas archivos privados ni datos sensibles en un reporte público.

## Desinstalación

Salí de WinLat desde la bandeja y desinstalalo desde **Configuración de Windows → Aplicaciones → WinLat**, o ejecutá el desinstalador de su carpeta de instalación.

Las preferencias de `%APPDATA%\ShiftConvert` y la caché de `%LOCALAPPDATA%\WinLat\Updates` pueden permanecer después de desinstalar. Los archivos que convertiste se guardan fuera de la carpeta de instalación y no se eliminan al desinstalar la aplicación.

## Cómo publicar una nueva versión

Esta sección es para quien mantiene el programa. Un commit en este repositorio no actualiza las instalaciones: para distribuir una versión nueva hay que publicar un **release estable con los archivos correctos**.

### Preparar el instalador desde el proyecto

En el proyecto ShiftConvert necesitás .NET 8 SDK, Inno Setup 6 y 7-Zip para generar el instalador. Actualizá la propiedad `<Version>` de `ShiftConvert.csproj` a una versión superior, por ejemplo:

```xml
<Version>1.2.1</Version>
```

Desde la carpeta del proyecto, ejecutá:

```powershell
.\scripts\build-installer.ps1
```

Si ya preparaste las herramientas portables y querés reutilizarlas:

```powershell
.\scripts\build-installer.ps1 -UsePreparedTools
```

El script publica la aplicación autocontenida, compila el instalador con la versión del proyecto y genera estos archivos en `Installer\output`:

```text
WinLat-Setup-x64.exe
WinLat-Setup-x64.exe.sha256
RELEASE-NOTES.md
```

### Publicar en GitHub

1. Abrí [crear un release](https://github.com/mangoostaa/WinLat-releases/releases/new) en **mangoostaa/WinLat-releases**.
2. Creá el tag con la versión compilada: por ejemplo, **`v1.2.1`**.
3. Usá un título como **WinLat 1.2.1** y describí los cambios para los usuarios.
4. Adjuntá **`WinLat-Setup-x64.exe`** y **`WinLat-Setup-x64.exe.sha256`**, sin renombrarlos.
5. Dejá desmarcada la opción **Set as a pre-release** y asegurate de que sea la última versión estable.
6. Publicá el release cuando ambos archivos hayan terminado de subirse.
7. Verificá el enlace a [la última versión](https://github.com/mangoostaa/WinLat-releases/releases/latest) y probá **Buscar actualizaciones** desde una instalación de una versión anterior con actualizador.

| Dato | Ejemplo correcto |
| --- | --- |
| Versión compilada | `1.2.1` |
| Tag del release | `v1.2.1` |
| Instalador | `WinLat-Setup-x64.exe` |
| Verificación | `WinLat-Setup-x64.exe.sha256` |
| Estado | Público, publicado y estable. |

Para la primera versión con actualizador, el tag corresponde a **`v1.2.0`**. Después aumentá la versión para cada actualización. Si recompilás el instalador, subí también el nuevo `.sha256`: el archivo de verificación debe corresponder exactamente a ese EXE.

Publicar una versión inferior no hace que el actualizador baje de versión. Para distribuir una corrección, publicá una versión superior.

### Comprobaciones del proyecto

El proyecto incluye comprobaciones de conversiones, cola, cancelación, versiones, enlaces del actualizador y SHA-256; además, pruebas de interfaz y una prueba de instalación temporal con reinicio real:

```powershell
dotnet run --project .\Tests\ShiftConvert.UnitTests.csproj
dotnet run --project .\Tests\Ui\ShiftConvert.UiTests.csproj
.\scripts\test-updater.ps1
```

La prueba de instalación temporal requiere que WinLat esté cerrado y permisos para ejecutar los instaladores de prueba. Se ejecuta desde el proyecto, no desde este repositorio de distribución.

## Componentes de terceros

Los conversores opcionales se distribuyen como herramientas independientes:

- **FFmpeg y ffprobe:** el build Essentials de Gyan.dev incluido está licenciado bajo GNU GPL v3. La información del build y su código fuente se publican en [Gyan.dev](https://www.gyan.dev/ffmpeg/builds/) y [FFmpeg](https://github.com/FFmpeg/FFmpeg). El texto de licencia se instala en `Licenses\GPL-3.0.txt`.
- **ImageMagick:** se distribuye bajo la [licencia de ImageMagick](https://imagemagick.org/license/). Su código está en [ImageMagick/ImageMagick](https://github.com/ImageMagick/ImageMagick) y la licencia incluida se instala en `Licenses\ImageMagick-LICENSE.html`.

El instalador incluye `THIRD-PARTY-NOTICES.txt` con información de los componentes. Las licencias de estas herramientas corresponden a cada proyecto y no establecen por sí mismas la licencia de WinLat.
