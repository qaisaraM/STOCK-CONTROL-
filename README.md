<div align="center">

# Drill Stock Control System

*Production-grade inventory management with facial recognition authentication,*
*multi-tier approval workflows, and real-time multi-user synchronisation*

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![PyQt5](https://img.shields.io/badge/PyQt5-Desktop_UI-41CD52?style=flat-square&logo=qt&logoColor=white)](https://riverbankcomputing.com/software/pyqt/)
[![OpenCV](https://img.shields.io/badge/OpenCV-Face_Recognition-5C3EE8?style=flat-square&logo=opencv&logoColor=white)](https://opencv.org)
[![facenet-pytorch](https://img.shields.io/badge/facenet--pytorch-MTCNN-7B2FBE?style=flat-square)](https://github.com/timesler/facenet-pytorch)
[![Pandas](https://img.shields.io/badge/Pandas-Data_Processing-150458?style=flat-square&logo=pandas&logoColor=white)](https://pandas.pydata.org)
[![ReportLab](https://img.shields.io/badge/ReportLab-PDF_Generation-E34F26?style=flat-square)](https://reportlab.com)
[![Status](https://img.shields.io/badge/Status-Production-success?style=flat-square)](https://github.com/qaisaraM/STOCK-CONTROL-SYSTEM)

**[🌐 Live Demo](https://qaisaram.github.io/STOCK-CONTROL-SYSTEM/) · [💻 Source Code](https://github.com/qaisaraM/STOCK-CONTROL-SYSTEM)**

</div>

---

## Abstract

Manual drill inventory management in precision manufacturing environments creates accountability gaps, stock discrepancy risks, and slow procurement cycles due to informal approval processes and paper-based records. This report describes the design and deployment of a production-grade desktop inventory system for a manufacturing Production Engineering department, addressing all three failure modes through a unified application. The system integrates facial recognition login (MTCNN face detection + LBPH recognition) for hands-free operator authentication, a structured two-tier order approval workflow with automated PDF generation and Outlook email notifications, real-time multi-user file synchronisation over a shared network drive, and an interactive analytics dashboard with six chart types built on Matplotlib. All data is persisted in Excel files — a deliberate infrastructure decision that aligns with existing department tooling and requires no additional server deployment.

> **Keywords:** facial recognition · MTCNN · LBPH · role-based access control · approval workflow · inventory management · PyQt5 · multi-user synchronisation · manufacturing informatics

---

## Table of Contents

1. [Problem Statement](#1-problem-statement)
2. [System Architecture](#2-system-architecture)
3. [Authentication System](#3-authentication-system)
4. [Role-Based Access Model](#4-role-based-access-model)
5. [Order Approval Workflow](#5-order-approval-workflow)
6. [Stock Management](#6-stock-management)
7. [Analytics Dashboard](#7-analytics-dashboard)
8. [Multi-User Synchronisation](#8-multi-user-synchronisation)
9. [Discussion](#9-discussion)
10. [Getting Started](#10-getting-started)
11. [Future Work](#11-future-work)

---

## 1. Problem Statement

Precision drill tools are high-value, consumable inventory in a CNC machining environment. Their management involves multiple stakeholders — engineers who consume them, planners who order them, and management who approve expenditure. Without a structured system, the following failure modes emerge:

| Failure Mode | Operational Impact |
|---|---|
| No accountability on withdrawals | Stock discrepancies with no audit trail |
| Informal approval via verbal or chat | Procurement decisions not documented |
| Manual low-stock detection | Orders placed too late; production stops |
| No email notifications | Approvers miss pending requests |
| Shared spreadsheet with no access control | Accidental overwrites, no role separation |
| No analytics | No visibility into spend trends or usage patterns |

> The challenge is not building a simple CRUD inventory app — it is building one that enforces accountability, fits real departmental workflows, and requires minimal behaviour change from operators already on the factory floor.

---

## 2. System Architecture

![End-to-End Pipeline](https://github.com/qaisaraM/STOCK-CONTROL-SYSTEM/blob/main/docs/architecture.png)

---

## 3. Authentication System

The authentication layer uses two distinct mechanisms matched to user type.

### Face Recognition Login (PE Members)

```
Camera feed (OpenCV)
        ↓
MTCNN face detection (facenet-pytorch)
        ↓
Face crop + alignment
        ↓
LBPH recogniser (cv2.face.LBPHFaceRecognizer)
        ↓
Confidence threshold check
        ↓
Identity resolved → role granted
```

MTCNN (Multi-Task Cascaded Convolutional Networks) handles detection and alignment; LBPH (Local Binary Patterns Histograms) performs the recognition. This combination was chosen for reliable performance on factory-floor hardware without a GPU requirement for inference.

**Enrollment flow:** A supervisor captures a face sample set per operator through `face_enroll.py`. MTCNN extracts and crops faces; the LBPH model is retrained on the updated dataset and saved as `trainer.yml`.

### Password Login (PIC / Approvers)

Standard employee number + password authentication with role lookup from `Namelist.xlsx`. Guest accounts enter employee number only and are placed in a pending approval queue.

---

## 4. Role-Based Access Model

Five distinct roles with strictly separated permissions:

| Role | Authentication | Key Permissions |
|------|---------------|-----------------|
| **PIC** | Password | Full drill database · stock in/out · order submission · analytics |
| **Approver 1** | Password | Order review per line item · employee account management · guest approval |
| **Approver 2** | Password | Final approval authority · PDF report generation · stakeholder broadcast |
| **PE Member** | Face scan | Stock out recording · stock status view |
| **Guest** | Employee number | Stock status browse only · self-registration |

Each role loads a completely different UI module on login — there is no shared screen with hidden/disabled elements. This prevents accidental privilege escalation and simplifies the codebase per role.

---

## 5. Order Approval Workflow

The approval workflow is the most complex subsystem. It mirrors a real procurement process with two independent authorisation levels and a full audit trail at each step.

```
1. PIC identifies low stock via dashboard alert
          │
          ▼
2. PIC sets order quantities → submits order
   → Automated email to Approver 1
          │
          ▼
3. Approver 1 reviews each line item individually
   → Approve or reject per item with remarks
   → On completion: automated email to Approver 2
          │
          ▼
4. Approver 2 performs final review
   → Final approval or rejection
   → Auto-generates PDF approval report (ReportLab)
   → Broadcast email to all stakeholders (win32com)
          │
          ▼
5. PIC records delivery against approved order
   → Stock levels auto-updated
   → Transaction logged with timestamp
```

### Per-Item Approval

A critical design decision was making approval per line item, not per order. An approver can approve 8 of 10 requested items and reject 2, with remarks per rejection. This reflects real procurement practice where budget constraints or supplier availability may affect individual items within a single order.

### Automated PDF Report

On Approver 2 final sign-off, ReportLab generates a structured approval document containing: order summary, per-item decisions, approver names and timestamps, total approved cost, and rejection remarks. This is emailed automatically to all parties and stored locally.

---

## 6. Stock Management

### Stock In / Out Recording

Every stock movement is recorded as a transaction with timestamp, quantity, operator identity, and linked order reference (for stock-in events). The transaction log in `Stocks.xlsx` is append-only — no records are modified or deleted, only added.

### Safety Stock Alerts

Each drill in `Database_V2.xlsx` carries a safety stock threshold. The system evaluates current stock against this threshold on every data refresh. Items below threshold trigger:

- Visual alert on the PIC dashboard
- Automated low-stock email notification via Outlook

### Stock Level Derivation

Current stock is not stored as a single mutable value. It is derived at runtime from the transaction log:

```python
current_stock = initial_stock + sum(stock_in_events) - sum(stock_out_events)
```

This immutable-log approach means historical stock levels at any past date are always reconstructible, and there is no single point of failure from a corrupted stock figure.

---

## 7. Analytics Dashboard

The monitoring module provides six interactive chart types, all built with Matplotlib and embedded in the PyQt5 UI via `FigureCanvasQTAgg`.

| Chart | Data Shown | Interaction |
|-------|-----------|-------------|
| Orders & Cost per Period | Order qty + RM cost on dual axis | Switchable by year / custom range |
| Stock In / Out Trends | Qty In vs Qty Out with Order Qty overlay | Bar chart, date filter |
| Top 10 Drill Purchases | Spend ranked by drill name | Colour-coded by drill type |
| Top 10 Drill Usage | Qty Out ranked by drill name | Horizontal bar |
| Cost by Drill Type | Total spend breakdown by category | Horizontal bar |
| Order Detail Table | Full filterable order history | Filter by month, type, name |

All charts support double-click to open a full-screen popup with Matplotlib's `NavigationToolbar2QT` for zoom and pan. Hover tooltips show exact values on mouseover.

---

## 8. Multi-User Synchronisation

Multiple users across different workstations may have the application open simultaneously, all reading from and writing to the same shared Excel files on a network drive. Without synchronisation, a PIC recording a stock movement would not be reflected on an Approver's open session until they restarted the application.

The synchronisation layer combines two mechanisms:

```python
# Mechanism 1: QFileSystemWatcher
# Fires immediately when the watched file changes on disk
watcher = QFileSystemWatcher([data_path])
watcher.fileChanged.connect(reload_data)

# Mechanism 2: 2-second polling fallback
# Catches network drive changes that don't fire OS file events
QTimer.singleShot(2000, poll_for_changes)
```

Safe write logic prevents data corruption when two users write simultaneously: the write operation uses a temporary file and an atomic rename, and includes retry logic with exponential backoff for file-lock conflicts.

---

## 9. Discussion

### Combining Classical CV with Deep Learning for Authentication

Using MTCNN for detection and LBPH for recognition is an intentional hybrid. MTCNN provides accurate, robust face detection and alignment across varying lighting and angles. LBPH, while less powerful than a deep embedding model for large populations, is fast, runs on CPU only, requires no cloud API, and performs reliably for a known, fixed employee roster of under 100 people. A full deep embedding model (e.g., ArcFace) would be over-engineered for this deployment and would introduce torch GPU requirements that the target hardware may not satisfy.

### Workflow Modelling as Software Architecture

The approval system required careful state modelling. An order exists in one of several states: `PENDING_A1 → PARTIAL_A1 → PENDING_A2 → APPROVED / REJECTED`. Each transition is guarded — an approver cannot complete their stage until every line item has a decision. This state machine approach prevents incomplete approvals from entering the system and makes the approval status unambiguous at all times.

### Infrastructure Constraints as Design Inputs

The choice of Excel as a data store — unusual by software engineering convention — was the right decision for this environment. The department already has established Excel workflows. Supervisors can open `Orders.xlsx` or `Stocks.xlsx` directly for ad-hoc queries or exports without requiring any additional tooling. This zero-infrastructure-overhead approach was a key factor in successful deployment and adoption.

### The Role Separation Pattern

Loading entirely separate UI modules per role (rather than a shared UI with conditionally hidden elements) proved to be a better architecture for maintainability. Each role's interface can be developed and tested independently. Adding a new role requires a new module, not a modification to shared UI logic — this follows the open/closed principle in a practical sense.

---

## 10. Getting Started

### Requirements

- Windows 10/11
- Python 3.x
- Webcam (for face recognition login)
- Microsoft Outlook installed (for email automation)
- Shared network drive for multi-user deployment

### Installation

```bash
git clone https://github.com/qaisaraM/STOCK-CONTROL-SYSTEM
cd STOCK-CONTROL-SYSTEM
pip install -r requirements.txt
python main.py
```

### Default Credentials

| Role | Method | Default Password |
|------|--------|-----------------|
| PIC | Employee No + Password | `1234` |
| Approver 1 | Employee No + Password | `1111` |
| Approver 2 | Employee No + Password | `2222` |
| PE Member | Face scan | — |
| Guest | Employee No | — |

> ⚠️ Change all default passwords before production deployment.

### Project Structure

```
CODE/
├── main.py                        # Application entry point
├── src/
│   ├── ui/
│   │   ├── gui.py                 # Login screens + role router
│   │   ├── pic.py                 # PIC dashboard, stock, orders
│   │   ├── user.py                # PE member interface
│   │   └── sidebar_gui.py         # Shared sidebar base class
│   └── Function/
│       ├── approval.py            # 2-tier approval dashboard
│       ├── face_enroll.py         # Face capture + LBPH training
│       ├── stock_utils.py         # Excel I/O + safe write
│       ├── sync_manager.py        # Multi-user synchronisation
│       ├── pdf.py                 # Approval PDF generator
│       └── mail.py                # Outlook email automation
└── _internal/DATA/
    ├── Database_V2.xlsx           # Drill master catalogue
    ├── Orders.xlsx                # Order records
    ├── Namelist.xlsx              # Employee roster + credentials
    ├── Stocks.xlsx                # Transaction log
    └── Faces/                     # Enrolled images + trainer.yml
```

### Technology Stack

| Component | Technology |
|-----------|------------|
| GUI Framework | PyQt5 |
| Face Detection | facenet-pytorch (MTCNN) |
| Face Recognition | OpenCV LBPH |
| Data Processing | Pandas + openpyxl |
| Analytics Charts | Matplotlib |
| PDF Generation | ReportLab |
| Email Automation | win32com (Outlook) |
| Deployment | PyInstaller |

---

## 11. Future Work

- Replace Excel data store with SQLite for concurrent write safety and query performance at scale
- Web dashboard for remote monitoring and approvals without desktop app installation
- Push notifications via email or Teams integration on approval status changes
- Biometric enrollment self-service kiosk to remove the supervisor dependency in face onboarding
- Predictive reorder alerts based on historical consumption rate modelling
- Audit log viewer UI for compliance and traceability reporting

---

> *This project is proprietary software presented here for portfolio purposes only.*

<div align="center">

*Production Inventory Management System · Deployed for Production Engineering Department · 2026*

**[LinkedIn](https://linkedin.com/in/qaisara-mardhiah) · [Portfolio](https://qaisaram.github.io/STOCK-CONTROL-SYSTEM/) · [GitHub](https://github.com/qaisaraM)**

</div>
