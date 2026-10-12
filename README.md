# muve

Cliente de **muve drive** para la terminal. Sube, baja y reproduce tus archivos, cifrados en tu
dispositivo antes de salir. Funciona en Linux, Nintendo Switch (L4T), Android (Termux) y Windows.

El programa no trae ningún dato tuyo. La primera vez, `muve setup` te pide el servidor, la llave y
el secreto, y los guarda solo en tu dispositivo.

## 1. Instalar

**Linux, Switch (L4T Ubuntu) y Android (Termux):**

```sh
curl -fsSL https://github.com/cHeRaZ-edu/muve/releases/latest/download/install.sh | sh
```

El instalador detecta tu dispositivo, revisa el checksum y deja `muve` en `~/.local/bin`, o en
`$PREFIX/bin` si usas Termux. Si te avisa que esa carpeta no está en tu `PATH`, copia el comando que
te da.

**Windows:** baja `muve-windows-amd64.exe` de la [última versión](https://github.com/cHeRaZ-edu/muve/releases/latest),
renómbralo a `muve.exe` y ponlo en una carpeta que esté en tu `PATH`.

## 2. Configurar

Antes de empezar, ten a la mano:

- **La dirección del servidor:** la misma del panel, pero sin `/panel`.
- **Una llave de la app:** en el panel, entra a la app y crea una llave para este dispositivo.
  Cópiala completa con el botón **Copiar**, porque solo se muestra una vez.
- **Tu secreto de cifrado:** el mismo en todos tus dispositivos. Si es la primera vez, invéntalo
  aquí y guárdalo en tu gestor de contraseñas. Si lo pierdes, lo cifrado no se recupera.

```sh
muve setup
```

El asistente revisa cada dato contra el servidor antes de guardarlo. Puedes correrlo otra vez
para agregar otra app, por ejemplo `musica` y `saves`.

## 3. Usar

```sh
muve ls                               # tus archivos (* = cifrado, L = copia local al dia)
muve put cancion.mp3 musica/          # subir
muve put album/*.flac "musica/Mi Album/"
muve get musica/cancion.mp3           # bajar (descifrado)
muve play musica/cancion.mp3          # reproducir con mpv mientras baja (se puede adelantar)
muve play "musica/Mi Album/" --shuffle   # una carpeta como lista
muve info musica/cancion.mp3          # detalles y versiones
muve rm musica/cancion.mp3            # a la papelera
muve restore musica/cancion.mp3       # sacarla de la papelera
muve --app saves ls                   # usar otra app
```

Sobrescribir guarda la versión anterior:

```sh
muve put user1.sav mh4u/ --overwrite
muve get mh4u/user1.sav --version 3
muve restore mh4u/user1.sav --version 3
```

### Online o local

- **online** (default en PC y Switch): lee de la nube y guarda una caché temporal que se limpia sola.
- **local** (default en Android): `muve sync` baja todo y lo deja en el dispositivo para usarlo sin
  internet. `muve verify --fix` repara los archivos dañados.

Para cambiarlo: `muve app set musica mode local`.

## 4. Actualizar

```sh
muve update          # baja la ultima version y revisa su checksum
muve update --check  # solo avisa si hay una nueva
```

## Dónde quedan tus datos

| Qué | Dónde |
|---|---|
| Configuración (servidor, llaves, secreto) | `~/.config/muve/config.json`, solo la lee tu usuario |
| Caché del modo online | `~/.cache/muve/` |
| Biblioteca del modo local | `~/.local/share/muve/` |

Para usar otro archivo de configuración: `muve --config /ruta/config.json ...` o la variable `MUVE_CONFIG`.
Todos los comandos aparecen en `muve help`.
