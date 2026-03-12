# chat-it — Product Requirements Document

> **Version:** 1.0.0 | **Date:** March 2025 | **Platform:** iOS & Android (Expo React Native) | **Status:** Draft — Ready for Engineering Review

---

## Table of Contents

1. [Product Overview](#1-product-overview)
2. [Technical Architecture](#2-technical-architecture)
3. [Authentication & Onboarding](#3-authentication--onboarding)
4. [Real-Time Chat](#4-real-time-chat)
5. [Announcement & Alarm System](#5-announcement--alarm-system)
6. [Voice Messages](#6-voice-messages)
7. [Status Feature](#7-status-feature)
8. [Contacts Integration](#8-contacts-integration)
9. [Native Call Integration](#9-native-call-integration)
10. [Notifications](#10-notifications)
11. [Screen Inventory & Navigation](#11-screen-inventory--navigation)
12. [Data Models](#12-data-models)
13. [Key npm Packages](#13-key-npm-packages)
14. [Non-Functional Requirements](#14-non-functional-requirements)
15. [MVP Scope vs Future Roadmap](#15-mvp-scope-vs-future-roadmap)
16. [Risks & Mitigations](#16-risks--mitigations)
17. [Open Questions](#17-open-questions)
18. [Document Sign-Off](#18-document-sign-off)

---

## 1. Product Overview

### 1.1 Executive Summary

**chat-it** is a cross-platform real-time messaging application built using Expo React Native. It provides individuals and groups with a modern, feature-rich communication platform combining instant messaging, voice messages, timed group announcements with alarm delivery, ephemeral status updates, and native call integration.

The app targets both iOS and Android users and is bootstrapped via:

```bash
npx create-expo-app chat-it
cd chat-it
```

### 1.2 Vision & Goals

- Deliver a WhatsApp/Telegram-class messaging experience with a unique **timed announcement alarm** system
- Enable seamless group coordination through scheduled, alarm-backed announcements that fire at the set time **and** 5 minutes before
- Provide voice messaging, typing indicators, and rich status features
- Leverage the device's native phone dialer — **no VoIP infrastructure required**
- Offer frictionless onboarding via **Email/Password** or **Google OAuth**
- Import contacts directly from the device phonebook for fast friend discovery

### 1.3 Target Audience

| User Segment | Use Case | Key Need |
|---|---|---|
| Students / Study Groups | Coordinate timed sessions, share notes | Announcement alarms, group chat |
| Professional Teams | Daily standups, project updates | Scheduled announcements, status |
| Communities & Clubs | Events, reminders, updates | Group announcements, voice messages |
| Family Groups | Stay connected, share moments | Easy contacts import, status updates |
| General Consumer | Day-to-day messaging | Fast, reliable real-time chat |

---

## 2. Technical Architecture

### 2.1 Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| Frontend Framework | Expo SDK 51+ / React Native | Cross-platform iOS & Android |
| Language | TypeScript | Type safety, maintainability |
| Navigation | Expo Router (file-based) | App navigation & deep linking |
| State Management | Zustand + React Query | Global state & server cache |
| Real-time Backend | Firebase Firestore + RTDB | Live message sync & presence |
| Authentication | Firebase Auth | Email/Password & Google OAuth |
| File / Media Storage | Firebase Storage | Voice messages, images, files |
| Push Notifications | Expo Notifications + FCM/APNs | Announcement alarms & alerts |
| Scheduled Alarms | expo-task-manager + expo-background-fetch | Background alarm triggers |
| Contacts | expo-contacts | Import mobile phonebook contacts |
| Phone Calls | expo-linking / Linking.openURL | Trigger native dialer |
| Audio Recording | expo-av | Voice message record & playback |
| Local Storage | expo-secure-store / AsyncStorage | Auth tokens, draft messages |
| UI Components | React Native Paper / custom | Consistent design system |

### 2.2 Project Initialization & Folder Structure

```bash
npx create-expo-app chat-it --template tabs
cd chat-it
```

**Recommended folder structure:**

```
chat-it/
├── app/                          # Expo Router screens
│   ├── (auth)/
│   │   ├── login.tsx             # Login — Email + Google OAuth
│   │   ├── register.tsx          # New account creation
│   │   └── setup.tsx             # Profile setup (avatar, name, phone)
│   ├── (tabs)/
│   │   ├── chats.tsx             # Chat list
│   │   ├── contacts.tsx          # Contacts screen
│   │   ├── status.tsx            # Status feed
│   │   ├── calls.tsx             # Call log
│   │   └── profile.tsx           # User profile & settings
│   ├── chat/
│   │   └── [id].tsx              # 1-on-1 chat screen
│   ├── group/
│   │   ├── [id].tsx              # Group chat screen
│   │   └── [id]/info.tsx         # Group info, members, media
│   ├── announcement/
│   │   └── [id].tsx              # Announcement detail + RSVP
│   ├── status/
│   │   ├── [userId].tsx          # Full-screen status viewer
│   │   └── create.tsx            # Create status
│   ├── contact/
│   │   └── [id].tsx              # Contact profile
│   ├── new-chat.tsx              # Start DM or create group
│   ├── create-group.tsx          # New group flow
│   ├── media/
│   │   └── [id].tsx              # Full-screen media viewer
│   └── settings.tsx              # App settings
├── components/                   # Shared UI components
│   ├── chat/                     # MessageBubble, VoicePlayer, TypingIndicator
│   ├── status/                   # StatusRing, StatusViewer
│   ├── announcement/             # AnnouncementBubble, AlarmCard
│   └── ui/                       # Buttons, Inputs, Avatars, Modals
├── hooks/                        # Custom React hooks
├── services/                     # Firebase, contacts, audio, notifications
├── stores/                       # Zustand state stores
├── utils/                        # Helpers, formatters, constants
└── constants/                    # Colors, config, TypeScript types
```

---

## 3. Authentication & Onboarding

### 3.1 Login Methods

#### 3.1.1 Email & Password

- User enters email address and password on the login screen
- Firebase Auth handles credential validation and session tokens
- Password reset flow via Firebase email reset link
- New users register with name, email, password, and optional profile photo
- Email verification required before accessing the main app

#### 3.1.2 Google OAuth (Sign in with Google)

- Integrated via `expo-auth-session` + Firebase Google provider
- One-tap Google sign-in on both iOS and Android
- Auto-populates display name and profile picture from the Google account
- Subsequent logins re-use the stored credential silently

### 3.2 Onboarding Flow

1. Splash screen → Choose **Login** or **Register**
2. **Login:** Enter credentials or tap "Sign in with Google"
3. **Register:** Enter name, email, password → verify email → proceed to profile setup
4. **Profile setup:** Upload avatar, set display name, optionally add phone number
5. **Contacts permission prompt:** Import contacts from phonebook (skippable)
6. Redirect to main **Chat List** screen

### 3.3 Session Management

- Firebase Auth persists sessions across app restarts via `AsyncStorage`
- Automatic token refresh in the background
- Logout clears all local state and navigates to the auth screen

---

## 4. Real-Time Chat

### 4.1 Chat List Screen

- Displays all active conversations (1-on-1 and groups) sorted by latest message timestamp
- Each row: avatar, name, last message preview, timestamp, unread badge count
- Swipe-left actions: **Mute**, **Archive**, **Delete**
- Search bar to filter conversations by name or message content
- FAB (Floating Action Button) to start a new chat or create a group

### 4.2 One-on-One Chat Screen

#### 4.2.1 Message Types Supported

| Message Type | Description | Implementation |
|---|---|---|
| Text | Plain text up to 4,096 characters | Firestore document |
| Voice Message | Recorded audio clips | Firebase Storage + expo-av |
| Image | Camera or gallery photos | Firebase Storage + expo-image-picker |
| File / Document | PDFs, docs, etc. | Firebase Storage |
| Emoji / Reactions | Inline emoji and message reactions | Unicode + Firestore field |
| Reply / Quote | Reply to a specific message | Firestore reference field |
| Link Preview | Auto-generated URL preview cards | Open Graph metadata fetch |

#### 4.2.2 Message States

| State | Indicator |
|---|---|
| Sending | Clock icon (optimistic UI) |
| Sent | Single grey tick ✓ |
| Delivered | Double grey tick ✓✓ |
| Read | Double blue tick ✓✓ |

#### 4.2.3 Typing Indicator

- When User A starts typing, a real-time presence flag is written to Firebase RTDB at `/typing/{chatId}/{userId}`
- User B observes this path and shows an animated **"typing..."** indicator (three-dot animation)
- The flag is cleared automatically after **3 seconds of inactivity** or when the message is sent
- In group chats: shows names of up to 3 users, e.g., *"Alice, Bob are typing..."*

### 4.3 Group Chat

- Create groups with a name, icon, and description
- Add members from contacts or by searching by `@username`
- **Admin roles:** Owner → Admin → Member (tiered permissions)
- Group info screen: member list, media gallery, shared files
- Admins can remove members, change group icon/name, restrict who can send messages
- Mention users with `@username` — triggers a high-priority notification
- Pin important messages visible at the top of the chat

---

## 5. Announcement & Alarm System

> ⭐ **Key Differentiating Feature**
>
> The announcement system transforms group messages into **scheduled alarms**. When a group admin posts an announcement with a target time, every group member's device fires a local alarm at both the scheduled time **AND** 5 minutes before. This makes chat-it uniquely effective for coordinating time-critical group events.

### 5.1 Creating an Announcement

1. Admin taps the **megaphone icon** in the group chat toolbar
2. Fills in the Announcement Composer:
   - **Title** (required, max 100 chars)
   - **Body / Message** (required, max 2,000 chars)
   - **Scheduled Date & Time** (required — future datetime picker)
   - **Pre-alarm lead time** (default: 5 minutes, configurable: 1–60 min)
   - **Repeat:** None / Daily / Weekly / Custom
   - **Importance:** Normal / Urgent *(Urgent overrides DND settings)*
3. Admin taps **"Schedule Announcement"** — document saved to Firestore
4. All group members receive a silent confirmation: *"New announcement scheduled"*

### 5.2 Alarm Delivery Architecture

| Event | Trigger | Notification Type | Behavior |
|---|---|---|---|
| 5-min warning | Scheduled time − 5 min | Push + Local Alarm | Sound, vibration, banner — "[Group] starts in 5 minutes" |
| Exact time alarm | Scheduled datetime | Push + Local Alarm | Full-screen alert, alarm sound until dismissed |
| Missed alarm | User opens app after event | In-app banner | "You missed: [Announcement Title]" |
| Cancelled alarm | Admin cancels before trigger | Silent push | Cancel pending local notification |

#### 5.2.1 Technical Implementation

- Announcements stored in Firestore: `groups/{groupId}/announcements/{announcementId}`
- **Firebase Cloud Functions** listen for new announcement documents and fan-out FCM messages to all group member device tokens
- On the device, `expo-notifications` schedules a **local notification** for both the warning time and the exact time — ensuring delivery even if the FCM message is delayed
- `expo-task-manager` + `BackgroundFetch` re-syncs missed announcements when the app is in the background
- For **Urgent** announcements:
  - Android: uses a full-screen intent (`fullScreenIntent`)
  - iOS: uses **Critical Alerts** entitlement *(requires Apple approval — ship without for v1.0, apply in v1.1)*
- If the device is offline, the local notification is still scheduled from cached announcement data in `AsyncStorage`

### 5.3 Announcement Display

- Announcements appear as **distinct styled bubbles** in the group chat (amber background, megaphone icon)
- A dedicated **"Announcements" tab** within group info shows all past and upcoming announcements
- **Countdown timer** shown on upcoming announcements (e.g., *"in 2 hours 15 minutes"*)
- Members can **RSVP:** Going / Maybe / Can't make it — count visible to the admin
- Admins can **edit or delete** scheduled announcements before they fire

---

## 6. Voice Messages

### 6.1 Recording

- User **presses and holds** the microphone button in the chat input bar to begin recording
- **Swipe-left** gesture while holding cancels the recording (visual "slide to cancel" hint shown)
- **Real-time waveform visualizer** during recording using `expo-av` audio metering data
- Maximum voice message duration: **15 minutes**
- Minimum duration: **1 second** (shorter recordings are discarded)
- Format: **AAC**, 44.1 kHz, 128 kbps

### 6.2 Sending & Storage

- On release, recording is uploaded to Firebase Storage at `audio/{userId}/{messageId}.aac`
- Optimistic UI shows a "sending" state with an **upload progress bar**
- On success, a Firestore message document is created with the audio download URL and duration metadata

### 6.3 Playback

- Inline **waveform player** rendered inside the message bubble
- Play/Pause button with animated **progress scrubber**
- Duration label updates as audio plays (e.g., `0:12 / 1:45`)
- **Playback speed toggle:** 1× → 1.5× → 2×
- **Audio ducking:** app lowers other media audio during playback
- **Listen-once mode:** message marked as "listened" with a distinct icon (similar to Instagram DMs)

---

## 7. Status Feature

### 7.1 Overview

Statuses are ephemeral, story-style updates visible to contacts for **24 hours** after posting — similar to WhatsApp Status / Instagram Stories.

### 7.2 Status Types

| Status Type | Content | Max Duration |
|---|---|---|
| Text | Colored background + text (up to 700 chars) | 24 hours |
| Photo | Image from camera or gallery + optional caption | 24 hours |
| Video | Video clip up to 30 seconds | 24 hours |
| Voice | Audio note up to 60 seconds over a static background | 24 hours |

### 7.3 Viewing & Privacy

- **Status ring** appears on a contact's avatar in the chat list when they have an active status
- **Status feed screen** shows all contacts' active statuses as circular story bubbles
- Tap to view in **full-screen** with a progress bar that auto-advances through slides
- **Privacy controls per status:**
  - My Contacts
  - My Contacts Except...
  - Only Share With...
- **View count** shown to the status owner (who viewed, and when)
- Viewers can **react** to a status with an emoji — sent as a private message to the poster
- Status **auto-expires** after 24 hours and is deleted from Firebase Storage

---

## 8. Contacts Integration

### 8.1 Mobile Contacts Import

- On first launch (post-login), the app requests `expo-contacts` READ permission
- If granted, all phone contacts are read and their phone numbers are **hashed client-side** (SHA-256)
- Hashed numbers are sent to a Firebase Cloud Function that checks which hashes match registered chat-it users
- Matched users are displayed as **"Contacts on chat-it"** — no raw phone numbers are stored server-side
- Unmatched contacts are shown as **"Invite to chat-it"** with a native share sheet

### 8.2 Contacts Screen

- Full-screen contacts list with **alphabetical index scroll** (A–Z jump bar)
- Search bar to filter by name or number
- Each row: avatar, display name, phone number, **online indicator** (green dot)
- Tap a contact → open or start a 1-on-1 chat
- Long-press a contact → options: View Profile, Block, Report
- Pull-to-refresh re-syncs with the device phonebook

### 8.3 Manual Add Contact

- **Add by @username** — users can set a unique handle in their profile
- **Scan QR Code** — each user has a personal QR code on their profile that opens a contact-add flow
- **Share my link** — deep link (`chatit://user/{userId}`) opens the app on the sender's profile

---

## 9. Native Call Integration

> **Design Principle:** chat-it does **not** implement an in-app VoIP calling system. All calls are delegated to the device's native phone dialer using `Linking.openURL('tel:+XXXXXXXXXX')`. This eliminates the need for WebRTC infrastructure while giving users a one-tap calling experience directly from within the chat.

### 9.1 Call Flow

1. User taps the **phone icon** in a chat header or on a contact's profile page
2. App looks up the contact's phone number from the contact record
3. `Linking.openURL("tel:" + phoneNumber)` is called
4. Device's **native dialer** opens with the number pre-filled — user taps call to proceed
5. After the call, user returns to chat-it; a call log entry is written to Firestore

### 9.2 Call Log Screen

- A dedicated **"Calls" tab** showing recent outgoing call attempts made from within chat-it
- Each entry: contact avatar, name, timestamp, type label ("Voice Call")
- Tap any entry to **call back** instantly (re-triggers the native dialer)
- Missed call notifications from the native OS function normally — chat-it has no control over these

### 9.3 Permissions & Edge Cases

- `CALL_PHONE` permission is **not required** — `Linking.openURL` uses the dialer, not a direct call API
- If the contact has no phone number stored, the call button is **greyed out** with tooltip: *"No phone number on file"*
- Users can add a phone number to their profile at any time from the Settings screen

---

## 10. Notifications

### 10.1 Notification Types

| Type | Trigger | Priority | Sound |
|---|---|---|---|
| New Message | Incoming direct message | Default | Default chime |
| Group Message | Message in a group chat | Default | Default chime |
| Mention (@user) | User mentioned in a group | High | Distinct tone |
| Announcement Warning | 5 min before scheduled announcement | High | Alarm tone |
| Announcement Alarm | At exact scheduled announcement time | Critical | Alarm loop until dismissed |
| Status Reply | Someone reacted to your status | Default | Default chime |
| Contact Joined | A phonebook contact joins chat-it | Low | Silent |

### 10.2 Notification Settings

- Global mute toggle (all notifications)
- Per-conversation mute: 1 hour / 8 hours / 1 week / Always
- **Announcement alarms cannot be fully muted** — volume can be reduced but not silenced, ensuring reliability
- Notification content preview: Show / Hide / Only when unlocked

---

## 11. Screen Inventory & Navigation

| Screen | Route | Description |
|---|---|---|
| Splash | `/` | App logo, auth state check, redirect |
| Login | `/(auth)/login` | Email login + Google OAuth |
| Register | `/(auth)/register` | New account creation |
| Profile Setup | `/(auth)/setup` | Avatar, display name, phone |
| Chat List | `/(tabs)/chats` | All conversations |
| Contacts | `/(tabs)/contacts` | Phonebook + chat-it users |
| Status Feed | `/(tabs)/status` | Story-style status updates |
| Call Log | `/(tabs)/calls` | Outgoing call history |
| Profile | `/(tabs)/profile` | Settings, logout, QR code |
| 1-on-1 Chat | `/chat/[id]` | Direct message thread |
| Group Chat | `/group/[id]` | Group message thread |
| Group Info | `/group/[id]/info` | Members, settings, media |
| Announcement Detail | `/announcement/[id]` | Full announcement + RSVP |
| New Chat | `/new-chat` | Start DM or create group |
| Create Group | `/create-group` | Group name, icon, member selection |
| Status Viewer | `/status/[userId]` | Full-screen status viewer |
| Create Status | `/status/create` | Text / Photo / Video / Voice status |
| Contact Profile | `/contact/[id]` | User profile, call button, message |
| Settings | `/settings` | Theme, notifications, privacy |
| Media Viewer | `/media/[id]` | Full-screen image / video viewer |

---

## 12. Data Models

### 12.1 Firestore Collections

#### `users/{userId}`

| Field | Type | Description |
|---|---|---|
| `uid` | `string` | Firebase Auth UID |
| `displayName` | `string` | User's display name |
| `email` | `string` | Email address |
| `phoneNumber` | `string?` | E.164 format phone number (optional) |
| `photoURL` | `string` | Profile picture URL |
| `handle` | `string` | Unique @username |
| `about` | `string` | Bio / about text |
| `fcmTokens` | `string[]` | Push notification tokens (multi-device) |
| `lastSeen` | `Timestamp` | Last online timestamp |
| `isOnline` | `boolean` | Real-time presence flag |
| `contactsHash` | `string[]` | Hashed phone numbers for contact matching |
| `createdAt` | `Timestamp` | Account creation date |

---

#### `chats/{chatId}`

| Field | Type | Description |
|---|---|---|
| `type` | `"direct" \| "group"` | Conversation type |
| `members` | `string[]` | Array of member UIDs |
| `createdBy` | `string` | Creator UID |
| `createdAt` | `Timestamp` | Creation date |
| `lastMessage` | `MessagePreview` | Denormalized last message for chat list |
| `lastActivity` | `Timestamp` | Used for sorting the chat list |
| `groupName` | `string?` | Group display name (group only) |
| `groupPhoto` | `string?` | Group icon URL (group only) |
| `admins` | `string[]?` | Admin UIDs (group only) |
| `isAnnouncement` | `boolean?` | Announcement-only mode toggle |
| `mutedBy` | `string[]` | UIDs who have muted this chat |

---

#### `chats/{chatId}/messages/{messageId}`

| Field | Type | Description |
|---|---|---|
| `senderId` | `string` | Sender UID |
| `type` | `enum` | `text \| voice \| image \| file \| announcement` |
| `content` | `string` | Text content or media URL |
| `duration` | `number?` | Voice message duration in seconds |
| `replyTo` | `string?` | Parent message ID for reply/quote |
| `readBy` | `map<uid, Timestamp>` | Read receipts per user |
| `reactions` | `map<uid, string>` | Emoji reactions per user |
| `createdAt` | `Timestamp` | Message send timestamp |
| `isDeleted` | `boolean` | Soft delete flag |

---

#### `groups/{groupId}/announcements/{announcementId}`

| Field | Type | Description |
|---|---|---|
| `title` | `string` | Announcement title (max 100 chars) |
| `body` | `string` | Full announcement text (max 2,000 chars) |
| `scheduledAt` | `Timestamp` | When the alarm fires |
| `warningMinutes` | `number` | Minutes before for the early alarm (default: 5) |
| `createdBy` | `string` | Admin UID who created it |
| `createdAt` | `Timestamp` | Document creation timestamp |
| `isUrgent` | `boolean` | Triggers critical alert behavior |
| `repeat` | `string` | `none \| daily \| weekly \| custom` |
| `status` | `string` | `scheduled \| fired \| cancelled` |
| `rsvp` | `map<uid, string>` | `going \| maybe \| no` per user |

---

#### `statuses/{statusId}`

| Field | Type | Description |
|---|---|---|
| `userId` | `string` | Owner UID |
| `type` | `enum` | `text \| photo \| video \| voice` |
| `content` | `string` | Text or media URL |
| `backgroundColor` | `string?` | Hex color for text statuses |
| `caption` | `string?` | Caption for media statuses |
| `viewedBy` | `map<uid, Timestamp>` | Who viewed and when |
| `privacy` | `string` | `all \| exclude \| only` |
| `privacyList` | `string[]` | UIDs to include/exclude based on privacy setting |
| `expiresAt` | `Timestamp` | Auto-delete at 24 hours |
| `createdAt` | `Timestamp` | Post creation time |

---

#### Firebase RTDB — Presence & Typing

```
/presence/{userId}
  isOnline: boolean
  lastSeen: number (Unix ms)

/typing/{chatId}/{userId}
  isTyping: boolean
  updatedAt: number (Unix ms)
```

---

## 13. Key npm Packages

| Package | Version | Purpose |
|---|---|---|
| `expo` | `~51.x` | Core Expo SDK |
| `expo-router` | `^3.x` | File-based navigation |
| `firebase` | `^10.x` | Auth, Firestore, Storage, RTDB |
| `expo-notifications` | `^0.28.x` | Local & push notifications |
| `expo-task-manager` | `^11.x` | Background tasks for alarms |
| `expo-background-fetch` | `^11.x` | Background fetch for alarm sync |
| `expo-av` | `^14.x` | Audio recording & playback |
| `expo-contacts` | `^12.x` | Read device phonebook |
| `expo-linking` | `^6.x` | Deep links & `tel:` URL for calls |
| `expo-image-picker` | `^15.x` | Camera & gallery access |
| `expo-camera` | `^15.x` | In-app camera for status creation |
| `expo-secure-store` | `^13.x` | Encrypted local token storage |
| `expo-auth-session` | `^5.x` | Google OAuth flow |
| `zustand` | `^4.x` | Lightweight global state management |
| `@tanstack/react-query` | `^5.x` | Server state caching & sync |
| `react-native-gifted-chat` | `^2.x` | Chat UI base components |
| `react-native-paper` | `^5.x` | Material Design UI components |
| `date-fns` | `^3.x` | Date formatting utilities |
| `@react-native-async-storage/async-storage` | `^1.x` | Local key-value persistence |

---

## 14. Non-Functional Requirements

| Category | Requirement | Target |
|---|---|---|
| Performance | Message delivery latency | < 500 ms on 4G |
| Performance | App cold start time | < 3 seconds |
| Performance | Chat list load (50 chats) | < 1 second |
| Reliability | Message delivery success rate | > 99.9% |
| Reliability | Announcement alarm fire rate | > 99.5% |
| Scalability | Concurrent users per group | Up to 1,024 members |
| Security | Messages in transit | TLS 1.3 (Firebase default) |
| Security | Auth tokens | Firebase JWT with automatic rotation |
| Privacy | Contact matching | One-way SHA-256 hash — no raw numbers stored |
| Accessibility | Screen reader support | WCAG 2.1 AA compliant |
| Offline | Message queue when offline | Local queue, auto-retry on reconnect |
| Storage | Max voice message size | 25 MB per message |
| Battery | Background alarm reliability | OS-scheduled tasks, no polling |

---

## 15. MVP Scope vs Future Roadmap

### 15.1 MVP — v1.0 (Must-Have)

- [x] Email & Google authentication
- [x] Real-time 1-on-1 and group text chat
- [x] Typing indicator
- [x] Read receipts (double-tick)
- [x] Voice messages (record, send, inline playback)
- [x] Group announcement with dual alarms (exact time + 5-min warning)
- [x] 24-hour text and photo statuses
- [x] Contacts import from phonebook
- [x] Native call trigger (`tel:` link — no VoIP)
- [x] Push notifications
- [x] Basic message emoji reactions

### 15.2 Short-term — v1.1

- [ ] Video status
- [ ] Voice status
- [ ] Message reply / quote
- [ ] File & document sharing
- [ ] Link preview cards
- [ ] RSVP on announcements
- [ ] Dark mode
- [ ] iOS Critical Alerts entitlement (for Urgent announcements)

### 15.3 Long-term — v2.0

- [ ] End-to-end encryption (Signal Protocol)
- [ ] Disappearing messages
- [ ] Polls in groups
- [ ] Sticker packs
- [ ] Multi-device sync
- [ ] Desktop web companion app
- [ ] Business accounts with broadcast lists

---

## 16. Risks & Mitigations

| Risk | Impact | Likelihood | Mitigation |
|---|---|---|---|
| iOS Critical Alerts require Apple entitlement | Announcement alarms may be muted on iOS | High | Ship without Critical Alerts in v1.0; apply for entitlement in v1.1 |
| Background alarm killed by OS battery optimization | Alarm may not fire on Doze-mode Android | Medium | Use FCM as server-side fallback; test across OEM devices (Samsung, Xiaomi, etc.) |
| Contact permission denied by user | No phonebook import | Medium | Provide manual add-by-@username as a full alternative flow |
| Firebase cost scaling at growth | High read/write costs | Low (MVP) | Implement Firestore pagination, denormalization, and aggressive client-side caching |
| Phone number unavailable for call | Call button non-functional | Medium | Grey out button with tooltip; prompt user to add number in profile settings |
| Large audio file uploads | Slow sends, high storage costs | Low | AAC compression + 15-min cap; Cloud Function auto-cleans old audio after 30 days |

---

## 17. Open Questions

1. Should announcement alarms be **configurable per-member** (opt-out) or **mandatory** for all group members?
2. What is the **maximum group size** for MVP? *(Suggested: 256 for v1.0, 1,024 for v2.0)*
3. Should deleted messages show a *"This message was deleted"* placeholder or be fully hidden?
4. Should the app support **multiple phone numbers per contact**, allowing the user to pick which number to call?
5. Do we need **message translation** (auto-translate to the user's device language)?
6. Should voice messages **auto-expire** after a set period to control storage costs?
7. Should group admins be able to **delete other members' messages** for moderation?

---

## 18. Document Sign-Off

| Role | Name | Status | Date |
|---|---|---|---|
| Product Owner | ___________________ | Pending | ___________ |
| Lead Developer | ___________________ | Pending | ___________ |
| UI/UX Designer | ___________________ | Pending | ___________ |
| QA Lead | ___________________ | Pending | ___________ |

---

*© 2025 chat-it. All rights reserved. — End of Document*
