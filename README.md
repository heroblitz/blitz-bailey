***

# <div align="center">⚡︎ ☠︎ BLITZ-BAILEY ⚡︎ ☠︎</div>

<div align="center">
  <img src="https://files.catbox.moe/c6m3xc.jpg" width="100%" alt="Blitz-bailey Banner">
</div>

<div align="center">

<svg width="600" height="60">
  <rect width="100%" height="100%" fill="none" />
  <text x="50%" y="40" font-family="Verdana" font-size="35" font-weight="bold" fill="#FFD700" text-anchor="middle">
    H I G H - V O L T A G E - A P I
    <animate attributeName="opacity" values="0;1;1;0" dur="1.5s" repeatCount="indefinite" />
    <animate attributeName="fill" values="#FFD700;#FFF;#FFD700" dur="0.5s" repeatCount="indefinite" />
  </text>
</svg>

![GitHub Stars](https://img.shields.io/github/stars/heroblitz/Blitz-bailey?style=for-the-badge&color=yellow&logo=github)
![Node Version](https://img.shields.io/badge/Node.js-%3E%3D20.0.0-green?style=for-the-badge&logo=node.js)
![Version](https://img.shields.io/badge/Version-26.0.4--Blitz-blue?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-red?style=for-the-badge)

<p align="center">
  <a href="https://t.me/blitzxdev">
    <img src="https://img.shields.io/badge/Telegram-Contact_Blitz-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram Blitz">
  </a>
  <a href="https://t.me/blitz_tech">
    <img src="https://img.shields.io/badge/Telegram-Channel-yellow?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram Channel">
  </a>
</p>

### **The Most Powerful WhatsApp Multi-Device Engine Ever Built.**
*Architected for ⚡︎ Speed, ☠︎ Precision, and 🎴 Advanced UI.*

---

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%">

</div>

## 📖 Table of Contents
<details>
<summary><b>Unfold the Blitz-bailey scroll</b></summary>

1. [⚡︎ Introduction](#-introduction)
2. [🚀 Installation](#-installation)
3. [🔌 Connectivity & Auth Logic](#-connectivity--auth-logic)
    - [The Multi-File State System](#the-multi-file-state-system)
    - [Pairing Code Logic](#pairing-code-logic)
4. [💬 Basic Messaging](#-basic-messaging)
    - [Text & Quoted Replies](#text--quoted-replies)
    - [Media (HD Support)](#media-hd-support)
    - [Polls & Reactions](#polls--reactions)
5. [🎴 Advanced UI: The Carousel System](#-advanced-ui-the-carousel-system)
    - [Horizontal Slides Implementation](#horizontal-slides-implementation)
6. [🔘 Interactive UI Elements](#-interactive-ui-elements)
    - [Native Flow Buttons](#native-flow-buttons)
    - [List & Section Menus](#list--section-menus)
7. [🤳 Status (Stories) God-Tier Logic](#-status-stories-god-tier-logic)
    - [Status Group Posting (Privacy Targeting)](#status-group-posting-privacy-targeting)
    - [Status Group Mentioning (Mass Notifying)](#status-group-mentioning-mass-notifying)
8. [📢 Newsletters & Channels](#-newsletters--channels)
    - [Channel ID Fetcher (Unmask JIDs)](#channel-id-fetcher-unmask-jids)
    - [Newsletter Interactions](#newsletter-interactions)
9. [🛡️ Group Engineering](#-group-engineering)
    - [Advanced Participant Control](#advanced-participant-control)
    - [Community & Approval Modes](#community--approval-modes)
10. [💼 Business & Payments](#-business--payments)
    - [Catalog & Product Sync](#catalog--product-sync)
    - [Order Detail Decoding](#order-detail-decoding)
11. [🛠️ Technical Utilities](#-technical-utilities)
    - [JID Decoder & LID Support](#jid-decoder--lid-support)
    - [Event Buffering & Mutex](#event-buffering--mutex)
12. [⚙️ Socket Configuration Reference](#-socket-configuration-reference)
13. [☠︎ Troubleshooting & Banning Prevention](#-troubleshooting--banning-prevention)
14. [⚡︎ Credits & Legal](#-credits--legal)
</details>

---

## ⚡︎ Introduction
**Blitz-bailey** is a total overhaul of the WhatsApp Multi-Device (MD) protocol handler. It doesn't just "talk" to WhatsApp; it masters it. Designed for developers who need to implement **Horizontal Slides**, **Channel Scrapers**, and **Mass-Mention Statuses**, this library provides the rawest and most direct access to the WhatsApp WebSocket layer.

---

## 🚀 Installation

Install directly from the Blitz source:
```bash
npm install heroblitz/Blitz-bailey
```

Or add it to your `package.json` dependencies:
```json
"dependencies": {
    "blitz-bailey": "github:heroblitz/Blitz-bailey"
}
```

---

## 🔌 Connectivity & Auth Logic

Blitz-bailey uses a high-performance authentication system that prevents session corruption and handles multiple device links seamlessly.

### The Multi-File State System
```javascript
const { default: makeWASocket, useMultiFileAuthState, delay } = require('blitz-bailey');

async function initializeBlitz() {
    const { state, saveCreds } = await useMultiFileAuthState('session_data');

    const sock = makeWASocket({
        auth: state,
        printQRInTerminal: true,
        version: [2, 3000, 1023223821], // ⚡︎ Hardcoded for stability
        syncFullHistory: true
    });

    sock.ev.on('creds.update', saveCreds);
    
    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect, qr } = update;
        if (connection === 'close') {
            console.log("☠︎ Connection Lost. Reconnecting...");
            initializeBlitz();
        } else if (connection === 'open') {
            console.log("⚡︎ BLITZ-BAILEY ONLINE ⚡︎");
        }
    });
}
```

### Pairing Code Logic
Link your bot without a QR code using the pairing engine.
```javascript
if (!sock.authState.creds.registered) {
    const phoneNumber = "23480xxxxxxxx"; 
    // Request a pairing code from WhatsApp
    const code = await sock.requestPairingCode(phoneNumber);
    console.log(`⚡︎ PAIRING CODE: ${code}`);
}
```

---

## 💬 Basic Messaging

### Text & Quoted Replies
```javascript
await sock.sendMessage(jid, { 
    text: 'Hello from Blitz-bailey!',
    contextInfo: { 
        mentionedJid: [jid],
        externalAdReply: {
            title: "Blitz Edition",
            body: "The King of APIs",
            thumbnailUrl: "https://files.catbox.moe/c6m3xc.jpg",
            sourceUrl: "https://t.me/blitz_tech",
            mediaType: 1,
            renderLargerThumbnail: true
        }
    }
}, { quoted: m });
```

### Media HD Support
```javascript
await sock.sendMessage(jid, {
    image: { url: 'https://files.catbox.moe/c6m3xc.jpg' },
    caption: 'High Voltage Media',
    viewOnce: true // ☠︎ Stealth mode
});
```

---

## 🎴 Advanced UI: The Carousel System

One of the unique features of Blitz-bailey is the ability to send **Horizontal Slides (Carousels)**.

```javascript
await sock.sendMessage(jid, {
    cards: [
        {
            title: "Blitz Feature A",
            body: "Lightning Fast Speed",
            footer: "Powered by @blitzxdev",
            image: { url: "https://example.com/slide1.jpg" },
            buttons: [
                { name: "cta_url", buttonParamsJson: '{"display_text":"Get Started","url":"https://t.me/blitzxdev"}' }
            ]
        },
        {
            title: "Blitz Feature B",
            body: "Skull Precision Power",
            footer: "Join @blitz_tech",
            image: { url: "https://example.com/slide2.jpg" },
            buttons: [
                { name: "quick_reply", buttonParamsJson: '{"display_text":"Join Channel","id":"join_id"}' }
            ]
        }
    ]
});
```

---

## 🤳 Status (Stories) God-Tier Logic

Blitz-bailey provides two distinct ways to manipulate WhatsApp Status:

### 1. Status Group Posting (Privacy Targeting)
Only show your status to a specific list of Groups or People.
```javascript
await sock.sendMessage("status@broadcast", {
    text: "This status is invisible to everyone except the JID list below."
}, {
    statusJidList: [
        "120363xxxx@g.us", // Target Group
        "23480xxxxxxxx@s.whatsapp.net" // Target User
    ]
});
```

### 2. Status Group Mentioning (Mass Notifying)
Post a status and "tag" people/groups so they receive a notification alert.
```javascript
await sock.sendStatusMentions(
    { text: "⚡︎ ☠︎ ATTENTION SQUAD! ☠︎ ⚡︎" }, 
    "status@broadcast", 
    ["120363xxxx@g.us", "234xxxxxxx@s.whatsapp.net"] // People notified immediately
);
```

---

## 📢 Newsletters & Channels

### Channel ID Fetcher (Unmask JIDs)
Public invite links don't show the real JID. Blitz-bailey can unmask any channel.
```javascript
const inviteCode = "0029VaXXXXX"; // The code from the link
const channel = await sock.newsletterMetadata("invite", inviteCode);

console.log(`⚡︎ ID: ${channel.id}`); // Returns 120363xxxx@newsletter
console.log(`⚡︎ Title: ${channel.name}`);
console.log(`⚡︎ Followers: ${channel.subscribers}`);
```

---

## 🛡️ Group Engineering

### Advanced Participant Control
```javascript
// Bulk Add with rejection handling
const response = await sock.groupParticipantsUpdate(jid, ["user1@s.whatsapp.net", "user2@s.whatsapp.net"], "add");

// Manage Join Requests (Approve/Reject)
await sock.groupRequestParticipantsUpdate(jid, ["user1@s.whatsapp.net"], "approve");
```

---

## ⚙️ Socket Configuration Reference

Blitz-bailey gives you deep configuration options found in `lib/Types/Socket.d.ts`:

| Property | Description |
| :--- | :--- |
| `waWebSocketUrl` | Set a custom WS endpoint or proxy. |
| `connectTimeoutMs` | How long to wait before declaring a connection dead. |
| `keepAliveIntervalMs` | Ping-pong interval to stay online. |
| `shouldSyncHistoryMessage` | Filter which history messages are processed. |
| `linkPreviewImageThumbnailWidth` | Set custom width for link previews. |
| `generateHighQualityLinkPreview` | Force HD thumbnails for links. |

---

## 🛠️ Technical Utilities

### JID Decoder & LID Support
WhatsApp is moving to **LID (Linked Identity)**. Blitz-bailey fully supports decoding these.
```javascript
const realJid = sock.decodeJid(m.key.remoteJid);
const lidUser = await sock.getLidUser(realJid);
console.log(`LID Address: ${lidUser[0].lid}`);
```

### Event Buffering
Use `makeEventBuffer` to handle massive amounts of messages without crashing the bot.
```javascript
sock.ev.process(async (events) => {
    if (events['messages.upsert']) {
        // Automatically buffered and batch-processed
    }
});
```

---

## ☠︎ Troubleshooting & Banning Prevention
1. **Version Control:** Ensure you use the version `[2, 3000, 1023223821]` to match current MD requirements.
2. **Rate Limiting:** Blitz-bailey is fast, but sending 100 messages a second will get you banned. Use `await delay(1000)`.
3. **Session Safety:** Always use `multi-file-auth`. `single-file-auth` is deprecated and unstable.

---

## ⚡︎ Credits & Legal

<div align="center">

**Project Lead:** Blitz  
**Support:** [@blitzxdev](https://t.me/blitzxdev)  
**Updates:** [@blitz_tech](https://t.me/blitz_tech)  

**LICENSE: MIT**  
*WhiskeySocket/Baileys was the inspiration, but Blitz-bailey is the evolution.*

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%">

### ⚡︎ ☠︎ DEVELOPED BY BLITZ ☠︎ ⚡︎

</div>

***
