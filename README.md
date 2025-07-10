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
      "firstName": "{{contact.first_name}}",
      "lastName": "{{contact.last_name}}",
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
- `your-phone-number-id` → located at **Phone Number** in the sidebar
- Make sure `{{contact.phone}}` matches your contact data format (10-digit U.S. numbers only)

## End-of-call Report: Data Capture

### Structured Data: `assistantOverrides.variableValues`

| Name               | Description                                                                                      |
| ------------------ | ------------------------------------------------------------------------------------------------ |
| firstName          | Either the provided {{firstName}} if the caller confirms it or the caller's first name.          |
| lastName           | Either the provided {{lastName}} if the caller confirms it or the caller's last name.            |
| phone              | Either the provided {{phone}} if the caller confirms it or the caller's phone number.            |
| streetAddress      | Either the provided {{streetAddress}} if the caller confirms it or the caller's street address.  |
| city               | Either the provided {{city}} if the caller confirms it or the caller's city.                     |
| state              | Either the provided {{state}} if the caller confirms it or the caller's state.                   |
| zip                | Caller's provided zip code.                                                                      |
| inquiryReason      | Caller's stated reason for the call or inquiry.                                                  |
| isQualified        | True if the caller showed HVAC intent or wasn’t disqualified. False if clearly not an HVAC lead. |
| callAnswered       | True if a real person answered the call (excludes voicemail or no pickup).                       |
| callCompleted      | True if the call ran to completion (not cut off or dropped early).                               |
| preferredDayOWeek  | Caller's preferred appointment day of the week.                                                  |
| preferredTimeOfDay | Caller's preferred appointment time (morning/afternoon).                                         |

### Reading Incoming Webhook Data

To learn more: https://docs.vapi.ai/quickstart/introduction
