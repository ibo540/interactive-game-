# Dark Alchemy - Real-time Multiplayer Server

This project now supports **real-time cross-device multiplayer** through a WebSocket server!

## 🚀 Quick Start

### 1. Install Dependencies

```bash
npm install
```

### 2. Start the Server

```bash
npm start
```

The server will run on `http://localhost:3000` by default.

### 3. Access the Game

- **Host/Control Panel**: Open `http://localhost:3000/session_control.html` in your browser
- **Players**: Open `http://localhost:3000/player.html` on any device (phone, tablet, laptop)

Players can now join from different devices using the session code!

## 📡 How It Works

### Architecture

- **Server** (`server.js`): Node.js + Express + Socket.io
  - Manages game sessions
  - Handles WebSocket connections
  - Broadcasts game state to all players

- **Client** (`websocket-channel.js`): Enhanced communication layer
  - Automatically detects server availability
  - Falls back to local mode (BroadcastChannel/LocalStorage) if server unavailable
  - Seamlessly switches between WebSocket and local communication

### Communication Modes

1. **WebSocket Mode** (Cross-device):
   - Players on different devices can join the same session
   - Real-time updates across all devices
   - Server manages session state

2. **Local Mode** (Fallback):
   - Works without server (for testing)
   - Uses BroadcastChannel for same-device communication
   - Uses LocalStorage as backup

## 🔧 Configuration

### Server URL

The client automatically detects the server URL:

- **Localhost**: Automatically uses `http://localhost:3000`
- **Production**: Set `window.GAME_SERVER_URL` in your HTML files

Example:
```javascript
// In session_control.html or player.html
window.GAME_SERVER_URL = 'https://your-domain.com';
```

### Environment Variables

- `PORT`: Server port (default: 3000)

```bash
PORT=8080 npm start
```

## 📦 Deployment

### Option 1: Vercel (Recommended)

1. Add `vercel.json` configuration:
```json
{
  "version": 2,
  "builds": [
    {
      "src": "server.js",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "/server.js"
    }
  ]
}
```

2. Deploy:
```bash
vercel
```

### Option 2: Heroku

1. Create `Procfile`:
```
web: node server.js
```

2. Deploy:
```bash
heroku create
git push heroku main
```

### Option 3: Railway / Render

Both platforms support Node.js apps. Just point them to `server.js` as the entry point.

## 🎮 Game Flow

1. **Host creates session** → Server creates session with code
2. **Players join** → Players connect via WebSocket using session code
3. **Game state syncs** → All players receive real-time updates
4. **Game actions** → Votes, allocations, etc. broadcast to all players

## 🐛 Troubleshooting

### Server not connecting?

1. Check server is running: `npm start`
2. Check browser console for errors
3. Verify server URL is correct
4. Check firewall/network settings

### Players can't join?

1. Ensure server is accessible from player devices
2. Check session code matches
3. Verify WebSocket connections in browser DevTools → Network

### Fallback to local mode?

- Server might be unavailable
- Check server logs for errors
- Verify Socket.io library loaded correctly

## 📝 Development

### Testing Locally

1. Start server: `npm start`
2. Open host page: `http://localhost:3000/session_control.html`
3. Open player page on phone: `http://[your-ip]:3000/player.html`
4. Join with session code!

### Testing Without Server

The game still works in local mode (same device only) if the server isn't running.

## 🔐 Security Notes

- Currently allows all origins (`origin: "*"`)
- For production, restrict CORS to your domain
- Consider adding authentication for session creation
- Rate limiting recommended for production

## 📚 Dependencies

- `express`: Web server
- `socket.io`: WebSocket communication
- `cors`: Cross-origin resource sharing

## 🎯 Next Steps

- [ ] Add authentication
- [ ] Add rate limiting
- [ ] Add session persistence
- [ ] Add reconnection handling
- [ ] Add admin dashboard
