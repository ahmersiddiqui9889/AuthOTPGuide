### setup new project in vscode

`npm create vite@latest frontend`

`cd frontend`

`npm install`

`npm install postcss autoprefixer react-router-dom axios`

`npm install -D tailwindcss`

`npx tailwindcss init -p`


### paste this in tailwind.config.js
```
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {
      animation: {
        fadeIn: 'fadeIn 1s ease-in-out',
      },
      keyframes: {
        fadeIn: {
          from: { opacity: 0 },
          to: { opacity: 1 },
        },
      },
    },
  },
  plugins: [],
};
```


### paste this in src/index.css
```
@tailwind base;
@tailwind components;
@tailwind utilities;
```


### create Pages folder in src
### create Dashboard.jsx in Pages
### paste this in Dashboard.jsx
```
import { useNavigate } from "react-router-dom"; // Assuming you're using React Router
import { useState, useEffect } from "react";

const Dashboard = () => {
  const navigate = useNavigate(); // Hook to navigate programmatically
  const [showLoader, setShowLoader] = useState(false);

  const handleLogout = () => {
    // Show loader for a better UX during logout
    setShowLoader(true);
    setTimeout(() => {
      setShowLoader(false); // Hide loader after a delay
      navigate("/login"); // Redirect to the login page
    }, 1500);
  };

  const handleContinue = () => {
    navigate("/continue"); // Redirect to Continue page
  };

  useEffect(() => {
    // Simulate a brief loading state when the dashboard first loads
    setShowLoader(true);
    setTimeout(() => setShowLoader(false), 1000);
  }, []);

  return (
    <div className="flex items-center justify-center min-h-screen bg-gradient-to-br from-blue-900 via-indigo-900 to-purple-900">
      {/* Loader */}
      {showLoader ? (
        <div className="flex flex-col items-center space-y-4">
          <div className="loader w-16 h-16 border-4 border-t-4 border-blue-300 rounded-full animate-spin"></div>
          <p className="text-white text-lg font-semibold">Processing...</p>
        </div>
      ) : (
        <div className="text-center text-white p-8 bg-gray-800 bg-opacity-90 rounded-lg shadow-xl w-full max-w-lg">
          <h1 className="text-5xl font-bold mb-6 animate-slideInUp">Dashboard</h1>
          <p className="text-lg mb-6">Welcome! Your email has been successfully verified.</p>
          <div className="space-y-6">
            <div className="bg-gradient-to-r from-purple-500 via-pink-500 to-red-500 p-6 rounded-lg shadow-inner">
              <p className="text-xl font-medium">Your Current Status</p>
              <ul className="text-left list-disc mt-4 ml-6 space-y-2">
                <li>You’ve logged in successfully.</li>
                <li>Email verified at {new Date().toLocaleTimeString()}.</li>
                <li>Your account is fully active.</li>
              </ul>
            </div>
            <div className="flex justify-between gap-4">
              <button
                onClick={handleLogout}
                className="px-6 py-3 bg-red-600 text-lg font-semibold rounded-lg hover:bg-red-700 transition duration-300 ease-in-out"
              >
                Logout
              </button>
              <button
                onClick={handleContinue}
                className="px-6 py-3 bg-green-600 text-lg font-semibold rounded-lg hover:bg-green-700 transition duration-300 ease-in-out"
              >
                Continue
              </button>
            </div>
          </div>
        </div>
      )}
    </div>
  );
};

export default Dashboard;
```






