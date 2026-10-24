# Support Request Notifier

A simple n8n workflow that lets customers or staff submit a support request through a web form. Urgent requests are posted to a Discord channel right away. Non-urgent requests are recorded but do not trigger an alert.

## Architecture

```
Form Trigger (Submit a Support Request)
   → Is it urgent? (Is this Urgent? = yes)
        ├── Yes → Send a message (Discord #support-alerts)
        └── No  → Not urgent (no action)
```

## Features

* Public form with two fields: issue description (required) and urgency (yes or no, required)
* Urgent requests are posted to a Discord channel with the issue text
* Non-urgent requests are not posted, so the channel only shows alerts that need attention
* Form is served by n8n, so no separate frontend is needed

## Tech Stack

* n8n 2.x (workflow automation, n8n Form Trigger)
* Discord Bot API (alert channel)

## Setup

1. Create a Discord server (or use an existing one) and add a channel named `support-alerts`.
2. Create a Discord bot in the Developer Portal. Invite it to your server with permission to send messages in that channel.
3. Import `support_request_notifier.json` into n8n.
4. Add a Discord Bot credential with your bot token. Assign it to the Send a message node.
5. In the Send a message node, select your server and the `support-alerts` channel. Replace `YOUR_DISCORD_SERVER_ID` and `YOUR_DISCORD_CHANNEL_ID` if they are still shown.
6. Activate the workflow. Open the form URL from the Form Trigger node and submit a test request marked "yes".

## Sample Submission

```
Describe the issue: Customer's AC is leaking water and the unit is off.
Is this Urgent?:    yes
```

This posts a message to `support-alerts`. A submission marked "no" is not posted.

## Project Notes

* Built as part of my AI Automation portfolio.
* The "Not urgent" branch is intentionally empty. It keeps the routing visible in the workflow and leaves room for a future step, such as logging to a sheet.
