# 🔐 NextAuth Authentication Setup (MERN + Next.js)

This project demonstrates how to implement authentication using **NextAuth.js** with custom credentials in a Next.js application, including secure password hashing using **bcryptjs**.

---

## 🚀 Features

* 🔑 Custom Credentials Login (Email & Password)
* 🔐 JWT-based Authentication
* 🧠 Session Handling
* 🔄 Redirect Control
* 🛡️ Secure Auth using Environment Variables
* 🔒 Password Hashing with bcryptjs
* 🎨 Custom Authentication Pages

---

## 📦 Technologies Used

* Next.js
* NextAuth.js
* Node.js
* MongoDB
* bcryptjs

---

## 📁 Project Structure

```
/src
 └── /app
      ├── /api
      │    └── /auth
      │         └── [...nextauth]
      │              └── route.js
      │
      └── /auth
           ├── /signin
           │     └── page.jsx
           ├── /signout
           │     └── page.jsx
           ├── /error
           │     └── page.jsx
           ├── /verify-request
           │     └── page.jsx
           └── /new-user
                 └── page.jsx
```

---

## ⚙️ Setup Instructions

### 1️⃣ Create Project

```bash
npx create-next-app@latest my-app
cd my-app
```

---

### 2️⃣ Install Dependencies

```bash
npm install next-auth bcryptjs mongoose
```

---

### 3️⃣ Environment Variables

Create a `.env.local` file:

```
NEXTAUTH_SECRET=your_secret_key_here
NEXTAUTH_URL=http://localhost:3000
MONGODB_URI=your_mongodb_connection_string
```

---

### 4️⃣ Run Project

```bash
npm run dev
```

---

## 🔑 NextAuth Configuration

```js
import NextAuth from "next-auth"
import CredentialsProvider from "next-auth/providers/credentials"
import bcrypt from "bcryptjs"

export const authOptions = {
  secret: process.env.NEXTAUTH_SECRET,

  pages: {
    signIn: '/auth/signin',
    signOut: '/auth/signout',
    error: '/auth/error',
    verifyRequest: '/auth/verify-request',
    newUser: '/auth/new-user'
  },

  providers: [
    CredentialsProvider({
      name: "Credentials",
      credentials: {
        email: {},
        password: {},
      },

      async authorize(credentials) {
        // Replace with DB user
        const user = {
          id: "1",
          email: "test@gmail.com",
          password: "$2a$10$examplehashedpassword"
        };

        const isMatch = await bcrypt.compare(
          credentials.password,
          user.password
        );

        if (!isMatch) return null;

        return {
          id: user.id,
          email: user.email,
        };
      },
    }),
  ],
}
```

---

## 🔒 bcryptjs Usage

### 🔑 Hash Password (Signup)

```js
import bcrypt from "bcryptjs";

const hashedPassword = await bcrypt.hash("123456", 10);
```

---

### 🔍 Compare Password (Login)

```js
const isMatch = await bcrypt.compare(
  enteredPassword,
  storedHashedPassword
);

if (!isMatch) {
  throw new Error("Invalid password");
}
```

---

## 🧑‍💻 Example: Sign In Page

📄 `/app/auth/signin/page.jsx`

```jsx
'use client'
import { signIn } from "next-auth/react";
import { useState } from "react";

export default function SignIn() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleLogin = async (e) => {
    e.preventDefault();

    await signIn("credentials", {
      email,
      password,
      redirect: true,
      callbackUrl: "/dashboard"
    });
  };

  return (
    <div className="flex items-center justify-center h-screen">
      <form onSubmit={handleLogin} className="p-6 shadow-lg rounded-xl w-80">
        <h2 className="text-xl font-bold mb-4">Login</h2>

        <input
          type="email"
          placeholder="Email"
          className="input input-bordered w-full mb-3"
          onChange={(e) => setEmail(e.target.value)}
        />

        <input
          type="password"
          placeholder="Password"
          className="input input-bordered w-full mb-3"
          onChange={(e) => setPassword(e.target.value)}
        />

        <button className="btn btn-primary w-full">
          Sign In
        </button>
      </form>
    </div>
  );
}
```

---

## ⚠️ Important Notes

* ❌ Never store plain text passwords
* ✅ Always hash passwords
* 🔐 Use bcrypt.compare() for login
* 🔑 Keep your NEXTAUTH_SECRET secure

---

## 🧪 Future Improvements

* 🌐 Google / GitHub login
* 🧑 Role-based authentication
* 📊 Admin dashboard
* 🔐 Two-factor authentication (2FA)

---

## 🧑‍💻 Author

**Your Name**

---

## 📜 License

This project is licensed under the MIT License.
