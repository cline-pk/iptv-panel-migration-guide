# IPTV Panel Migration Guide: Xtream UI, XUI.ONE, NXT to Xtream-Masters

Practical notes for moving an IPTV / OTT panel (Xtream UI, XUI.ONE, NXT and similar) to a new server or to the
Xtream-Masters OTT panel, without losing users, lines, bouquets, streams, movies or series.

> Xtream-Masters is a self-hosted IPTV/OTT panel (Xtream UI / XUI.ONE replacement) that installs on your own Ubuntu server
> and migrates your Xtream UI, XUI or NXT database in one step.
> Disclosure: this repository is maintained by the Xtream-Masters team.

## Contents
- [Before you start](#before-you-start)
- [Export your old panel database](#export-your-old-panel-database)
- [Migration options](#migration-options)
- [VOD paths per panel](#vod-paths-per-panel)
- [Ubuntu versions and FFmpeg](#ubuntu-versions-and-ffmpeg)
- [After migration checklist](#after-migration-checklist)
- [FAQ](#faq)

## Before you start
- Keep the old panel running until the new one is tested.
- Use a fresh Ubuntu install for the new main server (Ubuntu 24.04 recommended).
- Have SSH access to the main server and every load balancer.

## Export your old panel database
```bash
mysqldump -u USER -p DB_NAME > backup.sql
# optional: gzip backup.sql
```
Copy the file to the new main server as `/root/backup.sql` (or `.sql.gz`).

## Migration options
| Option | When to use | What you need |
|---|---|---|
| A. Remote connection | Old database is still reachable | host, port, database name, user, password |
| B. Backup file (recommended) | Old server is down or you want a clean copy | `/root/backup.sql` or `.sql.gz` on the new main server |

In the Xtream-Masters Master CMS panel: add the main server, wait for installation, click **Migrate Database**, choose A or B,
pick your old panel type, then **Migrate**. Afterwards reset the admin password (MasterAdmin > Reset Admin Password),
set the SSH password for the main server and every load balancer, and restart / re-install the balancers.

What moves: users, lines, bouquets, streams, movies, series, categories, resellers and settings.

## VOD paths per panel
After a migration, movies and episodes often stop playing because each panel stores files in a different folder.

| Panel | VOD path (movies and episodes) |
|---|---|
| Xtream UI | `/home/xtreamcodes/iptv_xtream_codes/movies/` |
| XUI.ONE | `/home/xui/content/vod/` |
| NXT | `/home/nxt/content/vod/` |
| Xtream-Masters | `/home/xtreamaster/movies/` |

If your VOD files were uploaded, symlinked or downloaded from URLs onto the server, move them to the new path on the main
server and on every load balancer that stores VOD. If you imported VOD directly from source URLs that still work, re-encode instead.

## Ubuntu versions and FFmpeg
| Ubuntu | FFmpeg build |
|---|---|
| 20.04 / 22.04 / 24.04 / 26.04 (x86) | FFmpeg v7 |
| 18.04 or older | FFmpeg v5 |

If every stream on one load balancer is down after a migration, the FFmpeg build may not match the OS: switch to the
matching build and restart the balancer service.

## After migration checklist
- [ ] Admin password reset and admin login works
- [ ] Main server and every load balancer online
- [ ] Test lines play live, movies and series
- [ ] EPG assigned (catchup needs a valid EPG)
- [ ] Access codes enabled for admin and reseller logins
- [ ] Backups enabled (local + cloud)

## FAQ
**Can I migrate from Xtream UI, XUI.ONE or NXT?** Yes, by remote database connection or a mysqldump backup file.

**Which Ubuntu version should I use?** A fresh Ubuntu 24.04 install is recommended; 20.04 to 26.04 are supported.

**What does Xtream-Masters cost?** €70/month from 1 January 2027, one license for the main server plus unlimited load balancers, with a 3-day refund window.

## Links
- Full FAQ and setup docs: https://ottpanel.tv/faqs
- Migration guide: https://xtream-masters.com/guide/xtream_ui_to_xtream_masters_migration.php
- Best IPTV panel software comparison: https://xtream-masters.com/guide/best_iptv_panel_software_2026.php
- Xtream-Masters OTT panel: https://xtream-masters.com/iptv_admin_panel_installer.php

## License
Documentation: CC BY 4.0. No panel software or source code is included in this repository.
