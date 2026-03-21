# Makan Island 🏝️

## 📌 About The Project
Makan Island is a responsive, interactive front-end web application that serves as a guide for discovering food, restaurants, and top dining spots across different Malaysia states—including Kedah, Penang, Kelantan, and Perak. 

Discover bold flavors, hidden gems, and trending bites from the streets you love. Every click brings you closer to your next favorite dish.

### ✨ Key Features
- **State-based Food Exploration**: Discover specific local foods and restaurants in different states (Kedah, Penang, Kelantan, Perak).
- **Interactive GSAP Animations**: Rich, smooth scroll and text animations powered by GSAP natively integrated.
- **Restaurant Rankings & Favorites**: Keep track of the best places to eat with the ranking and favorites pages.
- **Quizzes & Engagement**: Interactive content to keep users engaged and test their food knowledge.
- **User Authentication**: Secure Login/Signup with both Local Storage and Google Identity Services fallback.
- **Custom Theming & Responsive Design**: Designed to work flawlessly on both desktop and mobile devices.

### 🛠️ Built With
- **HTML5 & CSS3** (Custom Grid/Flexbox Layouts)
- **Vanilla JavaScript** (No heavy frontend frameworks)
- **GSAP (GreenSock Animation Platform)** for transitions and scroll triggers
- **Google Identity Services API** for authentication

---

## 🚀 Getting Started

To run this project locally:

1. Clone or download the repository.
2. Open the `src/index.html` file in your preferred web browser. Alternatively, serve the `src/` directory using a local development server like VSCode's Live Server.
3. No build steps (like `npm run build`) are strictly required, as the project leverages vanilla web technologies and includes local minified GSAP scripts.

---

# Login Page System Summary

## 📌 Overview
This web page provides a simple login system with two methods:
1. **Local Account Login** (username + password)
2. **Google Sign-In** (via Google Identity Services API)

Users can also create accounts locally through a sign-up modal. After successful login, a profile preview with avatar, name, and email is displayed.

---

## 📂 Data Storage

### Local Storage (persists until cleared by the user/browser)
- **`users_v1`** → Stores all locally registered users.  
  Example structure:
  ```json
  {
    "alex_tan": {
      "password": "mypassword",
      "email": "alex@example.com",
      "picture": "",
      "provider": "local"
    }
  }
  ```
- **`user`** → Stores the current session profile.  
  Example:
  ```json
  {
    "name": "Alex Tan",
    "email": "alex@example.com",
    "pic": "https://someurl.com/avatar.jpg",
    "provider": "local"
  }
  ```

⚠️ Note: Passwords are stored in **plain text** here for demonstration purposes only (not secure in real-world use).

---

### Cookies
- **Name:** `remember_user`  
- **Value:**  
  - Local login → the username  
  - Google login → the email (or name if email missing)  
- **Lifetime:** 3 days from last login  
- **Purpose:** Enables “auto-login” if no session is found in localStorage.

---

## 🔄 Login / Logout Flow

### Local Login
- User enters username and password.  
- System validates against `users_v1`.  
- On success:
  - Stores current profile in `user` (localStorage).  
  - Sets `remember_user` cookie (3 days).  
  - Displays profile.

### Google Login
- Uses Google Identity Services API.  
- Decodes ID token (JWT) to extract `name`, `email`, `picture`.  
- On success:
  - Stores current profile in `user` (localStorage).  
  - Sets `remember_user` cookie (3 days).  
  - Displays profile.

### Auto Login (on page load)
- If `user` exists in localStorage → show profile immediately.  
- Else if `remember_user` cookie exists:
  - If it matches a local account in `users_v1`, rebuild profile and show it.  
  - If it was a Google login, user may need to log in again.

### Logout
- Removes `user` from localStorage.  
- Deletes `remember_user` cookie.  
- Reloads the page.

---

## 🎨 UI and Responsiveness
- Background image applied to the page:
  ```css
  body {
    background-image: url("../elements/image/image 12.png");
    background-size: cover;
    background-position: center;
  }
  ```
- Side illustration (`image 10.png`) is hidden on small screens (≤ 768px):
  ```css
  @media (max-width: 768px) {
    .container > img { display: none; }
  }
  ```

---

## 🔐 Security Notes
- This is a **front-end only demo** (no server/database).  
- **Passwords are stored in plain text** in localStorage → not secure.  
- For real applications:
  - Use a backend server and secure database.  
  - Hash passwords (e.g., bcrypt, argon2).  
  - Use httpOnly secure cookies or tokens.  
  - Verify Google ID tokens server-side.


---

## ✅ Assignment Relevance
- **External API used:** Google Identity Services (RESTful).  
- **Client-side storage used:** localStorage + cookies.  
- **Session handling:** Auto login for 3 days via cookie.  
- **Responsiveness:** Adapts layout for desktop and mobile.

---

## 📊 Login Flow Diagram

```mermaid
flowchart TD
    A[User opens Login Page] --> B{Session in localStorage?}
    B -- Yes --> C[Show profile immediately]
    B -- No --> D{remember_user cookie exists?}
    D -- Yes --> E[Rebuild profile from local account]
    D -- No --> F[User must log in]

    F --> G[Local Login Form]
    F --> H[Google Sign-In]

    G --> I[Validate credentials in users_v1]
    I -- Success --> J[Save profile in localStorage + set cookie (3 days)]
    J --> C

    H --> K[Decode JWT from Google]
    K --> L[Save profile in localStorage + set cookie (3 days)]
    L --> C

    C --> M[User sees profile panel]
    M --> N[Logout]
    N --> O[Clear localStorage + delete cookie]
```
