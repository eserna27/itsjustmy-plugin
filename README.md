# itsjustmy.blog para tu agente · for your agent

[Español](#español) · [English](#english)

---

## Español

### Qué es

Un plugin para agentes (Grok Bot y Cursor) que le enseña a usar el CLI `itsjustmy` de [itsjustmy.blog](https://itsjustmy.blog). Le pides un post y tu agente deja un **borrador** en tu blog desde la terminal. Tú lo revisas y tú decides cuándo se publica.

Incluye un skill: `itsjustmy-cli`. No incluye servidor MCP; solo el CLI.

### Cómo instalar el plugin

- **Desde el marketplace:** busca "itsjustmy.blog" en el marketplace de Grok Bot o de Cursor e instálalo. *(Disponible cuando el plugin sea aprobado.)*
- **Local, para probarlo:** copia esta carpeta a `~/.cursor/plugins/local/itsjustmy/`.

Después, en la máquina donde corre tu agente:

1. Hace falta Ruby 3.2 o más nuevo (`ruby -v`). Si falta: `brew install ruby` (macOS) o `sudo apt install ruby-full` (Debian/Ubuntu).
2. Pídele al agente que instale el CLI, o hazlo tú:

   ```bash
   curl -fsSL https://itsjustmy.blog/install.sh | bash
   ```

3. Conecta tu blog: `itsjustmy login`. Se abre el navegador, eliges el blog y pulsas Conectar. El token se guarda en tu máquina, en `~/.config/itsjustmy/credentials.json`.

Para actualizar el CLI, vuelve a ejecutar el instalador.

### Ejemplos de prompts

- "Escribe un borrador sobre cómo preparo mi café y déjalo en mi blog."
- "Toma `notas.md`, conviértelo en un post con título y etiquetas, y súbelo como borrador con `portada.png` de portada."
- "Lista mis posts y dime cuáles siguen en borrador."
- "Sube `foto.png` y úsala en el post `mi-slug`."
- "Actualiza mi bio con este texto." *(requiere el permiso de perfil)*
- "Publica el post `mi-slug`." *(solo si diste el permiso de publicar)*

### Qué puede y qué no puede hacer el agente

**Puede:** instalar el CLI, crear y editar borradores en Markdown, subir imágenes (las imágenes locales del Markdown se suben solas) y portadas, listar posts, series y categorías, y ver tu perfil.

**Solo si tú lo permites:**

- **Publicar** y editar posts ya publicados: permiso `publish`.
- **Editar tu perfil:** permiso `profile`.

El token es de borradores por defecto. `publish` y `profile` son casillas que marcas tú al conectar, y puedes revocar el token cuando quieras desde el panel de tu blog (Ajustes, Escritores, Tokens del agente). Cerrar sesión con `itsjustmy logout` borra el token de tu máquina, pero no lo revoca.

**No hace:** publicar por su cuenta, mostrar o guardar tu token en un repositorio, ni saltarse un permiso que el token no tiene.

### Enlaces

- Guía del CLI: https://itsjustmy.blog/cli
- itsjustmy.blog: https://itsjustmy.blog
- Grok Bot: https://x.ai/bot

---

## English

### What it is

A plugin for agents (Grok Bot and Cursor) that teaches them to use the `itsjustmy` CLI from [itsjustmy.blog](https://itsjustmy.blog). Ask for a post and your agent leaves a **draft** on your blog from the terminal. You review it and you decide when it goes live.

It ships one skill, `itsjustmy-cli`. There is no MCP server; it is CLI only.

### How to install the plugin

- **From the marketplace:** search for "itsjustmy.blog" in the Grok Bot or Cursor marketplace and install it. *(Available once the plugin is approved.)*
- **Locally, to try it:** copy this folder to `~/.cursor/plugins/local/itsjustmy/`.

Then, on the machine where your agent runs:

1. You need Ruby 3.2 or newer (`ruby -v`). If it is missing: `brew install ruby` (macOS) or `sudo apt install ruby-full` (Debian/Ubuntu).
2. Ask the agent to install the CLI, or run it yourself:

   ```bash
   curl -fsSL https://itsjustmy.blog/install.sh | bash
   ```

3. Connect your blog: `itsjustmy login`. The browser opens, you pick the blog and press Connect. The token is stored on your machine at `~/.config/itsjustmy/credentials.json`.

To update the CLI, run the installer again.

### Example prompts

- "Write a draft about how I make my coffee and leave it on my blog."
- "Take `notes.md`, turn it into a post with a title and tags, and upload it as a draft with `cover.png` as the cover."
- "List my posts and tell me which ones are still drafts."
- "Upload `photo.png` and use it in the post `my-slug`."
- "Update my bio with this text." *(needs the profile scope)*
- "Publish the post `my-slug`." *(only if you granted the publish scope)*

### What the agent can and cannot do

**Can:** install the CLI, create and edit Markdown drafts, upload images (local images in the Markdown are uploaded automatically) and covers, list posts, series and categories, and read your profile.

**Only if you allow it:**

- **Publish** and edit already-published posts: `publish` scope.
- **Edit your profile:** `profile` scope.

Tokens are draft-only by default. `publish` and `profile` are checkboxes you tick when connecting, and you can revoke the token any time from your blog dashboard (Settings, Writers, agent tokens). `itsjustmy logout` deletes the token from your machine but does not revoke it.

**Does not:** publish on its own, print or commit your token, or work around a scope the token does not have.

### Links

- CLI guide: https://itsjustmy.blog/cli
- itsjustmy.blog: https://itsjustmy.blog
- Grok Bot: https://x.ai/bot

## License

MIT
