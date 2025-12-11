# Keyword Replies

Keyword replies allow you to configure automatic responses based on message content.

![Keyword Reply](../assets/keyword_reply.png)

## Creating a Keyword Reply

1. Go to **WhatsApp Keyword Reply** → **+ Add**
2. Fill in the fields below
3. Click **Save**

## Fields

### Basic Settings

| Field | Description |
|-------|-------------|
| **Title** | Name for this rule (e.g., "Greeting", "Business Hours") |
| **Enabled** | Enable/disable this rule |
| **Priority** | Higher priority rules match first (default: 10) |
| **WhatsApp Account** | Leave empty to apply to all accounts |

### Keyword Matching

| Field | Description |
|-------|-------------|
| **Keywords** | Comma-separated list (e.g., `hello, hi, hey`) |
| **Match Type** | How to match keywords (see below) |
| **Case Sensitive** | Enable for case-sensitive matching |

#### Match Types

| Type | Description | Example |
|------|-------------|---------|
| **Exact** | Message must match keyword exactly | `hello` matches "hello" but not "hello there" |
| **Contains** | Keyword anywhere in message | `price` matches "what's the price?" |
| **Starts With** | Message starts with keyword | `order` matches "order status" |
| **Regex** | Regular expression pattern | `order\s+\d+` matches "order 12345" |

### Response Configuration

| Field | Description |
|-------|-------------|
| **Response Type** | Text, Template, Media, Flow, or Script |

#### Response Type: Text

Simple text response.

```
Response Text: Hello! How can I help you today?
```

#### Response Type: Template

Send a WhatsApp-approved template.

| Field | Description |
|-------|-------------|
| **Response Template** | Link to WhatsApp Templates |
| **Template Parameters** | JSON parameters for the template |

#### Response Type: Media

Send an image, video, audio, or document.

| Field | Description |
|-------|-------------|
| **Media Type** | Image, Video, Audio, or Document |
| **Media URL** | Public URL to the media file |
| **Media Caption** | Optional caption |

#### Response Type: Flow

Trigger a conversation flow.

| Field | Description |
|-------|-------------|
| **Trigger Flow** | Link to WhatsApp Chatbot Flow |

#### Response Type: Script

Execute a Server Script or Python method. The script receives the WhatsApp Message document and can return a response.

| Field | Description |
|-------|-------------|
| **Script** | Server Script name (API type) or method path (e.g., `myapp.api.handle_message`) |

The script/method receives the WhatsApp Message document as `doc`:

```python
# Example method in myapp/api.py
def escalate_to_agent(doc):
    """
    doc is the WhatsApp Message document with fields:
    - doc.from_ or doc.get("from"): sender's phone number
    - doc.message: message text
    - doc.whatsapp_account: WhatsApp account used
    - doc.name: message ID
    """
    phone = doc.get("from") or doc.from_

    # Your custom logic here
    frappe.get_doc({
        "doctype": "Support Ticket",
        "phone": phone,
        "subject": f"WhatsApp escalation from {phone}"
    }).insert()

    # Return response text or dict
    return "You've been connected to our support team. An agent will respond shortly."
```

**Using Server Script (API type):**

1. Create a Server Script with Script Type = "API"
2. Set the API Method name (e.g., `handle_whatsapp`)
3. In the script, access the message via `frappe.form_dict.doc`

```python
# Server Script
doc = frappe.form_dict.doc
phone = doc.get("from") or doc.get("from_")
frappe.response["message"] = f"Hello {phone}, how can I help?"
```

### Advanced Settings

| Field | Description |
|-------|-------------|
| **Conditions** | Python expression for additional filtering |
| **Active From** | Rule active from this date/time |
| **Active Until** | Rule active until this date/time |

#### Conditions Example

Only match if message is longer than 10 characters:
```python
len(message) > 10
```

## Examples

### Basic Greeting

```
Title: Greeting
Keywords: hello, hi, hey, hola
Match Type: Exact
Response Type: Text
Response Text: Hello! Welcome to ABC Company. How can I help you?
```

### Business Hours

```
Title: Business Hours
Keywords: hours, timing, open, close, when
Match Type: Contains
Response Type: Text
Response Text: We're open Monday-Friday, 9 AM to 6 PM IST.
```

### Price Inquiry with Regex

```
Title: Price Inquiry
Keywords: price.*product|cost.*item
Match Type: Regex
Response Type: Text
Response Text: Check our pricing at https://example.com/pricing
```

### Promotional (Time-Limited)

```
Title: Holiday Sale
Keywords: sale, discount, offer
Match Type: Contains
Response Type: Text
Response Text: 🎉 Holiday Sale! Get 20% off with code HOLIDAY20
Active From: 2024-12-20 00:00
Active Until: 2024-12-31 23:59
```

### Agent Escalation (Script)

```
Title: Escalate to Agent
Keywords: agent, human, help, support
Match Type: Contains
Response Type: Script
Script: myapp.api.escalate_to_agent
```
