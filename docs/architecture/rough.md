flowchart TD
    subgraph Client ["🖥️ Frontend (Dispositivo: PC / Móvil)"]
        UI["Web App (Ej: React o Angular)"]
    end

    subgraph Server ["⚙️ Backend (Ej: NestJS / Node.js)"]
        API["API REST Principal"]
        Endpoints["Endpoints Principales:\n- POST /auth/login\n- GET /events (Filtros: ubicación, edad)\n- POST /events (Crear: Admin/Gestor)\n- POST /bookings (Reserva de plaza)\n- POST /payments (Checkout)"]
        API -.-> Endpoints
    end

    subgraph DB_Layer ["🗄️ Base de Datos (Relacional)"]
        DB[(MySQL)]
        Entities["Entidades Principales:\n1. Users (Roles: Admin, Gestor, Usuario)\n2. Events (Edad Min/Max, Ubicación, Aforo Max)\n3. Bookings (Asistencia de usuarios)\n4. Payments (Transacciones)"]
        DB -.-> Entities
    end

    subgraph External ["🌐 Servicios Externos"]
        Stripe["Pasarela de Cobro (Ej: Stripe)"]
        Email["Notificaciones (Ej: SendGrid / SMTP)"]
        Maps["Geolocalización (Ej: Google Maps API)"]
    end

    %% Flujo de datos Frontend <-> Backend
    UI -- "Peticiones de usuario (HTTPS + JSON)" --> API
    API -- "Datos de la App (HTTPS + JSON)" --> UI

    %% Flujo Backend <-> Base de Datos
    API -- "Lectura/Escritura (TCP / SQL)" --> DB
    DB -- "Resultados estructurados" --> API

    %% Flujos con Servicios Externos
    API -- "Crear Intento de Pago (HTTPS + JSON)" --> Stripe
    Stripe -- "Confirmación / Webhooks (HTTPS)" --> API

    API -- "Disparar correos (HTTPS / SMTP)" --> Email
    UI -- "Cargar mapas de eventos (HTTPS)" --> Maps