# MealMatch

**MealMatch** is a web application designed to reduce food waste by connecting restaurants and food shops with NGOs and volunteers. Restaurants can list their surplus food, NGOs can claim it for distribution, and volunteers can help deliver it — ensuring that leftover food reaches people or animals in need instead of being wasted.

---

## Problem Statement

Every day, restaurants and food shops throw away large amounts of perfectly edible food. At the same time, NGOs working with underprivileged communities and street animals often struggle to source food. MealMatch bridges this gap by creating a structured platform for food donation, collection, and delivery.

---

## Features

### For Restaurants / Food Shops
- Sign up and create a profile with location details
- List surplus food donations with description, quantity, food preference (people or animals), and expiry date
- Track the status of each donation (Pending → Accepted → Delivered)
- Cancel pending donations if needed
- View donation history

### For NGOs
- Sign up as either a **People-focused NGO** or an **Animal-focused NGO**
- Browse available donations that match their focus area and service location
- Accept a donation and assign an available volunteer for delivery
- Track all accepted and delivered donations

### For Volunteers
- Sign up and register a service area
- View donations assigned to them by NGOs
- Mark donations as delivered by uploading a photo proof and optional feedback
- Track completed deliveries

### General
- Role-based authentication and session management
- Public and private profile pages showing total donation counts
- Responsive UI built with Bootstrap 5

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, Flask |
| Database ORM | Flask-SQLAlchemy |
| Database | SQLite |
| Frontend | HTML, Jinja2 Templates, Bootstrap 5.3 |
| Session Management | Flask Sessions |

---

## Project Structure

```
Meal-Match/
│
├── app.py              # Main Flask application with all routes
├── models.py           # SQLAlchemy database models
├── init_db.py          # Script to initialize the database
├── requirements.txt    # Python dependencies
│
├── templates/
│   ├── bootstrap.html  # Base template with navbar (inherited by all pages)
│   ├── index.html      # Landing page
│   ├── login.html      # Login page
│   ├── signup.html     # Registration page (for all roles)
│   ├── restaurant.html # Restaurant dashboard
│   ├── ngo.html        # NGO dashboard
│   ├── volunteer.html  # Volunteer dashboard
│   ├── volunteers.html # Volunteer selection page (for NGOs)
│   └── profile.html    # User profile page
│
└── instance/
    └── users.db        # SQLite database (auto-generated)
```

---

## Database Models

- **User** — Stores all user accounts with roles: `restaurant`, `ngo`, or `volunteer`
- **Restaurant** — Extended profile for restaurant users with name and address
- **NGO** — Extended profile for NGOs with `focus_area` (people/animal) and `service_area`
  - **StreetAnimalNGO** — Subclass of NGO focused on street animals
  - **NeedyPeopleNGO** — Subclass of NGO focused on needy people
- **Volunteer** — Extended profile for volunteers with service area and availability
- **Donation** — Represents a food donation with description, quantity, preference, expiry date, and a status string (`pending`, `accepted,{ngo_id},{volunteer_id}`, `delivered,{ngo_id},{volunteer_id}`)
- **DeliveryProof** — Stores delivery photo and feedback submitted by volunteers

---

## Installation & Setup

### Prerequisites
- Python 3.8+
- pip

### Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/arpitatinni/Meal-Match.git
   cd Meal-Match
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Initialize the database**
   ```bash
   python init_db.py
   ```

4. **Run the application**
   ```bash
   python app.py
   ```

5. **Open in browser**
   ```
   http://localhost:6969
   ```

---

## How It Works — User Flow

```
Restaurant signs up → Lists surplus food with preference (People/Animals)
        ↓
NGO (matching focus area & location) sees the donation → Accepts it → Selects a Volunteer
        ↓
Volunteer is notified → Picks up food from restaurant → Delivers it → Uploads proof photo
        ↓
Donation status updated to "Delivered"
```

---

## Routes Overview

| Route | Method | Description |
|---|---|---|
| `/` | GET | Landing page |
| `/signup` | GET, POST | User registration |
| `/login` | GET, POST | User login |
| `/logout` | GET | Logout and clear session |
| `/dashboard` | GET | Redirects to role-specific dashboard |
| `/dashboard/restaurant` | GET, POST | Restaurant dashboard |
| `/dashboard/ngo` | GET | NGO dashboard |
| `/dashboard/volunteer` | GET | Volunteer dashboard |
| `/cancel_donation/<id>` | GET | Cancel a pending donation |
| `/choose_volunteer/<donation_id>` | GET | NGO selects a volunteer for a donation |
| `/accept_donation/<donation_id>/<volunteer_id>` | GET | NGO confirms volunteer assignment |
| `/deliver_donation/<donation_id>` | POST | Volunteer marks donation as delivered |
| `/profile` | GET, POST | View/edit own profile |
| `/profile/<role>/<id>` | GET | View public profile |

---

## User Roles

| Role | What They Do |
|---|---|
| **Restaurant** | Posts surplus food donations |
| **NGO (People)** | Claims food for underprivileged humans |
| **NGO (Animal)** | Claims food for street animals |
| **Volunteer** | Delivers food from restaurants to NGOs |

---

## Dependencies

```
Flask==3.0.0
Flask-SQLAlchemy==3.1.1
Werkzeug==3.0.1
```

---
