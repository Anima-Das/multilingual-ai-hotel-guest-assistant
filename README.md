<div align="center">

# 🏨 Multilingual AI Hotel Guest Assistant

**An n8n automation that answers hotel guests in their own language, hands sensitive cases to the front desk, and logs every conversation.**

<p>
  <img alt="n8n" src="https://img.shields.io/badge/n8n-Workflow-EA4B71?style=for-the-badge&logo=n8n&logoColor=white" />
  <img alt="GPT-4o-mini" src="https://img.shields.io/badge/OpenAI-GPT--4o--mini-412991?style=for-the-badge&logo=openai&logoColor=white" />
  <img alt="Multilingual AI" src="https://img.shields.io/badge/Multilingual-EN%20%7C%20EL%20%7C%20BG-0A84FF?style=for-the-badge" />
</p>

<p>
  <img alt="WhatsApp" src="https://img.shields.io/badge/WhatsApp-Replies-25D366?style=flat-square&logo=whatsapp&logoColor=white" />
  <img alt="Viber" src="https://img.shields.io/badge/Viber-Replies-7360F2?style=flat-square&logo=viber&logoColor=white" />
  <img alt="Slack" src="https://img.shields.io/badge/Slack-Human%20Handoff-4A154B?style=flat-square&logo=slack&logoColor=white" />
  <img alt="Google Sheets" src="https://img.shields.io/badge/Google%20Sheets-Logging-34A853?style=flat-square&logo=googlesheets&logoColor=white" />
  <img alt="Status" src="https://img.shields.io/badge/Status-Inactive%20by%20default-F59E0B?style=flat-square" />
</p>

</div>

---

## ✨ Overview

This repository contains an exported **n8n workflow** for a hospitality guest-support assistant. Guest messages arrive through a webhook and are normalized. A **GPT-4o-mini** agent with per-guest memory replies in the guest's language and returns a structured result. Cases that need a person are announced in a Slack **front-desk** channel. The reply goes out through **WhatsApp** or **Viber**, and the exchange is appended to **Google Sheets**.

> 📌 This README documents the workflow implementation exactly as exported in the JSON. It does not report production results or business metrics.

---

## 🧭 Project Snapshot

| 🧩 Area | ⚙️ Implementation |
|---|---|
| 📁 Project | Multilingual AI Hotel Guest Assistant |
| 🛠️ Platform | n8n (workflow name: `07 Multilingual AI Hotel Guest Assistant`) |
| 🏨 Domain | Hospitality / guest support |
| 🤖 AI Model | OpenAI `gpt-4o-mini`, temperature `0.3` |
| 🌍 Languages | English, Greek, Bulgarian, and other languages named in the assistant instructions |
| 🧠 Memory | Per-guest window memory, 12-message context window |
| 🧾 AI Output | Structured: `language`, `reply`, `escalate`, `category` |
| 📡 Guest Channels | WhatsApp and Viber |
| 🚨 Human Handoff | Slack channel `front-desk` |
| 📊 Logging | Google Sheets worksheet `Conversations` |
| 🔌 Input | `POST` webhook at path `guest-inbound` |
| 📤 Output | Channel reply plus a JSON response to the upstream gateway |
| 🛡️ Monitoring | Error Trigger with a Slack alert to `automation-alerts` |
| 🚦 Status | Exported for portfolio demonstration; `active: false` |

---

## 🖼️ Workflow Overview

<img width="1037" height="523" alt="07 Multilingual AI Hotel Guest Assistant" src="https://github.com/user-attachments/assets/01ac0f60-95e3-48e2-807c-71b28da83472" />


*The complete workflow canvas: guest intake, AI agent with its model, memory and parser, escalation, channel delivery, logging, and the error-monitoring branch.*

---

## 🎯 Skills Demonstrated

| 💼 Skill | 🔎 Where it shows up |
|---|---|
| n8n workflow automation | 15 nodes covering intake, AI, branching, delivery, logging, and error handling |
| AI agent orchestration | Agent node with a chat model, memory, and output parser attached |
| Multilingual conversational AI | Same-language replies driven by the system prompt |
| Structured AI outputs | JSON schema example enforced through an output parser |
| Contextual memory | Guest-keyed buffer window memory |
| Human-in-the-loop escalation | IF branch plus Slack notification |
| Multi-channel messaging | Switch routing to WhatsApp and Viber HTTP APIs |
| Reliability basics | Retry on outbound sends and an error-alert path |
| Operational logging | Google Sheets append with a defined column mapping |

---

## 🚀 Core Capabilities