### create Signup.jsx in Dashboard/Pages
### paste this in Signup.jsx
```
import { useState } from "react";
import axios from "axios";
import { useNavigate } from "react-router-dom"; // Assuming you are using react-router for navigation.

const Signup = () => {
  const [email, setEmail] = useState("");
  const [isOtpSent, setIsOtpSent] = useState(false);
  const [otp, setOtp] = useState("");
  const navigate = useNavigate();

  const sendOtp = async () => {
    try {
      const response = await axios.post("http://localhost:5000/send-otp", { email });
      alert(response.data.message);
      setIsOtpSent(true);
    } catch (error) {
      alert(error.response?.data?.message || "Error sending OTP");
    }
  };

  const verifyOtp = async () => {
    try {
       await axios.post("http://localhost:5000/verify-otp", { email, otp });
      alert("Email-Verified-Suucesfully");
      navigate("/dashboard"); // Redirect to a new page (e.g., Welcome page) after verification.
    } catch (error) {
      alert(error.response?.data?.message || "Error verifying OTP");
    }
  };

  return (
    <div className="flex flex-col md:flex-row min-h-screen bg-gray-100">
      {/* Left Section with Image */}
      <div className="hidden md:flex w-1/2 bg-gradient-to-r from-purple-500 via-indigo-500 to-blue-500 relative overflow-hidden">
        <img
          src="https://cdni.iconscout.com/illustration/premium/thumb/sign-up-page-1886582-1598253.png" // Replace with the actual path to your image
          alt="Quote Background"
          className="absolute w-full h-full object-cover"
        />
        <div className="relative z-10 text-white p-8">
          <p className="text-sm uppercase tracking-widest mb-2">A wise quote</p>
          <h1 className="text-5xl font-bold leading-tight mb-4">Get Everything You Want</h1>
          <p className="text-lg">
            You can get anything you want if you work hard enough for it.
          </p>
        </div>
      </div>

      {/* Right Section with Form */}
      <div className="flex items-center justify-center w-full md:w-1/2 bg-white p-8">
        <div className="w-full max-w-md space-y-6 animate-slideUp">
          <h2 className="text-3xl font-bold text-gray-800 text-center">Welcome Back</h2>
          <p className="text-sm text-gray-500 text-center">Enter your email to continue.</p>
          <form className="space-y-4">
            {/* Email Input */}
            <input
              type="email"
              placeholder="Enter your email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              className="w-full px-4 py-3 text-gray-700 bg-gray-100 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-indigo-500"
              required
              disabled={isOtpSent} // Disable email input after OTP is sent
            />

            {/* OTP Input (Visible only after OTP is sent) */}
            {isOtpSent && (
              <input
                type="text"
                placeholder="Enter OTP"
                value={otp}
                onChange={(e) => setOtp(e.target.value)}
                className="w-full px-4 py-3 text-gray-700 bg-gray-100 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-indigo-500"
                required
              />
            )}

            {/* Button */}
            <button
              type="button"
              onClick={isOtpSent ? verifyOtp : sendOtp}
              className="w-full px-4 py-3 font-semibold text-white bg-indigo-500 rounded hover:bg-indigo-600 transition duration-300 ease-in-out"
            >
              {isOtpSent ? "Verify OTP" : "Send OTP"}
            </button>
          </form>
        </div>
      </div>
    </div>
  );
};

export default Signup;
```




### create Login.jsx in Pages
### paste this in login.jsx
```
import { useState } from "react";

const Login = () => {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleLogin = (e) => {
    e.preventDefault();
    console.log("Login Data:", { email, password });
    alert("Login successful!");
    window.location.href = "/dashboard"; // Redirect after login
  };

  return (
    <div className="flex flex-col md:flex-row min-h-screen bg-gray-100">
      {/* Left Section with Image */}
      <div className="hidden md:flex w-1/2 bg-gradient-to-r from-purple-500 via-indigo-500 to-blue-500 relative overflow-hidden">
        <img
          src="https://cdni.iconscout.com/illustration/premium/thumb/login-page-1886583-1598255.png" // Replace with an appropriate image
          alt="Login Background"
          className="absolute w-full h-full object-cover"
        />
        <div className="relative z-10 text-white p-8">
          <p className="text-sm uppercase tracking-widest mb-2">A wise quote</p>
          <h1 className="text-5xl font-bold leading-tight mb-4">Welcome Back</h1>
          <p className="text-lg">
            Log in to access your personalized dashboard and explore more features.
          </p>
        </div>
      </div>

      {/* Right Section with Form */}
      <div className="flex items-center justify-center w-full md:w-1/2 bg-white p-8">
        <div className="w-full max-w-md space-y-6 animate-slideUp">
          <h2 className="text-3xl font-bold text-gray-800 text-center">Log In</h2>
          <p className="text-sm text-gray-500 text-center">
            Enter your credentials to access your account.
          </p>
          <form onSubmit={handleLogin} className="space-y-4">
            <input
              type="email"
              placeholder="Enter your email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              className="w-full px-4 py-3 text-gray-700 bg-gray-100 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-indigo-500"
              required
            />
            <input
              type="password"
              placeholder="Enter your password"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              className="w-full px-4 py-3 text-gray-700 bg-gray-100 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-indigo-500"
              required
            />
            <button
              type="submit"
              className="w-full px-4 py-3 font-semibold text-white bg-indigo-500 rounded hover:bg-indigo-600 transition duration-300 ease-in-out"
            >
              Log In
            </button>
          </form>
        </div>
      </div>
    </div>
  );
};

export default Login;
```







