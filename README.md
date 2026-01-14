# Minecraft Technical SMP Server

A Paper Minecraft server with automatic backups, cross-platform support (Java + Bedrock), and multi-version compatibility.

## Features

- **Paper Server**: Latest Paper version with Aikar's optimized flags for better performance
- **Cross-Platform**: Bedrock players can join via Geyser + Floodgate
- **Multi-Version Support**: ViaVersion and ViaBackwards allow older clients to connect
- **Automatic Backups**: DriveBackupV2 with Google Drive integration (every 2 hours)
- **GitOps Config**: Version-controlled server configuration

## Quick Start

```bash
# Clone the repository
git clone <your-repo-url>
cd minecraft

# Start the server
docker-compose up -d

# View logs
docker-compose logs -f

# Stop the server
docker-compose down
```

## Server Details

- **Server Type**: Paper (Latest)
- **Memory**: 8GB
- **Max Players**: 20
- **Java Port**: 25565
- **Bedrock Port**: 19132 (UDP)
- **RCON Port**: 25575

## Plugins

### Installed via Direct Download
- **Geyser**: Allows Bedrock Edition players to join
- **Floodgate**: Removes Xbox Live authentication requirement for Bedrock players

### Installed via Modrinth
- **ViaVersion**: Supports newer Minecraft versions
- **ViaBackwards**: Supports older Minecraft versions
- **DriveBackupV2**: Automatic cloud backups

## Backup Configuration

Backups are configured in `config/plugins/DriveBackupV2/config.yml`:
- **Frequency**: Every 2 hours (120 minutes)
- **Retention**: 10 cloud backups, 5 local backups
- **Storage**: Google Drive (configure credentials in the config file)
- **Backed up**: All world directories (`world*`)

To enable Google Drive backups:
1. Edit `config/plugins/DriveBackupV2/config.yml`
2. Set `googledrive.enabled: true`
3. Follow the plugin's authentication flow on first backup

## Configuration

### Server Properties
Server configuration is managed through:
1. Environment variables in `docker-compose.yaml`
2. GitOps config files in `./config/` directory

The config directory structure:
```
config/
├── config/           # Server config files
├── plugins/          # Plugin configurations
├── server.properties # Server properties
└── spigot.yml       # Spigot configuration
```

### Customizing Settings
Edit `docker-compose.yaml` environment variables or modify files in the `config/` directory. Changes are applied on container restart.

## Data Persistence

Server data is stored in `/home/itzmaniss/coolify/minecraft` on the host. Update the volume mount in `docker-compose.yaml` to change the location:

```yaml
volumes:
  - /your/path/here:/data
```

## RCON Access

RCON is enabled for remote console access:
- **Port**: 25575
- **Password**: `password` (change in docker-compose.yaml)

## Maintenance

### Updating the Server
```bash
docker-compose pull
docker-compose up -d
```

### Viewing Server Console
```bash
docker attach minecraft-technical-smp
```
Press `Ctrl+P, Ctrl+Q` to detach without stopping the server.

### Backing Up Manually
Backups run automatically, but you can trigger one via RCON:
```bash
docker exec minecraft-technical-smp rcon-cli backup
```

## Troubleshooting

### Permission Issues
The container automatically fixes ownership with `FIX_OWNERSHIP: "true"`. If you encounter permission errors, ensure the host directory is writable.

### Memory Issues
If the server runs out of memory, adjust the `MEMORY` environment variable in `docker-compose.yaml`.

### Port Conflicts
Ensure ports 25565, 19132, and 25575 are not in use by other services.

## License

This configuration is provided as-is for personal use.
