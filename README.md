# Distributed Video to MP3 Converter

This project is a distributed system for converting video files into MP3 audio. It is designed using a microservices architecture to ensure scalability, fault tolerance, and modularity. Each service is responsible for a specific task, such as user authentication, file conversion, notifications, and inter-service communication.

---

## Project Structure

video-to-mp3-converter/ ├── auth/ # Authentication service for user access and permissions ├── converter/ # Service to handle video-to-MP3 conversion ├── gateway/ # API gateway to route requests to the appropriate services ├── notification/ # Service to send notifications about conversion status ├── rabbit/ # RabbitMQ setup for message queueing between services

markdown
Copy code

### Service Overview

#### **1. Auth**
- Manages user authentication and authorization.
- Ensures that only authenticated users can access the system.
- Handles token-based authentication (e.g., JWT).

#### **2. Converter**
- The core service that performs video-to-MP3 conversions.
- Accepts video files via APIs or RabbitMQ messages.
- Uses libraries like `moviepy` or `ffmpeg` for conversion.

#### **3. Gateway**
- Acts as the entry point for all client requests.
- Routes requests to the appropriate microservice (e.g., `auth`, `converter`).
- Implements load balancing and request validation.

#### **4. Notification**
- Sends notifications to users about the status of their conversion.
- Supports email or real-time notifications via WebSocket or APIs.

#### **5. Rabbit**
- RabbitMQ setup for message passing between microservices.
- Handles tasks such as queuing video conversion requests and distributing work across multiple instances of the `converter` service.

---

## Technology Stack

- **Backend**: Python (FastAPI, Flask, or Django for microservices)
- **Message Queue**: RabbitMQ for inter-service communication.
- **Database**: PostgreSQL (for user data) and Redis (for caching and task state tracking).
- **Libraries**:
  - `moviepy` or `ffmpeg` for video processing.
  - `pika` for RabbitMQ integration.
  - `smtplib` or external services (e.g., Twilio, SendGrid) for notifications.
- **Containerization**: Docker for deploying services.
- **Orchestration**: Docker Compose or Kubernetes for managing the system.

---

## Setup and Installation

### **1. Prerequisites**
- Python 3.8 or higher
- Docker and Docker Compose
- RabbitMQ (optional: pre-installed locally or via Docker)

### **2. Clone the Repository**
```bash
git clone <repository-url>
cd video-to-mp3-converter
3. Build and Run Services
Ensure Docker is running.
Use Docker Compose to set up the system:
bash
Copy code
docker-compose up --build
The system will start all services, including RabbitMQ.
4. Environment Variables
Each service requires specific environment variables. These are defined in .env files in the respective service directories. Example for the auth service:

php
Copy code
JWT_SECRET_KEY=<your_secret_key>
DB_URL=postgresql://<username>:<password>@db:5432/auth
For RabbitMQ:

makefile
Copy code
RABBITMQ_HOST=rabbit
RABBITMQ_PORT=5672
RABBITMQ_USER=guest
RABBITMQ_PASSWORD=guest
API Endpoints
Gateway
POST /login: Authenticate a user and return a JWT token.
POST /upload: Upload a video file for conversion.
GET /status/{job_id}: Check the status of a conversion job.
GET /download/{job_id}: Download the converted MP3 file.
Converter
POST /convert: Internal API for processing video-to-MP3 requests.
RabbitMQ Queue: Listens for conversion jobs.
Notification
POST /notify: Internal API to trigger notifications.
Workflow
Authentication:

User logs in via the auth service and receives a JWT token.
All subsequent requests must include this token for authorization.
Video Upload:

User uploads a video file via the gateway.
The gateway validates the request and sends the file to the converter service (directly or via RabbitMQ).
Conversion:

The converter service processes the file and extracts the audio as an MP3.
The service updates the task status in the message queue or database.
Notification:

Once the conversion is complete, the notification service informs the user (via email or WebSocket).
Download:

The user retrieves the converted MP3 file via the gateway.
Scalability Features
Message Queue: RabbitMQ ensures smooth communication between services and distributes workload across multiple instances of the converter service.
Containerization: All services are containerized with Docker for portability and scalability.
API Gateway: Centralized routing and load balancing.
Microservices Architecture: Services are loosely coupled and independently scalable.
Future Enhancements
Add support for additional file formats (e.g., MP4, AVI).
Implement user analytics for tracking conversion usage.
Introduce real-time status updates using WebSockets.
Integrate a cloud storage solution (e.g., AWS S3) for storing uploaded and converted files.
Add CI/CD pipelines for automated deployment and testing.
License
This project is licensed under the MIT License. See the LICENSE file for details.