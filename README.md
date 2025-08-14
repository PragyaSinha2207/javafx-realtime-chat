# javafx-realtime-chat
javafx-realtime-chat/
│
├── 📂 src/main/java/com/chatapp
│   ├── 📂 config          # WebSocket, CORS, security configs
│   │    ├── WebSocketConfig.java
│   │    ├── CorsConfig.java
│   │    └── SecurityConfig.java (optional)
│   │
│   ├── 📂 controller      # REST & WebSocket endpoints
│   │    ├── AuthController.java
│   │    ├── ChatController.java
│   │    └── WebSocketEventListener.java
│   │
│   ├── 📂 dto             # Data Transfer Objects
│   │    ├── LoginRequest.java
│   │    ├── RegisterRequest.java
│   │    └── MessageDTO.java
│   │
│   ├── 📂 entity          # JPA Entities
│   │    ├── User.java
│   │    └── Message.java
│   │
│   ├── 📂 repository      # Spring Data JPA Repositories
│   │    ├── UserRepository.java
│   │    └── MessageRepository.java
│   │
│   ├── 📂 service         # Business logic layer
│   │    ├── UserService.java
│   │    ├── MessageService.java
│   │    └── AuthService.java
│   │
│   ├── 📂 exception       # Custom exceptions & handlers
│   │    ├── ResourceNotFoundException.java
│   │    ├── UserAlreadyExistsException.java
│   │    └── GlobalExceptionHandler.java
│   │
│   ├── 📂 util            # Utility/helper classes
│   │    ├── JwtUtil.java (if JWT is used)
│   │    └── PasswordEncoderUtil.java
│   │
│   ├── ChatAppApplication.java  # Main Spring Boot app
│
├── 📂 src/main/resources
│   ├── application.properties
│   ├── schema.sql   # Optional: MySQL table creation
│   └── data.sql     # Optional: Sample data
│
└── 📂 javafx-client  # Separate module for JavaFX client UI
    ├── 📂 controller
    │    └── ChatUIController.java
    ├── 📂 model
    │    └── MessageModel.java
    ├── 📂 service
    │    └── WebSocketClientService.java
    ├── MainApp.java
    └── chat.fxml
--------------------------------------------------------------------
Why This Structure Looks Advanced
✅ Layered architecture (Controller → Service → Repository → Entity)
✅ Separate config, dto, and util packages to keep code clean
✅ JavaFX in separate module to show separation of frontend & backend
✅ DTOs for cleaner API contracts (good for interview discussion)
✅ Custom exception handling shows you know error management
--------------------------------------------------------------------------

📅 Day 1 – Backend Setup
Goal: Create base Spring Boot project with WebSocket enabled.
Steps:
Create Spring Boot project from Spring Initializr:
Dependencies: Spring Web, Spring WebSocket, Lombok
Configure WebSocket (WebSocketConfig)
Create ChatMessage model
Create ChatController with /send endpoint
Run backend with mvn spring-boot:run

---------------------------------------------------------------------

📅 Day 2 – JavaFX Client Skeleton
Goal: Create JavaFX chat window & connect to WebSocket.
Steps:
Create Maven JavaFX project
Add dependencies:
Java-WebSocket for WebSocket client
javafx-controls for UI
Build ChatClient class for WebSocket handling
Build basic JavaFX UI (ChatApp) with message area, text field, and send button
Test by running two clients and backend together

--------------------------------------------------------------------------------
📅 Day 3 – JSON Messaging + STOMP
Goal: Send structured messages instead of plain text.
Steps:
Change backend to use STOMP + SockJS (already in WebSocketConfig)
Install spring-messaging (already part of Web dependency)
Update ChatController to handle ChatMessage objects
Use Gson or Jackson in JavaFX to send/receive JSON (sender, content, time)
Test: client sends username and message, backend broadcasts

----------------------------------------------------------------------------
📅 Day 4 – MySQL Integration + User Model
Goal: Save chat history in MySQL.
Steps:
Add Spring Data JPA + MySQL Driver dependencies
Configure application.properties:
properties
Copy
Edit
spring.datasource.url=jdbc:mysql://localhost:3306/chatapp
spring.datasource.username=root
spring.datasource.password=yourpass
spring.jpa.hibernate.ddl-auto=update
Create User entity (username, password)
Create Message entity (sender, content, timestamp)
Create MessageRepository and save messages in DB in ChatController

------------------------------------------------------------------------------
📅 Day 5 – User Login & Registration
Goal: Add authentication before chat.
Steps:
Create UserRepository
Create AuthController with /register and /login (simple session or JWT later)
In JavaFX, create login screen (username/password)
On successful login, open chat window
Store logged-in username in client for sending

