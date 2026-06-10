# ☕ Cafe POS & KDS (Enterprise Cafe Management System)

![Kotlin](https://img.shields.io/badge/Kotlin-1.9-blue.svg?logo=kotlin)
![Jetpack Compose](https://img.shields.io/badge/Jetpack%20Compose-Ready-4285F4?logo=android)
![Firebase](https://img.shields.io/badge/Firebase-Firestore%20%7C%20Crashlytics-FFCA28?logo=firebase)
![Clean Architecture](https://img.shields.io/badge/Architecture-Clean%20%7C%20MVVM-success)
![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)

**A production-ready Point of Sale (POS) and Kitchen Display System (KDS) designed for modern cafes, restaurants, and entertainment lounges.** Built from the ground up using **Native Android (Jetpack Compose)** and **Clean Architecture**, this system acts as a complete, scalable solution capable of standalone operations or White-Label B2B SaaS deployment.

---

## 🚀 The Story & Real-World Validation
This project was not built in a vacuum. It was developed to address the specific, chaotic operational challenges of the hospitality industry. 

The system seamlessly merges standard food and beverage ordering with **Dynamic Time-Based Billing** for gaming zones (e.g., PlayStation, Billiards). Throughout the development cycle, features were refined based on real-time operational feedback, leading to the creation of advanced features like **Direct Payment Terminal Integration**, **Network Printer Routing**, **Kiosk Mode**, and **ZATCA Phase 1 E-Invoicing**.

---

## ✨ Core Features & The Complete Workflow

### 📍 1. Dynamic Table & Zone Management
* **Custom Zones:** Unlimited custom zones (General, VIP, PlayStation, Terrace).
* **Live Table States:** Real-time visual indicators (Available, Occupied, Pending, Prepared).
* **Smart Entertainment Timer:** Dual billing modes (Hourly Rate vs. Fixed Match Rate) running persistently.

### 🛒 2. Advanced Cashier Operations
* **Draft Mode (Smart Editing):** Cashiers can edit/void items with "Void Reasons" before sending them to the kitchen.
* **Complex Modifiers & Custom Items:** Add kitchen notes (e.g., "Extra Sugar") or create custom on-the-fly items.
* **Split Checks & Mixed Payments:** Split bills equally or by specific items. Accept mixed payments (Cash + Visa) seamlessly.

### 💳 3. Direct Hardware & Payment Integration
* **Smart POS Integration:** System is programmed to send HTTP requests/Intents directly to Bank Visa Terminals, waiting for automatic success/failure callbacks (Eliminates human entry errors).
* **Hybrid Printing:** Supports both **Bluetooth Thermal Printers** and **Network/IP Printers**.
* **Z-Reports (Shift Closing):** Generates and prints detailed financial shift reports directly from the Admin settings.

### 👨‍🍳 4. Smart Kitchen Routing (KDS)
* **Station-Based Routing:** Admin can map specific categories to specific stations (Kitchen vs. Bar vs. Shisha).
* **Hybrid Station Modes:** A station can be operated via a **Screen (Tablet KDS)**, a **Network Printer**, or **Both**.
* **Real-Time Sync:** Instant UI updates across all devices via Firebase Snapshot Listeners.

### 🧾 5. ZATCA Approved E-Invoicing
* Generates a Base64 encoded TLV (Tag-Length-Value) QR Code containing the Cafe Name, VAT Number, Timestamp, Total, and Tax amount (Saudi Arabia Compliance).
* High-res Dynamic Receipt generation saved locally to the device gallery.

### ⚙️ 6. Enterprise-Grade Admin Control
* **Role-Based Access (RBAC):** PIN-protected access for Admin, Cashier, and Captain roles.
* **White-Labeling:** Instant, runtime switching between **Arabic/English** and **Light/Dark modes** without app restarts.

---

## 🛡️ Enterprise Stability & Security Features
To survive the harsh environment of a busy cafe, the app incorporates:
1. **Kiosk Mode (Screen Pinning):** Utilizing `startLockTask()` to lock the tablet to the POS app, preventing staff from accessing Android settings or other apps.
2. **Keep Screen On:** Implemented `FLAG_KEEP_SCREEN_ON` for uninterrupted Cashier and KDS visibility.
3. **Offline Persistence:** Firebase local caching is forcefully enabled. If the cafe's internet drops, the system continues to work and syncs automatically when the connection is restored.
4. **Anti-Piracy Licensing:** A real-time Firebase listener binds a unique License Code to the hardware's `ANDROID_ID`. Unauthorized devices are instantly force-logged out.
5. **Firebase Crashlytics:** Integrated for remote, real-time fatal/non-fatal crash tracking.

---

## 🔥 Engineering Challenges & Solutions

Building a concurrent, multi-device POS system brought significant technical hurdles. Here is how they were solved:

### Challenge 1: The "Draft vs. Server" State Conflict
* **Problem:** If a cashier modifies an order while the KDS is reading from the server, data overwrites occurred.
* **Solution:** Implemented a decoupled local state (`cartItems`) vs server state (`lastSentItems`) in the `CashierViewModel`. Edits remain local until the user explicitly triggers `sendToKitchen()`, which selectively updates the database and triggers printer intents only for `NEW` items.

### Challenge 2: Handling Hardware Device Fragmentation
* **Problem:** Cafes use a mix of Bluetooth printers for cashiers and IP-based LAN printers for kitchens.
* **Solution:** Abstracted a `PrinterManager` interface that dynamically resolves the connection protocol based on the Admin's saved configuration (MAC address vs IPv4). 

### Challenge 3: Missing Crashlytics Build IDs
* **Problem:** During the CI/CD pipeline, `FirebaseInitProvider` crashed due to missing Build IDs.
* **Solution:** Restructured the `build.gradle.kts` plugin hierarchy, ensuring `com.google.gms.google-services` executes strictly before the `crashlytics` plugin, paired with clean rebuilds to successfully inject the UUIDs into the APK.

### Challenge 4: Asynchronous Payment Terminal Locks
* **Problem:** Waiting for a physical Visa terminal to process a payment blocked the Main Thread.
* **Solution:** Wrapped the payment HTTP request inside `viewModelScope.launch(Dispatchers.IO)`. Displayed a blocking "Waiting for Terminal" bottom sheet. Depending on the HTTP response code (Success/Fail), `withContext(Dispatchers.Main)` safely updates the UI and either triggers the receipt printer or shows a decline error.

---

## 🛠️ Tech Stack & Architecture

Built adhering to **Modern Android Development (MAD)** standards.

### 🏗️ Architecture
* **Clean Architecture** with **MVVM (Model-View-ViewModel)**.
* **Presentation Layer:** Jetpack Compose (Declarative UI), StateFlow.
* **Domain Layer:** UseCases, Entities (Pure Kotlin Business Logic).
* **Data Layer:** Repositories, Firebase, SharedPreferences.

### 🚀 Libraries
* **Jetpack Compose:** Reactive UI.
* **Hilt (Dagger):** Dependency Injection.
* **Firebase:** Firestore (Real-time DB), Crashlytics, Analytics.
* **Coroutines & StateFlow:** Asynchronous programming.
* **ZXing:** TLV QR Code generation.

---

## 📸 System Showcase
<div align="center">

| | | |
|:---:|:---:|:---:|
| <img src="screens/1.jpg" width="250" /> | <img src="screens/2.jpg" width="250" /> | <img src="screens/3.jpg" width="250" /> |
| <img src="screens/4.jpg" width="250" /> | <img src="screens/5.jpg" width="250" /> | <img src="screens/6.jpg" width="250" /> |
| <img src="screens/7.jpg" width="250" /> | <img src="screens/8.jpg" width="250" /> | <img src="screens/9.jpg" width="250" /> |
| <img src="screens/10.jpg" width="250" /> | <img src="screens/11.jpg" width="250" /> | <img src="screens/12.jpg" width="250" /> |
| <img src="screens/13.jpg" width="250" /> | <img src="screens/14.jpg" width="250" /> | <img src="screens/15.jpg" width="250" /> |
| <img src="screens/16.jpg" width="250" /> | <img src="screens/17.jpg" width="250" /> | <img src="screens/18.jpg" width="250" /> |
| <img src="screens/19.jpg" width="250" /> | <img src="screens/20.jpg" width="250" /> | <img src="screens/21.jpg" width="250" /> |
| <img src="screens/22.jpg" width="250" /> | <img src="screens/23.jpg" width="250" /> | <img src="screens/24.jpg" width="250" /> |
| <img src="screens/25.jpg" width="250" /> | <img src="screens/26.jpg" width="250" /> | <img src="screens/27.jpg" width="250" /> |
| <img src="screens/28.jpg" width="250" /> | <img src="screens/29.jpg" width="250" /> | <img src="screens/30.jpg" width="250" /> |
| <img src="screens/31.jpg" width="250" /> | <img src="screens/32.jpg" width="250" /> | <img src="screens/33.jpg" width="250" /> |
| <img src="screens/34.jpg" width="250" /> | <img src="screens/35.jpg" width="250" /> | <img src="screens/36.jpg" width="250" /> |
| <img src="screens/37.jpg" width="250" /> | <img src="screens/38.jpg" width="250" /> | <img src="screens/39.jpg" width="250" /> |
| <img src="screens/40.jpg" width="250" /> | <img src="screens/41.jpg" width="250" /> | <img src="screens/42.png" width="250" /> |

</div>
---

## 🤝 White-Label & Commercial Deployment
This system architecture is heavily decoupled, making it extremely easy to **White-Label** for multiple clients. 
By altering `strings.xml`, `Theme.kt`, and swapping the `google-services.json`, a completely new branded POS system can be generated and deployed to a new client in under 15 minutes.

## 👤 Contact

**Eslam Ali Atta Rezq** - Android Software Engineer  
* Email: [eslameng776@gmail.com]  
* LinkedIn: [www.linkedin.com/in/eslam-ali-b0b874195]  
---
*Developed with passion to solve real operational bottlenecks in the F&B and hospitality industry.*
