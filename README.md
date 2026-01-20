
# BitShares Blockchain Explorer

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A real-time BitShares blockchain explorer built with vanilla JavaScript and the btsdex library.

![BitShares Explorer](https://bitshares.github.io/docs/images/logo.png)

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Quick Start](#quick-start)
- [Technical Architecture](#technical-architecture)
  - [Dependencies](#dependencies)
  - [Core Components](#core-components)
  - [Code Structure](#code-structure)
- [Node Configuration](#node-configuration)
- [Contributing](#contributing)
  - [Adding New Nodes](#adding-new-nodes)
  - [Adding Operations](#adding-operations)
  - [Modifying UI](#modifying-ui)
- [Debugging](#debugging)
- [Browser Compatibility](#browser-compatibility)
- [License](#license)

---

## Overview

`index.html` is a single-page blockchain explorer application for the BitShares network. It connects to BitShares WebSocket nodes to display real-time block data, transaction operations, and provides search functionality for blocks, accounts, and objects.

## Features

- **Real-time Block Streaming** - Subscribe to new blocks via WebSocket
- **Multiple Node Support** - Switch between 13+ BitShares nodes (Livenet and Testnet)
- **Block Information Display** - Shows block number, timestamp, witness ID, and transaction operations
- **Unified Search** - Search blocks by number, accounts by name, and objects by ID
- **Connection Management** - Automatic reconnection, timeout handling, and polling fallback
- **Dark Theme UI** - Clean, responsive interface with stats dashboard

## Quick Start

1. Clone the repository.

2. Open `index.html` in a web browser.

3. The explorer will automatically connect to the default WebSocket node and start displaying blocks.

## Technical Architecture

### Dependencies

| Dependency | Version | Purpose |
|------------|---------|---------|
| [btsdex.min.js](https://bitshares.network/btsdex.min.js) | Latest | BitShares JavaScript API library |

### Core Components

#### Connection Management

```javascript
// Connect to a WebSocket node
await BitShares.connect(url)

// Disconnect from current node
await BitShares.disconnect()

// Reconnect to a different node
await BitShares.reconnect(url)
```

#### Block Subscription

```javascript
// Subscribe to new blocks
BitShares.subscribe('block', (blockData) => {
    // Process block data
    console.log(blockData.head_block_number)
    console.log(blockData.current_witness)
})
```

#### Database Queries

```javascript
// Get dynamic global properties (head block number, etc.)
const props = await BitShares.db.get_objects(['2.1.0'])

// Get full block with transactions
const block = await BitShares.db.get_block(blockNumber)

// Search accounts by name
const account = await BitShares.db.getAccountByName('username')
```

### Code Structure

```text
index.html
├── HTML Structure
│   ├── Header (Logo, Status Indicator, Node Dropdown)
│   ├── Stats Bar (Current Block, Counts, Connection Time)
│   ├── Search Section
│   └── Main Content (Live Blocks & Search Results Panels)
│
├── CSS Styles
│   ├── Dark theme colors (#001530, #001C3F, #3EAFFF)
│   ├── Responsive grid layout
│   └── Animated block cards
│
└── JavaScript (App Object)
    ├── State Management
    │   ├── currentNode
    │   ├── blocks[]
    │   ├── blockCount, txCount
    │   └── isPaused
    │
    ├── Core Methods
    │   ├── init()
    │   ├── connect()
    │   ├── switchNode(url)
    │   ├── handleNewBlock()
    │   └── renderBlock()
    │
    └── Utility Methods
        ├── handleSearch()
        ├── getOperationName()
        ├── updateHeadBlock()
        └── togglePause()
```

### Key Functions Reference

| Function | Purpose | Returns |
|----------|---------|---------|
| `init()` | Initialize app, load preferred node | void |
| `connect()` | Connect to WebSocket node | Promise |
| `switchNode(url)` | Switch WebSocket node (reloads page) | void |
| `handleNewBlock(data)` | Process received block data | void |
| `renderBlock(block)` | Render block card to UI | void |
| `handleSearch()` | Unified search handler | void |
| `startHeadBlockPolling()` | Poll for head block every second | void |

---

## Node Configuration

The application supports 13+ WebSocket nodes organized by network:

### Livenet Nodes

| Node URL | Region | Country | Location |
|----------|--------|---------|----------|
| `ws://127.0.0.1:8090` | Local | - | Locally hosted |
| `wss://dex.iobanker.com/ws` | Western Europe | Germany | Frankfurt |
| `wss://api.bitshares.dev/ws` | Northern America | USA | Virginia |
| `wss://btsws.roelandp.nl/ws` | Northern Europe | Finland | Helsinki |
| `wss://api.dex.trading/` | Western Europe | France | Paris |
| `wss://public.xbts.io/ws` | Western Europe | Germany | Nuremberg |
| `wss://cloud.xbts.io/ws` | Northern America | USA | Ashburn |
| `wss://node.xbts.io/ws` | Western Europe | Germany | Falkenstein |
| `wss://api.btslebin.com/ws` | Eastern Asia | China | Hong Kong |
| `wss://bitsharesapi.loclx.io` | North America | USA | Chicago |

### Testnet Nodes

| Node URL | Region | Country | Location |
|----------|--------|---------|----------|
| `wss://testnet.dex.trading/` | Western Europe | France | Paris |
| `wss://testnet.xbts.io/ws` | Europe | Germany | Nuremberg |
| `wss://bitsharestestnet.loclx.io` | Northern America | USA | Chicago |

---

## Contributing

### Adding New Nodes

Edit the `App.nodes` array in `index.html`:

```javascript
{
    name: 'Network Name',
    nodes: [
        {
            url: "wss://new-node.example.com/ws",
            region: "Region",
            country: "Country",
            location: "City",
            operator: "Operator name",
            contact: "Contact info"
        }
    ]
}
```

### Adding Operations

Edit the `getOperationName()` function to add new operation types:

```javascript
async getOperationName(opType) {
    const operationNames = {
        0: 'transfer',
        1: 'limit_order_create',
        2: 'limit_order_cancel',
        // Add new operations here
        50: 'new_operation',
        51: 'another_operation',
        // ...
    }
    return operationNames[opType] || `operation_${opType}`
}
```

### Modifying UI

#### Colors

Modify CSS variables in the `<style>` section:

```css
:root {
    --bg-primary: #001530;      /* Main background */
    --bg-secondary: #001C3F;    /* Panels and cards */
    --accent-color: #3EAFFF;    /* Highlights and logos */
    --success-color: #00ff88;   /* Connected indicator */
    --error-color: #ff4444;     /* Error indicator */
}
```

#### Layout

Adjust container width and panel layouts:

```css
.container {
    max-width: 80%;  /* Main container width */
    margin: 0 auto;
    padding: 20px;
}

.main-content {
    display: grid;
    grid-template-columns: 1fr 1fr;  /* Two-column layout */
    gap: 20px;
}
```

#### Panel Heights

```css
.panel-content {
    max-height: 600px;  /* Scrollable panel height */
    overflow-y: auto;
}
```

---

## Debugging

Enable console logging by opening browser DevTools (F12). Key log messages include:

| Log Message | Description |
|-------------|-------------|
| `Block data received:` | Raw block data from subscription |
| `Connecting to node:` | Current node URL being connected |
| `Updated head block:` | Head block number fetched |
| `Operations extracted:` | Parsed operation names from transactions |

### Common Issues

1. **Blocks not appearing**
   - Check WebSocket connection status
   - Try switching to a different node
   - Check browser console for errors

2. **Node switch not working**
   - Node switching requires page reload (by design)
   - Preferred node is stored in `sessionStorage`

3. **Search not returning results**
   - Ensure node is connected
   - Check search format (numbers for blocks, names for accounts)

---

## Browser Compatibility

| Browser | Version | Status |
|---------|---------|--------|
| Chrome | 80+ | ✅ Fully Supported |
| Firefox | 75+ | ✅ Fully Supported |
| Edge | 80+ | ✅ Fully Supported |
| Safari | 13+ | ✅ Fully Supported |
| Chrome Mobile | 80+ | ✅ Fully Supported |
| Firefox Mobile | 68+ | ✅ Fully Supported |

**Requirements:**
- Modern browser with WebSocket support
- JavaScript enabled
- Stable internet connection

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Built with ❤️ for the BitShares Community**
