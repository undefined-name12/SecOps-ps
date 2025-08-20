# Script de PowerShell para Obtener Archivo de Registros

## Resumen
La función **`Obtener Archivo de Registros`** automatiza el archivado de archivos de registro antiguos y mantiene la integridad de las carpetas comprimiendo y eliminando archivos obsoletos. Está diseñada para garantizar una gestión eficiente de los archivos de registro en la infraestructura de TI.

---

## Características
- Archiva archivos de registro antiguos según su antigüedad.
- Elimina archivos obsoletos (`*.zip`).
- Mantiene un registro detallado de las acciones realizadas en un archivo de registro específico.
- Parámetros configurables por el usuario para mayor flexibilidad.

---

## Parámetros
| Parámetro | Descripción | Obligatorio |
|-----------------|------------------------------------------------------------------------------|-----------|
| `OldFilesAge` | Filtra los archivos de registro para archivar según la antigüedad especificada (en días). | Sí |
| `OldCabsAge` | Elimina los archivos comprimidos (`*.zip`) con una antigüedad mayor a la especificada (en días). | Sí |
| `LogFilePath` | Especifica la ruta al archivo de registro para guardar todas las acciones y estados. | Sí |

---

## Ejemplos

### Ejemplo 1: Archivar registros y eliminar archivos antiguos
```powershell
Get-Log-Archivation -OldFilesAge 7 -OldCabsAge 30 -LogFilePath 'C:\Temp\log.log'
```

- Archiva archivos de registro con más de 7 días de antigüedad.
- Elimina archivos de almacenamiento (`*.zip`) con más de 30 días de antigüedad.
- Registra todas las acciones en el archivo `C:\Temp\log.log`.

### Ejemplo 2: Archivado y eliminación inmediatos
```powershell
Get-Log-Archivation -OldFilesAge 0 -OldCabsAge 0 -LogFilePath 'C:\Temp\log.log'
```

- Archiva todos los archivos de registro, independientemente de su antigüedad.
- Elimina todos los archivos de almacenamiento (*.zip), independientemente de su antigüedad. - Registra todas las acciones en C:\Temp\log.log.

### Cómo funciona

1. Inicialización:
- El script se inicializa con un conjunto de rutas predefinidas para la monitorización.
- Escribe un mensaje de inicio en el archivo de registro especificado.

2. Proceso de archivado:
- Identifica los archivos de registro anteriores a OldFilesAge y los comprime en archivos .zip.
- Elimina los archivos de registro originales tras un archivado correcto.

3. Limpieza:
- Elimina archivos de almacenamiento (*.zip) anteriores a OldCabsAge.

4. Gestión de errores:
- Detecta y registra cualquier error que se produzca durante el proceso.

5. Finalización:
- Escribe un mensaje de fin en el archivo de registro tras una ejecución correcta.

### ⚠️ IMPORTANTE: MITIGACIÓN DE RIESGOS DE SEGURIDAD

1. Usar una cuenta de usuario normal:
- Si planea programar este script con un programador de tareas, asegúrese de que se ejecute con una cuenta de usuario normal sin privilegios. Evite usar cuentas administrativas o con privilegios elevados.

2. Establecer los permisos adecuados:
- Otorgar a la cuenta de usuario normal acceso de lectura y escritura a los directorios que contienen los registros y a la ubicación donde el script almacenará sus registros de acciones.
- Restringir el acceso a estas carpetas a todos los demás usuarios para minimizar la exposición.

3. Deshabilitar el inicio de sesión interactivo:
- Para la cuenta de usuario con la que se ejecutará este script, asegúrese de que el inicio de sesión interactivo esté deshabilitado. Esto evita que la cuenta se utilice para iniciar sesión directamente en los sistemas, lo que reduce el riesgo de acceso no autorizado.

### Configurar la política de auditoría para controlar el acceso al archivo de script

1. Habilitar la auditoría de acceso a objetos
```powershell
auditpol /set /subcategory:"Sistema de archivos" /success:enable /failure:enable
```

2. Verificar la configuración
```powershell
auditpol /get /category:"Acceso a objetos"
```

3. Habilitar la auditoría para el archivo de script
```powershell
# Ruta al archivo de script
$filePath = "C:\Path\To\Get-Log-Archivation.ps1"

# Establecer reglas de auditoría para Todos (Acceso correcto y modificar)
icacls $filePath /setaudit Everyone:(OI)(CI)(M) /t /c
```

4. Verificar los registros de eventos de auditoría para los mensajes

- ID de evento: 4663 (Acceso a archivos)

```powershell
# Definir variables
$LogName = "Seguridad"
$EventID = 4663
$FilePath = "C:\Path\To\Get-Log-Archivation.ps1"

# Filtrar y mostrar eventos
Get-WinEvent -LogName $LogName | Where-Object {
$_.Id -eq $EventID -and $_.Message -match $FilePath
} | Select-Object TimeCreated, Id, Message

# Exportar a CSV
Get-WinEvent -LogName $LogName | Where-Object {
$_.Id -eq $EventID -and $_.Message -match $FilePath
} | Select-Object TimeCreated, Id, Message | Export-Csv -Path "C:\Temp\FilteredEvents.csv" -NoTypeInformation

# Exportar a TXT
Get-WinEvent -LogName $LogName | Where-Object {
$_.Id -eq $EventID -and $_.Message -match $FilePath
} | Select-Object TimeCreated, Id, Message | Out-File -FilePath "C:\Temp\FilteredEvents.txt"

```
