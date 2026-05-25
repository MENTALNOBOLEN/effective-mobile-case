# 04. Задание 2 — Проектирование REST API

## Endpoint открытия экрана партнёрских магазинов

`GET /api/v1/partner-stores?cityId=1`

### Назначение

Возвращает список магазинов-партнёров для экрана «Выберите магазин».

### Параметры

- `cityId` (optional, integer) — идентификатор города.

### Поведение при клике на карточку магазина

При нажатии на карточку магазина мобильное приложение открывает `externalUrl` во встроенном WebView. Пользователь остаётся внутри приложения, а содержимое партнёрского магазина загружается по внешней ссылке.

### Пример успешного ответа (200)


```json
{
  "screenTitle": "Выберите магазин",
  "stores": [
    {
      "id": "metro",
      "name": "METRO",
      "logoUrl": "https://cdn.petrushka.ru/partners/metro.png",
      "delivery": {
        "type": "timeslot",
        "displayText": "Ближайшая доставка сегодня 21:00-23:00",
        "date": "2026-05-23",
        "slotFrom": "21:00",
        "slotTo": "23:00"
      },
      "externalUrl": "https://partner.example.com/metro",
      "isAvailable": true,
      "sortOrder": 1
    },
    {
      "id": "auchan",
      "name": "Ашан",
      "logoUrl": "https://cdn.petrushka.ru/partners/auchan.png",
      "delivery": {
        "type": "timeslot",
        "displayText": "Ближайшая доставка сегодня 18:00-20:00",
        "date": "2026-05-23",
        "slotFrom": "18:00",
        "slotTo": "20:00"
      },
      "externalUrl": "https://partner.example.com/auchan",
      "isAvailable": true,
      "sortOrder": 2
    },
    {
      "id": "vkusvill",
      "name": "ВкусВилл",
      "logoUrl": "https://cdn.petrushka.ru/partners/vkusvill.png",
      "delivery": {
        "type": "express",
        "displayText": "Быстрая доставка от 20 до 60 минут",
        "minMinutes": 20,
        "maxMinutes": 60
      },
      "externalUrl": "https://partner.example.com/vkusvill",
      "isAvailable": true,
      "sortOrder": 3
    },
    {
      "id": "victoria",
      "name": "ВИКТОРИЯ",
      "logoUrl": "https://cdn.petrushka.ru/partners/victoria.png",
      "delivery": {
        "type": "timeslot",
        "displayText": "Ближайшая доставка сегодня 17:00-19:00",
        "date": "2026-05-23",
        "slotFrom": "17:00",
        "slotTo": "19:00"
      },
      "externalUrl": "https://partner.example.com/victoria",
      "isAvailable": true,
      "sortOrder": 4
    }
  ]
}
```

### Коды ответов

- `200 OK` — список магазинов успешно получен.
- `400 Bad Request` — некорректные query-параметры.
- `404 Not Found` — для указанного города/локации партнёрские магазины не найдены.
- `500 Internal Server Error` — внутренняя техническая ошибка сервиса.

