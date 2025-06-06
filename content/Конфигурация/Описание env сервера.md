---
title: Backend .env
draft: false
---
## Основные настройки
| Переменная | Тип    | Описание                     |
|------------|--------|------------------------------|
| `PORT`     | number | Порт приложения              |
| `APP_ID`   | string | Уникальный идентификатор приложения (UUID) |
| `DOMAIN`   | string | Домен приложения             |
| `URL`      | string | Базовый URL приложения       |

## JWT Настройки
| Переменная                  | Тип    | Описание                              |
|-----------------------------|--------|---------------------------------------|
| `JWT_ACCESS_TIME`           | number | Время жизни access-токена (секунды)   |
| `JWT_CONFIRMATION_SECRET`   | string | Секрет для токенов подтверждения      |
| `JWT_CONFIRMATION_TIME`     | number | Время жизни confirmation-токена       |
| `JWT_REFRESH_SECRET`        | string | Секрет для refresh-токенов            |
| `JWT_REFRESH_TIME`          | number | Время жизни refresh-токена            |
| `JWT_RESET_SECRET`          | string | Секрет для токенов сброса пароля      |
| `JWT_RESET_TIME`            | number | Время жизни reset-токена              |

## Cookie Настройки
| Переменная          | Тип    | Описание                      |
|---------------------|--------|-------------------------------|
| `REFRESH_COOKIE`    | string | Имя cookie для refresh-токена |
| `COOKIE_SECRET`     | string | Секрет для подписи cookie     |

## Throttling
| Переменная          | Тип    | Описание                              |
|---------------------|--------|---------------------------------------|
| `THROTTLE_TTL`      | number | Время хранения записей (секунды)      |
| `THROTTLE_LIMIT`    | number | Лимит запросов за указанное время     |

## База данных
| Переменная       | Тип    | Описание                  |
|------------------|--------|---------------------------|
| `DATABASE_URL`   | string | PostgreSQL connection URL |

## OAuth (Google)
| Переменная             | Тип    | Описание             |
| ---------------------- | ------ | -------------------- |
| `GOOGLE_CLIENT_ID`     | string | Google Client ID     |
| `GOOGLE_CLIENT_SECRET` | string | Google Client Secret |
## OAuth (GitHub)
| Переменная             | Тип    | Описание             |
| ---------------------- | ------ | -------------------- |
| `GITHUB_CLIENT_ID`     | string | GitHub Client ID     |
| `GITHUB_CLIENT_SECRET` | string | GitHub Client Secret |

---

**Примечания:**
1. Все секретные значения должны быть криптостойкими
2. Временные параметры указаны в секундах
3. Для работы с Prisma используется ускоренное подключение
4. Настройки домена и URL должны соответствовать окружению
5. Для OAuth провайдеров необходимо зарегистрировать приложение в соответствующих сервисах