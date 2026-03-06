Каталог событий и команд.

|     |     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- |
| №   | Имя события / команды | Тип (событие / команда) | Инициатор (актор / система / политика) | Ключевые данные (payload) | Тип события (notification / state transfer / domain event) | Публичность (public / private) | Потребители |
| 1   | **КнигиПолучены** (BooksReceived) | Событие | Издательство / Администратор | publisherId, books\[\], timestamp | Domain event | Private | Контекст «Модерация» |
| 2   | **ПроверитьКнигу** (ValidateBook) | Команда | Система (после получения) | bookId, fileUrl, metadata | —   | Private | Агрегат «Книга на модерации» |
| 3   | **КнигаПроверена** (BookValidated) | Событие | Система (модерация) | bookId, status (approved/rejected), reason | Domain event | Private | Контекст «Каталог» |
| 4   | **КнигаОпубликована** (BookPublished) | Событие | Система (каталог) | bookId, title, author, genres\[\], publishedAt | State transfer | Public | Персонализация, Поиск, Уведомления |
| 5   | **КнигаЗаблокирована** (BookBlocked) | Событие | Администратор | bookId, reason, blockedAt | State transfer | Public | DRM, Читалка |
| 6   | **СоздатьЗаказ** (CreateOrder) | Команда | Читатель (UI) | userId, bookId, price, paymentMethod | —   | Private | Агрегат «Заказ» |
| 7   | **ЗаказСоздан** (OrderCreated) | Событие | Система (биллинг) | orderId, userId, bookId, amount, createdAt | Domain event | Private | Контекст «Биллинг» (внутр.) |
| 8   | **ИнициироватьПлатеж** (InitiatePayment) | Команда | Система (биллинг) | orderId, amount, paymentMethod, returnUrl | —   | Private | Платежный шлюз (внеш.) |
| 9   | **ПлатёжЗавершен** (PaymentCompleted) | Событие | Платежный шлюз (внеш.) | transactionId, orderId, status, timestamp | State transfer | Public | Контекст «Биллинг» |
| 10  | **ВыдатьЛицензию** (IssueLicense) | Команда | Система (политика) | userId, bookId, licenseType (purchase/subscription) | —   | Private | Агрегат «Лицензия» (DRM) |
| 11  | **ЛицензияВыдана** (LicenseIssued) | Событие | Система (DRM) | licenseId, userId, bookId, issuedAt, expiresAt | Domain event | Public | Читалка, Аналитика |
| 12  | **ОткрытьКнигу** (OpenBook) | Команда | Читатель (UI) | userId, bookId, device | —   | Private | Агрегат «Сессия чтения» |
| 13  | **КнигаОткрыта** (BookOpened) | Событие | Система (читалка) | userId, bookId, timestamp, device | Domain event | Public | Персонализация, Аналитика |
| 14  | **ПерелистнутьСтраницу** (TurnPage) | Команда | Читатель (UI) | userId, bookId, page | —   | Private | Агрегат «Прогресс» |
| 15  | **ПрогрессОбновлён** (ProgressUpdated) | Событие | Система (читалка) | userId, bookId, page, percentage, timestamp | Domain event | Public | Персонализация, Аналитика |
| 16  | **КнигаДочитана** (BookFinished) | Событие | Система (читалка) | userId, bookId, timestamp | Domain event | Public | Персонализация |
| 17  | **ДобавитьЗакладку** (AddBookmark) | Команда | Читатель (UI) | userId, bookId, page, note? | —   | Private | Агрегат «Закладка» |
| 18  | **ЗакладкаДобавлена** (BookmarkAdded) | Событие | Система (читалка) | bookmarkId, userId, bookId, page, addedAt | Domain event | Private | Контекст «Читалка» |
| 19  | **ПодпискаАктивирована** (SubscriptionActivated) | Событие | Система (биллинг) | subscriptionId, userId, plan, startDate, endDate | Domain event | Public | DRM, Уведомления |
| 20  | **ПодпискаИстекает** (SubscriptionExpiring) | Событие | Система (время) | subscriptionId, userId, expiresInDays | Notification | Public | Уведомления |
| 21  | **ПодпискаЗавершена** (SubscriptionExpired) | Событие | Система (биллинг) | subscriptionId, userId, endDate | State transfer | Public | DRM |
| 22  | **ОбновитьРекомендации** (UpdateRecommendations) | Команда | Система (политика) | userId, triggerEvent (bookFinished/progress) | —   | Private | Контекст «Персонализация» |
| 23  | **РекомендацииОбновлены** (RecommendationsUpdated) | Событие | Система (персонализация) | userId, recommendedBooks\[\], generatedAt | Notification | Public | UI, Уведомления |
| 24  | **ОтправитьУведомление** (SendNotification) | Команда | Система (политика) | userId, type (email/push), template, data | —   | Private | Email-провайдер, Push-сервис |
| 25  | **УведомлениеОтправлено** (NotificationSent) | Событие | Email-провайдер (внеш.) | notificationId, userId, status, sentAt | Notification | Private | Контекст «Уведомления» |