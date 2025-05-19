# 🔐 Protect‑FolderContent.ps1

Una función de PowerShell para **cifrar todos los archivos** de una carpeta (recursivamente), guardarlos en un directorio hermano `<folder>.enc` y generar un **registro CSV** con hashes SHA‑256. Admite la introducción de contraseñas **SecureString** para reducir el riesgo de fuga de claves.

---

## 🚀 Características

- 🔏 **Cifrado AES-256** del contenido de los archivos
- 🗂️ **Conserva la estructura del directorio** en `<SourceFolder>.enc`
- 🛡️ **Compatibilidad con SecureString** para la introducción de contraseñas
- 📊 **Genera CSV** con rutas originales/cifradas y hashes SHA-256
- ⚙️ **Compatibilidad con escenarios hipotéticos** para simulacros

---

## 📥 Instalación

1. **Descarga** `Protect-FolderContent.ps1` en tu carpeta de scripts.

2. **Dot-source** en tu sesión o perfil:
```powershell
. "C:\Path\To\Protect-FolderContent.ps1"
