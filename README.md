***

# <div align="center">⚡︎ ☠︎ BLITZ-BAILEY ⚡︎ ☠︎</div>

<div align="center">
  <img src="https://files.catbox.moe/c6m3xc.jpg" width="100%" alt="Blitz-bailey Banner">
</div>

<div align="center">

<svg width="600" height="100">
  <linearGradient id="blitzGradient" x1="0%" y1="0%" x2="100%" y2="0%">
    <stop offset="0%" style="stop-color:#FFD700;stop-opacity:1" />
    <stop offset="50%" style="stop-color:#FFFFFF;stop-opacity:1" />
    <stop offset="100%" style="stop-color:#FFD700;stop-opacity:1" />
  </linearGradient>
  <rect width="100%" height="100%" fill="none" />
  <text x="50%" y="50" font-family="Verdana" font-size="45" font-weight="bold" fill="url(#blitzGradient)" text-anchor="middle">
    B L I T Z - B A I L E Y
    <animate attributeName="opacity" values="0.4;1;0.4" dur="2s" repeatCount="indefinite" />
  </text>
  <text x="50%" y="85" font-family="Arial" font-size="18" fill="#909090" text-anchor="middle">
    PREMIUM WHATSAPP PROTOCOL ENGINE ⚡︎ ☠︎
  </text>
</svg>

