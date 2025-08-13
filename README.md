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
