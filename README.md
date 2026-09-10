# Outlook Follow-Up Checker for Mac

A lightweight Outlook add-in for macOS that scans recent Sent Items and helps identify messages that still need follow-up.

## What it does

- Scans recent Sent Items
- Detects later replies in the same conversation
- Shows who replied
- Filters by:
  - Needs follow-up
  - High importance
  - Flagged
- Searches by subject, recipient, or responder
- Opens the sent message or matching reply in Outlook on the web
- Exports the visible results to CSV

The add-in intentionally requests **delegated `Mail.ReadBasic`** access only. It does not request message bodies or attachments, does not use OpenAI, and does not use a third-party backend or database.

## Hosting

This repository is designed to be hosted with GitHub Pages at:

`https://alaadika.github.io/outlook-followup/`

The Outlook add-in manifest is:

`https://alaadika.github.io/outlook-followup/manifest.xml`

## 1. Enable GitHub Pages

In this repository, open **Settings > Pages** and publish from:

- Branch: `main`
- Folder: `/ (root)`

After GitHub Pages finishes publishing, open:

`https://alaadika.github.io/outlook-followup/`

## 2. Create the Microsoft Entra app registration

Go to Microsoft Entra admin center / App registrations and create a new registration.

Recommended settings:

- Name: `Outlook Follow-Up Checker`
- Supported account types: your organization only, or multitenant if you want to use multiple Microsoft 365 tenants
- Platform: **Single-page application (SPA)**
- Redirect URI:
  `https://alaadika.github.io/outlook-followup/auth.html`

Under **API permissions**, add delegated Microsoft Graph permissions:

- `Mail.ReadBasic`
- `User.Read`

No client secret is required for this browser-based SPA flow.

Copy the **Application (client) ID**. You enter it inside the add-in on first use. It is stored only in the browser storage for this GitHub Pages origin.

## 3. Add the manifest to Outlook

Use Outlook's Custom Add-ins flow and upload `manifest.xml` from this repository, or download the file first and upload it manually.

On current Outlook clients, Microsoft's sideloading page can be opened at:

`https://aka.ms/olksideload`

Then choose **My add-ins > Custom Addins > Add from File** and select `manifest.xml`.

## Security design

- Delegated sign-in only
- `Mail.ReadBasic` rather than `Mail.Read` or `Mail.ReadWrite`
- No client secret in GitHub
- Access token kept in memory only
- No message bodies
- No attachments
- No telemetry
- No analytics
- No third-party database
- No OpenAI/API integration

## Notes

Reply detection is based on Outlook conversation IDs and messages received after each sent message. This is a practical approximation and can occasionally differ from Outlook's native conversation interpretation in unusual forwarding/delegation scenarios.

The **Open Sent** and **Open Reply** actions use Microsoft Graph's `webLink`, so they open the item in Outlook on the web rather than forcing the native Mac Outlook window.