| 🧩 Capability | 📝 Implementation |
|---|---|
| 🏨 **Guest Assistance** | The agent is briefed as the AI Guest Assistant for a hospitality group running hotels and vacation rentals. It answers from a short built-in knowledge list. |
| 🌍 **Multilingual Communication** | The agent detects the guest's language, replies in the same language, mirrors tone, and keeps replies short and warm. |
| 🧠 **Conversational Memory** | Each guest ID gets its own memory session with a 12-message window, so follow-up questions keep context. |
| 🤖 **AI Response Generation** | `GPT 4o mini` runs inside the `Guest Assistant Agent`, with the normalized message as the prompt. |
| 🧾 **Structured Output** | A structured output parser returns `language`, `reply`, `escalate`, and `category`. |
| 🚨 **Human Escalation** | When `escalate` is true, a Slack message is posted to `front-desk`, and the guest is told in their own language that the front desk is being connected. |
| 💬 **Multi-Channel Messaging** | A Switch node routes to WhatsApp or Viber based on the normalized `channel` value. |
| 📊 **Conversation Logging** | Each exchange is appended as a row to the `Conversations` worksheet. |
| 🛡️ **Error Handling** | Outbound sends retry on failure, and a separate Error Trigger branch posts failures to `automation-alerts`. |

---

## 🌍 Multilingual AI Assistant

The assistant handles multilingual guests in one pass, without separate per-language flows.

| 🔤 Aspect | 📋 Behavior in the workflow |
|---|---|
| Input cleanup | `Normalize Payload` trims the incoming text and standardizes the channel to lowercase. |
| Language detection | Instructed in the system prompt, and returned as the `language` field. |
| Reply language | Always the same language the guest wrote in. |
| Tone | Short and warm, mirroring the guest's tone. |
| Knowledge | Fixed hotel facts embedded in the prompt (listed below). |
| Output | A structured response containing the reply, escalation flag, and category. |
| Uncertainty | Anything the assistant cannot confirm is escalated, not guessed. |

Designed to handle multilingual guest messages, with **English, Greek, Bulgarian, and other languages** explicitly named in the assistant instructions.

<details>
<summary><b>🏨 Hotel knowledge documented in the assistant instructions</b></summary>

<br />

| 🗂️ Topic | 📄 Documented detail |
|---|---|
| Check-in | 15:00 |
| Check-out | 11:00 |
| Early check-in / late check-out | Available on request |
| Breakfast | 07:00 to 10:30 in the lobby restaurant |
| Wi-Fi | Free; network `HotelGuest`; password is on the welcome card |
| Pets | Allowed in selected rooms |
| Parking | Available on request |
| Reception | Open 24 hours |

</details>

---

## 🧠 Conversation Memory

The **Guest Conversation Memory** node is a window buffer memory attached to the agent.

| ⚙️ Setting | 🔧 Value |
|---|---|
| Session type | Custom key |
| Session key | The normalized `guestId` |
| Context window length | `12` |

Because the key is the guest ID, each guest's messages are tracked in their own session. A guest can write "and what time is breakfast?" and the assistant still sees the recent exchange. This is a short rolling window, not a long-term database memory, and it does not store unlimited history.

---

## 🧾 Structured AI Output

The **Reply Structure Parser** gives the agent a JSON schema example, so downstream nodes can read reliable fields.

| 🧱 Field | 🎯 Purpose |
|---|---|
| `language` | Detected guest language (example value: `en`) |
| `reply` | Guest-facing response in the guest's language |
| `escalate` | Boolean that decides whether the front desk is notified |
| `category` | Classification of the request (example value: `general`) |

These fields drive escalation, the Slack message, the outbound reply text, the log row, and the gateway response.

---

## 🚨 Human Handoff: Front-Desk Escalation

The assistant sets `escalate` to `true` for these documented scenarios:

| 🚩 Scenario | 💡 Handling |
|---|---|
| 💸 Refunds | Escalated |
| 📅 Booking changes | Escalated |
| 😟 Complaints | Escalated |
| 💳 Payments | Escalated |
| 🩺 Medical issues | Escalated |
| 🛑 Safety issues | Escalated |
| ❓ Anything it cannot confirm | Escalated |

**What happens on escalation**

1. The `Needs Human?` IF node checks `output.escalate`.
2. `Notify Front Desk` posts a Slack message to the **`front-desk`** channel with the guest name, channel, original message, and AI-assigned category.
3. The flow then continues to channel routing, so the guest still receives the assistant's reply, which tells them in their own language that the front desk team is being connected.

