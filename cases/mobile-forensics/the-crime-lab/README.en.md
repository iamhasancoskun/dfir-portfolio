[Türkçe](README.md) | English (this page)

# Case Study: The Crime Lab - Android Phone Forensic Analysis

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
![ALEAPP](https://img.shields.io/badge/ALEAPP-3.4.0-blue)
![Platform](https://img.shields.io/badge/Platform-Android-lightgrey)
[![Author: hasancoskun](https://img.shields.io/badge/Author-hasancoskun-black)](https://github.com/hasancoskun)

| Category | Details |
| :--- | :--- |
| **Focus Area** | Android Phone Forensic Analysis, Digital Evidence Analysis |
| **Evidence** | `138-The-Crime.zip` (Android phone data) |
| **Tooling** | ALEAPP 3.4.0, DBeaver, SQLite |

---

## 1. Executive Summary

This case involves the digital forensic analysis of a victim's Android phone as part of a murder investigation. The victim was heavily involved in trading, which led to significant debt accumulation. On September 20, 2023, the victim left home without informing anyone and stayed at The Nile Ritz-Carlton in Cairo, planning to travel to Las Vegas. The phone analysis revealed critical information including the trading application (Olymp Trade), creditor (Shady Wahab), debt amount (250,000 EGP), and Discord communication about a meeting at The Mob Museum. ALEAPP 3.4.0 was successfully used to analyze Android phone data, providing crucial evidence that guided the investigation and revealed the victim's final activities.

---

## 2. Technical Analysis (Hypothesis‑Driven)
Per phase: hypothesis → test → finding. Images live under `Assets/`.

### Phase 1 — Android Phone Data Preparation
- **Hypothesis**: Android phone data extracted from the victim's device may contain information about trading activities, debt relationships, and travel plans
- **Tool**: ALEAPP 3.4.0 (Android Logs Events and Protobuf Parser)
- **Command**: `python3 aleappGUI.py`
- **Finding**: Android phone data was successfully analyzed and a web-based report was generated

### Phase 2 — Trading Application Detection (Q1)
- **Hypothesis**: Since the victim was interested in trading, trading applications should be found on the phone
- **Analysis**: Examined "Installed Apps (GMS) for user 0" section in ALEAPP report
- **Finding**: Olymp Trade application detected
  - **Package Name**: com.ticno.olymptrade
  - **SHA-256**: `4f168a772350f283a1c49e78c1548d7c2c6c05106d8b9feb825fdc3466e9df3c`

![Trading Application](assets/q1.1.png)
![SHA-256 Hash](assets/q1.2.png)

### Phase 3 — Debt Relationship Analysis (Q2 & Q3)
- **Hypothesis**: The victim may have communicated with the creditor via SMS
- **Analysis**: Examined "SMS messages" section in ALEAPP report
- **Finding**: 
  - **Debt Amount**: 250,000 EGP
  - **Creditor**: Shady Wahab (+201172137258)
  - **Message Content**: "It's time for you to pay back the money you owe me... Prepare the sum of 250,000 EGP"

![SMS Messages](assets/q2.png)
![Contacts List](assets/q3.png)

### Phase 4 — Location Analysis (Q4)
- **Hypothesis**: The victim's location on September 20, 2023 can be detected from Google Maps activity
- **Analysis**: Examined "Recent Activity" section in ALEAPP report
- **Finding**: 
  - **Date**: 2023-09-20 23:50:29
  - **Location**: The Nile Ritz-Carlton, Cairo
  - **Application**: Google Maps (com.google.android.apps.maps)

![Location Activity](assets/q4.1.png)
![Google Maps Screenshot](assets/q4.2.png)

### Phase 5 — Travel Plan Detection (Q5)
- **Hypothesis**: The victim may have stored flight ticket information on the phone
- **Analysis**: Searched for "ticket" in "Emulated Storage Metadata - Files" section of ALEAPP report
- **Finding**: 
  - **File**: Plane Ticket.png
  - **Date**: 2023-09-20 20:59:44
  - **Destination**: Las Vegas (Cairo → Las Vegas)
  - **Flight**: Egypt Airlines, 01.10.2023, 09:00 AM

![Flight Ticket Metadata](assets/q5.1.png)
![Flight Ticket Screenshot](assets/q5.2.png)

### Phase 6 — Discord Communication Analysis (Q6)
- **Hypothesis**: The victim may have made meeting plans with friends via Discord
- **Analysis**: Examined "Discord Chats" section in ALEAPP report
- **Finding**: 
  - **Meeting Location**: The Mob Museum
  - **Date**: 2023-09-20T20:46:02
  - **Message**: "We'll meet at The Mob Museum, I'll await your call when you arrive"

![Discord Messages](assets/q6.png)

---

## 3. Investigation Results

### Q1: Trading Application SHA256
**Answer**: `4f168a772350f283a1c49e78c1548d7c2c6c05106d8b9feb825fdc3466e9df3c`
- **Application**: Olymp Trade (com.ticno.olymptrade)
- **Detection Method**: ALEAPP "Installed Apps" section

### Q2: Debt Amount
**Answer**: 250,000 EGP
- **Source**: SMS message (from +201172137258)
- **Date**: 2023-09-20 20:09:49

### Q3: Creditor's Name
**Answer**: Shady Wahab
- **Phone**: +201172137258
- **Source**: Contacts list and SMS messages

### Q4: Location on September 20, 2023
**Answer**: The Nile Ritz-Carlton
- **Date**: 2023-09-20 23:50:29
- **Source**: Google Maps activity history

### Q5: Travel Destination
**Answer**: Las Vegas
- **Flight**: Egypt Airlines, 01.10.2023, 09:00 AM
- **Source**: Flight ticket (Plane Ticket.png)

### Q6: Discord Meeting Location
**Answer**: The Mob Museum
- **Date**: 2023-09-20T20:46:02
- **Source**: Discord message history

---

## 4. Conclusion and Assessment

This Android phone forensic analysis successfully uncovered the victim's digital footprint as part of the murder investigation. Using ALEAPP 3.4.0, the analysis revealed:

- **Trading Activity**: The victim used the Olymp Trade application and accumulated significant debt
- **Debt Relationship**: Borrowed 250,000 EGP from Shady Wahab and faced pressure for repayment
- **Travel Plans**: Planned to travel to Las Vegas and arranged a meeting at The Mob Museum
- **Location Data**: Confirmed stay at The Nile Ritz-Carlton on September 20, 2023

These findings played a crucial role in determining the investigation's direction and clearly revealed the victim's final activities before the incident.
