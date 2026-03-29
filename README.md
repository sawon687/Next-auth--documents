# Next-auth--documents

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
      └── /api
           └── /auth
                └── [...nextauth]
                     └── route.js
```

---

## ⚙️ Complete Setup Instructions

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

## 🔒 bcryptjs Usage

### 🔑 Hash Password (Signup)

```js
import bcrypt from "bcryptjs";

const password = "123456";

// hash password
const hashedPassword = await bcrypt.hash(password, 10);

// save to database
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

## 🔑 NextAuth Configuration (with bcrypt)

```js
import NextAuth from "next-auth"
import CredentialsProvider from "next-auth/providers/credentials"
import bcrypt from "bcryptjs"

export const authOptions = {
  secret: process.env.NEXTAUTH_SECRET,
  
  providers: [
    CredentialsProvider({
      name: "Credentials",
      credentials: {
        email: {},
        password: {},
      },
      
      async authorize(credentials) {

        // Example user (replace with DB)
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

## 🔍 How It Works

* User signup → password hashed using bcryptjs
* Password stored securely in database
* Login → password compared using bcrypt
* If matched → authentication success ✅

---

## ⚠️ Important Notes

* ❌ Never store plain text passwords
* ✅ Always hash passwords
* 🔐 Use `bcrypt.compare()` for login
* 🔑 Keep your `NEXTAUTH_SECRET` safe

---

## 🧪 Future Improvements

* 🌐 Google / GitHub login
* 🧑 Role-based authentication
* 📊 Admin dashboard
* 🔐 Advanced security (2FA)

---

## 🧑‍💻 Author

**Your Name Here**

---

## 📜 License

This project is open-source and available under the MIT License.
