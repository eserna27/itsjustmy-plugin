---
name: itsjustmy-cli
description: Write and manage posts on itsjustmy.blog from the terminal with the itsjustmy CLI. Use when the user wants to install or log in to the itsjustmy CLI, leave a blog draft, upload images or a cover, edit their itsjustmy profile, or (only when they explicitly ask) publish a post.
---

# itsjustmy CLI

`itsjustmy` is the command-line client for the itsjustmy.blog `/agent` API. It creates drafts, uploads images, edits the profile, and publishes only when the token allows it. This skill matches CLI 0.2.0. Docs: https://itsjustmy.blog/cli

The CLI's own `--help` text is in Spanish. Run `itsjustmy help COMMAND` or `itsjustmy tree` to confirm flags in the installed version. Do not guess flags.

## Rules

1. **Always create drafts.** `posts create` and `posts update` never publish.
2. **Never run `posts publish` on your own.** Run it only when the user explicitly asks to publish a specific post, and only if `whoami` lists the `publish` scope. Confirm the slug first. Without the scope the API answers 403; do not look for a way around it.
3. **Never print, paste, log, or commit the token.** Do not `cat ~/.config/itsjustmy/credentials.json`, do not echo `ITSJUSTMY_TOKEN`, do not put a token in a file inside a repository or in chat. `whoami` is safe: it shows only the blog, agent name, scopes, and a token prefix.
4. **Tokens are draft-only by default.** `publish` and `profile` are opt-in scopes the user ticks when connecting (or when creating a token). They can be revoked from the dashboard (Settings, Writers, agent tokens). If a command needs a scope the token lacks, tell the user which scope and let them reconnect. Do not retry.
5. **Write in the user's language** unless they say otherwise. Keep their voice; do not invent facts, quotes, or links.

## 1. Install

Check first:

```bash
command -v itsjustmy && itsjustmy --version
```

If missing, check Ruby. The installer needs Ruby 3.2 or newer with `ruby` and `gem` on the PATH:

```bash
ruby -v
gem -v
```

If either command is not found, or Ruby is older than 3.2, the installer stops with exit status 1, downloads nothing, and prints how to install Ruby. Tell the user and ask before installing system packages:

- macOS: `brew install ruby`
- Debian/Ubuntu: `sudo apt install ruby-full`

Then open a new terminal (or fix the PATH) and check `ruby -v` again. Install the CLI:

```bash
curl -fsSL https://itsjustmy.blog/install.sh | bash
export PATH="$HOME/.local/bin:$PATH"
itsjustmy --version
```

The script downloads the gem from itsjustmy.blog and installs it for the current user into `~/.local/bin`. If `~/.local/bin` is not on the PATH, the script says so; the user can add the `export` line to `~/.zshrc` or `~/.bashrc`. If the installer gets a 404, the user probably has an old copy of `install.sh`: run the command above again, and unset `ITSJUSTMY_INSTALL_HOST` unless it is intentional.

**Update:** re-run the same installer. No uninstall needed.

## 2. Log in

```bash
itsjustmy login
```

This opens the browser at itsjustmy.blog, where the user picks the blog and presses Connect (and ticks `publish` / `profile` only if they want them). The code is single-use. The command waits up to five minutes, then saves the token to `~/.config/itsjustmy/credentials.json` (readable only by the user). Tell the user to complete the browser step; if the browser did not open, they can copy the URL the command prints. If the code expired or was already used, run `itsjustmy login` again. A new account can use `itsjustmy register`.

Agent without a browser: the user creates a token in the dashboard (Settings, Writers, agent tokens) and exports `ITSJUSTMY_TOKEN` in the environment outside any repository. Precedence: `--token`, then `ITSJUSTMY_TOKEN`, then the credentials file.

Before writing, check who you are:

```bash
itsjustmy whoami --json
```

Read the scopes. Everyone has `draft`.

