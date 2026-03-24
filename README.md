<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" alt="WhatsApp" width="80" height="80">
</p>

<h1 align="center">WhatsApp Chat Module</h1>

<p align="center">
  <strong>A production-ready WhatsApp integration for Perfex CRM</strong>
</p>

<p align="center">
  <a href="#features">Features</a> •
  <a href="#installation">Installation</a> •
  <a href="#n8n-setup">n8n Setup</a> •
  <a href="#api-reference">API Reference</a> •
  <a href="#changelog">Changelog</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-1.0.3-green.svg" alt="Version">
  <img src="https://img.shields.io/badge/Perfex_CRM-2.3%2B-blue.svg" alt="Perfex CRM">
  <img src="https://img.shields.io/badge/PHP-7.4%2B-purple.svg" alt="PHP">
  <img src="https://img.shields.io/badge/license-MIT-orange.svg" alt="License">
</p>

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 💬 **Real-time Messages** | View inbound & outbound WhatsApp conversations in Perfex CRM |
| 🔄 **n8n Integration** | Secure API endpoint for n8n workflow automation |
| 🎨 **WhatsApp-style UI** | Beautiful two-panel chat interface |
| 🔗 **CRM Linking** | Auto-links conversations to Customers or Leads by phone |
| 🔔 **Unread Counts** | Badge notifications for new messages |
| ⚡ **AJAX Polling** | Auto-refresh without page reload |
| 🛡️ **Duplicate Prevention** | Smart `message_uid` deduplication |
| 🔐 **Bearer Token Auth** | Secure API authentication |

---

## 📋 Requirements

- **Perfex CRM** 2.3.0 or higher
- **PHP** 7.4+
- **MySQL** 5.7+ or MariaDB 10.3+
- **n8n** (for WhatsApp message sync)

---

## 🚀 Installation

```bash
# 1. Clone or download this repository
git clone https://github.com/shojeeb1943/n8n-whatsapp-to-perfex-crm.git

# 2. Copy to your Perfex modules folder
cp -r n8n-whatsapp-to-perfex-crm /path/to/perfex_crm/modules/whatsapp_chat
```

3. Go to **Setup → Modules** in Perfex CRM admin
4. Find **"WhatsApp Chat"** and click **Activate**
5. Navigate to **WhatsApp Messages → Settings** to get your API token

---

## 🔧 n8n Setup

### Endpoint

```
POST https://your-domain.com/whatsapp_chat/api/message
```

### Headers

```http
Authorization: Bearer YOUR_API_TOKEN
Content-Type: application/json
```

### Inbound Message (Customer → You)

```json
{
  "message_uid": "wamid.HBgNODgwMTgxNjEyMjE4OBUCABIYFjNFQjBG...",
  "phone": "+8801816122188",
  "display_name": "John Doe",
  "direction": "inbound",
  "sender_type": "customer",
  "message_type": "text",
  "message_text": "Hello, I need help!",
  "sent_at": "2024-01-15 14:30:00",
  "channel": "whatsapp"
}
```

### Outbound Message (Bot → Customer)

```json
{
  "message_uid": "bot-1705329000-8801816122188",
  "phone": "+8801816122188",
  "display_name": "John Doe",
  "direction": "outbound",
  "sender_type": "bot",
  "message_type": "text",
  "message_text": "Thanks for contacting us!",
  "sent_at": "2024-01-15 14:30:05",
  "channel": "whatsapp"
}
```

---

## 📖 API Reference

### Required Fields

| Field | Type | Description |
|:------|:-----|:------------|
| `message_uid` | `string` | Unique message ID (prevents duplicates) |
| `phone` | `string` | Phone number (E.164 format recommended) |
| `direction` | `string` | `inbound` or `outbound` |

### Optional Fields

| Field | Type | Default | Description |
|:------|:-----|:--------|:------------|
| `display_name` | `string` | phone | Contact name |
| `sender_type` | `string` | auto | `customer`, `bot`, `staff` |
| `message_type` | `string` | `text` | `text`, `image`, `audio`, `video`, `document`, `location`, `sticker`, `contact` |
| `message_text` | `string` | — | Message content |
| `media_url` | `string` | `null` | URL for media messages |
| `sent_at` | `string` | now | MySQL datetime format |
| `channel` | `string` | `whatsapp` | Channel identifier |

### Response Examples

<details>
<summary><strong>✅ Success (New Message)</strong></summary>

```json
{
  "success": true,
  "duplicate": false,
  "message": "Message stored",
  "conversation_id": 123,
  "message_id": 456
}
```
</details>

<details>
<summary><strong>✅ Success (Duplicate)</strong></summary>

```json
{
  "success": true,
  "duplicate": true,
  "message": "Duplicate message_uid, not inserted",
  "conversation_id": 123
}
```
</details>

<details>
<summary><strong>❌ Error</strong></summary>

```json
{
  "success": false,
  "error": "Error description"
}
```
</details>

---

## 🗄️ Database Schema

The module creates two tables on activation:

```
tblwhatsapp_conversations  →  Conversation threads
tblwhatsapp_messages       →  Individual messages
```

---

## 🔐 Permissions

| Permission | Description |
|:-----------|:------------|
| **View** | View WhatsApp Messages (Global) |
| **Create** | Send Messages (future feature) |

---

## 📝 Changelog

### v1.0.3 (Current)
- ✅ Production-ready release
- 🎨 WhatsApp SVG icon & double-check read receipts
- ⚙️ Settings button in header
- 🌐 Complete language translations
- 🔒 Security index.html files

### v1.0.2
- 🔧 Fixed helper loading for API controller
- 🔧 Fixed CSRF exclusion for external API
- 🔧 MySQL 5.7 compatibility

### v1.0.1
- 🎉 Initial release

---

## 🤝 Contributing

Contributions, issues and feature requests are welcome!

Feel free to check the [issues page](https://github.com/shojeeb1943/n8n-whatsapp-to-perfex-crm/issues).

---

## 👤 Author

**Shojeeb**

- GitHub: [@shojeeb1943](https://github.com/shojeeb1943)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<p align="center">
  Made with ❤️ for the Perfex CRM community
</p>
