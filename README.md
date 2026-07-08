# SealSuite Releases

Public binary downloads for SealSuite.

SealSuite is a standalone SealSuite (Corplink) VPN client with a bundled WireGuard core. It signs in to SealSuite, handles 2FA, lets you choose a VPN server, starts the tunnel, and cleans up the session on exit.

## Latest release

Latest version: `v1.6.1`

Release page: https://github.com/SkywardLab/SealSuite-releases/releases/tag/v1.6.1

All releases: https://github.com/SkywardLab/SealSuite-releases/releases

Choose the GUI package for the desktop app. Choose the CLI package for terminal or service usage.

## Downloads

### GUI packages

| Platform | CPU | Asset |
| --- | --- | --- |
| Linux | amd64 | [`SealSuite-GUI-v1.6.1-linux-amd64.tar.xz`](https://github.com/SkywardLab/SealSuite-releases/releases/download/v1.6.1/SealSuite-GUI-v1.6.1-linux-amd64.tar.xz) |
| macOS | Apple Silicon | [`SealSuite-GUI-v1.6.1-macos-arm64.tar.xz`](https://github.com/SkywardLab/SealSuite-releases/releases/download/v1.6.1/SealSuite-GUI-v1.6.1-macos-arm64.tar.xz) |
| Windows | amd64 | [`SealSuite-GUI-v1.6.1-windows-amd64.zip`](https://github.com/SkywardLab/SealSuite-releases/releases/download/v1.6.1/SealSuite-GUI-v1.6.1-windows-amd64.zip) |

### CLI packages

| Platform | CPU | Asset |
| --- | --- | --- |
| Linux | amd64 | [`SealSuite-CLI-v1.6.1-linux-amd64.tar.xz`](https://github.com/SkywardLab/SealSuite-releases/releases/download/v1.6.1/SealSuite-CLI-v1.6.1-linux-amd64.tar.xz) |
| macOS | Apple Silicon | [`SealSuite-CLI-v1.6.1-macos-arm64.tar.xz`](https://github.com/SkywardLab/SealSuite-releases/releases/download/v1.6.1/SealSuite-CLI-v1.6.1-macos-arm64.tar.xz) |
| Windows | amd64 | [`SealSuite-CLI-v1.6.1-windows-amd64.zip`](https://github.com/SkywardLab/SealSuite-releases/releases/download/v1.6.1/SealSuite-CLI-v1.6.1-windows-amd64.zip) |

### Arch Linux package

| Package | Asset |
| --- | --- |
| Runtime package | [`sealsuite-1.6.1-1-x86_64.pkg.tar.zst`](https://github.com/SkywardLab/SealSuite-releases/releases/download/v1.6.1/sealsuite-1.6.1-1-x86_64.pkg.tar.zst) |
| Debug package | [`sealsuite-debug-1.6.1-1-x86_64.pkg.tar.zst`](https://github.com/SkywardLab/SealSuite-releases/releases/download/v1.6.1/sealsuite-debug-1.6.1-1-x86_64.pkg.tar.zst) |

## Run SealSuite

Extract the archive and keep `config.json` beside the binary.

macOS or Linux:

```bash
./SealSuite ./config.json
```

Windows PowerShell:

```powershell
.\SealSuite.exe .\config.json
```

The GUI relaunches only its tunnel helper with administrator privileges when WireGuard or DNS setup needs system networking access. The CLI relaunches with root or administrator privileges when required.

Linux desktop users need a graphical privilege prompt provider such as `pkexec` for GUI tunnel startup. Terminal CLI runs can use `sudo`.

## Configuration

Minimal `config.json`:

```json
{
  "company_name": "company code",
  "username": "your_name",
  "password": "your_password"
}
```

The client saves generated fields back into the same config file, including `server`, `device_id`, WireGuard keys, login `state`, and saved TOTP secrets.

Required fields:

| Field | Description |
| --- | --- |
| `company_name` | SealSuite company code used to discover the server URL. Use this or `server`. |
| `server` | Direct SealSuite server URL. Use this or `company_name`. |
| `username` | SealSuite username. |
| `password` | SealSuite password. |
| `platform` | Optional login method. Defaults to `ldap`. |

Useful optional fields:

| Field | Default | Description |
| --- | --- | --- |
| `vpn_server_name` | `null` | Exact VPN `en_name` to select. |
| `vpn_select_strategy` | GUI selection, or `default` in non-TTY runs | `default` chooses the first pingable server; `latency` chooses the lowest-latency server. |
| `use_vpn_dns` | `true` | Applies VPN DNS while the tunnel runs. |
| `auto_setup_routes` | `true` | Automatically configures routes from the VPN response. |
| `route_mode` | `split` | Route mode saved in config. |
| `vpn_disallowed_routes` | `null` | CIDR routes carved out of WireGuard allowed IPs and OS route setup. |
| `force_protocol` | `null` | Optional WireGuard transport override. Use `udp` or `tcp`; unset follows supported server protocol modes. |
| `verify_tls` | `true` | TLS certificate verification. |
| `debug_wg` | `false` | Enables verbose WireGuard logs. |
| `state` | `null` | Login state cache. Set to `Init` to force a fresh login. |
| `socks5_listen` | `null` | Enables userspace netstack mode and exposes a SOCKS5 proxy at this address. |
| `socks5_username` | `null` | Optional SOCKS5 username. Empty value allows no-auth clients. |
| `socks5_password` | `null` | Optional SOCKS5 password used when `socks5_username` is set. |

Environment variables override selected config values:

| Environment variable | Config field |
| --- | --- |
| `CORPLINK_SERVER` | `server` |
| `CORPLINK_USERNAME` | `username` |
| `CORPLINK_PASSWORD` | `password` |
| `CORPLINK_PLATFORM` | `platform` |

## First login

On first login, SealSuite may ask for QR-based third-party authentication. The GUI shows a QR dialog. The CLI prints an ASCII QR code and login URL.

After a TOTP secret is saved into `code`, later VPN connections generate 2FA codes automatically.

## Verify a download

GitHub release asset metadata includes a SHA-256 digest for each uploaded file. Compare the digest after download.

macOS or Linux:

```bash
shasum -a 256 <archive>
```

Windows PowerShell:

```powershell
Get-FileHash <archive> -Algorithm SHA256
```

Check archive integrity before deployment:

```bash
tar -tJf <archive>.tar.xz
```

Windows PowerShell:

```powershell
Expand-Archive <archive>.zip -DestinationPath <tmpdir>
```

## Security notes

Keep real config files private. Treat `password`, `code`, cookies, private keys, and VPN tokens as secrets. Redact usernames, domains, cookies, tokens, OTP codes, private keys, and full response bodies before sharing logs.
