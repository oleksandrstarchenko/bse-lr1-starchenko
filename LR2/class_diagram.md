# Діаграма класів (Class Diagram)

```mermaid
classDiagram
    class User {
        <<abstract>>
        -userId : int
        -email : String
        +login() : bool
    }

    class Client {
        -subscriptionPlan : String
        +generateContent() : void
    }

    class Admin {
        -staffId : int
        +manageUsers() : void
    }

    class Preset {
        -presetId : int
        -toneOfVoice : String
        -language : String
    }

    class GeneratedContent {
        -contentId : int
        -text : String
        -seoTags : String
    }

    class AuthService {
        +authenticate(email, password) : bool
        +validateSession(token) : bool
    }

    class AIService {
        -apiKey : String
        +requestGeneration(prompt, preset) : GeneratedContent
    }

    class WordPressSite {
        -siteUrl : String
        -apiKey : String
        +deploy(content : GeneratedContent) : bool
    }

    class Dashboard {
        +getSiteStats() : List
    }

    %% Наслідування
    User <|-- Client : inheritance
    User <|-- Admin : inheritance
    
    %% Композиція та агрегація
    Client "1" *-- "*" Preset : owns
    WordPressSite "1" o-- "*" GeneratedContent : stores
    
    %% Асоціація та залежності
    Client "1" -- "1" Dashboard : uses
    Dashboard "1" -- "*" WordPressSite : monitors
    Client "1" ..> "1" AIService : requests
    Client "1" ..> "1" AuthService : authorizes
    AIService ..> GeneratedContent : creates