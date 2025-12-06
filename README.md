# VPN Start

A Bash script to automate starting Mullvad VPN while enabling split tunneling for specific applications.

## Description

This script is designed to streamline the process of connecting to Mullvad VPN on Linux systems. It automatically identifies running processes based on a user-defined list and excludes them from the VPN tunnel (split tunneling) before establishing the connection. It also handles graceful shutdown by disconnecting the VPN when the script is terminated.

## Features

- **Automated Split Tunneling**: Automatically finds PIDs for specified applications (e.g., Plex, SSH, Sonarr) and adds them to Mullvad's split tunnel.
- **Auto-Connect**: Connects to Mullvad VPN after configuring split tunneling.
- **Status Check**: Verifies and displays the connection status.
- **Graceful Exit**: Disconnects the VPN when the script is stopped (e.g., via Ctrl+C or systemd).

## Prerequisites

- **Mullvad VPN**: The Mullvad VPN application and CLI must be installed and configured.
- **Bash**: A standard Bash shell environment.
- **Root/Sudo Privileges**: May be required depending on your Mullvad CLI configuration and permissions.

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/Idowza/VPN-Start.git
   cd VPN-Start
   ```

2. Make the script executable:
   ```bash
   chmod +x VPNstart
   ```

## Configuration

The list of applications to exclude from the VPN is defined in the `VPNstart` script.

1. Open `VPNstart` in a text editor.
2. Locate the `EXCLUDED_PROCESSES` array:
   ```bash
   EXCLUDED_PROCESSES=(
       "vnc"
       "plex"
       "ssh"
       # ... add your applications here
   )
   ```
3. Add or remove process names as needed.

## Usage

### Manual Execution

Run the script directly from the terminal:

```bash
./VPNstart
```

To stop the script and disconnect the VPN, press `Ctrl+C`.

### Running as a Systemd Service

The script is designed to work well as a systemd service (daemon).

1. Create a service file (e.g., `/etc/systemd/system/vpn-start.service`).
2. Add the following configuration (adjust paths as necessary):
   ```ini
   [Unit]
   Description=Mullvad VPN Start Script
   After=network.target

   [Service]
   ExecStart=/path/to/VPN-Start/VPNstart
   Restart=always
   User=root

   [Install]
   WantedBy=multi-user.target
   ```
3. Enable and start the service:
   ```bash
   sudo systemctl enable --now vpn-start
   ```

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.
