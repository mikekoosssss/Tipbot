# Community Tipbot Configuration Guide

This guide explains all configuration options for the Community Tipbot.

## Configuration File Structure

The main configuration is stored in `config/config.json`. Here's a detailed breakdown of each section:

## Bot Configuration

```json
{
  "bot": {
    "token": "YOUR_BOT_TOKEN_HERE",
    "name": "Community Tipbot",
    "admin_users": [123456789],
    "allowed_groups": [],
    "cooldown_seconds": 30,
    "max_tip_amount": 1000000,
    "min_tip_amount": 0.001
  }
}
```

### Bot Settings

- **token**: Your Telegram bot token from @BotFather
- **name**: Display name for the bot
- **admin_users**: Array of Telegram user IDs with admin privileges
- **allowed_groups**: Array of group IDs where bot can operate (empty = all groups)
- **cooldown_seconds**: Minimum time between commands per user
- **max_tip_amount**: Maximum amount that can be tipped in one transaction
- **min_tip_amount**: Minimum amount that can be tipped

## Feature Toggles

```json
{
  "features": {
    "tipping": true,
    "rain": true,
    "airdrops": true,
    "withdrawals": true,
    "deposits": true,
    "backup": true
  }
}
```

### Feature Controls

- **tipping**: Enable/disable tip functionality
- **rain**: Enable/disable rain distribution
- **airdrops**: Enable/disable airdrop events
- **withdrawals**: Enable/disable coin withdrawals
- **deposits**: Enable/disable deposit detection
- **backup**: Enable/disable wallet backup feature

## Coin Configuration

```json
{
  "coins": {
    "AEGS": {
      "enabled": true,
      "cli_path": "aegisum-cli",
      "decimals": 8,
      "min_confirmations": 3,
      "withdrawal_fee": 0.001,
      "network_fee": 0.0001,
      "rpc_host": "127.0.0.1",
      "rpc_port": 8332,
      "rpc_user": "aegisumrpc",
      "rpc_password": "your_rpc_password"
    }
  }
}
```

### Coin Settings

- **enabled**: Whether this coin is active
- **cli_path**: Path to the coin's CLI executable
- **decimals**: Number of decimal places for the coin
- **min_confirmations**: Required confirmations for deposits
- **withdrawal_fee**: Fee charged for withdrawals
- **network_fee**: Default network transaction fee
- **rpc_host**: RPC server host (usually localhost)
- **rpc_port**: RPC server port
- **rpc_user**: RPC username
- **rpc_password**: RPC password

## Database Configuration

```json
{
  "database": {
    "path": "data/tipbot.db"
  }
}
```

### Database Settings

- **path**: Path to SQLite database file

## Security Configuration

```json
{
  "security": {
    "encryption_key": "GENERATE_RANDOM_KEY_HERE",
    "backup_encryption": true,
    "require_backup_confirmation": true
  }
}
```

### Security Settings

- **encryption_key**: Fernet encryption key for wallet backups
- **backup_encryption**: Whether to encrypt wallet backups
- **require_backup_confirmation**: Require user confirmation for backups

## Notification Settings

```json
{
  "notifications": {
    "pending_tx": true,
    "confirmed_tx": true,
    "deposit_detected": true,
    "withdrawal_sent": true
  }
}
```

### Notification Controls

- **pending_tx**: Send notifications for pending transactions
- **confirmed_tx**: Send notifications when transactions confirm
- **deposit_detected**: Notify users of new deposits
- **withdrawal_sent**: Notify users when withdrawals are sent

## Admin Dashboard Configuration

```json
{
  "admin_dashboard": {
    "enabled": true,
    "port": 12000,
    "host": "0.0.0.0",
    "username": "admin",
    "password": "change_this_password"
  }
}
```

### Dashboard Settings

- **enabled**: Whether admin dashboard is active
- **port**: Port for web dashboard
- **host**: Host binding (0.0.0.0 for all interfaces)
- **username**: Admin login username
- **password**: Admin login password

## Environment Variables

You can override configuration values using environment variables:

```bash
export TIPBOT_TOKEN="your_bot_token"
export TIPBOT_ADMIN_PASSWORD="secure_password"
export TIPBOT_ENCRYPTION_KEY="your_encryption_key"
```

## Advanced Configuration

### Custom Coin Addition

