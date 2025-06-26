# VAPI

This guide walks through how to trigger an outbound voice call using Vapi via a custom webhook inside a GoHighLevel (GHL) workflow.

## Pre-requisites

Vapi account with:

- Assistant already created
- Phone number added (Twilio preferred for production)
- GoHighLevel workflow with target contacts

## Outbound: Step-by-Step Instructions

### Step #1: Create or Open a GHL Workflow

- Go to Workflows in GoHighLevel
- Click + Create New Workflow or open an existing one
- Add a trigger (e.g., Tag Added, Pipeline Stage Changed, etc.)

### Step #2: Add a Webhook Step

- Inside the workflow, click + Add Action
- Select Custom Webhook

### Step #3: Configure Webhook Settings

- Event: Custom
- Method: POST
- URL: `https://api.vapi.ai/call`
- Authorization: Bearer Token
- Token: `YOUR_VAPI_API_KEY`
  - Located at **VAPI API Keys** in the sidebar
- Content-Type: `application/json`

### Step #4: Paste Webhook Body

In the Request Body (Raw) section, paste this:

```
{
  "assistantId": "your-assistant-id",
  "assistantOverrides": {
    "variableValues": {
      "first_name": "{{contact.first_name}}",
      "last_name": "{{contact.last_name}}",
      "phone": "+1{{contact.phone}}"
    }
  },
  "phoneNumberId": "your-phone-number-id",
  "customer": {
    "number": "+1{{contact.phone}}"
  }
}
```

Replace:

- `your-assistant-id` → located at **Assistants** in the sidebar
- `your-phone-number-id` → located at**Phone Number** in the sidebar
- Make sure `{{contact.phone}}` matches your contact data format (10-digit U.S. numbers only)
