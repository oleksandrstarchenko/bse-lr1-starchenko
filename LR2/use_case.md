# Діаграма прецедентів (Use Case Diagram)

```mermaid
flowchart LR
    Guest["«actor»<br>Гість"]
    Client["«actor»<br>Клієнт"]
    Admin["«actor»<br>Адміністратор"]
    AI["«actor»<br>OpenAI API"]

    subgraph ContentFlow ["ContentFlow System"]
        direction TB
        UC_Auth([Авторизація])
        UC_Demo([Демо-генерація])
        UC_Connect([Підключення WP])
        UC_Generate([Генерація SEO-контенту])
        UC_ToV([Налаштування ToV])
        UC_Deploy([Розгортання в 1 клік])
        UC_Dashboard([Моніторинг])
        UC_Prompts([Керування промптами])
    end

    %% Зв'язки акторів
    Guest --- UC_Demo
    Guest --- UC_Auth

    Client --- UC_Connect
    Client --- UC_Generate
    Client --- UC_Deploy
    Client --- UC_Dashboard
    Client --- UC_Auth

    Admin --- UC_Prompts
    Admin --- UC_Dashboard
    Admin --- UC_Auth

    AI --- UC_Generate

    %% Логічні зв'язки (include / extend)
    UC_Deploy -. "«include»" .-> UC_Generate
    UC_ToV -. "«extend»" .-> UC_Generate