# 🎟️ Ticket Booking Backend

A **Ticket Master Clone** — a RESTful backend API for event ticket booking, built with **FastAPI**, **SQLModel**, and **MySQL**. It supports full CRUD operations across users, events, venues, seats, payments, orders, and tickets, along with advanced analytics queries.

---

## 🚀 Tech Stack

| Layer | Technology |
|---|---|
| Framework | FastAPI 0.115 |
| ORM | SQLModel 0.0.22 + SQLAlchemy 2.0 |
| Database | MySQL (via PyMySQL) |
| Auth / Hashing | Passlib (bcrypt) |
| Migrations | Alembic |
| Server | Uvicorn |
| Validation | Pydantic v2 |

---

## 📁 Project Structure

```
ticket_booking_backend/
├── main.py          # FastAPI app entrypoint, CORS config, lifespan
├── api.py           # All API route definitions (APIRouter)
├── models.py        # SQLModel table definitions (ORM schemas)
├── crud.py          # Database operations (create, read, update, delete)
├── database.py      # DB engine setup, session management
├── config.py        # Configuration / environment variables
└── requirements.txt # Python dependencies
```

---

## 🗂️ Data Models

The application manages the following entities:

- **Role** — user roles (e.g. admin, customer)
- **User** — registered users with hashed passwords, roles, and profile info
- **Category / SubCategory** — event classification (e.g. Music > Rock)
- **Venue** — physical locations with capacity and contact details
- **Event** — events tied to a venue and category, with ticket availability tracking
- **Seat** — individual seats within a venue
- **Payment** — payment records with method and status
- **Order** — a user's purchase, linking to payment
- **Ticket** — individual tickets linked to an order and seat
- **UserEvent** — many-to-many relationship tracking which users attend which events

---

## 🔌 API Endpoints

Interactive docs are available at `http://localhost:8000/docs` once the server is running.

### Auth
| Method | Endpoint | Description |
|---|---|---|
| POST | `/login` | Login with email and password |

### Users
| Method | Endpoint | Description |
|---|---|---|
| POST | `/users/` | Create a user |
| GET | `/users/` | List all users |
| GET | `/users/{user_id}` | Get user by ID |
| PUT | `/users/{user_id}` | Update user |
| DELETE | `/users/{user_id}` | Delete user |

### Roles
| Method | Endpoint | Description |
|---|---|---|
| POST | `/roles/` | Create a role |
| GET | `/roles/` | List all roles |
| GET | `/roles/{role_id}` | Get role by ID |
| PUT | `/roles/{role_id}` | Update role |
| DELETE | `/roles/{role_id}` | Delete role |

### Categories & SubCategories
| Method | Endpoint | Description |
|---|---|---|
| POST/GET/PUT/DELETE | `/categories/` | CRUD for categories |
| POST/GET/PUT/DELETE | `/subcategories/` | CRUD for subcategories |

### Venues
| Method | Endpoint | Description |
|---|---|---|
| POST/GET/PUT/DELETE | `/venues/` | CRUD for venues |

### Events
| Method | Endpoint | Description |
|---|---|---|
| POST | `/events/` | Create an event |
| GET | `/events` | List all events |
| GET | `/events/{event_id}` | Get event by ID |
| GET | `/events/byCategory/{category_id}` | Filter events by category |
| PUT | `/events/{event_id}` | Update event |
| DELETE | `/events/{event_id}` | Delete event |

### Seats, Payments, Orders, Tickets
Full CRUD is available for each: `/seats/`, `/payments/`, `/orders/`, `/tickets/`

### User–Event Association
| Method | Endpoint | Description |
|---|---|---|
| POST | `/user_events/` | Link a user to an event |
| GET | `/user_events/{user_id}` | Get events for a user |
| GET | `/event_list_by_user_id/{user_id}` | Full event details for a user |
| PUT | `/user_events/{user_id}/{event_id}` | Update user–event record |

### Analytics
| Method | Endpoint | Description |
|---|---|---|
| GET | `/get_event_sales_summary` | Sales summary per event |
| GET | `/users_without-tickets` | Users who haven't booked any tickets (set difference / subquery) |
| GET | `/events_low-tickets` | Events with low ticket availability (set comparison) |
| GET | `/events_highest-revenue` | Highest revenue events (WITH clause + window functions) |

---

## ⚙️ Setup & Installation

### Prerequisites
- Python 3.10+
- MySQL database

### 1. Clone the repository
```bash
git clone https://github.com/Ayush29godara/ticket_booking_backend.git
cd ticket_booking_backend
```

### 2. Create and activate a virtual environment
```bash
python -m venv venv
source venv/bin/activate      # macOS/Linux
venv\Scripts\activate         # Windows
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure environment variables

Create a `.env` file in the project root:

```env
DATABASE_URL=mysql+pymysql://<user>:<password>@<host>/<dbname>
```

### 5. Run the application
```bash
uvicorn main:app --reload
```

The API will be available at `http://localhost:8000`.

---

## 📖 API Documentation

FastAPI auto-generates interactive documentation:

- **Swagger UI** → `http://localhost:8000/docs`
- **ReDoc** → `http://localhost:8000/redoc`

---

## 🌐 CORS

The server allows cross-origin requests from:
- `http://localhost:3000` (React default)
- `http://localhost:5173` (Vite default)

To add other origins, update the `origins` list in `main.py`.

---

## 🔐 Security

- Passwords are hashed using **bcrypt** via `passlib`.
- The `User.hash_password()` and `User.verify_password()` static methods handle all password operations.
- No JWT tokens are implemented in the current version — authentication returns the user object on successful login.

---

## 📦 Key Dependencies

```
fastapi==0.115.3
sqlmodel==0.0.22
SQLAlchemy==2.0.36
PyMySQL==1.1.1
uvicorn==0.32.0
passlib==1.7.4
alembic==1.13.3
pydantic==2.9.2
python-dotenv==1.0.1
```

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'Add your feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 📄 License

This project is open source. Feel free to use and modify it.