To add a new coin, add it to the coins section:

```json
{
  "NEWCOIN": {
    "enabled": true,
    "cli_path": "newcoin-cli",
    "decimals": 8,
    "min_confirmations": 6,
    "withdrawal_fee": 0.01,
    "network_fee": 0.001,
    "rpc_host": "127.0.0.1",
    "rpc_port": 8336,
    "rpc_user": "newcoinrpc",
    "rpc_password": "password"
  }
}
```

### Rate Limiting

Configure rate limiting per user:

```json
{
  "rate_limiting": {
    "tips_per_hour": 10,
    "rain_per_day": 3,
    "withdrawals_per_day": 5
  }
}
```

### Group Restrictions

Restrict bot to specific groups:

```json
{
  "bot": {
    "allowed_groups": [-1001234567890, -1001234567891]
  }
}
```

### Fee Structures

Configure dynamic fees:

```json
{
  "coins": {
    "AEGS": {
      "fee_structure": {
        "type": "percentage",
        "rate": 0.001,
        "min_fee": 0.0001,
        "max_fee": 0.01
      }
    }
  }
}
```

## Configuration Validation

The bot validates configuration on startup. Common validation errors:

### Missing Required Fields
```
Error: Bot token is required
Error: CLI path required for coin AEGS
```

### Invalid Values
```
Error: Decimals must be integer for coin AEGS
Error: admin_users must be a list
```

### Network Issues
```
Error: Cannot connect to AEGS daemon
Error: RPC authentication failed for SHIC
```

## Configuration Management

### Backup Configuration
```bash
cp config/config.json config/config.json.backup
```

### Validate Configuration
```bash
python3 -c "
import json
with open('config/config.json') as f:
    config = json.load(f)
print('Configuration is valid JSON')
"
```

### Update Configuration
1. Stop the bot service
2. Edit configuration file
3. Validate changes
4. Restart bot service

```bash
sudo systemctl stop community-tipbot
nano config/config.json
sudo systemctl start community-tipbot
```

## Security Best Practices

### Encryption Key Generation
```bash
python3 -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())"
```

### Secure Password Generation
```bash
openssl rand -base64 32
```

### File Permissions
```bash
chmod 600 config/config.json
chown tipbot:tipbot config/config.json
```

## Monitoring Configuration

### Log Levels
```json
{
  "logging": {
    "level": "INFO",
    "file": "logs/bot.log",
    "max_size": "10MB",
    "backup_count": 5
  }
}
```

### Health Checks
```json
{
  "health_checks": {
    "enabled": true,
    "interval": 60,
    "endpoints": [
      "http://localhost:8332",
      "http://localhost:8333"
    ]
  }
}
```

## Troubleshooting Configuration

### Common Issues

1. **Invalid JSON**: Use a JSON validator
2. **Wrong file paths**: Ensure paths are relative to bot directory
3. **Permission errors**: Check file ownership and permissions
4. **Network connectivity**: Verify RPC settings and daemon status

### Debug Mode
```json
{
  "debug": {
    "enabled": true,
    "log_level": "DEBUG",
    "log_sql": true
  }
}
```

### Configuration Testing
```bash
# Test configuration
python3 src/test_config.py

# Test coin connectivity
python3 src/test_coins.py

# Test database connection
python3 src/test_database.py
```

## Configuration Templates

### Minimal Configuration
```json
{
  "bot": {
    "token": "YOUR_TOKEN",
    "admin_users": [YOUR_USER_ID]
  },
  "coins": {
    "AEGS": {
      "enabled": true,
      "cli_path": "aegisum-cli"
    }
  },
  "features": {
    "tipping": true
  }
}
```

### Production Configuration
```json
{
  "bot": {
    "token": "YOUR_TOKEN",
    "admin_users": [YOUR_USER_ID],
    "cooldown_seconds": 10,
    "max_tip_amount": 100000
  },
  "coins": {
    "AEGS": {
      "enabled": true,
      "cli_path": "aegisum-cli",
      "min_confirmations": 6,
      "withdrawal_fee": 0.001
    }
  },
  "features": {
    "tipping": true,
    "rain": true,
    "withdrawals": true
  },
  "security": {
    "encryption_key": "SECURE_KEY",
    "backup_encryption": true
  }
}
```

---
*Powered By Aegisum EcoSystem*