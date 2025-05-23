# Ticket Reservation Bot - Visual Component Guide

## Main Workflow Components

### 🔑 Authentication Card
**Component: API Login Node**
```
┌─────────────────────────────────┐
│ API LOGIN                       │
│                                 │
│ Purpose: Authenticates with     │
│ ticketing service API           │
│                                 │
│ Input: Username & Password      │
│ Output: Authentication token    │
│                                 │
│ Environment Variables:          │
│ - TICKET_USER                   │
│ - TICKET_PASS                   │
└─────────────────────────────────┘
```

### 🔍 Ticket Search Card
**Component: Find Available Tickets Node**
```
┌─────────────────────────────────┐
│ FIND AVAILABLE TICKETS          │
│                                 │
│ Purpose: Searches for available │
│ tickets for a specific event    │
│                                 │
│ Input: Event ID, Auth token     │
│ Output: List of available seats │
│                                 │
│ Configuration:                  │
│ - Event ID from settings        │
└─────────────────────────────────┘
```

### 🎯 Ticket Selection Card
**Component: Select Best Tickets Node**
```
┌─────────────────────────────────┐
│ SELECT BEST TICKETS             │
│                                 │
│ Purpose: Filters and selects    │
│ the best tickets based on       │
│ preferences                     │
│                                 │
│ Input: Available tickets list   │
│ Output: Selected ticket(s)      │
│                                 │
│ Preferences:                    │
│ - Price limit                   │
│ - Preferred sections            │
│ - Number of tickets             │
└─────────────────────────────────┘
```

### 🎟️ Reservation Card
**Component: Reserve Tickets Node**
```
┌─────────────────────────────────┐
│ RESERVE TICKETS                 │
│                                 │
│ Purpose: Creates reservation    │
│ for selected tickets            │
│                                 │
│ Input: Selected ticket IDs      │
│ Output: Reservation details     │
│                                 │
│ Settings:                       │
│ - Hold duration                 │
└─────────────────────────────────┘
```

### ⏰ Monitoring Card
**Component: Check Reservation Status Node**
```
┌─────────────────────────────────┐
│ CHECK RESERVATION STATUS        │
│                                 │
│ Purpose: Monitors reservation   │
│ status and time remaining       │
│                                 │
│ Input: Reservation ID           │
│ Output: Updated status info     │
│                                 │
│ Interval:                       │
│ - Configurable check frequency  │
└─────────────────────────────────┘
```

### 🔄 Extension Card
**Component: Extend Reservation Node**
```
┌─────────────────────────────────┐
│ EXTEND RESERVATION              │
│                                 │
│ Purpose: Extends reservation    │
│ before it expires               │
│                                 │
│ Input: Reservation ID           │
│ Output: Updated expiration time │
│                                 │
│ Trigger:                        │
│ - When minutes remaining falls  │
│   below threshold               │
└─────────────────────────────────┘
```

### 📊 Database Card
**Component: Update/Save DB Nodes**
```
┌─────────────────────────────────┐
│ DATABASE OPERATIONS             │
│                                 │
│ Purpose: Stores and retrieves   │
│ reservation information         │
│                                 │
│ Operations:                     │
│ - Save new reservations         │
│ - Update existing reservations  │
│ - Load reservation history      │
│                                 │
│ Path: data/reservations.json    │
└─────────────────────────────────┘
```

### 🔔 Notification Card
**Component: Send Notification Nodes**
```
┌─────────────────────────────────┐
│ NOTIFICATIONS                   │
│                                 │
│ Purpose: Sends alerts about     │
│ reservation status              │
│                                 │
│ Types:                          │
│ - Success notifications         │
│ - Status updates                │
│ - Extension alerts              │
│ - Error notifications           │
│                                 │
│ Channel: Slack                  │
└─────────────────────────────────┘
```

## Error Recovery Workflow

### 🚨 Recovery Card
**Component: Error Recovery Workflow**
```
┌─────────────────────────────────┐
│ ERROR RECOVERY                  │
│                                 │
│ Purpose: Detects and recovers   │
│ stale or failed reservations    │
│                                 │
│ Process:                        │
│ 1. Find stale reservations      │
│ 2. Check if still valid         │
│ 3. Attempt to extend if active  │
│ 4. Send recovery notifications  │
│                                 │
│ Schedule: Regular interval      │
└─────────────────────────────────┘
```

## Multi-Ticket Workflow

### 🎭 Multi-Event Card
**Component: Multi-Ticket Workflow**
```
┌─────────────────────────────────┐
│ MULTI-EVENT HANDLER             │
│                                 │
│ Purpose: Manages tickets for    │
│ multiple events simultaneously  │
│                                 │
│ Features:                       │
│ - Process multiple event configs│
│ - Track separate reservations   │
│ - Aggregate status reporting    │
│                                 │
│ Configuration:                  │
│ - Array of event settings       │
└─────────────────────────────────┘
```

## Configuration Components

### ⚙️ Settings Card
**Component: Configuration File**
```
┌─────────────────────────────────┐
│ CONFIGURATION                   │
│                                 │
│ File: config/settings.json      │
│                                 │
│ Settings:                       │
│ - Ticket service URL            │
│ - Event IDs                     │
│ - Price thresholds              │
│ - Section preferences           │
│ - Hold duration                 │
│ - Extension timing              │
│ - Check intervals               │
│                                 │
│ Format: JSON                    │
└─────────────────────────────────┘
```

### 🔐 Environment Variables Card
**Component: Environment Variables**
```
┌─────────────────────────────────┐
│ ENVIRONMENT VARIABLES           │
│                                 │
│ File: .env                      │
│                                 │
│ Variables:                      │
│ - TICKET_USER: API username     │
│ - TICKET_PASS: API password     │
│ - CONFIG_PATH: Settings location│
│ - DB_PATH: Database location    │
│ - SLACK_TOKEN: For notifications│
│                                 │
│ Set via: n8n UI or .env file    │
└─────────────────────────────────┘
```

## How Components Work Together

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│              │     │              │     │              │
│ AUTHENTICATE ├────►│ FIND TICKETS ├────►│ SELECT BEST  │
│              │     │              │     │              │
└──────────────┘     └──────────────┘     └──────────────┘
                                                 │
┌──────────────┐     ┌──────────────┐     ┌──────▼───────┐
│              │     │              │     │              │
│   MONITOR    │◄────┤   SAVE TO    │◄────┤   RESERVE    │
│              │     │   DATABASE   │     │              │
└──────┬───────┘     └──────────────┘     └──────────────┘
       │
       ▼
┌──────────────┐     ┌──────────────┐
│  EXTENSION   │     │              │
│   NEEDED?    ├────►│    EXTEND    │
│              │ Yes │              │
└──────┬───────┘     └──────────────┘
       │ No
       ▼
┌──────────────┐
│   CONTINUE   │
│  MONITORING  │
│              │
└──────────────┘
```

This visual component guide should help you understand the structure and purpose of each part of the ticket reservation system.