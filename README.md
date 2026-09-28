# Teams Recording Automation

## 🚀 Project Overview

Built an automated Microsoft Teams recording management solution using **Power Automate, SharePoint Online, and Microsoft Teams**.

The solution automatically organizes Knowledge Exchange meeting recordings into structured SharePoint folders for long-term storage.

## 🧩 Business Problem

Knowledge Exchange sessions generate Teams recordings that are initially stored in the channel's **Recordings** folder and are subject to the organization's recording expiration policy.

Manually organizing and archiving each recording would require repetitive administrative work.

## 🛠️ Solution Architecture

**Microsoft Teams → SharePoint Online → Power Automate → Structured SharePoint Archive**

- Teams creates the meeting recording.
- SharePoint stores the recording in the channel's Recordings folder.
- Power Automate detects the new recording.
- A session-specific archive folder is created.
- The recording is automatically moved to the archive.

## ⚙️ Workflow Process

1. Detect a new recording in the Knowledge Exchange Recordings folder.
2. Extract the recording date and company name from file information.
3. Create a folder using the format:
   `yyyy-MM-dd - Company`
4. Move the recording into the newly created folder.

Example:

`Knowledge Exchange Presentations / Toronto Sessions / 2026-09-28 - ABC`

## 📊 Key Features

- Automatic Teams recording detection
- Dynamic folder creation
- Dynamic date and company-name extraction
- Automatic SharePoint file movement
- Consistent recording archive structure
- Reduces manual file-management tasks

## 🎯 Skills Demonstrated

- Microsoft Power Automate
- Microsoft Teams administration
- SharePoint Online
- Dynamic expressions
- File and folder automation
- Microsoft 365 workflow design

## 📚 Lessons Learned

- Teams channel recordings are stored in SharePoint.
- Teams recording expiration is controlled through meeting policies.
- Moving a recording from its original location removes it from the original Teams expiration behavior.
- Power Automate expressions can dynamically generate folder names from file metadata.
- User-based Teams meeting policies affect all applicable meetings organized by that user, not a specific SharePoint folder or channel.
