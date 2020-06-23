---
title: "Learn about retention for Exchange"
f1.keywords:
- NOCSH
ms.author: cabailey
author: cabailey
manager: laurawi
ms.date: 
audience: Admin
ms.topic: conceptual
ms.service: O365-seccomp
localization_priority: Priority
ms.collection: 
- M365-security-compliance
- SPO_Content
search.appverid: 
- MOE150
- MET150
description: "Learn about retention behavior that applies specifically to Exchange email and Exchange public folders."
---

# Learn about retention policies for Exchange

The information in this article supplements [Learn about retention policies](retention-policies.md) because it has information that's specific to Exchange.

## How a retention policy works with Exchange locations

For a user's mail, calendar, and other items, a retention policy is applied at the level of a mailbox.

For a public folder, a retention policy is applied at the folder level, not the mailbox level. 

Both a mailbox and a public folder use the Recoverable Items (located in Deleted Items folder) folder to retain items. Only people who have been assigned eDiscovery permissions can view items in another user's Recoverable Items folder.
  
When a person deletes a message in a folder other than the Deleted Items folder, by default, the message moves to the Deleted Items folder. When a person deletes an item in the Deleted Items folder, the message is moved to the Recoverable Items folder. However, a user can soft-delete an item (SHIFT+DELETE) in any folder, which bypasses the Deleted Items folder and moves the item directly to the Recoverable Items folder.
  
When you apply a retention policy to an Exchange location, a timer job periodically evaluates items in the Recoverable Items folder. If an item doesn't match the rules of at least one retention policy, the item is permanently deleted (also called hard deleted) from the Recoverable Items folder.

The timer job can take up to seven days to run and the Exchange location must contain at least 10 MB.
  
When a user attempts to change properties of a mailbox item—such as the subject, body, attachments, senders and recipients, or date sent or received for a message—a copy of the original item is saved to the Recoverable Items folder before the change is committed. This action happens for each subsequent change. At the end of the retention period, copies in the Recoverable Items folder are permanently deleted.

After a retention policy is assigned to a mailbox or public folder, the paths the content takes depend on whether the retention settings are to retain and delete, to retain only, or delete only.

When the retention settings are to retain and delete:

![Diagram of retention flow in email and public folders](../media/88f174cc-bbf4-4305-93d7-0515f496c8f9.png)

1. **If the item is modified or permanently deleted** by the user (either SHIFT+DELETE or deleted from Deleted Items) during the retention period: The item is moved (or copied, in the case of edit) to the Recoverable Items folder. There, a timer job runs periodically and identifies items whose retention period has expired, and these items are permanently deleted within 14 days of the end of the retention period. Note that 14 days is the default setting, but it can be configured up to 30 days.
    
2. **If the item is not modified or deleted** during the retention period: The same process runs periodically on all folders in the mailbox and identifies items whose retention period has expired, and these items are permanently deleted within 14 days of the end of the retention period. Note that 14 days is the default setting, but it can be configured up to 30 days. 

When the retention settings are retain-only, or delete-only, the contents paths are variations of retain and delete:

### Content paths for retain-only retention settings

1. **If the item is modified or deleted** during the retention period: A copy of the original item is created in the Recoverable Items folder and retained until the end of the retention period, when the copy in the Recoverable Items folder is permanently deleted within 14 days after the item expires. 

2. **If the item is not modified or deleted** during the retention period: Nothing happens before and after the retention period; the item remains in its original location.

### Content paths for delete-only retention settings

1. **If the item is not deleted** during the configured period: At the end of the configured period in the retention policy, the item is moved to the Recoverable items folder. 

2. **If the item is deleted** during the configured period: The item is immediately moved to the Recoverable Items folder. If a user deletes the item from there or empties the Recoverable items folder, the item is permanently deleted. Otherwise, the item is permanently deleted after being in the Recoverable items folder for 14 days. 

## Excluding specific types of Exchange items from a retention policy

By using PowerShell, you can exclude specific types of Exchange items from a retention policy. For example, you can exclude voicemail messages, IM conversations, and other Skype for Business Online content in mailboxes. You can also exclude calendar, note, and task items. This capability is available only by using PowerShell; it's not available when you create a retention policy by using the wizard in the Microsoft 365 compliance center.
  
To exclude your selected types for Exchange items in a retention policy, use the  `ExcludedItemClasses` parameter with the  `New-RetentionComplianceRule` and  `Set-RetentionComplianceRule` cmdlets.
Link to https://docs.microsoft.com/en-us/powershell/module/exchange/new-retentioncompliancerule?view=exchange-ps


### When a user leaves the organization 

If a user leaves your organization and the user's mailbox is included in a retention policy, the mailbox becomes an inactive mailbox when the user's Microsoft 365 account is deleted. The contents of an inactive mailbox are still subject to any retention policy that was placed on the mailbox before it was made inactive, and the contents are available to an eDiscovery search. For more information, see [Inactive mailboxes in Exchange Online](inactive-mailboxes-in-office-365.md). 

## How to configure a retention policy for Exchange

See [Create and configure retention policies](create-retention-policies.md).

For the **Choose locations** page of the wizard, select one of the following options:

- **Apply policy only to content in Exchange email, public folders, Office 365 groups, OneDrive and SharePoint documents**

- **Let me choose specific locations** > **Exchange email**, **Exchange public folders**, and **Office 365 groups**.

Even though a Microsoft 365 group has an Exchange mailbox, a retention policy that includes the entire **Exchange email** location won't include content in Microsoft 365 group mailboxes. To retain content in these mailboxes, select the **Office 365 groups** location.
Suggest adding screenshots of the policy fly out to help show what 'location' and 'status' button need to be activated here otherwise this reads as if you alrady know everything this is to knoe about rentention polices in Exchange.
