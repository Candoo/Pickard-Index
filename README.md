# Thornton Pickard Full-Stack Deployment

This repository contains the necessary configuration files (`docker-compose.yml` and `.env.example`) to run the full Thornton Pickard application suite, which comprises three separate repositories:

1.  **Go Gin API:** `thornton-pickard-api`
2.  **React UI:** `my-modern-react-setup`
3.  **PostgreSQL Database**

## ⚙️ Quick Start Setup

To run the entire stack, follow these steps:

1.  **Clone the deployment repository:**
    ```bash
    git clone <URL of this repo>
    cd thornton-pickard-fullstack-deployment
    ```

2.  **Clone the service repositories:**
    The API and UI repos must be cloned one directory up, next to this deployment folder.
    ```bash
    cd .. 
    git clone <URL for thornton-pickard-api>
    git clone <URL for my-modern-react-setup>
    ```

3.  **Configure environment:**
    Create your actual environment file based on the example:
    ```bash
    cp thornton-pickard-fullstack-deployment/.env.example thornton-pickard-fullstack-deployment/.env
    # NOTE: Edit the .env file to change credentials if needed.
    ```

4.  **Launch the stack:**
    ```bash
    cd thornton-pickard-fullstack-deployment
    docker compose up -d --build
    ```

The application should be available at `http://localhost:3000`.