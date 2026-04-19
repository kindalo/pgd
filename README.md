# PGD (Process Guardian Daemon)

**A Bash-based Process Supervisor**

A simple but feature-complete Bash script for monitoring and managing system processes. When a monitored process terminates unexpectedly, PGD automatically restarts it.

## Features

- 🔄 **Auto Restart**: Monitor specified processes and automatically restart them on failure
- ⚙️ **Dynamic Configuration**: Supports SIGHUP signal for hot-reloading configuration
- 🛡️ **Systemd Integration**: Can be registered as a systemd user service
- 📊 **Status Monitoring**: Real-time status of daemon and all monitored tasks
- 📝 **Logging**: Complete runtime logs and error tracking

## Installation

### Method 1: Install to System PATH

```bash
cd ~/Projects/pgd-git
./pgd install
```

After installation, you can run `pgd` from anywhere.

### Method 2: As systemd Service

```bash
cd ~/Projects/pgd-git
./pgd register
```

This registers PGD as a user-level systemd service with auto-start on login.

## Usage

### Basic Commands

```bash
# Start the daemon
./pgd daemon

# Check status
./pgd status

# Stop the daemon
./pgd stop

# View logs
./pgd log
```

### Managing Monitored Tasks

```bash
# Add a new monitored task
./pgd add <task_name> "<command>" <check_interval_seconds>

# Example: Monitor a Node.js app (check every 10 seconds)
./pgd add myapp "node /home/user/app.js" 10

# Monitor Redis server (check every 5 seconds)
./pgd add redis "redis-server /etc/redis/redis.conf" 5

# Remove a monitored task
./pgd delete <task_name>

# List all configured tasks
./pgd list
```

### Systemd Service Management

```bash
# Register as systemd service
./pgd register

# Check service status
systemctl --user status pgd.service

# Stop the service
systemctl --user stop pgd.service

# Start the service
systemctl --user start pgd.service

# Unregister
./pgd unregister
```

## Command Reference

| Command | Description |
|---------|-------------|
| `daemon` | Start the monitoring daemon |
| `add <name> <cmd> <int>` | Add a new monitored task |
| `delete <name>` | Remove a monitored task |
| `list` | List all configurations |
| `log` | View runtime logs |
| `status` | Show daemon and task status |
| `stop` | Stop the daemon |
| `register` | Register as systemd service |
| `unregister` | Unregister systemd service |
| `install` | Install to system PATH |

## Notes

- Configuration file uses `:` as delimiter. Commands containing `:` need special handling
- It's recommended to wrap commands in quotes to avoid special character issues
- The daemon must be running continuously to monitor processes
- Ensure execute permission before first use: `chmod +x pgd`

