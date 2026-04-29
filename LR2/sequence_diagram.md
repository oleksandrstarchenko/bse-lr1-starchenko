# Діаграма послідовності (Sequence Diagram)

```mermaid
sequenceDiagram
    autonumber
    actor c as c : Client
    participant auth as auth : AuthService
    participant ui as ui : Dashboard
    participant p as p : Preset
    participant ai as ai : AIService
    participant wp as wp : WordPressSite

    c->>auth: authenticate(email, pwd)
    activate auth
    auth-->>c: sessionToken
    deactivate auth

    c->>ui: Запит на генерацію (Title)
    activate ui
    
    ui->>auth: validateSession(token)
    auth-->>ui: valid

    %% Отримання пресета (ToV)
    ui->>p: getSettings()
    activate p
    p-->>ui: toneOfVoice, language
    deactivate p

    %% Генерація тексту
    ui->>ai: generateSEO(Title, toneOfVoice)
    activate ai
    ai-->>ui: GeneratedContent
    deactivate ai
    
    ui-->>c: Показ контенту (Preview)

    %% Опціональне розгортання з перевіркою помилок
    opt Розгортання
        c->>ui: Натискання "Deploy"
        ui->>wp: deploy(GeneratedContent)
        activate wp
        
        alt Успішне розгортання
            wp-->>ui: Success (201 Created)
            ui-->>c: Статус: Опубліковано
        else Помилка API
            wp-->>ui: Error (401 Unauthorized)
            ui-->>c: Помилка: Перевірте API-ключ
        end
        
        deactivate wp
    end
    deactivate ui