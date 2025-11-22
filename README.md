# MyContacts Backend API 📞

## Overview
This is a robust backend API built with Node.js and Express.js, leveraging MongoDB through Mongoose for data persistence. It provides secure user authentication via JWT and comprehensive CRUD operations for managing personal contacts, ensuring data isolation for each registered user.

## Features
-   **User Authentication**: Secure user registration, login, and access to current user details.
-   **JWT Authorization**: Implements JSON Web Tokens for secure and stateless API access.
-   **Contact Management**: Full CRUD (Create, Read, Update, Delete) functionality for contacts.
-   **Data Isolation**: Ensures users can only access and manage their own contacts.
-   **MongoDB Integration**: Utilizes Mongoose for elegant MongoDB object modeling.
-   **Global Error Handling**: Centralized middleware for handling API errors consistently.
-   **Asynchronous Request Handling**: `express-async-handler` for simplified asynchronous error management.

## Getting Started
To get this project up and running on your local machine, follow these steps.

### Installation
1.  **Clone the Repository**:
    ```bash
    git clone https://github.com/Kwanza247/mycontacts-backend.git
    cd mycontacts-backend
    ```

2.  **Install Dependencies**:
    ```bash
    npm install
    ```

### Environment Variables
Create a `.env` file in the root directory of the project and add the following variables:

```dotenv
PORT=5000
CONNECTION_STRING="mongodb+srv://<username>:<password>@cluster0.abcde.mongodb.net/mycontacts-backend?retryWrites=true&w=majority"
ACCESS_TOKEN_SECRET="youraccesstokensecretkey"
```

*   `PORT`: The port on which the server will run (e.g., `5000`).
*   `CONNECTION_STRING`: Your MongoDB connection string. Replace `<username>`, `<password>`, and `cluster0.abcde.mongodb.net` with your MongoDB Atlas or local MongoDB details. The database name is `mycontacts-backend`.
*   `ACCESS_TOKEN_SECRET`: A strong, random string used to sign JWTs. Generate a complex one for production.

### Running the Application
To start the server:

*   **Development Mode (with Nodemon for auto-restarts)**:
    ```bash
    npm run dev
    ```
*   **Production Mode**:
    ```bash
    npm start
    ```
The server will be running on the port specified in your `.env` file (defaulting to 5000).

## API Documentation

### Base URL
All API requests should be prefixed with `http://localhost:[PORT]/api` (replace `[PORT]` with your configured port, e.g., `http://localhost:5000/api`).

### Authentication
A valid JWT `accessToken` is required for protected routes. This token should be sent in the `Authorization` header as a Bearer token: `Authorization: Bearer <accessToken>`.

### Endpoints

#### POST /api/users/register
Registers a new user account.
**Request**:
```json
{
  "username": "john_doe",
  "email": "john.doe@example.com",
  "password": "SecurePassword123"
}
```
**Response**:
```json
{
  "_id": "65e6d6b8f8a1c3b2d1e0f9g8",
  "email": "john.doe@example.com"
}
```
**Errors**:
-   `400 Bad Request`: All fields are mandatory, User already registered, User data is not valid.