[![Version](https://img.shields.io/badge/Version-26.0.4--Blitz-blue?style=for-the-badge)](https://github.com/heroblitz/Blitz-bailey)
[![Node](https://img.shields.io/badge/Node.js-%3E%3D20.0.0-green?style=for-the-badge&logo=node.js)](https://nodejs.org)
[![Build](https://img.shields.io/badge/Build-Stable-brightgreen?style=for-the-badge)](https://github.com/heroblitz/Blitz-bailey)

<p align="center">
  <a href="https://t.me/blitzxdev">
    <img src="https://img.shields.io/badge/Telegram-@blitzxdev-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" />
  </a>
  <a href="https://t.me/blitz_tech">
    <img src="https://img.shields.io/badge/Channel-@blitz_tech-yellow?style=for-the-badge&logo=telegram&logoColor=white" />
  </a>
</p>

---

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%">

</div>

## 📖 Table of Contents
1.  [⚡︎ **Introduction**](#-introduction)
2.  [🚀 **Installation**](#-installation)
3.  [🔌 **Connectivity Logic**](#-connectivity-logic)
    - [The Multi-File State System](#the-multi-file-state-system)
    - [Reconnection Strategies](#reconnection-strategies)
    - [Pairing Code Logic (No QR)](#pairing-code-logic-no-qr)
4.  [💬 **Messaging Architecture**](#-messaging-architecture)
    - [Standard Text & Quoted Replies](#standard-text--quoted-replies)
    - [Album & Media Bursting](#album--media-bursting)
    - [Interactive Polls & Reactions](#interactive-polls--reactions)
5.  [🎴 **Advanced UI: The Carousel System**](#-advanced-ui-the-carousel-system)
    - [Horizontal Slides Implementation](#horizontal-slides-implementation)
6.  [🤳 **Status (Stories) Engineering**](#-status-stories-engineering)
    - [Status Group Posting (Privacy Targeting)](#status-group-posting-privacy-targeting)
    - [Status Group Mentioning (Mass Notifications)](#status-group-mentioning-mass-notifications)
7.  [📢 **Newsletters & Channels**](#-newsletters--channels)
    - [The Channel ID Fetcher](#the-channel-id-fetcher)
    - [Channel Interaction & MEX Queries](#channel-interaction--mex-queries)
8.  [🛡️ **Group Command & Control**](#-group-command--control)
    - [Join Request Management](#join-request-management)
    - [Participant & Admin Logic](#participant--admin-logic)
9.  [💼 **Business & Commerce Integration**](#-business--commerce-integration)
    - [Catalog Synchronization](#catalog-synchronization)
    - [Order Detail Decoding](#order-detail-decoding)
10. [🛠️ **Technical Utilities**](#-technical-utilities)
    - [The Blitz JID Decoder](#the-blitz-jid-decoder)
    - [USync & LID Addressing](#usync--lid-addressing)
11. [⚙️ **Full Socket Reference**](#-full-socket-reference)
12. [📡 **Event Bus System (sock.ev)**](#-event-bus-system-sockev)
13. [☠︎ **Security & Reliability**](#-security--reliability)
14. [⚡︎ **Credits & Legal**](#-credits--legal)

---

## ⚡︎ Introduction
**Blitz-bailey** is a total reconstruction of the `WhiskeySockets/Baileys` engine, optimized for speed and stripped of unnecessary overhead. While standard libraries focus on basic chat, Blitz-bailey provides low-level WebSocket access to specialized WhatsApp features including **Horizontal Card UI**, **Channel Metadata Unmasking**, and **Status Audience Targeting**. 

It is the definitive tool for developers building high-voltage automation systems on the WhatsApp Multi-Device (MD) protocol.

---

## 🚀 Installation

Install the library directly from the Blitz GitHub source:
```bash
npm install heroblitz/Blitz-bailey
```

Or add it to your `package.json`:
```json
"dependencies": {
    "blitz-bailey": "github:heroblitz/Blitz-bailey"
}
```

---

## 🔌 Connectivity Logic

Blitz-bailey implements a robust socket management layer that handles the Noise Handshake and E2EE key rotation automatically.

### The Multi-File State System
Standard authentication often fails on session reboot. Blitz-bailey utilizes a multi-file system to isolate credentials, keys, and session data.

```javascript
const { 
    default: makeWASocket, 
    useMultiFileAuthState, 
    DisconnectReason,
    makeInMemoryStore 
} = require('blitz-bailey');
const pino = require('pino');

async function initializeBlitz() {
    const { state, saveCreds } = await useMultiFileAuthState('blitz_sessions');
    const store = makeInMemoryStore({ logger: pino().child({ level: 'silent' }) });

    const sock = makeWASocket({
        auth: state,
        version: [2, 3000, 1023223821], // ⚡︎ Stabilized version for current MD
        printQRInTerminal: true,
        markOnlineOnConnect: true,
        logger: pino({ level: 'silent' })
    });

    sock.ev.on('creds.update', saveCreds);
}
```

### Reconnection Strategies
The engine is designed to auto-heal connections when WhatsApp drops the socket.

```javascript
sock.ev.on('connection.update', (update) => {
    const { connection, lastDisconnect } = update;
    if (connection === 'close') {
        const reason = lastDisconnect?.error?.output?.statusCode;
        if (reason !== DisconnectReason.loggedOut) {
            console.log("☠︎ Connection interrupted. Attempting Blitz-reconnect...");
            initializeBlitz();
        }
    } else if (connection === 'open') {
        console.log('⚡︎ BLITZ-BAILEY IS ONLINE ⚡︎');
    }
});
```

### Pairing Code Logic (No QR)
Link a bot using only a phone number. This is essential for server environments with no GUI.

```javascript
if (!sock.authState.creds.registered) {
    const phoneNumber = "234xxxxxxxxxx"; 
    const code = await sock.requestPairingCode(phoneNumber);
    console.log(`⚡︎ PAIRING CODE: ${code} ⚡︎`);
}
```

---

## 💬 Messaging Architecture

### Standard Text & Quoted Replies
Blitz-bailey simplifies the Protobuf structure so you can send rich replies with ease.

```javascript
await sock.sendMessage(jid, { 
    text: 'Command Executed ⚡︎',
    contextInfo: { 
        externalAdReply: {
            title: "Blitz Edition",
            body: "High Voltage API",
            thumbnailUrl: "https://files.catbox.moe/c6m3xc.jpg",
            sourceUrl: "https://t.me/blitz_tech",
            mediaType: 1
        }
    }
}, { quoted: m });
```

### Album & Media Bursting
Send groups of images or videos in a single "Album" stanza.
```javascript
await sock.sendAlbumMessage(jid, [
    { image: { url: './img1.jpg' }, caption: 'Blitz 1' },
    { image: { url: './img2.jpg' }, caption: 'Blitz 2' }
]);
```

---

## 🎴 Advanced UI: The Carousel System

One of the most advanced features of Blitz-bailey is the **Horizontal Slide (Carousel)** support. This allows the bot to send interactive, swipeable cards.

### Horizontal Slides Implementation
```javascript
await sock.sendMessage(jid, {
    cards: [
        {
            title: "Project Alpha",
            body: "The speed of lightning.",
            footer: "Blitz Dev",
            image: { url: "https://example.com/slide1.jpg" },
            buttons: [{ name: "cta_url", buttonParamsJson: '{"display_text":"Source","url":"https://t.me/blitzxdev"}' }]
        },
        {
            title: "Project Beta",
            body: "The power of a skull.",
            footer: "Blitz Dev",
            image: { url: "https://example.com/slide2.jpg" },
            buttons: [{ name: "quick_reply", buttonParamsJson: '{"display_text":"Join Chat","id":"join_blitz"}' }]
        }
    ]
});
```

---

## 🤳 Status (Stories) Engineering

> **Blitz-bailey creates a clear distinction between the two types of Status manipulation:**

### Status Group Posting (Privacy Targeting)
This feature allows you to upload a status that is **only** visible to specific groups or people. It essentially "posts" to a restricted audience.
```javascript
await sock.sendMessage("status@broadcast", {
    text: "Confidential Status Update"
}, {
    statusJidList: ["120363xxxx@g.us", "234xxxxxxxx@s.whatsapp.net"] 
});
```

### Status Group Mentioning (Mass Notifications)
This posts a status but **mentions** specific audience members so they get an alert notification on their phone.
```javascript
await sock.sendStatusMentions(
    { text: "⚡︎ ☠︎ URGENT ANNOUNCEMENT ⚡︎ ☠︎" }, 
    "status@broadcast", 
    ["120363xxxx@g.us", "234xxxxxxx@s.whatsapp.net"] 
);
```

---

## 📢 Newsletters & Channels

Blitz-bailey is the premier engine for Newsletter automation, utilizing MEX GraphQL queries to pull hidden data.

### The Channel ID Fetcher
WhatsApp Channel JIDs (e.g., `120363... @newsletter`) are usually hidden behind public invite links. This function unmasks them.

```javascript
const inviteLink = "0029VaXXXXX";
const metadata = await sock.newsletterMetadata("invite", inviteLink);

console.log(`⚡︎ Hidden JID: ${metadata.id}`);
console.log(`⚡︎ Name: ${metadata.name}`);
console.log(`⚡︎ Followers: ${metadata.subscribers}`);
```

### Channel Interaction
```javascript
await sock.newsletterFollow(jid);
await sock.newsletterReactMessage(jid, "101", "⚡︎"); // React to Post ID 101
```

---

## 🛡️ Group Command & Control

### Join Request Management
For groups with "Admin Approval" turned on, Blitz-bailey can manage the queue.
```javascript
const requests = await sock.groupRequestParticipantsList(groupJid);
// Approve specific user
await sock.groupRequestParticipantsUpdate(groupJid, [requests[0].jid], "approve");
```

---

## 🛠️ Technical Utilities

### The Blitz JID Decoder
Automatically strips device IDs and cleans JIDs for database storage.
```javascript
const cleanJid = sock.decodeJid("2348000000:1@s.whatsapp.net");
// Returns: 2348000000@s.whatsapp.net
```

### USync & LID Addressing
Support for the new **LID (Linked Identity)** protocol, allowing bots to interact with numbers that haven't set up a traditional JID profile.
```javascript
const lidData = await sock.getLidUser("234xxxxxxxx@s.whatsapp.net");
```

---

## ⚙️ Full Socket Reference

| Function | Description |
| :--- | :--- |
| `sendAlbumMessage` | Bundles multiple media items into one message. |
| `sendStatusMentions` | Posts status and tags recipients. |
| `newsletterMetadata` | Fetches hidden JID from Channel links. |
| `newsletterWMexQuery` | Executes raw GraphQL (MEX) commands. |
| `getOrderDetails` | Decodes Business Order tokens. |
| `groupRequestParticipantsList` | Fetches the pending join request list. |
| `updateCallPrivacy` | Controls who can call the bot account. |

---

## ☠︎ Security & Reliability
To prevent account termination and ensuring maximum uptime:
1.  **Delay Loops:** Always implement a delay between bulk messages (`await delay(1500)`).
2.  **Version Consistency:** Use the hardcoded version `[2, 3000, 1023223821]` to ensure protocol compliance.
3.  **LID Safety:** When interacting with strangers, use LID-based functions to avoid triggering WhatsApp's anti-spam filters.

---

## ⚡︎ Credits & Legal

<div align="center">

**Lead Developer:** Blitz  
**Telegram:** [@blitzxdev](https://t.me/blitzxdev)  
**Community:** [@blitz_tech](https://t.me/blitz_tech)  

**LICENSE: MIT**  
*Blitz-bailey is an independent modification. Use responsibly. Automated spamming is prohibited by WhatsApp Terms of Service.*

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%">

### ⚡︎ ☠︎ DEVELOPED BY BLITZ ☠︎ ⚡︎

</div>
