# 05. Задание 3 — Архитектура PUSH-уведомлений

## Верхнеуровневые компоненты

1. **Mobile App** — получает push, открывает экран по payload, отправляет токен устройства.
2. **API Gateway / BFF** — единая точка входа от мобильного клиента.
3. **Cart Service** — источник событий по корзине (например, брошенная корзина).
4. **Order Service** — источник событий по заказам (например, отмена заказа).
5. **Marketing Service** — источник маркетинговых кампаний.
6. **Scheduler** — запускает отложенные и периодические сценарии отправки.
7. **Event Bus / Message Broker** — доставка событий в асинхронный контур уведомлений.
8. **Notification Service** — оркестрация отправки, выбор шаблона, подготовка payload.
9. **Device Token Storage** — хранение push-токенов устройств пользователя.
10. **User Preferences / Consent Service** — согласия и настройки уведомлений.
11. **Template Storage** — шаблоны push по типам событий.
12. **Push Provider Adapter** — адаптер интеграции с push-провайдерами.
13. **FCM/APNs** — внешние каналы доставки push-уведомлений.
14. **Monitoring, Logs, Retry, DLQ** — наблюдаемость, повторные попытки, обработка неуспешных сообщений.

## Сценарий 1: регистрация push-token устройства

1. Mobile App получает device push-token от ОС.
2. Приложение отправляет токен через API Gateway / BFF.
3. BFF передаёт запрос в Notification Service.
4. Notification Service сохраняет токен в Device Token Storage (с привязкой к пользователю/устройству/платформе).
5. При необходимости обновляются настройки согласий в User Preferences / Consent Service.

## Сценарий 2: отправка push по событию (брошенная корзина / отмена заказа)

1. Cart Service или Order Service публикует событие в Event Bus / Message Broker.
2. Notification Service получает событие и определяет тип уведомления.
3. Notification Service проверяет согласия и настройки пользователя в User Preferences / Consent Service.
4. Notification Service получает шаблон из Template Storage и токены из Device Token Storage.
5. Сформированное сообщение отправляется в Push Provider Adapter, далее в FCM/APNs.
6. Статусы доставки и ошибки фиксируются в Monitoring/Logs; при временных сбоях применяются Retry, при исчерпании попыток — DLQ.

