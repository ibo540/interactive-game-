# Server Deployment Guide

## Option 1: Railway (Recommended - Easiest)

### Step 1: Create Railway Account
1. Go to https://railway.app
2. Sign up with GitHub

### Step 2: Deploy
1. Click "New Project"
2. Select "Deploy from GitHub repo"
3. Choose your repository: `ibo540/interactive-game-`
4. Railway will auto-detect `package.json` and deploy
5. Wait for deployment to complete

### Step 3: Get Your Server URL
1. Click on your project
2. Click on the service
3. Go to "Settings" → "Networking"
4. Copy the "Public Domain" (e.g., `your-app.railway.app`)

### Step 4: Update Client to Use Server
Add this to `DarkAlchemyProject/player.html` and `DarkAlchemyProject/session_control.html`:

```javascript
// Replace YOUR_RAILWAY_URL with your actual Railway domain
window.GAME_SERVER_URL = 'https://your-app.railway.app';
```

Or set it dynamically:
```javascript
// Auto-detect Railway URL if on same domain, otherwise use Railway
if (window.location.hostname.includes('railway.app') || window.location.hostname.includes('vercel.app')) {
    window.GAME_SERVER_URL = window.location.origin;
} else {
    window.GAME_SERVER_URL = 'https://your-app.railway.app';
}
```

---

## Option 2: Render

### Step 1: Create Render Account
1. Go to https://render.com
2. Sign up with GitHub

### Step 2: Deploy
1. Click "New" → "Web Service"
2. Connect your GitHub repository
3. Settings:
   - **Name**: `dark-alchemy-server`
   - **Root Directory**: (leave empty)
   - **Environment**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `node server.js`
4. Click "Create Web Service"

### Step 3: Get Your Server URL
- Render will give you a URL like: `https://dark-alchemy-server.onrender.com`
- Update `window.GAME_SERVER_URL` in your HTML files

---

## Option 3: Fly.io

### Step 1: Install Fly CLI
```bash
powershell -Command "iwr https://fly.io/install.ps1 -useb | iex"
```

### Step 2: Login
```bash
fly auth login
```

### Step 3: Deploy
```bash
fly launch
# Follow prompts, then:
fly deploy
```

---

## Option 4: Fix Vercel (Advanced)

Vercel can work but requires special configuration. The issue is that Vercel's serverless functions have limitations with WebSocket connections.

If you want to try Vercel:
1. We need to use Vercel's serverless functions properly
2. Socket.io needs special configuration for Vercel
3. It's more complex than other options

**Recommendation: Use Railway - it's the easiest and most reliable for WebSocket servers.**

---

## After Deployment

1. **Update your HTML files** to point to the server URL
2. **Test the connection** - open browser console and check for WebSocket connection
3. **Deploy updated HTML** to Vercel (or wherever you host static files)

The server will handle all game sessions and cross-device communication!
