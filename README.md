# LinkStack (LazyCat LPK)

Self-hosted link-in-bio page.

- Upstream: https://github.com/LinkStackOrg/LinkStack
- Image: `linkstackorg/linkstack:v4.8.8` (Docker Hub, mirror mode, digest-matched)
- License: AGPL-3.0 (full text in `content/licenses/LICENSE`)
- Containers: LinkStack (Apache/PHP) + MariaDB 10.11

## Version note

The image publishes `v`-prefixed tags (`v4.8.8`), so `tag_regex` matches the `v` and
`version_regex` captures the numeric part into a group named `version`. Without the named
group the action rejects the config with `CONFIG_INVALID`.

## Notes

- Port 80, user `apache:apache`, entry `docker-entrypoint.sh`.
- **`APP_KEY` is mandatory** — Laravel will not boot without it. It comes from a platform
  stable secret; the deploy-time value is never hardcoded.
- **Admin is created in the web installer, not by env vars.** `routes/web.php` registers
  `InstallerController` with `create-admin` / `db` / `mysql` / `options` steps, gated on
  `INSTALLING` / `INSTALLERLOCK` files and `storage/app/ISINSTALLED`. DB connection is already
  preconfigured here, so the operator only sets their own admin credentials in the wizard.
- `storage` persists on `/lzcapp/var/linkstack/storage`; MariaDB data on `/lzcapp/var/mysql`.
- No mail service configured.
- Not yet verified on a real device.
