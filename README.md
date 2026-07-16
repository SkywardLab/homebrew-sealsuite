# Homebrew SealSuite

Homebrew tap for the SealSuite CLI on Apple Silicon macOS. The service configuration runs SealSuite in userspace SOCKS5 mode under the current user account.

## Install

```bash
brew tap SkywardLab/sealsuite
brew install sealsuite
```

## Configure SOCKS5

```bash
cp /opt/homebrew/etc/sealsuite/config.json.example \
  /opt/homebrew/etc/sealsuite/config.json
chmod 600 /opt/homebrew/etc/sealsuite/config.json
nano /opt/homebrew/etc/sealsuite/config.json
```

The example listens on the local loopback address:

```json
"socks5_listen": "127.0.0.1:1080"
```

Keep the configuration file readable and writable by the macOS user who runs the service so SealSuite can save generated login fields.

## Complete First Authentication

Run SealSuite in the foreground once to complete QR authentication, OTP setup, activation, and VPN selection:

```bash
/opt/homebrew/bin/SealSuite /opt/homebrew/etc/sealsuite/config.json
```

Stop the foreground process after a successful SOCKS5 connection.

## Manage the Service

```bash
brew services start sealsuite
brew services list
brew services restart sealsuite
brew services stop sealsuite
```

The SOCKS5 service runs as a user LaunchAgent. Its logs are stored at:

```text
/opt/homebrew/var/log/sealsuite.log
/opt/homebrew/var/log/sealsuite-error.log
```

## Upgrade

```bash
brew update
brew upgrade sealsuite
brew services restart sealsuite
```

The active `/opt/homebrew/etc/sealsuite/config.json` remains in place during upgrades.

## Uninstall

```bash
brew services stop sealsuite
brew uninstall sealsuite
brew untap SkywardLab/sealsuite
```

Remove `/opt/homebrew/etc/sealsuite/config.json` separately when its stored credentials and generated state are no longer needed.
