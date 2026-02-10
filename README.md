
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# BitShares Blockchain Explorer

A real-time, single-file BitShares blockchain explorer built with vanilla HTML, CSS, and JavaScript. Connect to BitShares WebSocket nodes to stream live blocks, search for accounts/assets/objects, and view account transaction history.


## Features

- **Live Block Streaming**: Real-time blocks from BitShares WebSocket nodes
- **Multi-Node Support**: 11+ livenet nodes and 3 testnet nodes with dropdown selector
- **Advanced Search**: 
  - Blocks (numeric): `12345`
  - Accounts (lowercase): `bitsharesdex`
  - Assets (uppercase): `BTS`, `USD`
  - Objects (IDs): `1.2.123`, `1.3.0`, `1.6.188`
- **Account Transaction History**: Last 100 transactions with resolved names/symbols
- **Hash-Based Deep Linking**: Shareable URLs like `https://...#1.11.123456`
- **Click-to-Copy**: All IDs, names, and blocks copy shareable URLs
- **Responsive Design**: Mobile, tablet, and desktop support
- **Dark Theme**: Professional dark blue color scheme

## Quick Start

### Prerequisites

- Modern web browser (Chrome, Firefox, Edge, Safari)
- Internet connection (for WebSocket connection to BitShares nodes)

### Running the Explorer

1. Open `index.html` directly in your browser:
   ```bash
   # Windows
   start index.html
   
   # macOS
   open index.html
   
   # Linux
   xdg-open index.html
   ```

2. Or serve it with a local web server:
   ```bash
   # Python 3
   python -m http.server 8000
   
   # Node.js (npx)
   npx serve
   
   # PHP
   php -S localhost:8000
   ```

3. Open `http://localhost:8000` in your browser

## Code Structure

```
BitShares Explorer/
├── index.html          # Main application (HTML + CSS + JavaScript)
└── README.md           # This file
```

## Configuration Guide

### Adding New Nodes

Edit the `nodes` array in `index.html`:

```javascript
nodes: [
    {
        name: 'Livenet',
        nodes: [
            // Add new livenet node here
            {
                url: "wss://your-node-url.com/ws",
                region: "Your Region",
                country: "Your Country",
                location: "City",
                operator: "Witness: your-witness-name",
                contact: "telegram: yourhandle"
            }
        ]
    },
    {
        name: 'Testnet',
        nodes: [
            // Add new testnet node here
        ]
    }
]
```

**Node Properties:**

| Property | Required | Description |
|----------|----------|-------------|
| `url` | Yes | WebSocket endpoint |
| `region` | No | Geographic region |
| `country` | No | Country name |
| `location` | No | City/data center |
| `operator` | No | Witness or operator name |
| `contact` | No | Contact information |

### Modifying Default Node

Change the `currentNode` property:

```javascript
currentNode: 'wss://your-preferred-node.com/ws',
```

### Adding New Operation Types

Edit the `operationNames` object:

```javascript
operationNames: {
    0: 'transfer',
    1: 'limit_order_create',
    // Add new operation here
    999: 'your_new_operation',  // Use next available ID
    // ...
}
```

**Operation ID Reference:**
- IDs 0-99: Core operations (transfers, orders, accounts, assets)
- IDs 100+: Advanced operations (HTLC, liquidity pools, etc.)

### Debug Mode

Enable debug logging by changing `debug`:

```javascript
debug: true,  // Set to false to disable console logging
```

When enabled, logs are prefixed with `===` markers for easy filtering.

## Browser Compatibility

| Browser | Version | Status |
|---------|---------|--------|
| Chrome | 90+ | ✓ Supported |
| Firefox | 88+ | ✓ Supported |
| Edge | 90+ | ✓ Supported |
| Safari | 14+ | ✓ Supported |
| Chrome Mobile | 90+ | ✓ Supported |
| Firefox Mobile | 88+ | ✓ Supported |

## License

This project is open source and available for contributions from the BitShares community.

---

**Built with ❤️ for the BitShares Community**
