
# BitShares Blockchain Explorer

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A real-time BitShares blockchain explorer built as a single-file web application. Connects directly to BitShares WebSocket nodes to display live blocks, search blocks/accounts/objects, and view account transaction history.

## Features

### Live Block Streaming
- Real-time block updates from BitShares WebSocket nodes
- Displays block number, timestamp, witness, and operations
- Pause/Resume controls for live updates
- Automatic reconnection with polling fallback

### Multi-Node Support
- 13+ WebSocket nodes (Livenet + Testnet)
- Node selector dropdown in status bar
- Seamless switching with automatic reconnection

### Advanced Search
- **Block search**: Enter block number (e.g., `12345`)
- **Account search**: Enter username (e.g., `bitsharesdex`)
- **Object search**: Enter object ID (e.g., `1.2.123`, `1.11.1162425505`)
- Hash-based deep linking: `https://bitshares.network/#1.11.1162425505`

### Account Transaction History
- View last 100 transactions for any account
- Displays operation type, details, block number, timestamp, and transaction ID
- Automatic resolution of account names and asset symbols
- Clickable links to copy shareable URLs

### Smart Data Resolution
- **Account names**: Resolves `1.2.123` → `bitsharesdex`
- **Asset symbols**: Resolves `1.3.0` → `BTS`, `1.3.113` → `USD`
- **Amount precision**: Correctly displays amounts with proper precision (e.g., `10.00000 BTS`)

### Shareable Links
- Click any account name, object ID, or block number to copy a shareable URL
- URL format: `https://bitshares.network/#query`
- "Copy JSON" button for search results

### Responsive Design
- Desktop, tablet, and mobile optimized
- Touch-friendly scrolling on small screens
- Fluid typography using `em` units

### Features

- **Real-time Block Streaming** - Subscribe to new blocks via WebSocket
- **Multiple Node Support** - Switch between 13+ BitShares nodes (Livenet and Testnet)
- **Block Information Display** - Shows block number, timestamp, witness ID, and transaction operations
- **Unified Search** - Search blocks by number, accounts by name, and objects by ID
- **Connection Management** - Automatic reconnection, timeout handling, and polling fallback
- **Dark Theme UI** - Clean, responsive interface with stats dashboard

### Code Structure

```text
index.html
├── <head>
│   ├── Meta tags (viewport, charset)
│   ├── Favicon
│   └── CSS styles (inline, ~1500 lines)
├── <body>
│   ├── Header (logo, node selector)
│   ├── Stats bar (current block, tx count, connection time)
│   ├── Advance Search section
│   ├── Search Results panel
│   ├── Account Statement Search
│   ├── Account History panel
│   ├── Live Blocks panel
│   └── Footer
└── <script>
    ├── App configuration
    │   ├── nodes[] - WebSocket node list
    │   ├── accountNameCache - Account ID to name mapping
    │   └── assetSymbolCache - Asset ID to symbol/precision mapping
    │
    ├── DOM elements - All UI element references
    │
    ├── Initialization
    │   ├── init() - App initialization
    │   ├── bindEvents() - Event listeners
    │   ├── connect() - WebSocket connection
    │   └── handleHashSearch() - URL hash routing
    │
    ├── Node management
    │   ├── renderNodeDropdown() - Node selector UI
    │   ├── switchNode() - Node switching
    │   └── toggleNodeDropdown() - Dropdown toggle
    │
    ├── Connection handling
    │   ├── setStatus() - Connection status indicator
    │   ├── updateHeadBlock() - Head block polling
    │   ├── subscribeToBlocks() - Block subscription
    │   └── startPolling() - Polling fallback
    │
    ├── Block streaming
    │   ├── handleNewBlock() - Process new blocks
    │   ├── renderBlock() - Render block UI
    │   ├── getTransactionDetails() - Extract tx info
    │   ├── getOperationName() - Operation type lookup
    │   ├── getOperationPreview() - Operation preview
    │   ├── formatAmount() - Amount formatting
    │   └── cacheBlockAssets() - Asset metadata caching
    │
    ├── Search functionality
    │   ├── handleSearch() - Main search handler
    │   ├── renderSearchResult() - Display search results
    │   └── updateSearchHash() - Update URL hash
    │
    ├── Account history
    │   ├── handleAccountHistorySearch() - Account history fetch
    │   ├── renderAccountInfo() - Account info card
    │   ├── renderAccountHistory() - History table
    │   ├── clearAccountHistory() - Clear history
    │   ├── fetchAssetSymbol() - Asset metadata fetch
    │   └── account/asset resolution helpers
    │
    ├── Click-to-copy functionality
    │   ├── copyToClipboard() - Copy URL to clipboard
    │   ├── copyResultsJson() - Copy JSON results
    │   ├── copyAssetSymbol() - Copy asset link
    │   └── showTooltip() - Show "Copied!" notification
    │
    └── UI helpers
        ├── clearBlocks() - Clear blocks panel
        ├── clearResults() - Clear search results
        ├── togglePause() - Pause/resume streaming
        ├── formatTimestamp() - Timestamp formatting
        ├── updateConnectionTime() - Connection timer
        ├── getClickableAccount() - Clickable account
        ├── getClickableObject() - Clickable object
        └── getClickableBlock() - Clickable block number
```

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

## Browser Compatibility

- Chrome	80+	Supported
- Firefox	75+	Supported
- Safari	13+	Supported
- Edge	80+	Supported
- Mobile Chrome	80+	Supported
- Mobile Safari	13+	Supported

**Requirements:**
- Modern browser with WebSocket support
- JavaScript enabled
- Stable internet connection

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Built with ❤️ for the BitShares Community**
