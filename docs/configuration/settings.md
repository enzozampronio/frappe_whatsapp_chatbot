# Chatbot Settings

The main configuration for the chatbot is in the **WhatsApp Chatbot** single DocType.

![Chatbot Settings](../assets/chatbot.png)

## General Settings

| Setting | Description |
|---------|-------------|
| **Enabled** | Master switch to enable/disable the chatbot |
| **WhatsApp Account** | Specific account to process messages from |
| **Process All Accounts** | Process messages from all WhatsApp accounts |

## Default Responses

| Setting | Description |
|---------|-------------|
| **Default Response** | Message sent when no keyword or flow matches |

Example default response:
```
Sorry, I didn't understand that. Please try again or type 'help' for assistance.
```

## Business Hours

Restrict chatbot responses to specific hours for each day of the week.

| Setting | Description |
|---------|-------------|
| **Respond Only During Business Hours** | Enable time-based filtering |
| **Out of Hours Message** | Message sent outside business hours |
| **Populate Default Hours** | Button to auto-fill Mon-Fri 9AM-6PM, weekends closed |
| **Business Hours Schedule** | Table to configure hours for each day |

### Business Hours Schedule Table

| Column | Description |
|--------|-------------|
| **Day** | Day of the week (Monday-Sunday) |
| **Open** | Check if business is open on this day |
| **Start Time** | Opening time for this day |
| **End Time** | Closing time for this day |

### Example Configuration

| Day | Open | Start | End |
|-----|------|-------|-----|
| Monday | ✓ | 09:00 | 18:00 |
| Tuesday | ✓ | 09:00 | 18:00 |
| Wednesday | ✓ | 09:00 | 18:00 |
| Thursday | ✓ | 09:00 | 18:00 |
| Friday | ✓ | 09:00 | 17:00 |
| Saturday | ✓ | 10:00 | 14:00 |
| Sunday | ✗ | - | - |

**Tip:** Click "Populate Default Hours" to quickly set up a standard Mon-Fri 9AM-6PM schedule with weekends closed.

## Session Settings

| Setting | Description |
|---------|-------------|
| **Session Timeout (Minutes)** | Time before inactive flow sessions expire (default: 30) |
| **Log Conversations** | Log all chatbot conversations for analytics |

## Excluded Numbers

Add phone numbers that should not receive automated responses.

Use this for:
- VIP customers who should always talk to humans
- Internal team members
- Test numbers

## AI Configuration

See [AI Integration](ai.md) for detailed AI setup.

| Setting | Description |
|---------|-------------|
| **Enable AI** | Use AI for fallback responses |
| **AI Provider** | OpenAI, Anthropic, or Custom |
| **AI API Key** | Your API key |
| **AI Model** | Model name (e.g., gpt-4o-mini) |
| **Max Tokens** | Maximum response length |
| **Temperature** | Creativity (0 = deterministic, 1 = creative) |
| **System Prompt** | Instructions for the AI |
