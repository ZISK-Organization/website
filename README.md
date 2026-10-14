# ZISK website

Build the site locally with `hugo --minify --environment production`. Hugo writes
the complete site, including assets from `static/`, to `public/`.

## Deployment

`.github/workflows/deploy.yml` builds the site and uploads the contents of
`public/` using SCP on every push to `main`. Deployments run one at a time.
The pinned Hugo release comes from the [official Hugo releases](https://github.com/gohugoio/hugo/releases).

In GitHub, open **Settings → Secrets and variables → Actions** and add these
repository secrets:

| Secret | Value |
| --- | --- |
| `DEPLOY_HOST` | Server hostname or IPv4 address. |
| `DEPLOY_USER` | SSH user that can write to the destination directory. |
| `DEPLOY_PORT` | Optional SSH port; defaults to `22`. |
| `DEPLOY_PATH` | Existing absolute web root, e.g. `/var/www/zisk`. Use only letters, numbers, underscores, dots, slashes, and hyphens. |
| `DEPLOY_SSH_KEY` | Full private SSH key for deployment, without a passphrase. |
| `DEPLOY_KNOWN_HOSTS` | Trusted OpenSSH `known_hosts` entry for the server. For a nonstandard port, use `[hostname]:port` in the entry. |

Install the matching public key in the deployment user's `~/.ssh/authorized_keys`
and create the destination directory with permissions allowing that user to write.
The server must support SSH/SFTP and serve this directory as the site's web root.
Obtain the host key through a trusted connection or verify its fingerprint with
your server administrator before saving it in `DEPLOY_KNOWN_HOSTS`.

Uploads overwrite matching files and leave existing files that are absent from
the new build. SCP uploads are not atomic, so visitors may see mixed versions
while a deployment is in progress. The configured public URL is
`https://zisk-go.com/` in `hugo.toml`.
