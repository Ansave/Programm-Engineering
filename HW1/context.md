# Контекст решения

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

Person(admin, "Администратор")
Person(moderator, "Модератор")
Person(user, "Пользователь")

System(fellow_travaler_search_site, "Приложение для поиска попутчиков", "Сервис для поиска попутчиков")
Rel(admin, fellow_travaler_search_site, "добавление информации о популярных/интересных маршрутах")
Rel(moderator, fellow_travaler_search_site, "модерация пользователей")
Rel(user, fellow_travaler_search_site, "регистрация, поиск пользователей, создание поздки/маршрута и получение информации о поездках/маршрутах других пользователей, подключение к своей/чужой поездке")
@enduml
```

## Назначение систем
|Система| Описание|
|-------|---------|
|Сервис поиска попутчиков| Веб-интерфейс, обеспечивающий доступ к созданию поездок с привлечением попутчиков. Бэкенд сервиса реализован в виде микросервисной архитектуры|
