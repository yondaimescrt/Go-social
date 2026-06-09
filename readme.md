# Technical Assessment

This repo is a social networking platform built with Go. It allows users to register, create posts, follow/unfollow other users, comment on posts, and manage user roles. The project includes features like user authentication, email invitations, and role-based access control. PostgreSQL is used as the database, and SendGrid is integrated for sending email notifications.

---

## 🚀 Features

- User registration, login, and authentication
- Role-based access control
- Follow/unfollow users
- Create, update, and delete posts
- Comment on posts
- Email activation and invitations via SendGrid
- User feed with pagination, search, and tag filtering

---

## 🛠 Installation Guide

### Prerequisites

- **Go (>=1.20)** – [Install Go](https://golang.org/dl/)
- **Docker & Docker Compose** – [Install Docker](https://www.docker.com/)
- **SendGrid API Key** – [Create a SendGrid account](https://sendgrid.com/)

---

### 🔧 Setup Instructions

1. **Clone the Repository**
   ```bash
   git clone https://github.com/palat-resto/technical-assessment.git
   cd Go-social
   ```

2. **Set Up Environment Variables**

   Create a `.envrc` file in the root directory and add the following:

   ```env
   ADDR=:8080
   API_URL=http://localhost:8080
   FRONTEND_URL=http://localhost:4000
   DB_ADDR=postgres://admin:adminpassword@localhost/socialnetwork?sslmode=disable
   FROM_EMAIL=your-email@example.com
   SENDGRID_API_KEY=your-sendgrid-api-key
   TOKEN_SECRET=example
   ```

3. **Start PostgreSQL Database**

   ```bash
   docker-compose up -d
   ```

- **Install Go Dependencies**: Run `go mod tidy` to install and clean up dependencies.
- **Run** `go install github.com/air-verse/air@latest` for live reloading(optional)
- **Run** `go install -tags 'postgres' github.com/golang-migrate/migrate/v4/cmd/migrate@latest`


4. **Run Database Migrations**

   ```bash
   source .envrc
   make migrate -up
   ```

5. **Build and Run the Application**

   ```bash
   go build -o bin/main ./cmd/api
   ```

6. **Access the API**

   The backend API is available at: `http://localhost:8080`

---

## 📡 API Endpoints

### 🧑 Authentication

- **POST** `/v1/authentication/user` – Register a new user
  ```json
  {
    "username": "example",
    "email": "example@example.com",
    "password": "password123"
  }
  ```

- **POST** `/v1/authentication/token` – Login and get token
  ```json
  {
    "email": "example@example.com",
    "password": "password123"
  }
  ```

### 👤 User Management

- **GET** `v1/users/{userId}` – Get user details
- **POST** `v1/users/{userId}/follow` – Follow a user
- **POST** `v1/users/{userId}/unfollow` – Unfollow a user
- **GET** `v1/users/activate/{token}` – Activate a user account

### 📝 Posts

- **POST** `v1/posts` – Create a new post
  ```json
  {
    "title": "Post Title",
    "content": "Post Content",
    "tags": ["tag1", "tag2"]
  }
  ```

- **GET** `v1/posts/{postId}` – Get post by ID

- **PUT** `v1/posts/{postId}` – Update post
  ```json
  {
    "title": "Updated Title",
    "content": "Updated Content"
  }
  ```

- **DELETE** `v1/posts/{postId}` – Delete post

### 💬 Comments

- **POST** `v1/posts/{postId}/comments` – Add a comment
  ```json
  {
    "content": "This is a comment."
  }
  ```

- **GET** `v1/posts/{postId}/comments` – Get all comments on a post

### 📰 Feed

- **GET** `/v1/users/feed` – Get user feed
    - Query Parameters:
        - `limit`: Number of posts to fetch (default: 20)
        - `offset`: Offset for pagination (default: 0)
        - `sort`: Sorting order (`asc` or `desc`)
        - `tags`: Filter by tags (comma-separated)
        - `search`: Search by title or content

---