# 🛠️ Drill Stock Control System

A desktop-based inventory management system for tracking precision drill stock, usage, and approval workflows in a controlled production environment.

---

## 📌 Overview

This application is built using Python and PyQt5 to manage stock operations for precision drills across multiple categories and user roles.

The system helps teams:

* Track stock levels accurately
* Monitor usage (stock out)
* Manage stock replenishment (stock in)
* Control access through role-based permissions

> ⚠️ This is a **sanitized portfolio version** of a production system. All sensitive data, APIs, and company-specific details have been removed or replaced with mock/local implementations.

---

## 🧩 Product Scope

The system manages approximately **301 types of drills**, categorized into:

* ADRL
* ADR
* Endmill

### Example Drill Specification

* Model Number: ADRSL-0003
* Diameter: φ 0.03
* Blade Length: 0.5 ℓ
* Full Length: 38 L
* Shank: φ 3

---

## 🎯 Core Features

### 📥 Stock In

* Add new inventory items
* Record purchase entries
* Managed by authorized personnel (PIC)

### 📤 Stock Out

* Track usage of drills
* Maintain accurate deduction of stock
* Performed by authorized users

### 📊 Inventory Management

* Real-time stock tracking
* Drill categorization and filtering
* Low stock monitoring

### 🧾 Request & Approval Workflow

* Users can submit stock requests
* Approval required before processing
* Ensures controlled purchasing decisions

---

## 👥 User Roles & Permissions

### ✅ Approver

* Approves stock requests
* Approves guest access requests
* Oversees system operations

### 👨‍🔧 PIC (Person In Charge)

* Performs stock-in operations
* Creates purchase requests
* Manages inventory updates

### 👷 User

* Authenticated via face detection
* Allowed to perform stock-out only

### 👤 Guest

* External users from other departments
* Must register and request access
* Requires approval before gaining user privileges

---

## 🔐 Authentication & Security

* Face detection login for internal users
* Role-based access control
* Approval-based onboarding for new users

---

## 🛠️ Tech Stack

* Python
* PyQt5 (Desktop UI)
* OpenCV (Face Detection)
* SQLite (Local Database)

---

## 📁 Project Structure

```bash
project/
│
├── app/
│   ├── ui/            # UI components (PyQt)
│   ├── controllers/   # Business logic
│   ├── services/      # Data handling
│   └── models/        # Data structures
│
├── assets/            # Icons, images
├── screenshots/       # README visuals
├── main.py
├── requirements.txt
└── README.md
```

---

## 📸 Screenshots

(Add images in `/screenshots` folder)

Suggested:

* Login (face detection)
* Dashboard
* Stock In / Stock Out interface
* Approval panel

---

## ▶️ Demo

(Optional: Add a short demo video or GIF showing system workflow)

---

## ⚙️ Installation

```bash
git clone https://github.com/your-username/drill-stock-control-system.git
cd drill-stock-control-system
pip install -r requirements.txt
python main.py
```

---

## 🧠 What I Learned

* Designing a role-based access control system
* Implementing face recognition authentication
* Handling real-world inventory workflows
* Structuring a maintainable Python desktop application
* Working under restricted environments (no external APIs)

---

## ⚠️ Limitations

* Runs locally (no cloud deployment)
* Uses mock/local data instead of live company APIs
* Face detection optimized for controlled environment only

---

## 🚀 Future Improvements

* Convert to web-based system (React + API)
* Add reporting and analytics dashboard
* Improve UI/UX design
* Integrate real-time database or cloud storage

---

## 📄 License

This project is for educational and portfolio purposes only.
