## Диаграмм пользовательских сценариев
@startuml
left to right direction
actor USER
database "База данных" as DB

rectangle "Конвертация .figma в HTML/CSS" {
  USER --> (Загрузить макет из Figma)
  USER --> (Настроить параметры конвертации)
  USER --> (Запустить конвертацию)
  USER --> (Просмотреть полученный HTML/CSS)
  USER --> (Редактировать код при необходимости)
  USER --> (Сохранить изменения)
  USER --> (Скачать полученный код)
}

(Загрузить макет из Figma) --> (Проверка валидности файла)
(Проверка валидности файла) --> (Настроить параметры конвертации) : Успех
(Проверка валидности файла) --> (Ошибка загрузки) : Ошибка

(Ошибка загрузки) --> USER : Уведомление об ошибке
USER --> (Повторить загрузку макета из Figma)

(Настроить параметры конвертации) --> (Сохранить параметры в БД)
(Сохранить параметры в БД) --> DB : Сохранение параметров
DB --> (Сохранить параметры в БД) : Подтверждение сохранения

(Сохранить параметры в БД) --> (Запустить конвертацию)
(Запустить конвертацию) --> (Выполнение конвертации)
(Выполнение конвертации) --> (Просмотреть полученный HTML/CSS) : Успех
(Выполнение конвертации) --> (Ошибка конвертации) : Ошибка

(Ошибка конвертации) --> USER : Уведомление об ошибке
USER --> (Настроить параметры конвертации) : Повторить настройку

(Просмотреть полученный HTML/CSS) --> (Редактировать код при необходимости)
(Редактировать код при необходимости) --> (Сохранить изменения)
(Сохранить изменения) --> DB : Сохранение изменений
DB --> (Сохранить изменения) : Успех

(Сохранить изменения) --> (Скачать полученный код)
(Скачать полученный код) --> USER : Файл HTML/CSS

USER --> (Отправить отзыв) : Опционально
(Отправить отзыв) --> DB : Сохранение отзыва
DB --> (Отправить отзыв) : Подтверждение
@enduml

## Диаграмма последовательностей

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

## Диаграмма классов

@startuml
class FigmaDesign {
  +String filePath
  +String designData
  +boolean loadDesign()
  +boolean validateFile()
}

class LLM {
  -ConversionSettings settings
  +void preprocessData(designData: String)
  +void analyzeDesign()
  +String generateStructure() : String
  +String convertToHTMLCSS(designData: String, settings: ConversionSettings) : HTMLCSSResult
  +void setConversionSettings(settings: ConversionSettings)
}

class UserInterface {
  -FigmaDesign currentDesign
  -LLM conversionEngine
  -Database database
  -HTMLCSSResult currentResult
  -ConversionSettings currentSettings

  +void uploadDesign()
  +void displayError(message: String)
  +void configureSettings()
  +void startConversion()
  +void displayResult(result: HTMLCSSResult)
  +void editCode(result: HTMLCSSResult) : HTMLCSSResult
  +void saveCode(result: HTMLCSSResult)
  +void downloadCode(result: HTMLCSSResult)
  +void sendFeedback(feedback: String)
}

class Database {
  +void saveSettings(settings: ConversionSettings) : boolean
  +void saveCode(userId: String, projectId: String, html: String, css: String) : boolean
  +void saveFeedback(userId: String, feedback: String) : boolean
  +ConversionSettings loadSettings(userId: String) : ConversionSettings
}

class HTMLCSSResult {
  +String html
  +String css
}

class ConversionSettings {
  +String framework
  +boolean responsive
  +String namingConvention
  +... // Другие настройки
}

FigmaDesign --> LLM : Передает данные
LLM --> UserInterface : Передает HTMLCSSResult
UserInterface --> FigmaDesign : Управляет загрузкой
UserInterface --> LLM : Инициирует конвертацию
UserInterface --> Database : Взаимодействует
UserInterface --> HTMLCSSResult : Отображает/Редактирует
UserInterface --> ConversionSettings : Управляет
LLM --> ConversionSettings : Использует
Database --> ConversionSettings : Загружает
@enduml
