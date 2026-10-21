# WinLat

**Convert files from Windows with a radial menu, a job queue, and automatic updates.**

[![Windows](https://img.shields.io/badge/Windows-10%20%2F%2011-0078D4)](#requirements)
[![Architecture](https://img.shields.io/badge/architecture-x64-555555)](#requirements)
[![Language](https://img.shields.io/badge/interface-espa%C3%B1ol%20%2F%20English-orange)](#settings)

**[Download from Releases](https://github.com/mangoostaa/WinLat-releases/releases)** · **[Latest published version](https://github.com/mangoostaa/WinLat-releases/releases/latest)** · **[Report a problem](https://github.com/mangoostaa/WinLat-releases/issues)**

This repository distributes WinLat's installers and release notes. Installation files are published in **Releases**. To install the application, download the `WinLat-Setup-x64.exe` file attached to a version; the automatic **Source code (zip)** and **Source code (tar.gz)** files are repository archives and do not contain the installer.

> Automatic updating has been built in since **WinLat 1.2.0**. If you are using an earlier version without the updater, you need to install a version with the updater once. After that, WinLat will be able to download and install future versions automatically.

If no version appears in Releases yet, the installer has not been published. The link to the latest version will be available once the first stable release is published.

## Contents

- [What you can do](#what-you-can-do)
- [Requirements](#requirements)
- [Download and installation](#download-and-installation)
- [How to use WinLat](#how-to-use-winlat)
- [Formats and conversion types](#formats-and-conversion-types)
- [Automatic updates](#automatic-updates)
- [Settings](#settings)
- [Additional tools](#additional-tools)
- [Files, preferences, and logs](#files-preferences-and-logs)
- [Troubleshooting](#troubleshooting)
- [Uninstalling](#uninstalling)
- [How to publish a new version](#how-to-publish-a-new-version)
- [Third-party components](#third-party-components)

## What you can do

- Convert images, audio, and video using the optional components included in the installer.
- Copy text with a different extension, wrap content in templates, and transform data in the available combinations.
- Open a radial menu from Explorer with **Space + drag**.
- Add files from the main window and process them in a queue, with progress, cancellation, and retries.
- Choose between keeping the original or replacing it once the conversion finishes successfully.
- Create application shortcuts on the Desktop from a radial menu.
- Keep WinLat in the system tray while you continue working.
- Use the interface in Spanish, English, or according to the Windows language.
- Receive new versions automatically from this repository.

Conversions process files on your computer. The updater needs an Internet connection to query GitHub and download new versions; local conversions can be used offline if the necessary tools are already installed.

## Requirements

| Requirement | Details |
| --- | --- |
| Operating system | Windows 10 or 11. |
| Architecture | x64 installer. No 32-bit installer is distributed in this repository. |
| Permissions | The standard installation is performed for the current user, without administrator permissions. |
| .NET | Included in the release. You do not need to install the SDK or the runtime separately. |
| Internet | Required to download the installer and receive updates. |
| Disk space | Depends on the selected components. During an update, the downloaded installer is also stored temporarily. |

## Download and installation

1. Open the [Releases](https://github.com/mangoostaa/WinLat-releases/releases) page.
2. Choose the most recent stable version and expand **Assets** if the files are not visible.
3. Download **`WinLat-Setup-x64.exe`**. The **`WinLat-Setup-x64.exe.sha256`** file is used to verify the integrity of the download; it is not executed.
4. Run the installer and choose **Full installation** or **Choose components**.
5. Select whether you want a Desktop shortcut and whether WinLat should start with Windows.
6. Finish the installation and open WinLat.

The default folder is:

```text
%LOCALAPPDATA%\Programs\WinLat
```

### Installer components

| Component | What it enables | Included tool |
| --- | --- | --- |
| WinLat base | Text extension changes, templates, and available data conversions. Required. | Application and .NET runtime. |
| Shortcuts | Radial application menu for creating Desktop shortcuts. | Does not need an external converter. |
| Images | Image conversions such as JPG to PNG or PNG to WEBP. | Portable ImageMagick. |
| Audio and video | Multimedia conversions, audio extraction, and GIF generation in the available combinations. | Portable FFmpeg and ffprobe. |

ImageMagick and FFmpeg are stored alongside WinLat and run when a conversion needs them. The installer does not add them to the system `PATH`. You can run the installer again to change the selected components.

Conversions that depend on Pandoc, LibreOffice, or Poppler require installing those tools separately. Choosing the full installation includes the components in the table, but not those additional tools.

### Verifying the download with SHA-256

If you want to verify the installer before running it, also download its `.sha256` file, open PowerShell in the folder where you saved both, and run:

```powershell
$actualHash = (Get-FileHash -LiteralPath '.\WinLat-Setup-x64.exe' -Algorithm SHA256).Hash
$expectedHash = (Get-Content -LiteralPath '.\WinLat-Setup-x64.exe.sha256' -Raw).Trim().Split(' ')[0]
if ($actualHash -ne $expectedHash) {
    throw 'The download does not match the published SHA-256.'
}
Write-Output 'SHA-256 is correct.'
```

The checksum verifies that the file matches the published one. The version 1.2.0 installer does not have an Authenticode signature; Windows may display a SmartScreen warning during a manual installation. Download the files from this repository's releases.

## How to use WinLat

### From Explorer: radial menu

1. Select one or more files in Windows Explorer.
2. Hold down the **Space** key and drag the files to open the radial menu.
3. Move the cursor to a format and drop the files on that option.
4. Choose whether you want to **create a new file** or **replace the original when finished**.
5. Confirm the operation. WinLat adds the job to the queue and notifies you when it finishes.

The ring shows the suggested options first. To explore more formats, hold the cursor over **Other families** and then over the desired family. Submenus open after holding the cursor for about 300 ms. The center lets you go back; you can also type to filter the options while the menu is open.

For options that support these modes, **Ctrl** forces copy/extension change and **Alt** forces a template.

### From the main window: conversion queue

1. Open **Conversions**.
2. Use **Add files** or drag files into the selection area.
3. Choose a compatible format.
4. Click **Add to queue** and confirm how to handle the originals.

Batches are processed in order, with one active file at a time. An error in one file does not stop the others. You can cancel jobs, retry failed or canceled files, open result folders, and view the details of each batch.

FFmpeg can show a percentage and remaining time when it knows the file's duration. Other operations show **Processing…** if they do not provide a percentage. **Clear finished** removes jobs from the visible history and keeps the generated files.

### What happens to the original file

- **Create a new file:** keeps the original and saves the result next to it.
- **Replace when finished:** keeps the original during the conversion and deletes it only after the result has been generated successfully.
- **Destination name already in use:** uses a name such as `file (1).png` to avoid overwriting another file.
- **Error or cancellation before the conversion completes:** keeps the original and cleans up the temporary files from that operation.

In a batch, the choice applies to all files in that batch. For important jobs, check the results before deleting other copies of the material.

### Desktop shortcuts

If you installed the **Shortcuts** component, hold **Space** and right-click an empty area of the Desktop. Choose an application from the menu to create its shortcut. If the name already exists, WinLat adds a numeric suffix.

### System tray

Closing the window keeps WinLat running in the tray. From its icon you can open the application, open Settings, pause or resume the gesture, and exit completely. If you choose **Exit** with pending conversions, WinLat asks you to confirm their cancellation.

The queue is kept during the current session: it is not restored after exiting or restarting the application.

## Formats and conversion types

The options depend on the selected file, the available rules, and the installed tools. The presence of a format within a family does not imply that all combinations between formats are available.

| Category | Example options | Requirement or scope |
| --- | --- | --- |
| Text | TXT to CFG, LOG, JSON, or a code file. | Copy, template, or transformation depending on the chosen option. |
| Data | JSON to YAML, TOML, XML, or CSV; CSV to JSON. | Structural conversion in the implemented combinations. Nested data is flattened when exporting JSON to CSV. |
| Images | JPG to PNG, PNG to WEBP, images to BMP or ICO. | Images component. Some options depend on the format and on ImageMagick delegates. |
| Audio | MP3, WAV, OGG, and FLAC in the available combinations. | Audio and video component. |
| Video | AVI or MKV to MP4, MP4 to WEBM, extraction to MP3, or GIF generation. | Audio and video component. The result also depends on the file's codecs. |
| Documents | TXT to DOCX or PDF; PDF to PNG. | Additional tools. Pandoc may need an installed PDF engine. PDF to PNG uses Poppler. |
| Spreadsheets | CSV to XLSX. | LibreOffice for the available rule. |

WinLat distinguishes between these operations:

- **Extension change/copy:** keeps the text content and changes the destination name.
- **Template:** wraps the content to generate the destination file.
- **Structural conversion:** interprets data and writes it in another format.
- **Tool-based conversion:** runs an engine such as ImageMagick, FFmpeg, Pandoc, or LibreOffice.

Changing a `.py` file to `.js` does not translate Python into JavaScript: the content is copied or wrapped depending on the option. Binary files are not converted simply by changing their extension. WinLat also inspects the content to detect certain formats even when the extension does not match.

## Automatic updates

### How it works

Automatic updating is enabled by default since **1.2.0** and works with installations made through the WinLat setup.

1. WinLat checks the latest stable release of this repository about **15 seconds after starting**.
2. While it stays open, it checks again **every 6 hours**.
3. If the published version is higher than the installed one, it downloads the installer in the background.
4. It verifies the link's origin, the file size, and its **SHA-256**.
5. It waits for conversions to finish and for any conversion confirmation in progress to be closed.
6. It closes WinLat, runs the installer silently, and reopens the application.

User settings, the installation folder, and the previously selected components and tasks are preserved. If WinLat was hidden in the tray, it starts again in the background.

### Checking for or disabling updates

Open **Settings → Updates** to see the installed version and the updater's status.

- **Check for updates** checks whether a new version is available and starts its update if applicable.
- To disable automatic checking and installation, uncheck **Update WinLat automatically** and click **Save changes**.

The manual check button remains available if you disabled automatic updating.

### Which versions get installed

The updater uses the latest stable release returned by GitHub. It ignores drafts and versions marked as **pre-release**, and does not install versions equal to or lower than the current one.

The application does not need a GitHub account or a token to update. It requires this repository and the release files to be public. The portable executable and runs from `dotnet run` do not perform automatic installation.

### If an update fails

A download that does not match the published size or SHA-256 is not installed. Errors appear in **Diagnostics**, and the application may try again in a later check. If the installer fails after WinLat has been closed, the updater tries to reopen whichever version remains installed; it does not implement a full rollback to a previous version.

## Settings

In **Settings** you can change:

| Option | Purpose |
| --- | --- |
| Language | Spanish, English, or automatic selection based on Windows. |
| Start with Windows | Opens WinLat when you sign in. |
| Tools folder | Lets you specify where the executables of additional tools are located. |
| Default text-to-code mode | Choose copy/extension change or wrapping in a template. |
| Automatic updates | Enable or disable updating from GitHub. |

Click **Save changes** to apply the preferences. In **Tools** you can view the detected engines and search for them again after installing one.

## Additional tools

For certain document and spreadsheet conversions you can install these tools. The commands are optional and may request permissions depending on each vendor's installer:

```powershell
# Documents: Pandoc.
winget install -e --id JohnMacFarlane.Pandoc

# Documents and spreadsheets: LibreOffice.
winget install -e --id TheDocumentFoundation.LibreOffice

# PDF to image: Poppler, which includes pdftoppm.
winget install -e --id oschwartz10612.Poppler
```

Afterwards, restart WinLat or use **Tools → Detect again**. If the executable is not found, specify its folder from Settings. To convert to PDF with Pandoc, you may also need to install a compatible PDF generation engine.

## Files, preferences, and logs

Although the visible name is **WinLat**, the executable and some internal folders keep the name **ShiftConvert**.

| Location | Contents |
| --- | --- |
| `%LOCALAPPDATA%\Programs\WinLat` | Default installation, executable, and optional components. |
| `%APPDATA%\ShiftConvert\settings.json` | Preferences and format choice history. |
| `%TEMP%\ShiftConvert\diagnostic.log` | Session log visible in Diagnostics. May include file paths. |
| `%LOCALAPPDATA%\WinLat\Updates` | Updater downloads and logs. Each version has its own folder. |
| `setup.log` inside an update folder | Details of the silent installation. |
| `update.log` inside an update folder | Result of running the installer. |

Conversion results are saved next to the input files. If you share a log to report a problem, review the paths and file names it contains.

## Troubleshooting

| Problem | What to check |
| --- | --- |
| There are no files to download in Releases. | Verify that a published version exists and that it has assets. The repository's source code does not replace the installer. |
| Windows shows SmartScreen. | Check that you downloaded the installer from this repository and that the SHA-256 matches. Version 1.2.0 is not signed with Authenticode. |
| Image or video conversions do not appear. | Check whether you installed the corresponding component. You can run the setup again to add it. |
| A conversion reports a missing tool. | Open Tools, install the engine named in the error, and detect it again or configure its folder. |
| I changed the code extension but the content was not translated. | Text-to-code options copy or wrap the content; they are not translators between languages. |
| I closed the window and WinLat is still running. | This is the tray behavior. Use Exit from its menu to close it completely. |
| An old version does not update. | Install a version with the updater (1.2.0 or later) once through the setup. |
| There is a new release but WinLat does not install it. | Check that it is stable, has a higher version, and contains the EXE and the `.sha256` with their exact names. Use Check for updates. |
| The update is waiting. | Finish or cancel pending jobs and close conversion confirmations. |
| The download or installation failed. | Check your connection, available space, Diagnostics, and the update logs. A file that fails verification is not installed. |

To report a problem, include the WinLat version, the Windows version, the steps to reproduce it, the input and output format, and the error message. Use the [Issues section of this repository](https://github.com/mangoostaa/WinLat-releases/issues). Do not include private files or sensitive data in a public report.

## Uninstalling

Exit WinLat from the tray and uninstall it from **Windows Settings → Apps → WinLat**, or run the uninstaller in its installation folder.

The preferences in `%APPDATA%\ShiftConvert` and the cache in `%LOCALAPPDATA%\WinLat\Updates` may remain after uninstalling. The files you converted are saved outside the installation folder and are not deleted when the application is uninstalled.

## How to publish a new version

This section is for whoever maintains the program. A commit in this repository does not update installations: to distribute a new version you must publish a **stable release with the correct files**.

### Preparing the installer from the project

In the ShiftConvert project you need the .NET 8 SDK, Inno Setup 6, and 7-Zip to generate the installer. Update the `<Version>` property in `ShiftConvert.csproj` to a higher version, for example:

```xml
<Version>1.2.1</Version>
```

From the project folder, run:

```powershell
.\scripts\build-installer.ps1
```

If you have already prepared the portable tools and want to reuse them:

```powershell
.\scripts\build-installer.ps1 -UsePreparedTools
```

The script publishes the self-contained application, builds the installer with the project's version, and generates these files in `Installer\output`:

```text
WinLat-Setup-x64.exe
WinLat-Setup-x64.exe.sha256
RELEASE-NOTES.md
```

### Publishing on GitHub

1. Open [create a release](https://github.com/mangoostaa/WinLat-releases/releases/new) in **mangoostaa/WinLat-releases**.
2. Create the tag with the compiled version: for example, **`v1.2.1`**.
3. Use a title such as **WinLat 1.2.1** and describe the changes for users.
4. Attach **`WinLat-Setup-x64.exe`** and **`WinLat-Setup-x64.exe.sha256`**, without renaming them.
5. Leave **Set as a pre-release** unchecked and make sure it is the latest stable version.
6. Publish the release once both files have finished uploading.
7. Verify the link to [the latest version](https://github.com/mangoostaa/WinLat-releases/releases/latest) and test **Check for updates** from an installation of an earlier version with the updater.

| Item | Correct example |
| --- | --- |
| Compiled version | `1.2.1` |
| Release tag | `v1.2.1` |
| Installer | `WinLat-Setup-x64.exe` |
| Verification | `WinLat-Setup-x64.exe.sha256` |
| Status | Public, published, and stable. |

For the first version with the updater, the tag is **`v1.2.0`**. After that, increase the version for each update. If you rebuild the installer, also upload the new `.sha256`: the verification file must correspond exactly to that EXE.

Publishing a lower version does not make the updater downgrade. To distribute a fix, publish a higher version.

### Project checks

The project includes checks for conversions, queue, cancellation, versions, updater links, and SHA-256; plus interface tests and a temporary installation test with a real restart:

```powershell
dotnet run --project .\Tests\ShiftConvert.UnitTests.csproj
dotnet run --project .\Tests\Ui\ShiftConvert.UiTests.csproj
.\scripts\test-updater.ps1
```

The temporary installation test requires WinLat to be closed and permissions to run the test installers. It is run from the project, not from this distribution repository.

## Third-party components

The optional converters are distributed as independent tools:

- **FFmpeg and ffprobe:** the included Gyan.dev Essentials build is licensed under GNU GPL v3. The build information and its source code are published at [Gyan.dev](https://www.gyan.dev/ffmpeg/builds/) and [FFmpeg](https://github.com/FFmpeg/FFmpeg). The license text is installed in `Licenses\GPL-3.0.txt`.
- **ImageMagick:** distributed under the [ImageMagick license](https://imagemagick.org/license/). Its code is at [ImageMagick/ImageMagick](https://github.com/ImageMagick/ImageMagick) and the included license is installed in `Licenses\ImageMagick-LICENSE.html`.

The installer includes `THIRD-PARTY-NOTICES.txt` with information about the components. The licenses of these tools belong to each project and do not by themselves establish the license of WinLat.
