# Компонентная архитектура

## Компонентная диаграмма

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(admin, "Администратор")
Person(moderator, "Модератор")
Person(user, "Пользователь")
System_Ext(web_site, "Приложение", "Swift, Kotlin")

System_Boundary(fellow_travaler_search_site, "Сервис поиска попутчиков") {
   Container(client_service, "Сервис авторизации", "C++", "Сервис управления пользователями", $tags = "microService")
   Container(route_service, "Сервис маршрутов", "C++", "Сервис построения маршрутов", $tags = "microService") 
   Container(ride_service, "Сервис поездок", "C++", "Сервис управления/мониторинга поездками", $tags = "microService")   
   ContainerDb(db, "База данных", "MySQL", "Хранение данных о маршрутах, поездках и пользователях", $tags = "storage")  
}

Rel(admin, web_site, "добавление информации о популярных/интересных маршрутах")
Rel(moderator, web_site, "модерация пользователей")
Rel(user, web_site, "регистрация, поиск пользователей, создание поздки/маршрута и получение информации о поездках/маршрутах других пользователей, подключение к своей/чужой поездке")

Rel(web_site, client_service, "Работа с пользователями", "localhost/person")
Rel(client_service, db, "INSERT/SELECT/UPDATE", "SQL")

Rel(web_site, route_service, "Работа с маршрутами", "localhost/route")
Rel(route_service, db, "INSERT/SELECT/UPDATE", "SQL")

Rel(web_site, ride_service, "Работа с поездками", "localhost/ride")
Rel(ride_service, db, "INSERT/SELECT/UPDATE", "SQL")
@enduml
```

## Список компонентов  

### Сервис авторизации
**API**:
-	Создание нового пользователя
      - входные параметры: логин, пароль, имя, фамилия, номер телефона
      - выходные параметры: отсутствуют
-	Поиск пользователя по логину
     - входные параметры:  логин
     - выходные параметры: логин, имя, фамилия, номер телефона
-	Поиск пользователя по маске имени и фамилии
     - входные параметры: маска фамилии, маска имени
     - выходные параметры: логин, имя, фамилия, номер телефона

### Сервис маршрутов
**API**:
- Создание маршрута
  - Входные параметры: Начало маршрута, конец маршрута
  - Выходные параметры: идентификатор
- Получение маршрутов пользователя
  - Входные параметры: логин
  - Выходные параметры: список маршрутов, содержащий информацию о начале и конце маршрута, времени и длительности поездки.

### Сервис поездок
**API**:
- Создание поездки
  - Входные параметры: название, инициатир поездки, начало маршрута, конец маршрута, транспорт, максимальное кол-во попутчиков. 
  - Выходыне параметры: идентификатор поездки
- Подключение пользователей к поездке
  - Входные параметры: пункт назначения 
  - Выходные параметры: список ближайших маршрутов, содержащих информацию о количестве оставшихся мест и времени прибытия в указанный пункт назначения
- Получение информации о поездке
  - Входные параметры: идентификатор
  - Выходные параметры: название, инициатор поездки, список пользователей, количество оставшихся мест, история поездки и оставшееся до прибытия время, транспорт