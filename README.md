# acmsigchi_event-reg
SRM ACM SIGCHI CLUB TASK - Event Registration Website
# Event Registration Project

## Description
A simple event registration system using Node.js, Express, and vanilla JavaScript.

## Features
- Submit name and email through a web form
- Backend logs registrations and stores them in a JSON file
- Error handling for empty fields and server connection

## Setup Instructions
1. Clone the repository:
   ```bash
   git clone https://github.com/Arjun-jpeg/acmsigchi_event-reg
   cd event-registration

   ## **Initialize Git**

Open PowerShell in the root of your project (`event-registration`) and run:

```powershell
git init
git add .
git commit -m "Initial commit: Event registration project"

Demo video link: https://drive.google.com/file/d/1Ew69caBcRLi-oJv8Nh5rkn0hqLGpNl7V/view?usp=sharing


Backend- Node.js

const fs = require('fs');
const path = require('path');
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');

const app = express();
const PORT = 5000;

// Middleware
app.use(bodyParser.json());
app.use(cors());

// POST route for registration
app.post('/register', (req, res) => {
    const { name, email } = req.body;
    const date = new Date();
    const registration = { name, email, date };

    console.log("New Registration:", registration);

    // Path to JSON file
    const filePath = path.join(__dirname, 'registrations.json');

    // Read existing registrations
    fs.readFile(filePath, (err, data) => {
        let registrations = [];
        if (!err && data.length) {
            registrations = JSON.parse(data);
        }

        // Add new registration
        registrations.push(registration);

        // Save back to file
        fs.writeFile(filePath, JSON.stringify(registrations, null, 2), (err) => {
            if (err) {
                console.error(err);
                res.json({ success: false, message: "Failed to save registration." });
            } else {
                res.json({ success: true, message: "Registration successful!" });
            }
        });
    });
});

// Start server
app.listen(PORT, () => {
    console.log(`Server running on http://localhost:${PORT}`);
});
/*const form = document.getElementById('registrationForm');
const message = document.getElementById('message');

form.addEventListener('submit', async (e) => {
    e.preventDefault();

    const name = document.getElementById('name').value;
    const email = document.getElementById('email').value;

    if (!name || !email) {
        message.textContent = "Please fill in all fields.";
        message.style.color = "red";
        return;
    }

    try {
        const response = await fetch('http://localhost:5000/register', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ name, email })
        });

        const data = await response.json();
        if (data.success) {
            message.textContent = "Registration successful!";
            message.style.color = "green";
            form.reset();
        } else {
            message.textContent = "Registration failed.";
            message.style.color = "red";
        }
    } catch (err) {
        message.textContent = "Error connecting to server.";
        message.style.color = "red";
    }
});

app.post('/register', (req, res) => {
    const { name, email } = req.body;
    const date = new Date();

    console.log("New Registration:", { name, email, date });

    // Send response back to frontend
    if (name && email) {
        res.json({ success: true, message: "Registration successful!" });
    } else {
        res.json({ success: false, message: "Registration failed." });
    }
});*/

Frontend -HTML
<!DOCTYPE html>
<html>
<head>
    <title>Event Registration</title>
</head>
<body>
    <h1>Event Registration</h1>
    <form id="registrationForm">
        <label>Name:</label><br>
        <input type="text" id="name" required><br><br>
        <label>Email:</label><br>
        <input type="email" id="email" required><br><br>
        <button type="submit">Register</button>
    </form>
    <p id="message" style="color:green;"></p>

    <script>
        const form = document.getElementById('registrationForm');
        form.addEventListener('submit', async (e) => {
            e.preventDefault();
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;

            if (!name || !email) {
                alert("Please fill in all fields");
                return;
            }

            try {
                const response = await fetch('http://localhost:5000/register', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ name, email })
                });
                const data = await response.json();
                document.getElementById('message').innerText = data.message;
                form.reset();
            } catch (err) {
                console.error(err);
                alert("Error submitting the form");
            }
        });
    </script>
</body>
</html>


