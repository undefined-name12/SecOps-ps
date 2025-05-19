# 🎲 Get‑RandomPassphrase

Genera una contraseña aleatoria criptográficamente segura y devuelve su hash SHA‑512 en formato hexadecimal.

---

## ✨ Características

- 🔐 **Seguro**: Utiliza `RandomNumberGenerator` para una aleatoriedad real
- 🧮 **Flexible**: Personaliza la longitud de los bytes aleatorios
- 🔎 **SHA‑512**: Genera una cadena hexadecimal de 128 caracteres

---

## ⚙️ Parámetros

| Nombre | Tipo | Descripción | Predeterminado |
|-------------|------|-----------------------------------------------------|---------|
| `ByteLength`| Entero | Número de bytes aleatorios a generar (en bytes) | `32` |

---

## 🚀 Ejemplos de uso

```powershell
# Generar una contraseña aleatoria de 32 bytes (predeterminada)
Get-RandomPassphrase
🔐: 51FAD825C9DEE5414XXXXXX8AD978BDC257A3C7BCB3EF2990BD7DACXXXXXCAC31186F8292CECCF73.....
