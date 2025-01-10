## Диаграмма последовательностей
@startuml
actor USER 
participant SYSTEM 
database DATABASE

USER --> SYSTEM: Загрузить макет из Figma
SYSTEM --> SYSTEM: Настроить параметры конвертации
SYSTEM --> SYSTEM: Запустить конвертацию
SYSTEM --> USER: Показать предварительный результат
USER --> SYSTEM: Внести промты для доработки результата
SYSTEM --> DATABASE: Сохранить полученный макет
SYSTEM --> USER: Уведомить о возможности сохранения результата
USER --> SYSTEM: Сохранить полученный результат

@enduml
