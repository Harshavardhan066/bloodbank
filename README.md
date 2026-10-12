# PulseLife — Blood Bank Management System

A full-stack, enterprise-grade **Blood Bank Management System** built with **Java (Spring Boot)**, **MySQL**, and a **Modern Responsive Web UI**.

Developed strictly according to the system specifications:
- **Centralized Platform**: Automated management of blood donors, blood groups (A+, A-, B+, B-, AB+, AB-, O+, O-), blood collections, live stock inventory, urgent blood requests, and distribution.
- **Donor Portal**: Donors can register, log in, schedule donation appointments, review their donation history, view their digital donor ID card, and monitor lives saved.
- **Patient & Hospital Desk**: Submit blood requisitions specifying required blood group, units, hospital, doctor, and urgency level (Normal / Urgent / Critical).
- **Blood Availability Monitor**: Real-time stock monitor with capacity bars, low-stock warnings, and an interactive Transfusion Compatibility Matrix.
- **Administrator Dashboard**: Full control over donor records, collection verifications, inventory adjustments, request triage/approvals, and blood issuance with digital audit vouchers.

---

## 🏗️ Architecture & Technology Stack

| Component | Technology | Description |
|---|---|---|
| **Backend** | **Java 17+ / Spring Boot 3.x** | RESTful API services, Spring Data JPA, Hibernate ORM, Bean Validation |
| **Database** | **MySQL 8.0+** | Relational schema with foreign keys, cascading constraints, and transactions |
| **Frontend** | **HTML5, CSS3, JavaScript (ES6+)** | Glassmorphism UI, Responsive CSS Grid/Flexbox, dynamic SPA views |
| **Configuration** | **Maven (`pom.xml`)** | Dependency management and build packaging |

---

## 🗄️ Database Architecture (`blood_bank_db`)

The database consists of 5 relational entities:
1. **`users`**: Stores user authentication credentials, contact info, blood group, age, gender, and role (`ROLE_ADMIN`, `ROLE_DONOR`, `ROLE_PATIENT`).
2. **`blood_stock`**: Tracks available blood units and minimum safety thresholds for all 8 blood types.
3. **`donations`**: Logs donor appointments, screening vitals (hemoglobin, weight, blood pressure), collection status (`PENDING`, `APPROVED`, `COMPLETED`, `REJECTED`), and auto-increments inventory upon completion.
4. **`blood_requests`**: Stores transfusion requisitions from patients/hospitals, urgency level (`NORMAL`, `URGENT`, `CRITICAL`), and status (`PENDING`, `APPROVED`, `ISSUED`, `REJECTED`).
5. **`blood_issues`**: Audit logs of dispatched blood units, destination hospitals, issuing officers, and unique verification certificate numbers (`ISS-YYYYMMDD-XXXXXX`).

---

## 🚀 Key Modules & Features

### 1. Public Portal & Live Blood Inventory
- **Dynamic Stats**: Real-time metrics for lives impacted, active donors, and available units.
- **Live Inventory Cards**: Real-time stock levels for `A+`, `A-`, `B+`, `B-`, `AB+`, `AB-`, `O+`, `O-` with capacity indicators.
- **Interactive Compatibility Matrix**: Click any blood group to immediately see who can donate to whom and who can safely receive blood.
- **4-Step Donation Journey**: Guidance on registration, health screening, sterile collection, and recovery.

### 2. Administrator Dashboard
- **Executive KPI Cards**: Total registered donors, total stock units, blood requests, and total units issued.
- **Inventory Control**: Live unit adjustments (`+1`, `-1`, or custom set) and threshold configuration.
- **Request Triage**: Filter requests by urgency, approve requisitions, or reject with reason notes.
- **Dispatch & Issuance**: Deduct stock automatically upon issuing blood and generate printable **Official Blood Issuance Certificates**.
- **Collection Verification**: Verify donor screening vitals and mark collections as completed.

### 3. Donor Portal
- **Digital Donor ID Card**: Personalized card with blood group badge, verification status, and QR signature.
- **Schedule Donation**: Date picker, center selection, and health questionnaire.
- **Donation History**: Complete log of all past donations and fulfillment statuses.
- **Life Saver Impact**: Calculates estimated patient lives saved.

### 4. Patient / Hospital Requisition Desk
- **Urgent Blood Request Form**: Specify units, blood type, hospital unit, attending physician, and reason.
- **Real-Time Request Tracker**: Track status from Pending Review to Approved and Dispatched.
- **Digital Receipts**: View and print official issuance vouchers once blood is issued.

---

## ⚡ Pre-configured Demo Accounts

For instant testing and evaluation:

| Role | Username | Password | Purpose |
|---|---|---|---|
| **Administrator** | `admin` | `admin123` | Full dashboard access, approvals, inventory control |
| **Donor** | `donor_alex` | `donor123` | Blood donor portal, appointment booking |
| **Patient** | `patient_sarah` | `patient123` | Patient requisition desk & emergency requests |

*(You can also use the 1-click Fast Login buttons in the sign-in modal)*

---

## 🛠️ How to Run the Project

### Option A: Running the Spring Boot Backend (Full-Stack)
1. **Ensure MySQL is installed and running** on `localhost:3306`.
2. **Create database** (or let Spring Boot auto-create it):
   ```sql
   CREATE DATABASE blood_bank_db;
   ```
3. **Configure MySQL Credentials** in [`src/main/resources/application.properties`](file:///c:/Users/harsh/OneDrive/Desktop/TT/src/main/resources/application.properties):
   ```properties
   spring.datasource.username=root
   spring.datasource.password=your_mysql_password
   ```
4. **Build and start Spring Boot**:
   ```bash
   mvn clean spring-boot:run
   ```
5. Open your browser and navigate to: **`http://localhost:8080`**

---

### Option B: Instant Web UI Preview (Node.js)
To preview the interactive web interface immediately with persistent local storage and mock API fallback:
1. Run:
   ```bash
   node server.js
   ```
2. Open your browser and navigate to: **`http://localhost:3000`**

---

## 📡 REST API Reference

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/auth/register` | Register a new Donor, Patient, or Admin |
| `POST` | `/api/auth/login` | Authenticate user and return session profile |
| `GET` | `/api/stock` | Retrieve all blood stock inventory levels |
| `PUT` | `/api/stock/update` | Update blood group units and threshold |
| `GET` | `/api/stock/check-availability` | Check if requested units are available |
| `GET` | `/api/donations` | List all donation appointments |
| `POST` | `/api/donations` | Schedule a new blood donation |
| `PATCH`| `/api/donations/{id}/status` | Update donation status (auto-increments stock on `COMPLETED`) |
| `GET` | `/api/requests` | List all blood requisitions |
| `POST` | `/api/requests` | Submit a blood requisition |
| `PATCH`| `/api/requests/{id}/status` | Update requisition status |
| `POST` | `/api/requests/{id}/issue` | Issue blood units (auto-deducts stock & generates certificate) |
| `GET` | `/api/issues` | Retrieve all blood issuance audit records |
| `GET` | `/api/admin/dashboard` | Aggregated executive KPIs and metrics |
