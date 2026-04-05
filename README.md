***

# <div align="center">⚡︎ ☠︎ BLITZ-BAILEY ⚡︎ ☠︎</div>

<div align="center">
  <img src="https://files.catbox.moe/c6m3xc.jpg" width="100%" alt="Blitz-bailey Banner">
</div>

<div align="center">

![GitHub Stars](https://img.shields.io/github/stars/heroblitz/Blitz-bailey?style=for-the-badge&color=yellow&logo=github)
![Node Version](https://img.shields.io/badge/Node.js-%3E%3D20.0.0-green?style=for-the-badge&logo=node.js)
![Version](https://img.shields.io/badge/Version-1.0.4--Blitz-blue?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-red?style=for-the-badge)

<p align="center">
  <a href="https://t.me/blitzxdev">
    <img src="https://img.shields.io/badge/Telegram-Contact_Blitz-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram Blitz">
  </a>
  <a href="https://t.me/blitz_tech">
    <img src="https://img.shields.io/badge/Telegram-Channel-yellow?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram Channel">
  </a>
</p>

### **The Most Advanced, Lightning-Fast WhatsApp Web API Engine.**
*Modified and Optimized by **Blitz**.*

---

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%">

</div>

## 📖 Table of Contents
1. [Introduction](#-introduction)
2. [Key Features](#-key-features)
3. [Installation](#-installation)
4. [Connecting to WhatsApp](#-connecting-to-whatsapp)
    - [QR Code Login](#qr-code-login)
    - [Custom Pairing Code (Blitz Special)](#custom-pairing-code-blitz-special)
5. [Core Messaging](#-core-messaging)
    - [Text & Quoted](#text--quoted)
    - [Media (Image, Video, Audio)](#media-image-video-audio)
    - [Reactions](#reactions)
    - [Poll Messages](#poll-messages)
6. [Advanced UI Elements](#-advanced-ui-elements)
    - [Horizontal Slides / Carousels](#horizontal-slides--carousels)
    - [Interactive Buttons](#interactive-buttons)
    - [List Menus](#list-menus)
7. [Group Management](#-group-management)
8. [Newsletters & Channels](#-newsletters--channels)
    - [ID Fetching](#id-fetching)
9. [Status / Stories](#-status--stories)
    - [Status Mentions](#status-mentions)
10. [Business Features](#-business-features)
11. [Event Handling](#-event-handling)
12. [Configuration (SocketConfig)](#-configuration-socketconfig)
13. [Utility Functions](#-utility-functions)
14. [Troubleshooting & FAQ](#-troubleshooting--faq)
15. [Contact & Credits](#-contact--credits)

---

## ⚡︎ Introduction
**Blitz-bailey** is a high-performance, modular WhatsApp Web API library built in TypeScript. Unlike other libraries that use heavy browsers like Puppeteer, Blitz-bailey connects directly to WhatsApp's servers via WebSockets, making it the fastest and most efficient way to build WhatsApp bots and automation systems.

---

## ☠︎ Key Features
- **Fastest Performance:** Direct WebSocket connection.
- **Multi-Device Support:** Stays online even when the phone is dead.
- **Carousel Support:** Send swipeable card messages.
- **Newsletter Logic:** Fully integrated Channel/Newsletter functions.
- **Custom Pairing:** Generate specific pairing codes (e.g., `BLITZACE`).
- **Status God Mode:** Post status updates with mass mentions.
- **Interactive UI:** Lists, Native Flow Buttons, and Templates.
- **Native Polls:** Create and track real-time poll updates.

---

## 🚀 Installation

Install via NPM:
```bash
npm install github:heroblitz/Blitz-bailey
```

Or clone and build:
```bash
git clone https://github.com/heroblitz/Blitz-bailey.git
cd Blitz-bailey
npm install
npm run build
```

---

## 🔌 Connecting to WhatsApp

### QR Code Login
The classic way to connect your bot.
```javascript
const makeWASocket = require('blitz-bailey').default;
const { useMultiFileAuthState } = require('blitz-bailey');

async function connect() {
    const { state, saveCreds } = await useMultiFileAuthState('session');
    const sock = makeWASocket({
        auth: state,
        printQRInTerminal: true
    });
    sock.ev.on('creds.update', saveCreds);
}
connect();
```

### Custom Pairing Code (Blitz Special)
Don't want to scan a QR? Use the **Custom Pairing Code** feature. This allows you to generate a specific code.

```javascript
const phoneNumber = "23480xxxxxxxx"; // Your number
const customCode = "BLITZACE"; // ⚡︎ Custom code logic

const sock = makeWASocket({
    auth: state,
    printQRInTerminal: false
});

if (!sock.authState.creds.registered) {
    // Request code from WhatsApp servers
    const code = await sock.requestPairingCode(phoneNumber, customCode);
    console.log(`Your Blitz Pairing Code: ${code}`);
}
```

---

## 💬 Core Messaging

### Text & Quoted
```javascript
// Normal Text
await sock.sendMessage(jid, { text: 'Hello from Blitz-bailey! ⚡︎' });

// Quoted Message
await sock.sendMessage(jid, { text: 'This is a reply' }, { quoted: m });
```

### Media (Image, Video, Audio)
```javascript
// Send Image
await sock.sendMessage(jid, { 
    image: { url: './path/to/img.jpg' }, 
    caption: 'Powered by Blitz' 
});

// Send Voice Note (PTT)
await sock.sendMessage(jid, { 
    audio: { url: './path/to/voice.mp3' }, 
    ptt: true 
}, { mimetype: 'audio/mpeg' });
```

### Reactions
```javascript
const reactionMessage = {
    react: {
        text: "⚡︎", 
        key: m.key // Key of the message you want to react to
    }
}
await sock.sendMessage(jid, reactionMessage);
```

### Poll Messages
```javascript
await sock.sendMessage(jid, {
    poll: {
        name: "Which is the best lib?",
        values: ["Blitz-bailey", "Others"],
        selectableCount: 1
    }
});
```

---

## 🎴 Advanced UI Elements

### Horizontal Slides / Carousels
Blitz-bailey supports sending swipeable cards, perfect for menus.

```javascript
await sock.sendMessage(jid, {
    cards: [
        {
            title: "Feature 1",
            body: "Lightning fast speed",
            footer: "Blitz Edition",
            image: { url: "https://example.com/1.jpg" },
            buttons: [{ name: "quick_reply", buttonParamsJson: '{"display_text":"Order Now","id":"id1"}' }]
        },
        {
            title: "Feature 2",
            body: "Custom UI Elements",
            footer: "Blitz Edition",
            image: { url: "https://example.com/2.jpg" },
            buttons: [{ name: "quick_reply", buttonParamsJson: '{"display_text":"View More","id":"id2"}' }]
        }
    ]
});
```

### Interactive Buttons
```javascript
const buttons = [
  { name: 'button_id_1', buttonParamsJson: '{"display_text":"Option 1","id":"1"}' },
  { name: 'button_id_2', buttonParamsJson: '{"display_text":"Option 2","id":"2"}' }
];

await sock.sendMessage(jid, {
    text: "Please choose an option:",
    footer: "Blitz-bailey Tech",
    buttons: buttons,
    headerType: 1
});
```

---

## 🛡️ Group Management
Blitz-bailey provides full control over your communities.

```javascript
// Create a Group
const group = await sock.groupCreate("Blitz Squad", ["234xxx@s.whatsapp.net"]);

// Add Member
await sock.groupParticipantsUpdate(jid, ["234xxx@s.whatsapp.net"], "add");

// Promote to Admin
await sock.groupParticipantsUpdate(jid, ["234xxx@s.whatsapp.net"], "promote");

// Fetch Join Requests
const requests = await sock.groupRequestParticipantsList(jid);
```

---

## 📢 Newsletters & Channels
This is a premium feature implemented in Blitz-bailey.

### ID Fetching
Convert a public invite link to a Newsletter JID.
```javascript
const inviteCode = "0029VaXXXXX";
const metadata = await sock.newsletterMetadata("invite", inviteCode);
console.log("Newsletter JID:", metadata.id); // Returns 120363xxxx@newsletter
```

### Reacting to Channel Posts
```javascript
await sock.newsletterReactMessage(newsletterJid, serverId, "🔥");
```

---

## 🤳 Status / Stories
Blitz-bailey can manage your WhatsApp Statuses with high-level functions.

### Status Mentions
Mention everyone in a group or specific people in your status so they get notified!
```javascript
await sock.sendStatusMentions(
    { text: "Check out Blitz-bailey! ⚡︎" }, 
    "status@broadcast", 
    ["234xxx@s.whatsapp.net"] // People who will be notified
);
```

---

## 💼 Business Features
Build a full marketplace inside WhatsApp.

```javascript
// Fetch Business Profile
const profile = await sock.getBusinessProfile("jid@s.whatsapp.net");

// Fetch Catalog
const catalog = await sock.getCatalog({ jid: "jid@s.whatsapp.net" });
```

---

## 🛠️ Event Handling
Stay updated with everything happening in the socket.

```javascript
sock.ev.process(async (events) => {
    // Connection Update (QR, Logged In, Disconnected)
    if (events['connection.update']) {
        const update = events['connection.update'];
        console.log('Connection Status:', update.connection);
    }

    // New Message Received
    if (events['messages.upsert']) {
        const upsert = events['messages.upsert'];
        console.log('New Message:', JSON.stringify(upsert, null, 2));
    }

    // Participants Update (Groups)
    if (events['group-participants.update']) {
        console.log('Group Update:', events['group-participants.update']);
    }
});
```

---

## ⚙️ Configuration (SocketConfig)
You can fine-tune your connection:

| Property | Type | Description |
| :--- | :--- | :--- |
| `logger` | `ILogger` | Custom PINO logger for debugging. |
| `browser` | `[string, string, string]` | The User-Agent (e.g., ["Blitz", "Safari", "1.0"]). |
| `printQRInTerminal` | `boolean` | Whether to show the QR in console. |
| `auth` | `AuthenticationState` | Your session state. |
| `markOnlineOnConnect` | `boolean` | Automatically show as "Online". |
| `retryRequestDelayMs` | `number` | Time to wait before retrying a failed message. |

---

## 🧰 Utility Functions

### JID Decoding
Clean those messy JIDs automatically.
```javascript
const cleanJid = sock.decodeJid("2348000000:1@s.whatsapp.net");
// Returns: 2348000000@s.whatsapp.net
```

---

## ☠︎ Troubleshooting & FAQ

**Q: Why am I getting "Timed Out"?**
*A: Check your internet connection or your proxy settings. WhatsApp servers are sensitive to latency.*

**Q: Can I use this for mass spamming?**
*A: Blitz-bailey is built for automation. Abuse will get your number banned. Use responsibly.*

**Q: Is it safe?**
*A: Yes, it uses official E2E encryption protocols. Your messages are as safe as using the official app.*

---

## ⚡︎ Contact & Credits
**Blitz-bailey** is maintained by **Blitz**.

- **Author:** Blitz
- **Telegram:** [@blitzxdev](https://t.me/blitzxdev)
- **Official Channel:** [@blitz_tech](https://t.me/blitz_tech)
- **Base:** Baileys (MD)

---

<div align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%">
  <p><strong>Developed with ⚡︎ by Blitz.</strong></p>
  <img src="https://img.shields.io/badge/Status-Stable-brightgreen?style=for-the-badge" />
</div>

***

*Note: This library is for educational purposes. We are not responsible for any account bans or data loss.*
