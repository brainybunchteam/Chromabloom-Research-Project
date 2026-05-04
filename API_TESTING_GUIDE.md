# 🛠 ChromaBloom: API Testing Guide (Postman)

This guide explains how to test the ChromaBloom Backend and ML services using Postman or any other API client. Since the services are hosted on Render, you do not need the source code to run these tests.

---

## 🌐 Service Base URLs

| Service | Production URL |
| :--- | :--- |
| **Node.js Backend** | `https://chromabloom-backend.onrender.com` |
| **Python ML Service** | `https://chromabloom-ml.onrender.com` |

---

## 🔐 Authentication
Most routes in this API are protected. You must first log in to receive a **JWT Token**.

1.  **Login Request:**
    *   **Method:** `POST`
    *   **URL:** `{{BASE_URL}}/chromabloom/caregivers/login` (or `/therapists/login`)
    *   **Body (JSON):**
        ```json
        {
          "email": "user@example.com",
          "password": "password123"
        }
        ```
2.  **Using the Token:**
    *   Copy the `token` from the response.
    *   In your next Postman request, go to the **Authorization** tab.
    *   Select **Type:** `Bearer Token`.
    *   Paste the token into the **Token** field.

---

## 🚀 Key Endpoints for Testing

### 1. Drawing Lessons (Backend)
*   **Get All Lessons:** `GET /chromabloom/drawing-lessons`
*   **Get Single Lesson:** `GET /chromabloom/drawing-lessons/:id`

### 2. Quizzes (Backend)
*   **Get All Quizzes:** `GET /chromabloom/quizes`
*   **Get Quizzes by Lesson:** `GET /chromabloom/quizes/lesson/:lessonId`

### 3. Problem Solving (Backend)
*   **Get All Lessons:** `GET /chromabloom/problem-solving-lessons`

### 4. Machine Learning (Direct Service)
*   **Health Check:** `GET https://chromabloom-ml.onrender.com/health`
*   **Drawing Prediction:** `POST https://chromabloom-ml.onrender.com/predict` (Requires a `multipart/form-data` image file)

---

## ⚠️ Notes
*   **Cold Starts:** Since we are using the Render Free Tier, the server may "sleep" after 15 minutes of inactivity. The first request might take **30-60 seconds** to respond while the server wakes up.
*   **Headers:** Ensure you always set `Content-Type: application/json` in your request headers for JSON bodies.