| Scope | Allows |
| --- | --- |
| `draft` (always) | Create and edit drafts, upload images, list posts, create series and categories. Not editing published posts or their covers. |
| `publish` (opt-in) | Publish; edit published posts and their covers; `blog update` (name, tagline, bio). |
| `profile` (opt-in) | `profile update`. `profile show` works with any token. |

## 3. Commands

Add `--json` to get machine-readable output, and `--dry-run` to print the request without sending it (a dry run also skips uploading local images).

```bash
itsjustmy whoami
itsjustmy --version                      # also: itsjustmy version, -v
itsjustmy posts list                     # up to 100: slug, status, title
itsjustmy posts get SLUG
itsjustmy categories list
itsjustmy series list
itsjustmy blog show
itsjustmy profile show
```

### Create a draft

`--title` is required, plus `--body` or `--body-file` (Markdown; `-` reads stdin; if both are given the file wins).

```bash
itsjustmy posts create --title "Título" --body-file borrador.md --json
echo '# Nota' | itsjustmy posts create --title "Nota" --body-file -
itsjustmy posts create --title "Título" --body-file borrador.md \
  --slug mi-slug --tags "uno, dos" --category-id 3 --series-id 8 --cover portada.png
```

Optional flags: `--slug` (defaults from the title), `--tags` (comma-separated), `--category-id` and `--series-id` (get ids from `categories list` and `series list`; create with `categories create --name NAME` / `series create --name NAME`), `--cover FILE`.

Write the Markdown to a file first (for example `draft.md` in a temp or working directory), run once with `--dry-run` if unsure, then create it. Report the slug and tell the user the post is a draft they can review in the dashboard.

### Images

- **In the body:** a local image in the Markdown, outside a code block, is uploaded automatically on `posts create` / `posts update`. PNG, JPEG, or WebP, max 5 MB. The path is relative to the Markdown file (or to the current directory when the body comes from `--body` or stdin). `http(s)` URLs are left alone; anything inside a fenced code block is not touched.

  ```markdown
  ![Mesa](./foto.png)
  ```

- **Get a URL first:** `itsjustmy images upload foto.png` prints a URL to paste in Markdown.
- **Cover** (different from images in the text): `--cover FILE` on create/update, or later:

  ```bash
  itsjustmy posts cover SLUG --file portada.png
  ```

  JPEG, PNG, or WebP, max 5 MB. A cover on an already-published post needs `publish`.

### Update a post

```bash
itsjustmy posts update SLUG --body-file borrador.md
```

Same flags as `create`, all optional. It does not publish. If the post is already published, the token needs `publish`.

### Profile (needs `profile`)

```bash
itsjustmy profile show
itsjustmy profile update --bio "Texto" --tagline "Frase" --website https://ejemplo.com
```

Flags: `--name`, `--last-name`, `--bio`, `--tagline`, `--cta-label`, `--cta-url`, `--website`, `--twitter`, `--instagram`, `--linkedin`, `--youtube`, `--github` (URLs), `--avatar FILE` (JPEG/PNG/WebP, max 5 MB), `--link "etiqueta|https://url"` (repeatable; replaces the extra-links list), `--clear-links`. At least one flag is required. Show the user the exact change before running it, since it edits their public profile.

### Publish (only when the user explicitly asks; needs `publish`)

```bash
itsjustmy posts publish SLUG
```

### Log out

```bash
itsjustmy logout
```

This deletes the token on this machine only. It does not revoke it: the user revokes it in the dashboard (Writers, agent tokens).

## Errors

- `No hay token. Ejecuta itsjustmy login.`: no token anywhere. Run `itsjustmy login`.
- `401 Token inválido`: revoked, incomplete, or from another environment. `logout` does not fix it; the user must connect again or create a new token.
- `403`: a missing scope, not a broken token. Run `whoami`, name the scope, stop.
- `command not found: itsjustmy`: `export PATH="$HOME/.local/bin:$PATH"` and check `which itsjustmy`.
