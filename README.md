
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# BitShares Blockchain Explorer

A single-file, standalone BitShares Blockchain Explorer that runs directly in the browser without requiring any external dependencies or a backend server.

## Features

- **Live Block Monitoring** - Real-time polling of the BitShares blockchain for new blocks
- **Block History** - View recent blocks with transactions and operation details
- **Advanced Search** - Search for blocks, accounts, assets, and objects by ID, name, or symbol
- **Account History** - View transaction history for any account
- **Multiple Nodes** - Connect to various BitShares public nodes
- **Responsive Design** - Works on desktop, tablet, and mobile devices
- **Dark Theme** - Modern dark UI optimized for readability

## Running the Explorer

### Option 1: Direct File Opening
Simply open `index.html` in a modern web browser. The explorer will connect to the default public node and start displaying blockchain data.

### Option 2: Local Server (Recommended)
For the best experience, run a local HTTP server:

```bash
# Python 3
python -m http.server 8080

# Node.js (if npx is available)
npx serve .

# PHP
php -S localhost:8080
```

Then open `http://localhost:8080` in your browser.

## How It Works

The explorer uses direct WebSocket connections to BitShares blockchain nodes. It communicates using the Graphene API via JSON-RPC over WebSocket:

```javascript
ws.send(JSON.stringify({
    jsonrpc: '2.0',
    method: 'call',
    params: ['database', 'get_block', [blockNum]],
    id: 1
}));
```

### API Methods Used

- `get_objects` - Fetch blockchain objects (blocks, accounts, assets)
- `get_block` - Fetch specific block data
- `get_account_by_name` - Lookup account by username
- `get_account_history` - Fetch account transaction history
- `lookup_asset_symbols` - Find assets by symbol

## Search Examples

- **Block**: Enter a block number (e.g., `12345`)
- **Account**: Enter an account name (e.g., `bitsharesdex`)
- **Asset**: Enter asset symbol in CAPS (e.g., `BTS`, `USD`)
- **Object**: Enter object ID (e.g., `1.2.123`, `1.3.0`)

## Architecture

- **Single File**: All HTML, CSS, and JavaScript in one file (~100KB)
- **No Dependencies**: Pure vanilla JavaScript, no external libraries
- **Direct WebSocket**: All blockchain communication via native WebSocket API
- **Polling Mode**: Efficient 3-second polling interval for block updates

## Public Nodes

The explorer connects to these public BitShares nodes by default:

| Node | Region |
|------|--------|
| wss://dex.iobanker.com/ws | Germany (Frankfurt) |
| wss://api.bitshares.dev/ws | USA (Virginia) |
| wss://btsws.roelandp.nl/ws | Finland (Helsinki) |
| wss://api.dex.trading/ws | France (Paris) |
| wss://public.xbts.io/ws | Germany (Nuremberg) |

You can switch nodes by clicking the status indicator in the header.

## Files

- `index.html` - The complete application (HTML + CSS + JavaScript)
- `README.md` - This documentation file

## Browser Compatibility

Tested and working in:
- Firefox (recommended)
- Chrome
- Edge
- Safari

## Known Limitations

1. **Polling vs WebSocket**: The explorer uses polling instead of WebSocket subscriptions for better reliability across different network conditions
2. **Rate Limiting**: Excessive API calls may trigger rate limits on public nodes
3. **Historical Data**: Some older blocks may not be available on all nodes

## Troubleshooting

### Blocks not loading
- Check your internet connection
- Try switching to a different node
- Wait a few moments and refresh

### Account search not working
- Ensure the account name is spelled correctly
- Account names are case-sensitive (lowercase)
- Some accounts may not exist on all nodes

### Witness names not showing
- Witness name resolution can be slow due to additional API calls
- Names are cached after first lookup

## License

This project is open source and available for community use and improvement.

---

Built for the BitShares Community