#### POST /api/users/login
Authenticates a user and provides a JWT access token.
**Request**:
```json
{
  "email": "john.doe@example.com",
  "password": "SecurePassword123"
}
```
**Response**:
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjp7InVzZXJuYW1lIjoiam9obl9kb2UiLCJlbWFpbCI6ImpvaG4uZG9lQGV4YW1wbGUuY29tIiwiaWQiOiI2NWU2ZDZiOGY4YTFjM2IyZDFlMGY5ZzgifSwiaWF0IjoxNzA5NjQ3ODk1LCJleHAiOjE3MDk2NDg3OTV9.abcdefghijklmnopqrstuvwxyz1234567890"
}
```
**Errors**:
-   `400 Bad Request`: All fields are mandatory.
-   `401 Unauthorized`: Email or Password is not valid.

#### GET /api/users/current
Retrieves the profile of the currently authenticated user.
**Request**:
Requires `Authorization: Bearer <accessToken>`.
**Response**:
```json
{
  "username": "john_doe",
  "email": "john.doe@example.com",
  "_id": "65e6d6b8f8a1c3b2d1e0f9g8",
  "id": "65e6d6b8f8a1c3b2d1e0f9g8"
}
```
**Errors**:
-   `401 Unauthorized`: User is not authorized or token is missing/invalid.

#### GET /api/contacts
Retrieves all contacts for the authenticated user.
**Request**:
Requires `Authorization: Bearer <accessToken>`.
**Response**:
```json
[
  {
    "_id": "65e6d8a2f8a1c3b2d1e0f9g9",
    "user_id": "65e6d6b8f8a1c3b2d1e0f9g8",
    "name": "Alice Smith",
    "email": "alice.smith@example.com",
    "phone": "111-222-3333",
    "createdAt": "2024-03-05T10:31:30.908Z",
    "updatedAt": "2024-03-05T10:31:30.908Z",
    "__v": 0
  },
  {
    "_id": "65e6d9b3f8a1c3b2d1e0f9h0",
    "user_id": "65e6d6b8f8a1c3b2d1e0f9g8",
    "name": "Bob Johnson",
    "email": "bob.johnson@example.com",
    "phone": "444-555-6666",
    "createdAt": "2024-03-05T10:31:30.908Z",
    "updatedAt": "2024-03-05T10:31:30.908Z",
    "__v": 0
  }
]
```
**Errors**:
-   `401 Unauthorized`: User is not authorized or token is missing/invalid.

#### POST /api/contacts
Creates a new contact for the authenticated user.
**Request**:
Requires `Authorization: Bearer <accessToken>`.
```json
{
  "name": "Charlie Brown",
  "email": "charlie.brown@example.com",
  "phone": "777-888-9999"
}
```
**Response**:
```json
{
  "_id": "65e6da1cf8a1c3b2d1e0f9h1",
  "user_id": "65e6d6b8f8a1c3b2d1e0f9g8",
  "name": "Charlie Brown",
  "email": "charlie.brown@example.com",
  "phone": "777-888-9999",
  "createdAt": "2024-03-05T10:31:30.908Z",
  "updatedAt": "2024-03-05T10:31:30.908Z",
  "__v": 0
}
```
**Errors**:
-   `400 Bad Request`: All fields are mandatory.
-   `401 Unauthorized`: User is not authorized or token is missing/invalid.

#### GET /api/contacts/:id
Retrieves a specific contact by ID for the authenticated user.
**Request**:
Requires `Authorization: Bearer <accessToken>`.
**Response**:
```json
{
  "_id": "65e6d8a2f8a1c3b2d1e0f9g9",
  "user_id": "65e6d6b8f8a1c3b2d1e0f9g8",
  "name": "Alice Smith",
  "email": "alice.smith@example.com",
  "phone": "111-222-3333",
  "createdAt": "2024-03-05T10:31:30.908Z",
  "updatedAt": "2024-03-05T10:31:30.908Z",
  "__v": 0
}
```
**Errors**:
-   `401 Unauthorized`: User is not authorized or token is missing/invalid.
-   `404 Not Found`: Contact not found.

#### PUT /api/contacts/:id
Updates a specific contact by ID for the authenticated user.
**Request**:
Requires `Authorization: Bearer <accessToken>`.
```json
{
  "name": "Alice M. Smith",
  "phone": "111-222-4444"
}
```
**Response**:
```json
{
  "_id": "65e6d8a2f8a1c3b2d1e0f9g9",
  "user_id": "65e6d6b8f8a1c3b2d1e0f9g8",
  "name": "Alice M. Smith",
  "email": "alice.smith@example.com",
  "phone": "111-222-4444",
  "createdAt": "2024-03-05T10:31:30.908Z",
  "updatedAt": "2024-03-05T10:35:00.123Z",
  "__v": 0
}
```
**Errors**:
-   `401 Unauthorized`: User is not authorized or token is missing/invalid.
-   `403 Forbidden`: User does not have permission to update this contact.
-   `404 Not Found`: Contact not found.

#### DELETE /api/contacts/:id
Deletes a specific contact by ID for the authenticated user.
**Request**:
Requires `Authorization: Bearer <accessToken>`.
**Response**:
```json
{
  "_id": "65e6d8a2f8a1c3b2d1e0f9g9",
  "user_id": "65e6d6b8f8a1c3b2d1e0f9g8",
  "name": "Alice Smith",
  "email": "alice.smith@example.com",
  "phone": "111-222-3333",
  "createdAt": "2024-03-05T10:31:30.908Z",
  "updatedAt": "2024-03-05T10:31:30.908Z",
  "__v": 0
}
```
**Errors**:
-   `401 Unauthorized`: User is not authorized or token is missing/invalid.
-   `403 Forbidden`: User does not have permission to delete this contact.
-   `404 Not Found`: Contact not found.

## Usage
Once the server is running, you can interact with the API using tools like Postman, Insomnia, or cURL.

1.  **Register a User**: Make a POST request to `/api/users/register` with your desired username, email, and password.
2.  **Login**: Make a POST request to `/api/users/login` with your registered email and password to receive an `accessToken`.
3.  **Access Protected Routes**: Include the `accessToken` in the `Authorization: Bearer <accessToken>` header for all contact-related endpoints and `/api/users/current`.
4.  **Manage Contacts**: Use the contact endpoints to create, retrieve, update, and delete your contacts.

## Technologies Used
| Technology         | Description                                        | Link                                                                        |
| :----------------- | :------------------------------------------------- | :-------------------------------------------------------------------------- |
| **Node.js**        | JavaScript runtime environment                     | [https://nodejs.org/](https://nodejs.org/)                                  |
| **Express.js**     | Fast, unopinionated, minimalist web framework      | [https://expressjs.com/](https://expressjs.com/)                            |
| **Mongoose**       | MongoDB object modeling for Node.js                | [https://mongoosejs.com/](https://mongoosejs.com/)                          |
| **MongoDB Atlas**  | Cloud database service                             | [https://www.mongodb.com/atlas](https://www.mongodb.com/atlas)              |
| **Bcrypt**         | Library for hashing passwords                      | [https://www.npmjs.com/package/bcrypt](https://www.npmjs.com/package/bcrypt) |
| **JSON Web Token** | Standard for creating access tokens                | [https://jwt.io/](https://jwt.io/)                                           |
| **Dotenv**         | Loads environment variables from a .env file       | [https://www.npmjs.com/package/dotenv](https://www.npmjs.com/package/dotenv) |
| **express-async-handler** | Simple middleware for handling exceptions inside of Express async routes | [https://www.npmjs.com/package/express-async-handler](https://www.npmjs.com/package/express-async-handler) |
| **Nodemon**        | Utility that monitors for changes and restarts     | [https://nodemon.io/](https://nodemon.io/)                                  |

## Contributing
Contributions are welcome! If you'd like to contribute, please follow these steps:

✨ Fork the repository.
🌿 Create a new branch for your feature or bug fix: `git checkout -b feature/your-feature-name`.
🚀 Make your changes and commit them with clear, concise messages.
🧪 Ensure your code adheres to the project's coding standards.
📝 Write or update tests as appropriate.
⬆️ Push your branch to your forked repository.
🤝 Open a pull request to the `main` branch of this repository.

## License
This project is licensed under the MIT License - see the [LICENSE](https://opensource.org/licenses/MIT) file for details.

## Author Info
Developed with passion by Ibrahim Adegboye.

🔗 **Connect with me**:
*   **LinkedIn**: [https://www.linkedin.com/in/ibrahim-adegboye](https://www.linkedin.com/in/ibrahim-adegboye) (Placeholder, please replace with actual link)
*   **Portfolio**: [https://your-portfolio.com](https://your-portfolio.com) (Placeholder)

---
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express.js](https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Mongoose](https://img.shields.io/badge/Mongoose-800000?style=for-the-badge&logo=mongoose&logoColor=white)](https://mongoosejs.com/)
[![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)](https://jwt.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen)](https://github.com/Kwanza247/mycontacts-backend/actions)

[![Readme was generated by Dokugen](https://img.shields.io/badge/Readme%20was%20generated%20by-Dokugen-brightgreen)](https://www.npmjs.com/package/dokugen)