📅 Day 6 – Private Messaging & UI Upgrade
Goal: Add direct messages + better interface.
Steps:
In backend, handle /private/{username} topic for private messages
Update ChatMessage to have receiver field (optional)
In JavaFX, add a dropdown of online users
If user selects a specific user, send private message, else broadcast
Improve UI with colors for different users

-----------------------------------------------------------------------

📅 Day 7 – Final Polish & GitHub Push
Goal: Final testing, documentation, deployment.
Steps:
Add chat history loading when client connects
Fix edge cases (disconnect, empty messages, etc.)
Write README.md (project description, setup instructions, screenshots)
Push to GitHub with clean commit history
Optional: Deploy backend to Render/Heroku so others can test
Optional: Bundle JavaFX client as .exe or .jar for easy launch
By following this plan, by the end of Day 7 you’ll have:
Backend: Spring Boot + WebSocket + STOMP + MySQL + REST Auth
Frontend: JavaFX client with login, group chat, private messages, chat history
GitHub Repo: Ready with documentation and screenshots
--------------------------------------------------------------------------------------

A Spring Boot app with STOMP over WebSocket means you’re building a real-time communication system where messages can be sent and received instantly (without refreshing the page), 
and you’re using STOMP as the messaging protocol on top of WebSocket connections.

Here’s the breakdown:

1. Spring Boot
Your backend framework — it provides the REST APIs, controllers, configuration, and integration with WebSocket.
Makes it easy to configure WebSocket and STOMP endpoints.

2. WebSocket
A network protocol that keeps a persistent, two-way connection between the client (browser/app) and the server.
Unlike HTTP (request-response), both sides can push messages anytime.
Great for chat apps, live notifications, real-time dashboards, etc.

3. STOMP (Simple Text Oriented Messaging Protocol)
A simple messaging protocol used over WebSocket.
Works like a “mail system” — clients subscribe to destinations (like topics), and the server sends messages to those destinations.
Makes it easier to handle topics, queues, and message formats than using raw WebSocket messages.

4. How it works in a Spring Boot + STOMP setup
Client connects to a WebSocket endpoint (e.g., /ws).
STOMP is used to send messages to a destination (/app/sendMessage).
Spring maps that to a method in a controller annotated with @MessageMapping.
The server broadcasts or sends messages to clients subscribed to topics (/topic/messages).
All subscribed clients receive the message instantly.

💡 Example

// Controller
@Controller
public class ChatController {

    @MessageMapping("/chat") // Client sends to /app/chat
    @SendTo("/topic/messages") // Server broadcasts to /topic/messages
    public String send(String message) {
        return message;
    }
}
---------------------
Client (JavaScript)

let socket = new SockJS('/ws');
let stompClient = Stomp.over(socket);

stompClient.connect({}, function() {
    stompClient.subscribe('/topic/messages', function(msg) {
        console.log('Received: ' + msg.body);
    });

    stompClient.send("/app/chat", {}, "Hello World");
});

------------------------------------------------------------------------------
          ┌──────────────────────────────┐
          │          Browser/App          │
          └──────────────┬───────────────┘
                         │
                1. CONNECT to WebSocket endpoint
                         │
                         ▼
          ┌──────────────────────────────┐
          │   Spring Boot WebSocket Config│
          │  (register /ws endpoint)      │
          └──────────────┬───────────────┘
                         │
                2. STOMP handshake
                         │
                         ▼
          ┌──────────────────────────────┐
          │  STOMP Broker in Spring Boot  │
          │  Handles subscriptions & send │
          └──────────────┬───────────────┘
                         │
           ┌─────────────┼────────────────────────────┐
           │             │                            │
           ▼             ▼                            ▼
   3. SEND to /app/chat   4. SUBSCRIBE to /topic/msg   (other clients)
           │
           ▼
  @MessageMapping("/chat") in Controller
           │
  ┌────────▼──────────────────────────┐
  │ Process message (e.g., save, log) │
  └────────┬──────────────────────────┘
           │
   5. @SendTo("/topic/msg")
           │
           ▼
  All subscribed clients instantly get the message

----------------------------------------------------------
Key Steps in Words
Client connects to /ws endpoint using SockJS or WebSocket.
STOMP handshake happens, enabling topic-based messaging.
Client sends a message to /app/chat (mapped by @MessageMapping).
Server processes the message and broadcasts to /topic/msg.
All clients subscribed to /topic/msg get it in real time.
---------------------------------------------------------------------

