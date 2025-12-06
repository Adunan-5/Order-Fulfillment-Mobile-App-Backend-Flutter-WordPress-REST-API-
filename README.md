# Order Fulfillment App Backend (WordPress REST API) — Architecture Overview

This repository documents the backend REST API architecture built using WordPress plugins for a mobile order fulfillment app used by retail outlet staff.  
The original source code is proprietary; this README outlines workflows, API logic, and system behavior.

---

## 📦 Project Overview

The mobile app (Flutter frontend) allows outlet staff to:

- Log in based on outlet location
- Retrieve outlet-specific orders
- Process orders using barcode scanning
- Update order stages (Pending → Processing → Ready)
- Trigger customer notifications
- Integrate with delivery providers (ex: Grab)

The backend powering this is a custom WordPress plugin exposing REST APIs.

---

## 🔐 Staff Authentication with GPS Validation

### Login Flow:
1. Staff enters credentials.
2. Device sends GPS coordinates to API.
3. Backend validates:
   - Credentials
   - Assigned outlet
   - Staff must be within **100m radius** of the outlet

If outside radius →  
`"Please move near your outlet to log in."`

This ensures secure, on-premise access.

---

## 📥 Order Retrieval API

Returns only orders associated with the staff member’s outlet.

Includes:

- Order ID
- Customer details (masked)
- Items & quantities
- Delivery/Pickup mode
- Timeslot
- Barcode for each item
- Processing state

---

## 🔄 Barcode Processing Workflow

### Flow:
1. Staff scans barcode.
2. API verifies:
   - Item belongs to this order
   - Quantity remaining to process
3. System updates progress:
   - Partial → final fulfillment
4. When all items completed:
   - Order status → **Ready**

### Duplicate scans:
API returns → `"Item already fulfilled"` to prevent over-processing.

---

## 🚚 Delivery & Pickup Completion

### Pickup:
- Customer notified when order becomes "Ready for Pickup".

### Delivery:
- API integrates with external delivery platform.
- Dispatch request triggered automatically.

---

## 🧱 Backend Architecture

- WordPress REST API routes
- Custom plugin handling:
  - Authentication
  - Outlet mapping
  - GPS radius calculation
  - Barcode validation
  - Order progress updates
  - Integration hooks
- Custom DB tables for:
  - Staff → Outlet mapping
  - Barcode → Product mapping
  - Process logs

---

## 👨‍💻 My Responsibilities

- Developed all custom REST API endpoints
- Implemented GPS-based radius login validation
- Built barcode scanning logic
- Created order lifecycle logic
- Integrated customer notification triggers
- Connected order flows with delivery provider API
- Coordinated with Flutter mobile team to finalize API contracts

---

## ⚠️ Disclaimer
This repository includes **architecture only**.  
Actual source code and plugin files cannot be published.
