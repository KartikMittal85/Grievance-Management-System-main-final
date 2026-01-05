Grievance Management System – Deep Explanation

This project is a role-based Grievance Management System built using Flask (Python web framework). It allows users to register, submit grievances, track status, while admins and VIPs can review, manage, and resolve complaints through dashboards.

1️⃣ Overall Architecture

The project follows the MVC-style structure (Model–View–Controller):

Layer	Role	Files
Model	Database schema & ORM	models.py
View	UI (HTML/CSS/JS)	templates/, static/
Controller	Business logic & routing	app.py

Flask acts as the controller, handling HTTP requests and coordinating between database models and templates.

2️⃣ Backend (Flask Application)
📌 app.py – Core of the System

This is the brain of the application.

a) Flask App Initialization

Creates Flask instance

Configures:

Secret key (sessions & security)

Database URI (SQLite)

Connects SQLAlchemy with Flask

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///grievance.db'

b) User Authentication System

Login & Signup

Users register with:

Username

Password

Role (student/admin/VIP)

Passwords are stored securely (usually hashed)

Session management keeps users logged in

Flow:

User → Login Page → Credentials Verified → Redirect to Dashboard

c) Role-Based Access Control (RBAC)

Different users see different dashboards:

Role	Permissions
Student	Submit & track grievances
Admin	View & update grievances
VIP	Priority handling & overview

Flask routes check role before allowing access:

if session['role'] != 'admin':
    redirect('login')

d) Grievance Submission Logic

When a student submits a grievance:

Form data is collected

Stored in database with:

Title

Description

Category

Status (default: Pending)

User ID

This ensures traceability and accountability.

e) Grievance Tracking

Each grievance has:

Unique ID

Status: Pending / In Progress / Resolved

Students can:

Enter grievance ID

View live status updates

f) Admin / VIP Management

Admins can:

View all grievances

Update grievance status

Add remarks or decisions

VIP dashboard usually provides:

High-priority grievances

Overview of grievance statistics

3️⃣ Database Layer (models.py)
📌 SQLAlchemy ORM

Instead of writing raw SQL, the project uses ORM (Object Relational Mapping).

a) User Model

Stores:

User ID

Username

Password

Role

class User(db.Model):
    id
    username
    password
    role

b) Grievance Model

Stores:

Grievance ID

User ID (Foreign Key)

Title

Description

Category

Status

Timestamp

class Grievance(db.Model):
    id
    user_id
    status
    created_at

c) Why ORM?

✔ Safer (prevents SQL injection)
✔ Easier to maintain
✔ Database-independent

4️⃣ Database Initialization
📌 create_db.py

Creates database tables

Runs once during setup

python create_db.py


SQLite is used because:

Lightweight

No server required

Ideal for academic projects

5️⃣ Frontend Layer
📂 templates/ (HTML + Jinja2)

Flask uses Jinja2 templating, which allows:

Dynamic data rendering

Control structures (for, if)

Template inheritance

Example:

{% for grievance in grievances %}
<tr>{{ grievance.status }}</tr>
{% endfor %}

Key Pages Explained
Page	Purpose
login.html	Authentication
signup.html	User registration
student_dashboard.html	Student view
submit_grievance.html	Complaint form
track_grievance.html	Status tracking
admin_dashboard.html	Admin controls
vip_dashboard.html	VIP oversight
6️⃣ Static Files
📂 static/css/style.css

Controls UI styling

Improves readability & layout

📂 static/js/script.js

Client-side interactions

Form validation

UI enhancements

7️⃣ Application Flow (End-to-End)
User Registers
      ↓
User Logs In
      ↓
Role-Based Dashboard
      ↓
Submit Grievance
      ↓
Stored in Database
      ↓
Admin Reviews
      ↓
Status Updated
      ↓
User Tracks Grievance

8️⃣ Security Aspects

✔ Session-based authentication
✔ Role validation
✔ ORM prevents SQL injection
✔ Secret key for session security

(Password hashing can be improved using werkzeug.security)

9️⃣ Why This Project is Important
Real-World Use Cases

Colleges & Universities

Government grievance portals

Corporate HR complaint systems

Academic Value

Demonstrates:

Web development

Database design

Role-based systems

MVC architecture

🔟 Possible Improvements (Advanced)

Password hashing (bcrypt)

Email notifications

REST API version

Cloud deployment

Graphical analytics dashboard

JWT authentication