### paste this in App.jsx
```
// import React from "react";
import { BrowserRouter as Router, Route, Routes } from "react-router-dom";
import Signup from "./Pages/Signup";
import Login from "./Pages/Login";
import Dashboard from "./Pages/Dashboard";
import Continue from './Pages/Continue';

const App = () => {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Signup />} />
        <Route path="/login" element={<Login />} />
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/continue" element={<Continue />} />
      </Routes>
    </Router>
  );
};

export default App;
```







### download and paste logobr.png in src/assets

![logobr.png](./logobr.png)
logobr.png




## frontend completed

---

### Open new terminal

`mkdir backend`

`cd backend`

`npm init -y`

`npm install express cors body-parser nodemailer mongoose bcrypt jsonwebtoken dotenv nodemon`



### create a file named server.js in backend folder


### paste this in server.js
```const express = require("express");
const bodyParser = require("body-parser");
const nodemailer = require("nodemailer");
const cors = require("cors");
require("dotenv").config();

const app = express();
const PORT = 5000;

app.use(bodyParser.json());
app.use(cors());

const users = {};
const otpStore = {};

// Configure Nodemailer
const transporter = nodemailer.createTransport({
  service: "Gmail",
  auth: {
    user: process.env.EMAIL, // Your email
    pass: process.env.PASSWORD, // Your email password
  },
});

// Send OTP
app.post("/send-otp", (req, res) => {
  const { email } = req.body;

  if (!email) {
    return res.status(400).json({ message: "Email is required" });
  }

  const otp = Math.floor(100000 + Math.random() * 900000); // Generate a 6-digit OTP
  otpStore[email] = otp;

  const mailOptions = {
    from: process.env.EMAIL,
    to: email,
    subject: "Your OTP for Signup",
    text: `Your OTP is: ${otp}`,
  };

  transporter.sendMail(mailOptions, (error) => {
    if (error) {
      return res.status(500).json({ message: "Error sending OTP" });
    }
    res.status(200).json({ message: "OTP sent successfully" });
  });
});

// Verify OTP
app.post("/verify-otp", (req, res) => {
  const { email, otp } = req.body;

  if (!email || !otp) {
    return res.status(400).json({ message: "Email and OTP are required" });
  }

  if (otpStore[email] && otpStore[email] === parseInt(otp)) {
    delete otpStore[email];
    return res.status(200).json({ success: true });
  }

  res.status(400).json({ message: "Invalid OTP" });
});

// Signup
app.post("/signup", (req, res) => {
  const { email } = req.body;

  if (!email) {
    return res.status(400).json({ message: "Email is required" });
  }

  if (users[email]) {
    return res.status(400).json({ message: "Email is already registered" });
  }

  users[email] = { email };
  res.status(201).json({ message: "Signup successful" });
});

// Start Server
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```




### create a file named .env
### paste EMAIL and PASSWORD


## DONE 🙂

---

Now you need to get your private password from google.

## For EMAIL and PASSWORD
### 1. goto myaccount.google.com
### 2. click on security tab on left hand side
### 3. search for 2 step verification
### 4. type password
### 5. turn on 2 step verification
### 6. then go back
### 7. search for app password and open it
### 8. Type EmailAuthTest

## copy the text, this is your password

### paste this in .env file as EMAIL and PASSWORD
```
EMAIL = <your email here>
PASSWORD = <your password here>```
