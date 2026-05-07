🔗 [View Source Code](https://github.com/qaisaraM/STOCK-CONTROL-SYSTEM)  
🌐 [Live Demo](https://qaisaram.github.io/STOCK-CONTROL-SYSTEM/)

<div align="center">

# ⚙️ Drill Stock Control System

**AI-powered inventory management with facial recognition, multi-tier approvals & real-time sync**

![Python](https://img.shields.io/badge/Python-3.x-blue?style=flat-square&logo=python&logoColor=white)
![PyQt5](https://img.shields.io/badge/PyQt5-Desktop_UI-blue?style=flat-square)
![OpenCV](https://img.shields.io/badge/OpenCV-Face_Recognition-orange?style=flat-square&logo=opencv)
![Pandas](https://img.shields.io/badge/Pandas-Data_Processing-green?style=flat-square&logo=pandas)
![facenet-pytorch](https://img.shields.io/badge/facenet--pytorch-MTCNN-purple?style=flat-square)
![ReportLab](https://img.shields.io/badge/ReportLab-PDF_Generation-cyan?style=flat-square)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Analytics-green?style=flat-square)
![win32com](https://img.shields.io/badge/win32com-Outlook_Email-red?style=flat-square)

</div>

---

## 📌 Overview

A production-grade desktop application for managing drill tool inventory in a manufacturing environment. Built with PyQt5, it features facial recognition login, a structured 2-tier order approval workflow, real-time multi-user file synchronization, automated email notifications, and a rich analytics dashboard — all backed by Excel files shared over a network drive.

> Developed for Production Engineering department.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔐 **Multi-Role Auth** | Face recognition (MTCNN + LBPH) for PE members; password login for PIC/Approvers; guest signup with approval queue |
| 📦 **Stock Tracking** | Real-time stock in/out recording with safety stock alerts and automated low-stock email notifications |
| ✅ **2-Tier Approvals** | Per-line-item approve/reject flow: Approver 1 → Approver 2 → auto PDF + email summary |
| 📊 **Analytics Dashboard** | Interactive Matplotlib charts with hover tooltips and popup zoom: orders, cost trends, top-10 usage, type breakdown |
| 🔄 **Multi-User Sync** | `QFileSystemWatcher` + 2-second polling keeps all open sessions in sync over a shared network path |
| 📧 **Outlook Automation** | Automated notifications via `win32com` for every workflow event: submission, approval, delivery, guest registration |

---

## 👥 User Roles

### 🏭 PIC (Person In Charge)
- Full drill database management (add/edit/delete)
- Stock In and Stock Out recording
- Order submission with cost estimation
- Order history and delivery tracking
- Full analytics access

### ✔ Approver 1
- Review and approve/reject orders per line item
- Notify Approver 2 on completion
- Manage employee accounts and pending signups

### ✔✔ Approver 2
- Final approval authority
- Auto-generates PDF approval report
- Broadcasts summary email to all stakeholders

### 👤 PE Member / User
- Face scan login only
- Stock Out recording
- Stock Status view

### 🧑‍💻 Guest
- Limited access — stock status browse
- Self-registration with Approver 1 email notification

---

## 🔄 Order Approval Workflow

```
PIC identifies low stock
        │
        ▼
PIC sets qty → submits order → emails Approver 1
        │
        ▼
Approver 1: toggle approve/reject per item
        │
        ▼
Approver 2: final decision
        │
        ▼
Auto PDF report + email to all stakeholders
        │
        ▼
PIC records delivery → stock auto-updated
```

---

## 🛠 Tech Stack

| Library | Purpose |
|---|---|
| `PyQt5` | Desktop UI framework |
| `facenet-pytorch` | Face detection via MTCNN |
| `OpenCV (cv2)` | Face recognition via LBPH |
| `Pandas` | Data processing and Excel I/O |
| `openpyxl` | Low-level Excel read/write |
| `Matplotlib` | Analytics charts with hover tooltips |
| `ReportLab` | PDF approval report generation |
| `win32com` | Outlook email automation |
| `torch` | Backend for MTCNN inference |

---

## 📁 Project Structure

```
CODE/
├── src/
│   ├── ui/
│   │   ├── gui.py            # Main window, login screens, role routing
│   │   ├── pic.py            # PIC interface: dashboard, stock, orders, monitoring
│   │   ├── user.py           # PE member interface: stock out & status
│   │   └── sidebar_gui.py    # Shared sidebar window base class
│   └── Function/
│       ├── approval.py       # 2-tier approval dashboard + pages
│       ├── face_enroll.py    # Face capture, MTCNN detection, LBPH training
│       ├── stock_utils.py    # Excel I/O, safe write, data readers
│       ├── sync_manager.py   # QFileSystemWatcher + poll-based multi-user sync
│       ├── pdf.py            # ReportLab approval PDF generator
│       └── mail.py           # Outlook automation via win32com
├── main.py                   # Entry point
└── _internal/DATA/
    ├── Database_V2.xlsx      # Drill master data
    ├── Orders.xlsx           # Order records
    ├── Namelist.xlsx         # Employee roster
    ├── Stocks.xlsx           # Transaction log
    └── Faces/                # Enrolled face images + trainer.yml
```

---

## 🔐 Default Login Credentials

| Role | Method | Default Password |
|---|---|---|
| PIC | Employee No + Password | `1234` |
| Approver 1 | Employee No + Password | `1111` |
| Approver 2 | Employee No + Password | `2222` |
| PE Member | Face Scan | — |
| Guest | Employee No (no password) | — |

> ⚠️ Change default passwords before deployment.

---

## 📊 Analytics Module

The Monitoring page provides:

- **Orders & Cost per Period** — dual-axis bar + line chart (Qty vs RM cost), switchable by year or custom date range
- **Stock In / Out trends** — double bar chart showing Qty In vs Qty Out with Order Qty overlay
- **Top 10 Drill Purchases** — horizontal bar chart by drill name, coloured by drill type
- **Top 10 Drill Usage** — horizontal bar chart of highest Qty Out items
- **Cost by Drill Type** — horizontal bar breakdown of total spend per type
- **Order Detail Table** — filterable by month, drill type, and drill name

All charts support double-click to open a full-screen popup with zoom/pan via Matplotlib's NavigationToolbar.

---

---

## 📄 License

This project is proprietary software and is presented here for portfolio purposes only.

---

<div align="center">

built with **PyQt5** · **OpenCV** · **facenet-pytorch** · **pandas**

*Production Inventory Management System · QaisaraM*

</div>
