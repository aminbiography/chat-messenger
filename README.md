Live URL:    https://aminbiography.github.io/chat-messenger/ 

-------------------------------------------------------------------------------
 
# Chat Messenger

A **real-time web-based chat application** built with **HTML, CSS, and JavaScript**, using **Firebase Realtime Database** as the backend.
This simple but functional app demonstrates **two-user chat simulation**, **real-time message updates**, and **database syncing** — ideal for beginners learning Firebase or developers exploring front-end chat logic.

---

## Overview

The Chat Messenger app allows two users (User 1 and User 2) to exchange messages that are instantly synchronized through Firebase.
Each message is saved to the Realtime Database and rendered live in the chat window — no refresh needed.

---

## Project Structure

| Component                      | Description                                                                                         |
| ------------------------------ | --------------------------------------------------------------------------------------------------- |
| **HTML**                       | Defines the UI — two message inputs (one per user), a message display area, and a refresh button.   |
| **CSS**                        | Styles the chat container, messages, and buttons with a clean layout and responsive behavior.       |
| **JavaScript**                 | Handles Firebase initialization, message sending, real-time database updates, and UI refresh logic. |
| **Firebase Realtime Database** | Acts as the backend, storing all messages and broadcasting updates instantly.                       |

---

## How It Works

1. **Firebase Initialization**
   The Firebase SDK connects the app to your Realtime Database using credentials stored in the `firebaseConfig` object.

2. **Sending Messages**
   Each user has their own input field and “Chat as User X” button.
   When a message is sent:

   ```js
   database.ref('messages').push({
     user: user,
     text: messageText
   });
   ```

   This pushes the message to Firebase under the `messages` node.

3. **Real-Time Message Updates**
   The app listens for new database entries:

   ```js
   database.ref('messages').on('child_added', (snapshot) => { ... });
   ```

   Whenever a new message appears, the listener adds it dynamically to the chat window — instantly updating both users’ screens.

4. **Refresh Chat**
   The “Refresh Chat” button clears the local chat box for a fresh start (it doesn’t delete Firebase data — only the local display).

5. **Keyboard Shortcuts**
   Press **Enter** to send a message quickly instead of clicking the button.

---

## Firebase Setup Guide

Follow these steps to configure Firebase for your chat app.

### Step 1: Create a Firebase Project

