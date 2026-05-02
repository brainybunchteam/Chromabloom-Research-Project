# 🚀 ChromaBloom: Cloud Deployment & APK Build Guide (A-Z)

This guide explains how to host the Backend and ML Services on Render and build the Flutter APK on Codemagic without using a local machine.

---

## 🛠 Prerequisites (Accounts Needed)
1. GitHub Account: To host your code.
2. Render Account: To host your Node.js Backend and Python ML Services.
3. Codemagic Account: To build your Android APK.
4. MongoDB Atlas Account: For your cloud database.

---

## Phase A: Centralize Code on GitHub

1. Create a Private Repository on GitHub named ChromaBloom-Research-Project.
2. Upload Your Files: Use the "Upload files" button to drag and drop your folders (backend, frontend-mobile, ml_services, etc.) into the repository.
   - Note: Ensure the folder structure matches your local one.

---

## Phase B: Set Up MongoDB Atlas (Cloud Database)

1. Sign in to [mongodb.com](https://www.mongodb.com/cloud/atlas).
2. Create a New Project: Name it ChromaBloom.
3. Build a Cluster: Choose the FREE tier (M0). Select a region near you (e.g., N. Virginia or Mumbai).
4. Create a Database User: Set a username and a strong password. Save these!
5. Configure Network Access:
   - Click Network Access in the sidebar.
   - Click Add IP Address.
   - Choose Allow Access from Anywhere (0.0.0.0/0). This is required for Render.
6. Get Connection String:
   - Go to Database in the sidebar.
   - Click Connect on your Cluster.
   - Choose Drivers.
   - Copy the connection string. It looks like: `mongodb+srv://<username>:<password>@cluster0.mongodb.net/chromabloom?retryWrites=true&w=majority`
   - Replace `<password>` with the password you created in step 4.

---

## Phase C: Deploy ML Services (Python) to Render

1. Sign in to Render.com using GitHub.
2. Click New + > Web Service.
3. Select your GitHub repository.
4. Configure Settings:
   - Name: chromabloom-ml
   - Root Directory: ml_services
   - Runtime: Python
   - Build Command: pip install -r requirements.txt
   - Start Command: python main.py
5. Click Create Web Service.
6. Wait for Success: Once live, copy the URL (e.g., https://chromabloom-ml.onrender.com).

---

## Phase D: Deploy Backend (Node.js) to Render

1. Click New + > Web Service on Render.
2. Select the same GitHub repository.
3. Configure Settings:
   - Name: chromabloom-backend
   - Root Directory: backend
   - Runtime: Node
   - Build Command: npm install
   - Start Command: npm start
4. Environment Variables:
   - Click the Environment tab.
   - Add PORT: 5000
   - Add MONGO_URI: (Your MongoDB Atlas connection string from Phase B)
   - Add ML_SERVICE_URL: (The URL you copied from Phase C)
5. Click Create Web Service.
6. Wait for Success: Copy this Backend URL (e.g., https://chromabloom-backend.onrender.com).

---

## Phase E: Update Flutter App (Cloud URL)

1. Go to your GitHub Repository in your browser.
2. Navigate to: frontend-mobile/lib/services/api_config.dart.
3. Click the Edit (pencil) icon.
4. Replace the localhost URL with your Backend URL from Phase D:
   ```dart
   static const String baseUrl = 'https://chromabloom-backend.onrender.com';
   ```
5. Click Commit changes to save.

---

## Phase F: Build the APK on Codemagic

1. Sign in to Codemagic.io using GitHub.
2. Click Add Application and select your GitHub repo.
3. Configure Build:
   - Project Type: Flutter App.
   - Build Platform: Android.
   - Build Channel: Stable.
   - Mode: Release.
4. Start Build: Click Start New Build.
5. Download: Once it finishes (approx. 5-10 mins), Codemagic will provide a download link for your app-release.apk.

---

## ⚠️ Important Notes
- Render Free Tier: If you use the free tier, the Backend and ML services will sleep after 15 minutes of inactivity. The first time you open the app, it may take 30-60 seconds to wake up the server.
- Database: Your connection string must include the password you set in MongoDB Atlas.
- Troubleshooting: If the app doesn't log in, check the Logs tab on Render to see if there are any errors in the Backend or ML service.