This workflow notifies Slack only. It does **not** create a ticket or write to a CRM.

---

## 📡 Multi-Channel Delivery and Integrations

| 📡 Integration | 🔧 Role | ✅ Status |
|---|---|---|
| WhatsApp | Guest reply delivery via HTTP request | Implemented |
| Viber | Guest reply delivery via HTTP request | Implemented |
| Slack | Front-desk escalation and engineering alerts | Implemented |
| Google Sheets | Conversation logging | Implemented |
| OpenAI | `gpt-4o-mini` chat model | Implemented |

### 💚 WhatsApp
`Send WhatsApp Reply` sends a `POST` request to the Facebook Graph messages endpoint (`/v20.0/{phone id}/messages`) using the WhatsApp text message format. The recipient is the normalized `sender`, and the body is the AI `reply`. Authentication uses an n8n generic HTTP header credential.

> ⚠️ The URL contains the placeholder **`REPLACE_WHATSAPP_PHONE_ID`**, which must be replaced during setup. No production number is included.

### 💜 Viber
`Send Viber Reply` sends a `POST` request to the Viber `send_message` API. The payload contains the `receiver` (the normalized `sender`), the type `text`, and the AI `reply` as the text. It also authenticates with an n8n generic HTTP header credential.

---

## 📊 Google Sheets Logging

`Log Conversation` appends one row per processed exchange to the worksheet **`Conversations`**.

| 🗃️ Column | 📝 Source |
|---|---|
| `timestamp` | Time the payload was received |
| `guest_id` | Normalized guest identifier |
| `channel` | Normalized channel |
| `language` | AI-detected language |
| `message` | Original guest message |
| `reply` | AI reply |
| `escalated` | AI escalation flag |
| `category` | AI request category |

> ⚠️ The document ID is currently the placeholder **`REPLACE_SHEET_ID`**. Replace it and make sure the connected Google account can edit the sheet.

---

## 🛡️ Error Handling and Operations

| 🔧 Mechanism | 📋 Behavior |
|---|---|
| **Retry on fail** | `Send WhatsApp Reply` and `Send Viber Reply` both retry up to **3 tries**, waiting **2000 ms** between tries. |
| **On error setting** | Both send nodes use `continueRegularOutput`, so the workflow continues to logging and the gateway response even if a send fails after retries. |
| **Error Trigger** | A separate `Error Trigger` branch runs when the workflow itself fails. |
| **Engineering alert** | `Alert Engineering` posts to the Slack channel **`automation-alerts`** with the workflow name, last executed node, and error message. |

Because the send nodes continue on error, the final `status: sent` response reflects that the flow completed, not a confirmed delivery receipt.

---

## 🔒 Safety Boundaries

These rules come directly from the assistant's instructions:

- 🚫 **Never invent prices.**
- 🚫 **Never invent availability.**
- 🚫 **Never invent policies.**
- 🚨 Escalate medical and safety issues.
- 🚨 Escalate refunds, booking changes, complaints, and payments.
- 🚨 Escalate anything the assistant cannot confirm.

The workflow does not add authentication on the inbound webhook, so protect that endpoint at the gateway or n8n level as appropriate for your deployment.

---

## 🧩 Workflow Components

| 🧩 Component | 🛠️ Node / Technology | 🎯 Responsibility |
|---|---|---|
| Inbound Gateway | `Inbound Message Webhook` (Webhook, `POST`, `guest-inbound`) | Receive guest messages; response is sent by a Respond to Webhook node |
| Payload Normalization | `Normalize Payload` (Code) | Build `guestId`, `channel`, `sender`, `guestName`, `message`, `receivedAt` |
| AI Assistant | `Guest Assistant Agent` (n8n AI Agent) | Generate the guest reply and escalation decision |
| LLM | `GPT 4o mini` (OpenAI chat model) | Language model, temperature 0.3 |
| Memory | `Guest Conversation Memory` (Window Buffer Memory) | Keep guest context, 12-message window |
| Output Parser | `Reply Structure Parser` (Structured Output Parser) | Enforce `language`, `reply`, `escalate`, `category` |
| Escalation Check | `Needs Human?` (IF) | Branch on the escalate flag |
| Front-Desk Alert | `Notify Front Desk` (Slack) | Post to `front-desk` |
| Channel Router | `Route By Channel` (Switch) | Select WhatsApp or Viber |
| Guest Delivery | `Send WhatsApp Reply`, `Send Viber Reply` (HTTP Request) | Send the AI reply |
| Conversation Log | `Log Conversation` (Google Sheets) | Append the interaction row |
| Gateway Response | `Respond to Guest Gateway` (Respond to Webhook) | Return a JSON status |
| Error Monitoring | `Error Trigger` and `Alert Engineering` (Slack) | Notify engineering of failures |

