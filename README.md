---

# 🛠️ Drill Stock Control System

A desktop-based inventory management system for tracking precision drill stock, usage, and approval workflows in a controlled production environment.

---

## 📌 Overview

The Drill Stock Control System is built using Python and PyQt5 to manage inventory operations for precision cutting tools used in production.

The system is designed to support:

* Accurate stock level tracking
* Stock-out (usage) monitoring
* Stock-in (replenishment) management
* Role-based access control and approval workflows

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
* Record purchase and replenishment entries
* Restricted to authorized personnel (PIC role)

### 📤 Stock Out (Usage Tracking)

* Track consumption of drills in production
* Automatically update inventory levels
* Controlled access based on user roles

### 📊 Inventory Management

* Real-time stock tracking
* Drill categorization (ADRL / ADR / Endmill)
* Low-stock monitoring for replenishment planning

### 🧾 Request & Approval Workflow

* Users submit stock requests
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
* Requires approval before becoming an active user

---

## 🔐 Authentication & Security

* Face detection-based login for internal users
* Role-based access control (RBAC)
* Approval-based onboarding workflow
* Secure separation of user permissions

---

## 🛠️ Tech Stack

* Python
* PyQt5 (Desktop GUI)
* OpenCV (Face Recognition Authentication)
* Pandas / OpenPyXL (Excel data handling)
* Microsoft SharePoint (file-based storage system)

---

## 📁 Data Storage Architecture

Instead of a traditional database, the system uses **Excel files as a modular transactional data layer**.

Each file represents a specific business function to ensure traceability and prevent data conflicts in a shared environment.

### 📂 SharePoint Structure

```bash id="sp_final"
SharePoint / StockSystem /
│
├── stock_master.xlsx         # Master drill inventory (301 types)
├── Orders.xlsx               # Stock-in orders & approval workflow
├── Received.xlsx             # Stock received confirmation logs
├── Stocks.xlsx               # Real-time stock balance & movement tracking
│
├── Namelist.xlsx            # User roles and permissions (RBAC)
├── Pending_employees.xlsx   # User / guest access requests
```

---

## 🧠 System Architecture

The system follows a **lightweight ERP-style workflow architecture** using Excel as a transactional data layer.

### 🔄 Workflow Process

```
Order Request → Approval → Stock Received → Stock Update → Stock Usage
```

### 📊 Data Flow Design

* Orders.xlsx → Handles stock requests & approvals
* Received.xlsx → Confirms physical stock arrival
* Stocks.xlsx → Maintains real-time inventory state
* stock_master.xlsx → Reference catalog for all drill types

---

## 🧩 Design Philosophy

This system was designed to replicate a real-world manufacturing inventory environment where:

* Lightweight tools are preferred over full database systems
* Shared file-based systems are used across departments
* Traceability and auditability are critical
* Role-based workflows control all operations

---

## 📁 Project Structure

```bash id="proj_final"
project/
│
├── app/
│   ├── ui/            # PyQt5 interface components
│   ├── controllers/   # Business logic layer
│   ├── services/      # Excel data processing layer
│   └── models/        # Data structures
│
├── assets/            # Icons, images
├── screenshots/       # UI screenshots
├── main.py            # Application entry point
├── requirements.txt
└── README.md
```

---

## 📸 Screenshots

Add images in `/screenshots` folder:

* Login (Face Detection)
* Dashboard
* Stock In / Stock Out screens
* Approval workflow panel
* Inventory overview

---

## ▶️ Demo

(Optional but recommended)

Include:

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
* Building file-based data architectures using Excel
* Designing real-world inventory workflows
* Structuring production-style Python desktop applications

---

## ⚠️ Limitations

* Runs locally (no cloud deployment)
* Uses Excel instead of a relational database
* Face recognition requires controlled environment conditions
* Designed for internal enterprise simulation

---

## 🚀 Future Improvements

* Migrate Excel system to database (SQLite / PostgreSQL)
* Convert into web application (React + API backend)
* Improve face recognition accuracy and speed
* Add analytics dashboard for inventory insights
* Enable cloud synchronization via SharePoint API

---

## 📄 License

This project is intended for **portfolio and educational purposes only**.

---
