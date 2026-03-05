Context map

|     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- |
| №   | BC1<br><br>(upstream) | BC2<br><br>(downstream) | Тип<br><br>Отношений | Интеграционный<br><br>паттерн | Формат /<br><br>Контракт |
| 1   | Управление каталогом | Читалка | Потребитель-поставщик (Customer-Supplier) | OHS (Каталог предоставляет API) + ACL (Читалка преобразует данные) | REST API, JSON. Контракт: GET /api/v1/books/{id} → { id, title, author, coverUrl } |
| 2   | Управление каталогом | Персонализация | Сервис с открытым протоколом (OHS) | Публикация событий (Publish Language) | События через Kafka/RabbitMQ. Контракт: BookUpdated { bookId, title, genres\[\] } |
| 3   | Управление каталогом | Управление доступом (DRM) | Потребитель-поставщик (Customer-Supplier) | OHS | REST API. Контракт: GET /api/v1/books/exists/{id} → { exists: true } |
| 4   | Биллинг | Управление доступом (DRM) | Сервис с открытым протоколом (OHS) | Публикация событий | События. Контракт: PaymentSucceeded { userId, bookId, transactionId } |
| 5   | Управление доступом (DRM) | Читалка | Потребитель-поставщик (Customer-Supplier) | OHS (DRM предоставляет API) + ACL (Читалка защищает свою модель) | REST API, HTTPS. Контракт: POST /api/v1/token/issue → { token: "xyz", expiresIn: 3600 } |
| 6   | Пользователи (Identity) | Биллинг | Конформист (Conformist) | OHS | REST API. Контракт: GET /api/v1/users/{id} → { id, email } |
| 7   | Пользователи (Identity) | Персонализация | Конформист (Conformist) | OHS | REST API. Контракт: GET /api/v1/users/{id} → { id } |
| 8   | Читалка | Персонализация | Сервис с открытым протоколом (OHS) | Публикация событий | События. Контракт: ProgressUpdated { userId, bookId, page, timestamp } |
| 9   | Управление каталогом | Модерация контента | Партнерство (Partnership) | Общий канал (Shared Kernel) или двухсторонний API | Совместная БД или синхронные вызовы. Контракт: При модерации книга блокируется в каталоге. |
| 10  | Управление каталогом | Биллинг | Разные пути (Separate Ways) | Нет прямого взаимодействия | Не применимо |