---

## 🔄 Workflow Lifecycle

| # | 🧭 Stage | 📋 What happens |
|---|---|---|
| 1 | 📥 Intake | A guest message arrives at the `guest-inbound` webhook. |
| 2 | 🧹 Normalization | `Normalize Payload` maps the body into a consistent structure. |
| 3 | 🤖 AI processing | The agent uses the message, the guest's memory, and the system prompt. |
| 4 | 🧾 Structured output | The parser returns language, reply, escalation flag, and category. |
| 5 | 🔍 Escalation check | `Needs Human?` evaluates the escalate flag. |
| 6 | 🚨 Front-desk alert | If true, a Slack message is sent to `front-desk`. |
| 7 | 📡 Channel selection | `Route By Channel` picks WhatsApp or Viber. |
| 8 | 💬 Reply delivery | The matching HTTP request sends the reply, with up to 3 tries. |
| 9 | 📊 Logging | The conversation row is appended to Google Sheets. |
| 10 | ✅ Gateway response | The upstream gateway gets JSON with `status`, `language`, and `escalated`. |

---

## 🔧 Technical Implementation

<details>
<summary><b>Node-by-node notes</b></summary>

<br />

- **Inbound Message Webhook**: Accepts `POST` at `guest-inbound` with `responseMode` set to a response node, so the reply to the gateway is controlled by `Respond to Guest Gateway`.
- **Normalize Payload**: Reads the request `body`. The guest ID comes from `guest_id`, falling back to `from`, then `unknown`. The channel defaults to `whatsapp` and is lowercased. The name comes from `profile_name` or `name`, defaulting to `Guest`. The message comes from `text` or `message`, trimmed.
- **Guest Assistant Agent**: Uses a defined prompt with the normalized message as input, a detailed system message, and an attached output parser.
- **GPT 4o mini**: `gpt-4o-mini` with temperature `0.3`.
- **Guest Conversation Memory**: Custom session key set to the guest ID; context window 12.
- **Reply Structure Parser**: JSON schema example with four fields.
- **Needs Human?**: Boolean true check on `output.escalate`. The true branch notifies Slack; the false branch goes straight to channel routing.
- **Notify Front Desk**: Slack message to `front-desk` including guest name, channel, message, and category.
- **Route By Channel**: Two rules, `whatsapp` and `viber`, compared case-insensitively. A channel value outside these two has no configured route.
- **Send WhatsApp Reply / Send Viber Reply**: HTTP `POST` requests with header-based credentials, `retryOnFail`, 3 tries, 2 second wait, and `continueRegularOutput`.
- **Log Conversation**: Google Sheets `append` with eight explicitly mapped columns.
- **Respond to Guest Gateway**: Returns `{ status: 'sent', language, escalated }`.
- **Error Trigger / Alert Engineering**: Posts the failed workflow's name, last node, and error message to `automation-alerts`.

</details>

---

## ⚙️ Setup and Configuration

### 1. Import
Import `07 Multilingual AI Hotel Guest Assistant.json` into n8n. The workflow imports as **inactive**.

### 2. Replace placeholders

| 🔖 Placeholder | 📍 Location | 📝 Replace with |
|---|---|---|
| `REPLACE_WHATSAPP_PHONE_ID` | URL of `Send WhatsApp Reply` | Your WhatsApp Business phone number ID |
| `REPLACE_SHEET_ID` | Document ID in `Log Conversation` | The ID of your Google Sheet |

### 3. Connect credentials in n8n

| 🔌 Service | 🧩 Used by |
|---|---|
| OpenAI | `GPT 4o mini` |
| HTTP header auth (generic) | `Send WhatsApp Reply` |
| HTTP header auth (generic) | `Send Viber Reply` |
| Slack | `Notify Front Desk`, `Alert Engineering` |
| Google Sheets | `Log Conversation` |

Credentials are configured inside n8n and are not stored in this repository.

### 4. Prepare destinations
- Create Slack channels named `front-desk` and `automation-alerts`.
- Create a Google Sheet with a worksheet named `Conversations` and the eight column headers listed above.

### 5. Set the error workflow
Assign this workflow as its own error workflow in n8n settings so the `Error Trigger` branch can fire.

### 6. Activate
Activate the workflow when your credentials, channels, and sheet are ready.

---

## 🧪 Running the Workflow

Execution begins with an inbound `POST` to the webhook. `Normalize Payload` reads these fields from the request body:

