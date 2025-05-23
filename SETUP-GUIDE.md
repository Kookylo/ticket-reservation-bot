# Ticket Reservation Bot - Setup Guide

This guide will walk you through setting up and configuring the Ticket Reservation Bot system using n8n.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation Steps](#installation-steps)
- [Configuration](#configuration)
- [Testing](#testing)
- [Production Deployment](#production-deployment)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before starting, make sure you have:

- n8n installed (version 0.214.0 or higher)
- Node.js (v16+)
- API credentials for your ticketing service
- Slack webhook URL (optional, for notifications)

## Installation Steps

### 1. Install n8n (if not already installed)

```bash
# Using npm
npm install n8n -g

# Or using Docker
docker run -it --rm --name n8n -p 5678:5678 n8nio/n8n

# Or using Docker Compose
# Create a docker-compose.yml file with n8n configuration
# Then run:
docker-compose up -d
```

### 2. Set Up Your Project Files

Download the workflow files from this repository:
- `workflows/main-workflow.json`
- `workflows/error-recovery.json`
- `workflows/multi-ticket.json`

And the configuration files:
- `config/settings.json`
- `.env.example`

### 3. Create Environment Variables

1. Copy the environment template:
   ```bash
   cp .env.example .env
   ```

2. Edit the `.env` file with your credentials:
   ```
   # Ticket Service API Credentials
   TICKET_USER=your_username
   TICKET_PASS=your_password

   # Configuration Paths
   CONFIG_PATH=config/settings.json
   DB_PATH=data/reservations.json

   # Notification Settings (for Slack integration)
   SLACK_TOKEN=xoxb-your-slack-token
   SLACK_CHANNEL=ticket-alerts
   ```

### 4. Import Workflows into n8n

1. Start n8n:
   ```bash
   n8n start
   ```

2. Access the n8n web interface:
   - Open `http://localhost:5678` in your browser
   - Log in with your credentials

3. Import each workflow:
   - Go to **Workflows** → **Import from File**
   - Select each workflow JSON file from the `workflows/` folder:
     - `main-workflow.json`
     - `error-recovery.json`
     - `multi-ticket.json`
   - Click **Import** for each file

## Configuration

### 1. Set Up Environment Variables in n8n

1. In the n8n web interface, go to **Settings** → **Environment Variables**
2. Click **Add New Variable**
3. Add each variable from your `.env` file:
   - `TICKET_USER`
   - `TICKET_PASS`
   - `CONFIG_PATH`
   - `DB_PATH`
   - `SLACK_TOKEN` (if using Slack notifications)
   - `SLACK_CHANNEL` (if using Slack notifications)
4. Click **Save**
5. Restart n8n for the changes to take effect

### 2. Configure Settings

1. Open `config/settings.json`
2. Modify the settings to match your requirements:

```json
{
  "ticketService": "https://api.ticketservice.com",
  "holdDuration": 600,
  "extensionThreshold": 5,
  "checkInterval": 1,
  "retryAttempts": 3,
  "retryDelay": 10,
  "events": [
    {
      "eventId": "YOUR_EVENT_ID",
      "maxPrice": 500,
      "preferredSections": ["VIP", "Floor", "General"],
      "numTickets": 2
    }
  ]
}
```

Key settings to modify:
- `ticketService`: URL of your ticketing API
- `events`: Array of events to monitor (with IDs, price limits, etc.)
- `holdDuration`: How long to hold tickets (in seconds)
- `checkInterval`: How often to check reservation status (in minutes)

### 3. Configure Credentials in n8n

If your workflow uses any additional credentials (e.g., Slack):

1. Go to **Credentials** in n8n
2. Create any required credential entries
3. Update the workflow nodes to use these credentials

## Testing

### 1. Test Individual Workflows

1. Open each workflow in n8n
2. Click **Execute Workflow** to run a test
3. Check the execution results to ensure:
   - API connections are working
   - Authentication is successful
   - Ticket search is working
   - Reservation creation succeeds

### 2. Debug Common Issues

- Check the **Executions** tab for any error messages
- Verify credentials are correctly set up
- Ensure the ticketing service API is accessible
- Check that event IDs are valid

## Production Deployment

### 1. Activate Workflows

Once testing is successful:

1. Open each workflow
2. Toggle the "Active" switch in the top-right corner
3. Set appropriate trigger schedules if needed

### 2. Monitor Initial Executions

1. Watch the first few automated executions
2. Check for any errors or unexpected behavior
3. Verify notifications are being sent correctly

### 3. Data Management

- Monitor the `data/reservations.json` file to ensure data is being stored correctly
- Set up regular backups of this file if managing critical reservations

## Troubleshooting

### Common Issues and Solutions

| Issue | Solution |
|-------|----------|
| Authentication fails | Check username/password in environment variables |
| No tickets found | Verify event ID and ticketing service URL |
| Reservation creation fails | Check API permissions and request format |
| Extension not working | Verify reservation ID format and permissions |
| Notifications not sending | Check Slack token and channel configuration |

### Logs and Debugging

- n8n logs are available in the UI under **Executions**
- For more detailed logs, run n8n with the `--verbose` flag:
  ```bash
  n8n start --verbose
  ```

### Getting Help

- Check the n8n documentation: https://docs.n8n.io/
- Visit the n8n community forum: https://community.n8n.io/
- Review the workflow node documentation for specific node issues

---

## Next Steps

After successful setup, consider:

1. Customizing notification formats
2. Adding more events to monitor
3. Implementing additional error handling
4. Setting up monitoring for the n8n instance itself

By following this guide, you should have a fully operational Ticket Reservation Bot that automatically finds, reserves, and extends ticket reservations according to your preferences.