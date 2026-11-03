# PHP2Shell (CVE-2026-109618, CVE-2026-109619): what IPTV panel owners should check

On 21 October 2026 the PHP project released security updates for two critical flaws, together called **PHP2Shell**.
An attacker can trigger memory errors in the PHP request cycle and reach **remote code execution without logging in**.
CISA added both CVEs to its Known Exploited Vulnerabilities list on 26 October 2026.

| | |
|---|---|
| CVE | [CVE-2026-109618](https://nvd.nist.gov/vuln/detail/CVE-2026-109618), [CVE-2026-109619](https://nvd.nist.gov/vuln/detail/CVE-2026-109619) |
| Affected (PHP advisory) | PHP 8.2 before 8.2.35, 8.3 before 8.3.36, 8.4 before 8.4.27, 8.5 before 8.5.12 |
| Fixed in | 8.2.35, 8.3.36, 8.4.27, 8.5.12 |
| Severity (NVD CVSS 3.1) | CVE-2026-109618: 9.8, CVE-2026-109619: 8.1 (PHP CVSS 4.0: 9.2 for both) |
| Actively exploited | Yes, CISA KEV since 2026-10-26 |
| PHP 7.x | End of life since 2022: not assessed and not patched by the PHP project |
| Advisories | [GHSA-w8hw-w9fx-j79w](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w), [GHSA-6wmf-4rv5-p3rp](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp) |

## Why IPTV panels are a special case
Most IPTV panels (Xtream UI, XUI.ONE, Xtream Codes forks) ship **their own bundled PHP binary** inside the panel folder.
`apt upgrade` does not update that binary. If the panel is no longer maintained, nobody ships a fixed build.

## 1. Find which PHP your panel really runs
```bash
# list running PHP-FPM / PHP processes and the binary path they use
ps -eo pid,cmd | grep -E "php(-fpm)?" | grep -v grep

# then ask that exact binary for its version, for example:
/path/from/above/php -v
```
Check every main server and every load balancer.

## 2. Reduce exposure now (any panel)
- Put the admin and reseller login behind access codes or a non-default path, and firewall the panel port to known IPs where you can.
- Firewall SSH and database ports to known IPs.
- Keep streaming ports open only on the servers that need them.
- Take and download a fresh database backup before changing anything.

## 3. Fix it properly
- **Maintained panel:** apply the vendor's PHP update on every server.
- **Unmaintained panel (no vendor updates):** a bundled PHP that nobody patches stays exposed. Plan a migration to a maintained panel. Your database (users, lines, bouquets, streams, movies, series, resellers) can be moved, see [README](README.md).

## Xtream-Masters servers
Disclosure: this repository is maintained by the Xtream-Masters team.
- In the panel: **Manage Servers**, then for every server without the **Solo** tag: Main server, **Migrate to Solo**; Load Balancer, **Reinstall Balancer** and select **Solo**.
- If you cannot move to Solo yet, Xtream-Masters provides a patched PHP 7.4 build for its own servers (shown in the panel's security notice). It only works on Xtream-Masters installations.
- Migrating from Xtream UI, XUI.ONE or NXT: [migration guide](https://xtream-masters.com/guide/xtream_ui_to_xtream_masters_migration.php).
- Full guide with FAQ: [PHP2Shell IPTV panel PHP patch](https://xtream-masters.com/guide/php2shell_iptv_panel_php_patch.php).

Sources: NVD entries above (checked 2026-11-03), PHP security advisories GHSA-w8hw-w9fx-j79w and GHSA-6wmf-4rv5-p3rp, CISA KEV, oss-security list 2026-10-21.