1. Visit [Firebase Console](https://console.firebase.google.com/).
2. Click **Add project** and follow the setup wizard.
3. No Google Analytics is required for this simple project.

### Step 2: Enable Realtime Database

1. In the project dashboard, go to **Build → Realtime Database**.
2. Click **Create Database** → choose your location.
3. Start in **Test Mode** (for development).

### Step 3: Add Firebase to Your Web App

1. Go to **Project Settings → Your Apps → Web**.
2. Register your app name.
3. Copy the Firebase configuration snippet and replace the placeholders in your HTML:

   ```js
   const firebaseConfig = {
     apiKey: "YOUR_API_KEY",
     authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
     databaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
     projectId: "YOUR_PROJECT_ID",
     storageBucket: "YOUR_PROJECT_ID.appspot.com",
     messagingSenderId: "YOUR_SENDER_ID",
     appId: "YOUR_APP_ID"
   };
   ```

### Step 4: Set Database Rules (Development Only)

For testing, you can make the database public:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> For production, **never** keep open rules — use Firebase Authentication to secure reads and writes.

### Step 5: Run and Test

1. Save the HTML file.
2. Open it directly in your browser.
3. Send messages as **User 1** and **User 2** — they’ll appear live for both users.
4. Check your Firebase Console to see messages appear under the **messages** node.

---

## UI Behavior

* **User 1** messages appear in **blue** on the left.
* **User 2** messages appear in **green** on the right.
* **Hover effects** enhance buttons for better user experience.
* The chat window auto-scrolls to show the latest message.

---

## Developer Notes

* **Tech Stack:** HTML + CSS + JavaScript (no frameworks)
* **Backend:** Firebase Realtime Database
* **Hosting:** Works on any static host — GitHub Pages, Netlify, Firebase Hosting
* **Dependencies:** None beyond Firebase SDK 9

---

## Future Enhancements

Developers can improve this project by adding:

| Feature                     | Description                                                               |
| --------------------------- | ------------------------------------------------------------------------- |
| **User Authentication**     | Let users sign in with Google, email, or anonymous login before chatting. |
| **Message Deletion & Edit** | Add delete or edit options for each message.                              |
| **Timestamp Display**       | Show message time next to each bubble.                                    |
| **Persistent Chat History** | Load messages from Firebase even after page refresh.                      |
| **Typing Indicators**       | Show “User 1 is typing…” feedback.                                        |
| **Responsive Mobile UI**    | Enhance layout for small screens.                                         |
| **Theme Mode**              | Add light/dark mode toggle using CSS variables.                           |
| **Deployment**              | Host on Firebase Hosting or Vercel for easy access.                       |

---

## Example Firebase Data Structure

```json
{
  "messages": {
    "-N4sxx12yz890": {
      "user": "user1",
      "text": "Hello!"
    },
    "-N4sxx45ab123": {
      "user": "user2",
      "text": "Hey there!"
    }
  }
}
```

----------------------------------------------------------------------------

Codes:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chat Messenger</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .chat-container {
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0,0,0,0.1);
            width: 400px;
            max-width: 100%;
            padding: 20px;
            display: flex;
            flex-direction: column;
        }
        .chat-box {
            border: 1px solid #ddd;
            height: 300px;
            padding: 10px;
            overflow-y: auto;
            margin-bottom: 20px;
            background-color: #f9f9f9;
            border-radius: 5px;
            display: flex;
            flex-direction: column;
        }
        .chat-box .message {
            margin-bottom: 10px;
            padding: 8px;
            border-radius: 15px;
            width: fit-content;
            max-width: 70%;
            word-wrap: break-word;
        }
        .chat-box .message.user1 {
            background-color: #20a7db;
            color: #fff;
            align-self: flex-start;
            border-bottom-left-radius: 0;
        }
        .chat-box .message.user2 {
            background-color: #28E326;
            color: #fff;
            align-self: flex-end;
            border-bottom-right-radius: 0;
        }
        .input-container {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
        }
        .input-container input[type="text"] {
            flex: 1;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ddd;
            margin-right: 10px;
        }
        .input-container button {
            color: #fff;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
        }
        .input-container button:hover {
            opacity: 0.9;
        }
        /* User 1 button color */
        .input-container button.user1-btn {
            background-color: #20a7db;
        }
        .input-container button.user1-btn:hover {
            background-color: #62c1e5;
        }
        /* User 2 button color */
        .input-container button.user2-btn {
            background-color: #13D00C;
        }
        .input-container button.user2-btn:hover {
            background-color: #00ff00;
        }
        .refresh-button {
            display: flex;
            justify-content: center;
            margin-top: 10px;
        }
        .refresh-button button {
            background-color: #be29ec;
            color: #fff;
            border: none;
            padding: 8px 20px;
            border-radius: 5px;
            cursor: pointer;
        }
        .refresh-button button:hover {
            background-color: #E03FD8;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <h2 style="text-align: center;">Chat Messenger</h2>
        <div id="chat-box" class="chat-box">
            <!-- Messages will appear here -->
        </div>

        <!-- User 1 message input and send button -->
        <div class="input-container">
            <input type="text" id="user1-message" placeholder="User 1 message">
            <button class="user1-btn" onclick="sendMessage('user1')">Chat as User 1</button>
        </div>

        <!-- User 2 message input and send button -->
        <div class="input-container">
            <input type="text" id="user2-message" placeholder="User 2 message">
            <button class="user2-btn" onclick="sendMessage('user2')">Chat as User 2</button>
        </div>

        <!-- Single refresh button -->
        <div class="refresh-button">
            <button onclick="refreshMessages()">Refresh Chat</button>
        </div>
    </div>

    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-database.js"></script>

    <script>
        // Your web app’s Firebase configuration
        const firebaseConfig = {
            apiKey: "YOUR_API_KEY",
            authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
            databaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
            projectId: "YOUR_PROJECT_ID",
            storageBucket: "YOUR_PROJECT_ID.appspot.com",
            messagingSenderId: "YOUR_SENDER_ID",
            appId: "YOUR_APP_ID"
        };
        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();

        // Function to send a message
        function sendMessage(user) {
            const messageInput = document.getElementById(user + '-message');
            const messageText = messageInput.value.trim();

            if (messageText !== '') {
                const chatBox = document.getElementById('chat-box');
                const newMessageRef = database.ref('messages').push();
                newMessageRef.set({
                    user: user,
                    text: messageText
                });

                messageInput.value = '';
            }
        }

        // Function to simulate refreshing the chat (clear messages)
        function refreshMessages() {
            const chatBox = document.getElementById('chat-box');
            chatBox.innerHTML = ''; // Clears all messages
        }

        // Listen for new messages
        database.ref('messages').on('child_added', (snapshot) => {
            const message = snapshot.val();
            const chatBox = document.getElementById('chat-box');

            const messageDiv = document.createElement('div');
            messageDiv.classList.add('message', message.user);
            messageDiv.textContent = message.text;

            chatBox.appendChild(messageDiv);
            chatBox.scrollTop = chatBox.scrollHeight;
        });

        // Detect "Enter" key press in the input box for User 1 and User 2
        document.getElementById('user1-message').addEventListener('keypress', function(event) {
            if (event.key === 'Enter') {
                sendMessage('user1');
            }
        });
        document.getElementById('user2-message').addEventListener('keypress', function(event) {
            if (event.key === 'Enter') {
                sendMessage('user2');
            }
        });
    </script>
</body>
</html>
```


# Description of the Code

This code represents a simple Chat Messenger web app built using HTML, CSS, and JavaScript, with Firebase Realtime Database as the backend for storing and retrieving messages. Here's how the code works:

HTML Structure:

The chat app consists of two main input fields, one for each user (User 1 and User 2), and a chat box that displays the messages.
It has buttons to send messages for each user and a refresh button to clear the chat.
CSS Styling:

Basic styling is provided to make the chat container look clean and centered on the page.
Messages are styled differently depending on the user (blue for User 1 and green for User 2).
Hover effects on buttons are applied for better interaction.
JavaScript Logic:

Firebase SDK is used to initialize Firebase and connect to the Realtime Database.
sendMessage function sends messages to the Firebase database when either User 1 or User 2 submits a message.
refreshMessages clears the chat box locally.
A Firebase listener (child_added) monitors new messages being added to the database and updates the chat box in real-time with the new message.
Users can also send messages by pressing the "Enter" key.
Firebase Configuration:

The Firebase credentials need to be set up (API key, project ID, etc.). These are placeholders in the code, and you need to replace them with actual values.
Setting Up the Backend with Firebase
Here’s how you can set up the backend using Firebase:

Step 1: Create a Firebase Project
Go to the Firebase Console.
Click on Add Project.
Give your project a name and complete the setup by following the steps in the wizard.

Step 2: Enable Realtime Database
In your Firebase project dashboard, go to Build -> Realtime Database.
Click on Create Database.
Set the database location and security rules (you can start with public rules for testing but make sure to secure them later).

Step 3: Add Firebase SDK to Your Project
From the Firebase console, click on the Settings icon (⚙️) and go to Project Settings.
Scroll down to Your apps and choose Web.
Register your app and Firebase will provide you with the necessary Firebase SDK configuration (similar to what's in the code).
Replace the placeholder in the code (firebaseConfig) with your actual Firebase credentials:
javascript
Copy code
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    databaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
};

Step 4: Set Up Firebase Rules (Optional but Recommended)
For a simple chat app, you may want to allow read and write access for authenticated users or for testing purposes. You can set your rules to public during development:

Go to the Realtime Database section in Firebase.
Navigate to the Rules tab.
Use the following rule to allow read and write access to anyone (for testing):
json
Copy code
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
For production, consider using authenticated access to secure your database.

Step 5: Deploy and Test
After setting up Firebase and updating the configuration in your code, save the HTML file and open it in a browser.
You should now be able to send messages as User 1 and User 2. Messages will appear in the chat box in real-time.
Check the Firebase Console to see the messages stored in your Realtime Database under the messages node.
This setup will allow you to store and retrieve messages in real-time, providing a functional backend for the chat app.

---

## Author

**Developed by [Mohammad Aminul Islam (Amein)](https://github.com/aminbiography)**
*Web Developer | Cyber Threat Intelligence Associate*

---

## License

This project is licensed under the **MIT License**.
You are free to use, modify, and share it — attribution is appreciated.

---


