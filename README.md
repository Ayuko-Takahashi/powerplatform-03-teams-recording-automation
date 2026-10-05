# Microsoft Teams Recording Automation

## 🚀 Project Overview

This project automates the management and archival of recurring Microsoft Teams Knowledge Exchange meeting recordings using Microsoft Teams, OneDrive, SharePoint, and Power Automate.

Knowledge Exchange sessions are held approximately twice per week, and recordings typically range from 120 MB to 350 MB.

The solution automatically identifies Knowledge Exchange recordings saved in the meeting organizer's OneDrive, creates a structured archive folder in SharePoint based on the meeting date and company name, and copies the recording to the appropriate folder.

---

## 🧩 Business Problem

The HR and Marketing teams host recurring Knowledge Exchange sessions in Microsoft Teams.

The sessions may include:

- Corporate Communications team members
- Internal employees who are not members of the Team
- External presenters or clients

The original solution used a Teams channel meeting. However, this created an access limitation because users who were not members of the Team could not access the channel meeting chat.

Adding every attendee or external presenter as a Team member was not appropriate because they should not automatically gain access to the Corporate Communications Team and its SharePoint content.

The meeting architecture was therefore changed from a Teams channel meeting to a regular Teams calendar meeting.

With a regular Teams meeting, invited attendees can participate in the meeting and meeting chat without needing to become members of the Corporate Communications Team.

---

## 🔄 Architecture Change

### Original Architecture

Teams Channel Meeting  
↓  
Teams Channel SharePoint Recordings Folder  
↓  
Power Automate  
↓  
Structured SharePoint Archive

### Limitation

Channel meetings did not meet the business requirement because meeting chat access was restricted for attendees who were not members of the Team.

### Revised Architecture

Regular Teams Calendar Meeting  
↓  
Organizer's OneDrive `/Recordings`  
↓  
Power Automate  
↓  
Filter Knowledge Exchange Recordings  
↓  
Corporate Communications SharePoint Archive

This separates meeting participation from access to the internal Corporate Communications Team and SharePoint site.

---

## ⚙️ Workflow Process

### 1. Recording Created

A regular Teams meeting is recorded.

Microsoft Teams automatically saves the recording to the meeting organizer's OneDrive:

`OneDrive → Recordings`

### 2. Power Automate Trigger

A Power Automate flow monitors the organizer's OneDrive `/Recordings` folder using:

`When a file is created`

Because this folder may contain recordings from other meetings, the flow must identify only Knowledge Exchange recordings.

### 3. Decode Recording Filename

The OneDrive trigger returns the recording filename in an encoded format.

A Compose action decodes the filename so it can be evaluated by the flow.

Example decoded filename:

`Learning _ Develop_ABC-20261002_175146UTC-Meeting Recording.mp4`

### 4. Filter Knowledge Exchange Recordings

A Condition checks whether the decoded filename begins with the Knowledge Exchange naming convention:

`Learning _ Develop_`

If True:

→ Continue the archival process

If False:

→ Take no action

This prevents unrelated Teams recordings in the organizer's OneDrive from being copied to the Corporate Communications archive.

### 5. Extract Company Name

Power Automate extracts the company name from the recording filename.

Example:

`Learning _ Develop_ABC-20261002...`

Company:

`ABC`

### 6. Create Archive Folder

Power Automate creates a structured SharePoint folder using the meeting date and company name.

Example:

`2026-10-02 - ABC`

Archive structure:

`Corporate Communications`
→ `Documents`
→ `Knowledge Exchange Presentations`
→ `Toronto Sessions`
→ `2026-10-02 - ABC`

### 7. Archive Recording

The recording is copied from the organizer's OneDrive into the newly created SharePoint folder.

This provides a centralized internal archive while allowing the meeting itself to remain accessible to invited attendees without granting them access to the Corporate Communications Team.

---

## 📊 Key Features

- Automatically detects new Teams recordings in OneDrive
- Filters recordings based on the Knowledge Exchange naming convention
- Prevents unrelated meeting recordings from being archived
- Decodes OneDrive filename information for processing
- Extracts the company name automatically
- Generates date-based SharePoint folders
- Copies recordings into a centralized Corporate Communications archive
- Separates meeting attendee access from internal Team/SharePoint access
- Reduces manual recording management

---

## 💾 Storage Considerations

Knowledge Exchange recordings are approximately 120–350 MB and sessions occur roughly twice per week.

Azure Blob Storage was considered as an alternative for long-term archival because lower-cost storage tiers such as Cool Storage can be suitable for recordings that are rarely accessed.

For the current scenario, however, the organization still has plenty of unused SharePoint storage capacity already included with its Microsoft 365 environment. Moving the recordings to Azure Blob Storage would therefore not necessarily provide immediate cost savings.

In addition, recording usage should be analyzed to determine which recordings actually require long-term retention rather than automatically archiving every recording indefinitely.

### Future Consideration

As recording volume and SharePoint storage consumption increase, the architecture could be extended to use:

`OneDrive → Power Automate → Azure Blob Storage`

with Azure storage lifecycle policies used to move older, infrequently accessed recordings to lower-cost storage tiers.

This decision should be based on:

- SharePoint tenant storage utilization
- Recording growth rate
- Recording viewing activity
- Business retention requirements
- Retrieval requirements
- Azure storage and retrieval costs

---

## 🎯 Skills Demonstrated

- Microsoft Teams meeting architecture
- Microsoft OneDrive for Business
- Microsoft SharePoint Online
- Microsoft Power Automate
- Conditional workflow logic
- File naming and string manipulation
- Base64 filename decoding
- Automated folder creation
- Cross-service file management
- Microsoft 365 permissions and access considerations
- Storage lifecycle planning
- Business requirement analysis
- Solution architecture redesign

---

## 📚 Lessons Learned

This project demonstrated that a technically working automation does not necessarily mean the overall architecture meets the business requirements.

The original Teams channel-based design successfully automated recording management, but testing identified an important meeting chat limitation for attendees who were not Team members.

The solution was redesigned to use regular Teams meetings and the organizer's OneDrive while keeping the final recording archive within the internal Corporate Communications SharePoint environment.

The project also highlighted the importance of considering storage growth and access patterns. Azure Blob Storage may become a better long-term archival solution as data grows, but moving data to another storage platform does not automatically reduce costs when sufficient SharePoint capacity is already included in the organization's Microsoft 365 licensing.

The final design therefore balances accessibility, permissions, automation, storage usage, and future scalability.
