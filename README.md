# SealSuite Releases

Public release assets for SealSuite.

SealSuite (Corplink) is a standalone VPN client with a bundled WireGuard core. It logs in to SealSuite, handles 2FA, lets you choose a VPN server, starts the tunnel, and cleans up the session on exit.

## Download

- Latest release: https://github.com/SkywardLab/SealSuite-releases/releases/latest
- All releases: https://github.com/SkywardLab/SealSuite-releases/releases

Pick the archive that matches your platform and CPU architecture.

### Asset names

CLI archives:
- `SealSuite-CLI-vX.Y.Z-linux-amd64.tar.xz`
- `SealSuite-CLI-vX.Y.Z-linux-arm64.tar.xz`
- `SealSuite-CLI-vX.Y.Z-macos-arm64.tar.xz`
- `SealSuite-CLI-vX.Y.Z-macos-x86_64.tar.xz`
- `SealSuite-CLI-vX.Y.Z-windows-amd64.zip`
- `SealSuite-CLI-vX.Y.Z-windows-arm64.zip`

GUI archives:
- `SealSuite-GUI-vX.Y.Z-linux-amd64.tar.xz`
- `SealSuite-GUI-vX.Y.Z-linux-arm64.tar.xz`
- `SealSuite-GUI-vX.Y.Z-macos-arm64.tar.xz`
- `SealSuite-GUI-vX.Y.Z-macos-x86_64.tar.xz`
- `SealSuite-GUI-vX.Y.Z-windows-amd64.zip`
- `SealSuite-GUI-vX.Y.Z-windows-arm64.zip`

Arch Linux package:
- one `*.pkg.tar.zst` artifact on the release page

## Version notes

- Each release uses a Git tag such as `v1.2.5`.
- The tag appears in every archive name.
- CLI and GUI packages ship as separate archives.
- Release notes live on the GitHub release page for each tag.

## Usage

### 1. Download and extract

Unpack the archive into a local folder.

Examples:

- Linux: `tar -xJf SealSuite-GUI-vX.Y.Z-linux-amd64.tar.xz`
- macOS: `tar -xJf SealSuite-GUI-vX.Y.Z-macos-arm64.tar.xz`
- Windows: extract the `.zip` archive with Explorer or PowerShell

### 2. Prepare the configuration file

Each packaged archive includes a starter `config.json` beside the binary.

Default configuration paths:

- Windows: `%APPDATA%\Corplink\config.json`
- macOS: `~/.config/corplink/config.json`
- Linux: `~/.config/corplink/config.json`

Copy the starter file into the default path, then edit the values for your account.

Required fields:

| Field | Description |
| --- | --- |
| `company_name` | SealSuite company code used to discover the server URL. Use this or `server`. |
| `username` | SealSuite username. |
| `password` | SealSuite password. |
| `platform` | Optional login method. Defaults to `ldap`. |

Useful optional fields:

| Field | Default | Description |
| --- | --- | --- |
| `vpn_server_name` | `null` | Exact VPN `en_name` to select. |
| `vpn_select_strategy` | GUI selection. | `default` chooses the first pingable server; `latency` chooses the lowest-latency server. |
| `use_vpn_dns` | `false` | Applies VPN DNS while the tunnel runs. |
| `auto_setup_routes` | `true` | Automatically configures routes from the VPN response. |
| `route_mode` | `split` | Route mode saved in config. |
| `vpn_disallowed_routes` | `null` | CIDR routes to keep in WireGuard allowed IPs while skipping OS route setup. |
| `verify_tls` | `true` | TLS certificate verification. |
| `debug_wg` | `false` | Enables verbose WireGuard logs. |
| `state` | `null` | Login state cache. Set to `Init` to force a fresh login. |

The app may add fields such as `code` and `activation_key` after the first successful run.

### 3. Start the client

GUI:

- Linux/macOS: run the GUI binary from the extracted folder
- Windows: run `SealSuite.exe`

CLI:

- Linux/macOS: run the CLI binary from the extracted folder
- Windows: run `SealSuite.exe`

You can also pass a config file path on the command line when you want to use a custom location.

## Verification guide

Use a local checksum and archive test after download.

### SHA-256

- macOS/Linux: `shasum -a 256 <archive>` or `sha256sum <archive>`
- Windows PowerShell: `Get-FileHash <archive> -Algorithm SHA256`

Save the hash in your deployment record, then compare it with your own approved reference for the same release.

### Archive integrity

- Linux/macOS tarball: `tar -tJf <archive>`
- Windows zip: `Expand-Archive <archive> -DestinationPath <tmpdir>`

A successful extraction confirms the archive structure and file list.