| 📥 Field | 📝 Used for | 🔁 Fallback |
|---|---|---|
| `guest_id` | Guest identifier and memory key | `from`, then `unknown` |
| `channel` | Routing (`whatsapp` or `viber`) | `whatsapp` |
| `from` | Recipient identifier for replies | Empty string |
| `profile_name` | Guest name in Slack alerts | `name`, then `Guest` |
| `text` | Guest message | `message` |

Illustrative request body (placeholder values only):

```json
{
  "guest_id": "guest-123",
  "channel": "whatsapp",
  "from": "<recipient-id>",
  "profile_name": "Alex",
  "text": "What time is breakfast?"
}
```

This reflects what `Normalize Payload` reads. It is not a finalized production contract.

---

## 💬 Example Interactions

> The following are **fictional examples** that illustrate the intended behavior. They are not execution logs or measured outputs.

| 🌐 Guest language | 🗨️ Guest message | 🤖 Expected behavior |
|---|---|---|
| 🇬🇧 English | "What time is check-out?" | Replies in English with check-out at 11:00; `escalate` false. |
| 🇬🇷 Greek | A question about breakfast hours | Replies in Greek with breakfast from 07:00 to 10:30. |
| 🇧🇬 Bulgarian | A question about Wi-Fi | Replies in Bulgarian with the network name `HotelGuest` and a pointer to the welcome card. |
| 🚨 Any | "I need a refund for my booking." | Replies in the guest's language that the front desk is being connected; `escalate` true; Slack alert to `front-desk`. |

---

## 🧰 Tech Stack

<p>
  <img alt="n8n" src="https://img.shields.io/badge/n8n-EA4B71?style=flat-square&logo=n8n&logoColor=white" />
  <img alt="OpenAI" src="https://img.shields.io/badge/OpenAI-412991?style=flat-square&logo=openai&logoColor=white" />
  <img alt="WhatsApp" src="https://img.shields.io/badge/WhatsApp-25D366?style=flat-square&logo=whatsapp&logoColor=white" />
  <img alt="Viber" src="https://img.shields.io/badge/Viber-7360F2?style=flat-square&logo=viber&logoColor=white" />
  <img alt="Slack" src="https://img.shields.io/badge/Slack-4A154B?style=flat-square&logo=slack&logoColor=white" />
  <img alt="Google Sheets" src="https://img.shields.io/badge/Google%20Sheets-34A853?style=flat-square&logo=googlesheets&logoColor=white" />
  <img alt="JavaScript" src="https://img.shields.io/badge/JavaScript-Code%20Node-F7DF1E?style=flat-square&logo=javascript&logoColor=black" />
</p>

Webhooks and REST-style HTTP requests connect the pieces.

---

## 📋 Current Status and Deployment Requirements

| 📌 Item | 📝 Detail |
|---|---|
| Activation | The workflow is exported inactive (`active: false`). |
| Placeholders | `REPLACE_WHATSAPP_PHONE_ID` and `REPLACE_SHEET_ID` must be replaced. |
| Credentials | Real delivery needs configured WhatsApp and Viber header credentials, plus OpenAI, Slack, and Google Sheets connections. |
| Google Sheets | Needs the correct document ID, a `Conversations` worksheet, and edit access. |
| Channel coverage | Routing is defined for `whatsapp` and `viber` only. |
| Delivery status | The send nodes continue on error, so the gateway `sent` status does not confirm delivery. |
| AI quality | Reply quality depends on the model configuration and the prompt design. |
| Scope | This README documents the implementation, not production business results. |

---

## 🔐 Security and Privacy Notes

- Manage secrets through n8n credentials or environment management, and never commit API keys or tokens.
- Do not commit real guest personal data, conversations, or exports to a public repository.
- Replace all placeholders before any production deployment.
- Apply appropriate access controls to the Google Sheet, the Slack channels, and the messaging provider accounts.
- Review your own legal and data-protection obligations before processing real guest data. This project makes no compliance claims.

---

## 📁 Repository Structure

```text
README.md
07 Multilingual AI Hotel Guest Assistant.json
screenshots/
    workflow-overview.png
```

---

<div align="center">

🏨 Hospitality &nbsp;·&nbsp; 🤖 AI &nbsp;·&nbsp; 🌍 Multilingual &nbsp;·&nbsp; 💬 Messaging &nbsp;·&nbsp; 🚨 Human Handoff &nbsp;·&nbsp; 📊 Logging

*Built with n8n as a portfolio demonstration of practical hospitality automation.*

</div>
