# ansible-isc-dhcp-server

An Ansible role for installing and configuring ISC DHCP Server on Linux systems.

## Features

- Multi-distribution support (Debian/Ubuntu, RHEL/CentOS, SUSE)
- Configurable network settings
- Static host reservations support
- Proper service management with handlers
- Backup of existing configuration
- Ansible tags for selective execution

## Requirements

- Ansible 2.9 or higher
- Target system with systemd
- Root privileges (using `become: true`)

## Role Variables

### Network Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `dhcp_subnet` | `192.168.1.0` | DHCP subnet |
| `dhcp_netmask` | `255.255.255.0` | Subnet mask |
| `dhcp_range_start` | `192.168.1.100` | Start of IP range |
| `dhcp_range_end` | `192.168.1.200` | End of IP range |
| `dhcp_gateway` | `192.168.1.1` | Default gateway |
| `dhcp_dns_servers` | `"8.8.8.8, 8.8.4.4"` | DNS servers |

### Static Host Reservations

Optional static IP assignments for specific MAC addresses:

```yaml
dhcp_static_hosts:
  - hostname: workstation1
    mac_address: "00:11:22:33:44:55"
    ip_address: "192.168.1.50"
    dns_servers: "8.8.8.8, 8.8.4.4"  # Optional, uses global DNS if not specified
  - hostname: server1
    mac_address: "aa:bb:cc:dd:ee:ff"
    ip_address: "192.168.1.51"
```

### System Configuration

These variables are automatically set based on the target OS:

| Variable | Description |
|----------|-------------|
| `dhcp_package_name` | OS-specific package name |
| `dhcp_service_name` | OS-specific service name |
| `dhcp_config_path` | OS-specific config file path |

## Dependencies

None.

## Example Playbook

### Basic Usage

```yaml
- hosts: dhcp-servers
  become: true
  roles:
    - ansible-isc-dhcp-server
```

### Custom Configuration

```yaml
- hosts: dhcp-servers
  become: true
  vars:
    dhcp_subnet: 10.0.0.0
    dhcp_netmask: 255.255.255.0
    dhcp_range_start: 10.0.0.100
    dhcp_range_end: 10.0.0.199
    dhcp_gateway: 10.0.0.1
    dhcp_dns_servers: "10.0.0.1, 8.8.8.8"
    dhcp_static_hosts:
      - hostname: printer
        mac_address: "08:00:27:12:34:56"
        ip_address: "10.0.0.50"
  roles:
    - ansible-isc-dhcp-server
```

## Tags

Use tags to run specific parts of the role:

- `dhcp_install` - Only install the package
- `dhcp_config` - Only update configuration
- `dhcp_service` - Only manage service state

Example:
```bash
ansible-playbook playbook.yml --tags dhcp_config
```

## Supported Platforms

- Debian/Ubuntu (tested)
- RHEL/CentOS 7+ (tested)
- SUSE Linux Enterprise

## Directory Structure

```
roles/dhcp-server/
├── defaults/
│   └── main.yml          # Default variables
├── handlers/
│   └── main.yml          # Service restart/reload handlers
├── tasks/
│   └── main.yml          # Main tasks
└── templates/
    └── dhcpd.conf.j2     # DHCP server configuration template
```

## License

MIT

## Author Information

Created by [Your Name] - 2024