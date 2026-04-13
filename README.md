# 💧 AquaTrack — Water Can Service Management App

A **mobile-first Progressive Web App (PWA)** for managing your water can delivery business.

---

## ✅ Features

| Feature | Details |
|---|---|
| **Vendor Management** | Name, phone, address, service tenure, notes |
| **Delivery Logging** | Cans given, empties returned, date, can type |
| **Pricing Engine** | Multiple can types, price history with effective dates |
| **Reports** | Monthly summary by vendor & can type |
| **Dashboard** | Real-time stats: revenue, cans, pending empties |
| **Android Install** | Works as a PWA — installable on Android home screen |

---

## 🚀 How to Run

### Option 1: Open Directly (Quickest)
Just open `index.html` in Chrome on your phone. Then:
1. Tap the **3-dot menu** (⋮) → **"Add to Home Screen"**
2. The app installs like a native Android app!

### Option 2: Host Locally (For Network Access)
```bash
# Python (most systems have this)
python3 -m http.server 8080

# Then open on phone: http://YOUR_PC_IP:8080
```

### Option 3: Deploy Online (Best for Production)
Upload to any static hosting:
- **Netlify** (free): Drag & drop the folder at netlify.com
- **GitHub Pages** (free): Push to repo → enable Pages
- **Firebase Hosting**: `firebase deploy`

---

## 📱 Android App (Full Native — Future Step)

To convert to a real APK for Play Store using **Capacitor** (100% free & open source):

```bash
npm install -g @capacitor/cli
npm init
npm install @capacitor/core @capacitor/android
npx cap init "AquaTrack" "com.aquatrack.app"
npx cap add android
# Copy index.html to www/ folder
npx cap sync android
npx cap open android
# Build APK in Android Studio
```

---

## 🗄️ Database (MySQL — Production Ready)

The current version uses **localStorage** as the database (works offline, no setup needed).

To migrate to **MySQL** (recommended for multi-device / multi-user):

### Recommended Stack
| Layer | Technology | Why |
|---|---|---|
| **Database** | MySQL 8.x | Free, reliable, your choice |
| **Backend API** | Node.js + Express OR PHP Laravel | Easy to learn |
| **Frontend** | This HTML/JS (unchanged) | Just change DB calls |
| **App** | Capacitor wrapping this frontend | Native Android |

### MySQL Schema
```sql
CREATE DATABASE aquatrack;
USE aquatrack;

CREATE TABLE vendors (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  phone VARCHAR(15) NOT NULL,
  address TEXT,
  service_start DATE,
  service_end DATE,
  notes TEXT,
  color TINYINT DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE can_prices (
  id INT AUTO_INCREMENT PRIMARY KEY,
  can_type VARCHAR(100) NOT NULL,
  price DECIMAL(10,2) NOT NULL,
  effective_from DATE NOT NULL,
  notes TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE deliveries (
  id INT AUTO_INCREMENT PRIMARY KEY,
  vendor_id INT NOT NULL,
  delivery_date DATE NOT NULL,
  can_type VARCHAR(100) NOT NULL,
  cans_given INT DEFAULT 0,
  empties_returned INT DEFAULT 0,
  price_per_can DECIMAL(10,2) NOT NULL,
  total_amount DECIMAL(10,2) NOT NULL,
  remarks TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (vendor_id) REFERENCES vendors(id) ON DELETE CASCADE
);

-- Indexes for performance
CREATE INDEX idx_deliveries_date ON deliveries(delivery_date);
CREATE INDEX idx_deliveries_vendor ON deliveries(vendor_id);
CREATE INDEX idx_prices_type ON can_prices(can_type, effective_from);
```

---

## 🔮 Future Features (Easy to Add)
- [ ] **WhatsApp/SMS reminders** — using Twilio or WhatsApp Business API
- [ ] **Receipt printing** — PDF generation per delivery
- [ ] **Payment tracking** — paid / pending / partial
- [ ] **Route planning** — delivery order on a map
- [ ] **Multi-user** — separate logins for drivers and admin
- [ ] **Subscription billing** — monthly auto-invoicing
- [ ] **Photo capture** — take photo of empty can handover
- [ ] **Backup to Google Drive** — auto export

---

## 💡 Data Storage

**Current**: Browser `localStorage` (no server needed, works offline)
**Next step**: Replace `DB.get()` and `DB.set()` calls in `index.html` with `fetch()` API calls to your MySQL backend — the UI stays exactly the same.

---

*Built with ❤️ — Pure HTML/CSS/JS, no frameworks needed for basic use.*
# aquatrack
