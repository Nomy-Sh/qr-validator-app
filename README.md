# 🎫 QR-Based Event Ticket Validation System

A full-stack Ruby on Rails web application for digitizing event ticket validation through real-time QR code scanning. Built to handle large-scale events with 10,000+ attendees and tiered access control.

![Ruby](https://img.shields.io/badge/Ruby-3.0.0-red)
![Rails](https://img.shields.io/badge/Rails-7.0.3-red)
![Firebase](https://img.shields.io/badge/Firebase-Realtime%20DB-orange)
![License](https://img.shields.io/badge/License-MIT-blue)

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [API Endpoints](#api-endpoints)
- [QR Code Generation](#qr-code-generation)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

This application was developed for professional event managers to replace manual ticket verification with an automated, scalable digital system. It provides real-time QR code validation, multi-user support for parallel entry checkpoints, and comprehensive analytics for event management.

### Business Problem Solved
- **Manual ticket checking** was time-consuming and error-prone
- **Duplicate entry fraud** was difficult to prevent
- **Real-time attendance metrics** were unavailable
- **Multiple entry points** couldn't be synchronized

### Solution Delivered
- ✅ 80% reduction in entry processing time
- ✅ Zero-tolerance duplicate ticket validation
- ✅ Real-time synchronized validation across checkpoints
- ✅ Comprehensive scan history and analytics

## ✨ Features

### Core Functionality
- 📱 **Real-time QR Code Scanning** - Browser-based camera access for instant ticket validation
- 🎟️ **Tiered Ticket System** - Gold, Silver, and Bronze categories with individual access rules
- 👥 **Multi-user Support** - Concurrent validation across multiple entry checkpoints
- 🔒 **Duplicate Prevention** - Centralized validation prevents re-entry with same QR code
- 📊 **Analytics Dashboard** - Real-time scan statistics and validator activity tracking
- 📱 **Progressive Web App** - Offline-capable, mobile-optimized interface
- 🔐 **Secure Authentication** - Firebase authentication with email/password login

### User Features
- **Scanner Interface** - Clean, intuitive QR scanning with visual feedback (green/red indicators)
- **Profile Dashboard** - Personal scan count and activity statistics
- **History Tracking** - Complete scan history with timestamps and ticket metadata
- **Settings** - Camera preferences and user customization
- **Responsive Design** - Works seamlessly on mobile devices and tablets

## 🛠️ Tech Stack

### Backend
- **Ruby** 3.0.0
- **Ruby on Rails** 7.0.3
- **Firebase Realtime Database** - Real-time data synchronization
- **Firebase Authentication** - User authentication and session management
- **SQLite3** - Local database (development)

### Frontend
- **HTML5/CSS3** - Semantic markup and styling
- **JavaScript (ES6+)** - QR scanner integration
- **Bootstrap 5** - Responsive UI framework
- **jQuery** - DOM manipulation and AJAX calls
- **Stimulus** - Modest JavaScript framework

### Additional Tools
- **Python 3.x** - QR code bulk generation script
- **PIL (Pillow)** - Image manipulation for ticket stamping
- **Puma** - Web server
- **Turbo Rails** - SPA-like page accelerator

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

- **Ruby** 3.0.0 or higher ([Install Ruby](https://www.ruby-lang.org/en/documentation/installation/))
- **Rails** 7.0.3 or higher
- **Node.js** and **npm** (for JavaScript dependencies)
- **SQLite3** (for development database)
- **Python** 3.x (for QR code generation scripts)
- **Git** (for version control)

### Firebase Setup
You'll need a Firebase project with:
- Firebase Realtime Database enabled
- Firebase Authentication enabled (Email/Password provider)
- Firebase Web API Key

## 🚀 Installation

### 1. Clone the Repository
```bash
git clone https://github.com/Nomy-Sh/qr-validator-app.git
cd qr-validator-app
```

### 2. Install Ruby Dependencies
```bash
bundle install
```

### 3. Install JavaScript Dependencies
```bash
npm install
# or
yarn install
```

### 4. Set Up Environment Variables

Create a `.env` file in the root directory (or use Rails credentials):

```bash
# .env
firebase_web_api_key=your_firebase_web_api_key
firebase_database_url=https://your-project.firebaseio.com
firebase_user_db_root_path=/users
firebase_qr_db_root_path=/qr_codes
```

**How to get Firebase credentials:**
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select your project (or create a new one)
3. Go to Project Settings → General
4. Scroll to "Your apps" and copy the Web API Key
5. Go to Realtime Database and copy the database URL

### 5. Set Up Database
```bash
rails db:create
rails db:migrate
rails db:seed  # Optional: seed initial data
```

### 6. Configure Firebase Realtime Database

In your Firebase Console, set up the following database structure:

```json
{
  "users": {
    "user_id_1": {
      "name": "John Doe",
      "email": "john@example.com",
      "qr_scan_count": 0,
      "cam_pref": "back"
    }
  },
  "qr_codes": {
    "qr_code_hash_1": {
      "status": 0,
      "sr-no": 1,
      "scanned-at": null,
      "scanned-by": null
    }
  }
}
```

**Database Security Rules (for development):**
```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

## ⚙️ Configuration

### Firebase Authentication Setup

1. Enable Email/Password authentication in Firebase Console:
   - Go to Authentication → Sign-in method
   - Enable "Email/Password" provider
   - Optionally enable Google Sign-In

2. (Optional) Enable Google OAuth:
   - Add Google as a sign-in provider
   - Configure OAuth consent screen
   - Update authentication controller accordingly

### Environment-Specific Settings

**Development (config/environments/development.rb):**
- Debug mode enabled
- Hot reloading active
- Verbose logging

**Production (config/environments/production.rb):**
- Asset precompilation required
- Use production Firebase database
- Enable caching and compression

## 🎮 Usage

### Starting the Development Server

```bash
rails server
# or
rails s
```

Visit `http://localhost:3000` in your browser.

### User Workflow

#### 1. Sign Up / Login
- Navigate to `/signup` or `/login`
- Create an account with email and password
- Or sign in with existing credentials

#### 2. Scan Tickets
- Click on "Scanner" in the navigation
- Allow camera access when prompted
- Point camera at QR code
- Wait for validation result (green = approved, red = rejected/duplicate)

#### 3. View History
- Click on "History" to see all scanned tickets
- Filter by date, status, or ticket type
- Export scan data (if implemented)

#### 4. Profile & Settings
- View total scan count
- Update camera preferences (front/back)
- Manage account settings

### Admin Workflow

#### View All QR History
```ruby
# Access via console or admin panel
QrHistoryController.get_all_qr_history
```

## 📁 Project Structure

```
qr-validator-app/
├── app/
│   ├── assets/           # CSS, images, JavaScript assets
│   ├── channels/         # Action Cable channels
│   ├── controllers/      # Application controllers
│   │   ├── application_controller.rb
│   │   ├── home_controller.rb          # Authentication
│   │   ├── qr_codes_controller.rb      # QR validation
│   │   └── qr_codes_base_controller.rb # Base QR logic
│   ├── helpers/          # View helpers
│   ├── javascript/       # JavaScript files
│   ├── models/           # Application models
│   │   ├── qr_code.rb                  # QR code model
│   │   └── user.rb                     # User model
│   └── views/            # HTML templates
│       ├── home/                        # Login/signup views
│       └── qr_codes/                    # Scanner, history, profile
├── config/               # Configuration files
│   ├── routes.rb         # Application routes
│   ├── database.yml      # Database configuration
│   └── environments/     # Environment-specific config
├── db/                   # Database files
│   ├── migrate/          # Migration files
│   └── seeds.rb          # Seed data
├── public/               # Static files
├── Gemfile               # Ruby dependencies
├── package.json          # JavaScript dependencies
└── README.md             # This file
```

## 🔌 API Endpoints

### Authentication
- `GET /login` - Login page
- `POST /login` - Login action
- `GET /signup` - Signup page
- `POST /signup` - Signup action
- `GET /logout` - Logout action

### QR Code Validation
- `GET /scanner` - Scanner interface
- `GET /qr_validate?qr_code=<code>` - Validate QR code
- `GET /qr_approve_ticket` - Approve scanned ticket
- `GET /qr_reject_ticket` - Reject scanned ticket

### User Dashboard
- `GET /profile` - User profile and stats
- `GET /history` - Scan history page
- `GET /history_data` - Fetch history data (JSON)
- `GET /settings` - User settings

### History & Analytics
- `POST /get_crnt_usr_qr_history` - Get current user's scan history
- `POST /get_all_qr_history` - Get all QR scan history (admin)

## 🎨 QR Code Generation

### Python Script for Bulk QR Generation

The project includes Python automation for generating 10,000+ unique QR codes and stamping them on printable tickets.

**Requirements:**
```bash
pip install pillow qrcode
```

**Example Script (scripts/generate_qr_codes.py):**
```python
import qrcode
from PIL import Image, ImageDraw
import os

# Generate QR codes for three tiers
tiers = {
    'Gold': 3000,
    'Silver': 4000,
    'Bronze': 3000
}

for tier, count in tiers.items():
    for i in range(1, count + 1):
        # Generate unique QR data
        qr_data = f"{tier.upper()}-{i:05d}-{hash_function()}"

        # Create QR code
        qr = qrcode.QRCode(version=1, box_size=10, border=5)
        qr.add_data(qr_data)
        qr.make(fit=True)

        # Generate image
        img = qr.make_image(fill_color="black", back_color="white")

        # Stamp on ticket template
        ticket = Image.open(f'templates/{tier}_template.png')
        ticket.paste(img, (x_position, y_position))

        # Save stamped ticket
        ticket.save(f'output/{tier}/{tier}_{i:05d}.png')

        print(f"Generated {tier} ticket {i}/{count}")
```

### Saving QR Codes to Firebase

After generation, upload QR codes to Firebase Realtime Database:

```ruby
# seeds.rb or custom script
require 'firebase'

firebase = Firebase::Client.new(ENV['firebase_database_url'])

qr_codes = [
  { code: 'GOLD-00001-hash', status: 0, sr_no: 1 },
  # ... more codes
]

qr_codes.each do |qr|
  firebase.set("qr_codes/#{qr[:code]}", qr)
end
```

## 🚢 Deployment

### Heroku Deployment

1. **Create Heroku app:**
```bash
heroku create your-app-name
```

2. **Set environment variables:**
```bash
heroku config:set firebase_web_api_key=your_key
heroku config:set firebase_database_url=your_url
```

3. **Deploy:**
```bash
git push heroku main
```

4. **Run migrations:**
```bash
heroku run rails db:migrate
```

### Other Platforms

- **AWS Elastic Beanstalk** - Use `.ebextensions` for configuration
- **DigitalOcean App Platform** - Push from GitHub repo
- **Railway** - Connect GitHub and deploy

**Important:** Ensure environment variables are set on your hosting platform!

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Development Guidelines
- Follow Ruby Style Guide
- Write meaningful commit messages
- Add tests for new features
- Update documentation as needed

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Noman Sheikh**
- GitHub: [@Nomy-Sh](https://github.com/Nomy-Sh)
- LinkedIn: [Noman Sheikh](https://www.linkedin.com/in/noman-sheikh)

## 🙏 Acknowledgments

- Firebase for real-time database and authentication
- Bootstrap team for the UI framework
- Ruby on Rails community for excellent documentation
---

**Built with ❤️ for seamless event management**
