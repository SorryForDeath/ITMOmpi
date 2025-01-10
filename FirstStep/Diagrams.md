@startuml
actor USER 
participant FRONTEND 
participant LLM
database DATABASE
USER --> FRONTEND: Загрузить макет из Figma
FRONTEND --> LLM: Передать макет
LLM --> LLM: Запустить конвертацию
LLM --> FRONTEND: Передать резльтат конвертации
FRONTEND --> USER: Показать предварительный результат
USER --> FRONTEND: Внести промты для доработки результата
FRONTEND --> LLM: Передать промпты
LLM --> LLM: Доработать результат согласно промптам
LLM --> FRONTEND: Передать измененный результат
LLM --> DATABASE: Сохранить полученный макет
FRONTEND --> USER: Уведомить о возможности сохранения результата
USER --> FRONTEND: Сохранить полученный результат
@enduml
