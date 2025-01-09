# MERN Stack Employee Record Application

## Overview
This project is a very simple **MERN stack application** for managing employee records. The architecture consists of:
- **Frontend**: React application accessible on port `5173`.
- **Backend**: Node.js application connected to a MongoDB database.
- **Database**: MongoDB configured with a connection URL in the `db` folder. The Database is attached to a docker volume on the host system for data persistance.

The entire setup is containerized using Docker, with a `docker-compose.yml` file orchestrating the frontend, backend, and database services.

---

## Quick Setup

Follow these steps to get the project running:

1. **Clone this repository:**
   ```bash
   git clone https://github.com/YashPatil1609/Docker-Projects.git
   ```

2. **Navigate to the project folder:**
   ```bash
   cd MERN-Dockerised
   ```

3. **Run the Docker containers:**
   ```bash
   docker-compose up
   ```

4. **Access the application:**
   - **Frontend**: Open your browser and go to `http://localhost:5173`.
   - **Backend**: The backend runs on the default port specified in the Docker configuration.

---

## Blog
For a detailed explanation of the project, including architecture and implementation steps, visit [this blog post](https://yashpatilofficial.hashnode.dev/containerizing-a-mern-stack-application).

---

Thank you for checking out this project! Feedback and contributions are always welcome.
