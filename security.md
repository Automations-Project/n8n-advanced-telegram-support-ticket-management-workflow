For Security Reasons we will summuray the n8n template code into text as follow:
**Workflow Summary:**
**Additional Processing:**
- **Sticky Note4** (n8n-nodes-base.stickyNote)
  Details:
    - **Content**:
```plaintext
## Channel Side (Broadcasting)
**This Part** where the support of brand broadcasting message to all previous users who used this bot before.
``` 
- **Telegram-Bot** (n8n-nodes-base.telegramTrigger)
- **Bot-Config** (n8n-nodes-base.set)
- **Sticky Note3** (n8n-nodes-base.stickyNote)
  Details:
    - **Content**:
```plaintext
## User Side
**This Part** is meant to save user data on a RAM database which is fast, and in same time forward the message to support after creating a new ticket (Topic) dedciated for the user id in the support group.
``` 
- **Sticky Note2** (n8n-nodes-base.stickyNote)
  Details:
    - **Content**:
```plaintext
## Support Side
**This Part** is meant to forward replies that sent by support (members in the group)
``` 
- **Sticky Note1** (n8n-nodes-base.stickyNote)
  Details:
    - **Content**:
```plaintext
## Re Create New Topic
**Sometimes** in support group may the team delete or close a ticket (topic) in case of that this steps will create topic again for the user, and store the new ticket id (topic/thread ID).
``` 
- **Format** (n8n-nodes-base.code)
- **Bot-Fields** (n8n-nodes-base.set)
- **1st** (n8n-nodes-base.switch)
**Error Handling:**
- **IF Verified Channel** (n8n-nodes-base.if)
**Additional Processing:**
- **Check User in Database1** (n8n-nodes-base.redis)
  Details:
    - **Notes**: ```plaintext
Search Key``` 
- **Format Users** (n8n-nodes-base.code)
- **Filter Blocked Users** (n8n-nodes-base.filter)
- **Split In Batches1** (n8n-nodes-base.splitInBatches)
  Details:
    - **Notes**: ```plaintext
Telegram Limitation 29/sec``` 

**Data Fetching and Processing:**
- **Broadcast Channel Post into Users** (n8n-nodes-base.httpRequest)
**Additional Processing:**
- **Set Blocked Member** (n8n-nodes-base.redis)
- **Wait1** (n8n-nodes-base.wait)
**Error Handling:**
- **Support Forum** (n8n-nodes-base.if)
- **From Ticket** (n8n-nodes-base.if)
- **IF Topic Created** (n8n-nodes-base.if)
**Additional Processing:**
- **Send User Ticket Created Notification** (n8n-nodes-base.telegram)
**Data Fetching and Processing:**
- **Forward Support Reply To User** (n8n-nodes-base.httpRequest)
**Additional Processing:**
- **Check User in Database** (n8n-nodes-base.redis)
  Details:
    - **Notes**: ```plaintext
Search Key``` 

**Error Handling:**
- **New User ?** (n8n-nodes-base.if)
**Additional Processing:**
- **Update User Data** (n8n-nodes-base.redis)
- **Get User Chat Topic** (n8n-nodes-base.redis)
- **Save User Data** (n8n-nodes-base.redis)
**Data Fetching and Processing:**
- **Create Topic (Chat Ticket)** (n8n-nodes-base.httpRequest)
**Additional Processing:**
- **Save Topic ID** (n8n-nodes-base.redis)
**Data Fetching and Processing:**
- **Forward New Message** (n8n-nodes-base.httpRequest)
**Error Handling:**
- **IF No Topic Created** (n8n-nodes-base.if)
**Data Fetching and Processing:**
- **ReCreate Topic (Chat Ticket)** (n8n-nodes-base.httpRequest)
**Additional Processing:**
- **ReSave Topic ID** (n8n-nodes-base.redis)
**Data Fetching and Processing:**
- **Forward New Message to the recrated topic** (n8n-nodes-base.httpRequest)
**Additional Processing:**
- **No Operation, do nothing** (n8n-nodes-base.noOp)
- **Sticky Note** (n8n-nodes-base.stickyNote)
  Details:
    - **Content**:
```plaintext
## Use **Config Bot** to setup your telegram details, like:
1- Telegram Group ID (Don't forget add bot as admin)
2- Telegram Channel ID (Don't forget add bot as admin)
3- Your telegram Bot Token. (Generate through @BotFather)










## Setup data & filter & route to the correct Side.
0- None of them - Soon - Wait V2
1- Chat Type (`Private`)
2- Chat Type (`Supergroup`)
3- Chat Type (`Channel`)















## Remember:
* Do not make your support group public. Every message sent in the group on various topics will be forwarded to the user's ticket.
* There is no need to promote your broadcasting channel; the main reason for the channel is to organize and broadcast messages.
* You can host a Redis database without any coding/server management skills through Coolify.io.
* In the next version, I will add the **edit messages** feature, where the forwarded messages will be updated with the new edited one.
## Why use this method?
* If you deal with Telegram P2P, anyone can delete messages from both sides. If you run a business, then one of your clients may delete all messages, causing you to lose the history. This solution prevents people from deleting messages; every message forwarded into the support group will not be possible to delete by the sender.
* Team collaboration: Why share one account when you can convert the whole group into a ticketing system? With this project, you can invite all your coworkers to reply and provide support to your clients through Telegram.
* Integrate with third-party services? Using N8N will pave the way for integrating your Telegram users' data into a CRM. In V2, we will enable the option to force new users to share their leads before receiving support.
``` 
