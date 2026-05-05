---

# 🛠️ Drill Stock Control System

A desktop-based inventory management system for tracking precision drill stock, usage, and approval workflows in a controlled production environment.

---

## 📌 Overview

The Drill Stock Control System is built using Python and PyQt5 to manage inventory operations for precision cutting tools used in production.

It is designed to:

* Track stock levels accurately
* Monitor usage (stock-out operations)
* Manage stock replenishment (stock-in operations)
* Enforce role-based access control and approval workflows

> ⚠️ This is a **portfolio version** of a real production system. Sensitive company data, APIs, and internal infrastructure have been replaced with local/mock implementations.

---

## 🧩 Product Scope

The system manages approximately **301 types of drill tools**, categorized into:

* ADRL series
* ADR series
* Endmill series

### 📌 Example Drill Specification

* Model Number: ADRSL-0003
* Diameter: φ 0.03
* Blade Length: 0.5 ℓ
* Full Length: 38 L
* Shank: φ 3

---

## 🎯 Core Features

### 📥 Stock In (Inventory Addition)

* Add new drill items into the system
* Record purchase and restock entries
* Restricted to authorized personnel (PIC role)

### 📤 Stock Out (Usage Tracking)

* Track consumption of drills in production
* Automatically update inventory levels
* Controlled access based on user role

### 📊 Inventory Management

* Real-time stock tracking
* Drill categorization (ADRL / ADR / Endmill)
* Low-stock monitoring for replenishment planning

### 🧾 Request & Approval Workflow

* Users can submit stock requests
* Approver validates and approves/rejects requests
* Ensures controlled and traceable procurement process

---

## 👥 User Roles & Permissions

### ✅ Approver

* Approves stock requests
* Approves guest access requests
* Oversees system-wide inventory control

---

### 👨‍🔧 PIC (Person In Charge)

* Handles stock-in operations
* Creates purchase and replenishment entries
* Maintains inventory accuracy

---

### 👷 User

* Internal production team member
* Authenticated via face detection
* Allowed only to perform stock-out operations

---

### 👤 Guest

* External or non-team users
* Must register and request access
* Requires approval before becoming a system user

---

## 🔐 Authentication & Security

* Face detection-based login for internal users
* Role-based access control (RBAC)
* Approval-based onboarding for new users
* Secure separation of user permissions

---

## 🛠️ Tech Stack

* Python
* PyQt5 (Desktop UI Framework)
* OpenCV (Face Detection Authentication)
* Pandas / OpenPyXL (Excel file handling)
* Microsoft SharePoint (file-based storage system)

---

## 📁 Data Storage Architecture

Instead of a traditional database, the system uses **Excel files as structured storage units**.

Each module is separated into independent Excel files to ensure:

* No merge conflicts in shared environment
* Easier auditing and traceability
* Modular data management

### 📂 SharePoint Structure

```bash id="g7x2kp"
SharePoint / StockSystem /
│
├── stock_master.xlsx        # Master drill inventory (301 types)
├── stock_in.xlsx            # Stock-in transaction logs
├── stock_out.xlsx           # Stock-out usage records
├── user_access.xlsx         # User roles and permissions
├── approval_requests.xlsx   # Stock request workflow
```

---

## 📁 Project Structure

```bash id="p9v2xq"
project/
│
├── app/
│   ├── ui/            # PyQt5 UI components
│   ├── controllers/   # Business logic layer
│   ├── services/      # Excel data handling logic
│   └── models/       # Data structures
│
├── assets/            # Icons, images
├── screenshots/       # UI screenshots
├── main.py            # Application entry point
├── requirements.txt
└── README.md
```

---

## 📸 Screenshots

Add images inside `/screenshots` folder.

Recommended views:

* Login (Face Detection screen)
* Main Dashboard
* Stock In / Stock Out interface
* Approval workflow panel
* Inventory overview

---

## ▶️ Demo

(Optional but recommended)

Add:

* Screen recording (MP4 / GIF)
* Or YouTube unlisted demo link

Show:

* Login flow
* Stock transaction
* Approval process

---

## 🧠 Key Learning Outcomes

* Designing role-based access control (RBAC) systems
* Implementing face recognition authentication in desktop applications
* Managing file-based data architecture using Excel
* Building production-like workflows in a constrained environment
* Structuring scalable Python desktop applications

---

## ⚠️ Limitations

* Runs locally (no cloud deployment)
* Uses Excel instead of a relational database
* Face detection requires controlled lighting/environment
* Designed for internal/enterprise-style simulation only

---

## 🚀 Future Improvements

* Migrate Excel storage to database (SQLite/PostgreSQL)
* Convert system into web application (React + API backend)
* Improve face recognition accuracy and speed
* Add analytics dashboard for inventory insights
* Enable cloud-based synchronization via SharePoint API

---

## 📄 License

This project is intended for **portfolio and educational purposes only**.

---


