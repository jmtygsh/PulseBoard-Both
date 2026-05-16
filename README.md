# PulseBoard - Live Polls For Feedback

<!-- Add your image here -->
![PulseBoard Banner](link-to-your-image.png)

PulseBoard is a full-stack web application that allows users to create, share, and vote on live polls in real-time. It features a comprehensive dashboard for poll management, real-time analytics using WebSockets, and public poll discovery.

## 🚀 Quick Access for Reviewers

To quickly explore the application without registering, use the following test credentials:
- **Email:** `ygshtest@gmail.com`
- **Password:** `Chaiaurcode26`

---

## 💻 Tech Stack

### Frontend (Client)
- **Framework:** React.js (Vite)
- **Routing:** React Router v6
- **Styling:** Tailwind CSS & Shadcn UI
- **Real-time:** Socket.io-client
- **Charts:** Recharts
- **HTTP Client:** Axios
- **Icons:** Lucide React

### Backend (Server)
- **Runtime:** Node.js
- **Framework:** Express.js
- **Language:** TypeScript
- **Database:** MongoDB (via Mongoose)
- **Real-time:** Socket.io
- **Authentication:** JSON Web Tokens (JWT) & bcrypt
- **Validation:** Zod

---

## 📁 File Structure

### Frontend Structure
```
PulseBoard-Client/
├── src/
│   ├── api/             # Axios configuration and interceptors
│   ├── components/      # Reusable UI components (Shadcn, Header, etc.)
│   ├── constants/       # Global constants (e.g., Colors)
│   ├── hooks/           # Custom React hooks (useAuth)
│   ├── lib/             # Utility functions (Tailwind merge)
│   ├── pages/           # Main route components (Dashboard, CreatePoll, etc.)
│   ├── App.jsx          # Root component
│   └── main.jsx         # Entry point and Router configuration
```

### Backend Structure
```
PulseBoard/
├── src/
│   ├── common/          # Shared utilities across the app
│   │   ├── config/      # DB, CORS, Email, and Socket configurations
│   │   ├── middleware/  # Auth, validation, and error handling
│   │   └── utils/       # JWT helpers, hashing, custom API Error classes
│   ├── modules/         # Feature-based modular architecture
│   │   ├── auth/        # Auth logic, routes, controllers, and Zod DTOs
│   │   └── poll/        # Poll logic, models, controllers, and routes
│   ├── app.ts           # Express app setup and middleware wiring
│   └── server.ts        # Server entry point and DB connection
```

---

## 🛡️ Edge Cases Handled

1. **Graceful Error Boundaries:** Implemented both `react-error-boundary` for component crashes and React Router's `errorElement` to elegantly handle 404 routes without breaking the UI.
2. **Soft Deletion:** Polls are never permanently wiped from the database. Instead, an `isDeleted` flag is toggled, preventing broken relations while immediately hiding the poll from the UI and API responses.
3. **Poll Expiration:** Backend dynamically validates the `expiresAt` timestamp against the current date. If a poll is expired, the backend blocks submissions (409 Conflict) and the frontend disables the voting UI while closing the WebSocket room to save server resources.
4. **Resilient Registration:** If the SMTP server fails to send a verification email during registration, the system catches the error and still creates the user account so the user is not permanently locked out.
5. **Anonymous vs Authenticated Voting:** Polls can be configured to require authentication. If open to the public, the frontend generates a persistent `anonymousId` stored in `localStorage` to enforce a 1-vote-per-user rule without requiring an account.
6. **Rolling Sessions:** Secure HTTP-only cookies are used for refresh tokens. Every time a refresh token is used, a new one is issued, extending the session seamlessly while maintaining high security.
7. **CORS Flexibility:** Dynamically supports multiple allowed origins, ensuring smooth local development (`localhost`) while strictly securing production requests.