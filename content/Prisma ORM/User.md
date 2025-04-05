---
title: User Model
draft: false
---
---

Модель `User` описывает пользователя в проекте. Она используется для хранения основной информации о пользователе и связей с учетными данными и OAuth-провайдерами.

## Описание полей

| Поле            | Тип       | Описание                                                     | Ограничения                     |
|-----------------|-----------|--------------------------------------------------------------|---------------------------------|
| `id`            | `String`  | Уникальный идентификатор пользователя                        | `@id`, `@unique`, `@default(cuid())` |
| `username`      | `String`  | Уникальное имя пользователя                                  | `@unique`                       |
| `email`         | `String`  | Уникальный email пользователя                                | `@unique`                       |
| `password`      | `String`  | Пароль пользователя (хранится в зашифрованном виде)          | -                               |
| `credentialsId` | `String`  | Уникальный идентификатор учетных данных пользователя         | `@unique`                       |

## Отношения

### [[Credentials]]
- **Тип связи**: Один-к-одному  
- **Описание**: Связывает пользователя с моделью `[[Credentials]]` через поле `credentialsId`.  
- **Поле связи**: `credentialsId` ссылается на `id` в `[[Credentials]]`.  
- **Пример**: `Credentials @relation(fields: [credentialsId], references: [id])`.

### [[OAuthProvider]]
- **Тип связи**: Один-ко-многим  
- **Описание**: Пользователь может быть связан с несколькими OAuth-провайдерами (например, Google, GitHub).  
- **Пример**: `OAuthProvider[]`.

## Код модели

```prisma
model User {
  id       String @id @unique @default(cuid())
  username String @unique
  email    String @unique
  password String

  credentials Credentials @relation(fields: [credentialsId], references: [id])
  credentialsId String @unique

  oauthProviders OAuthProvider[] 
}
```

---
## Особенности реализации

- **Генерация ID**: Поле `id` автоматически заполняется с помощью функции `cuid()` для обеспечения уникальности и безопасности.  
- **Уникальность**: Поля `username` и `email` имеют ограничение `@unique`, что исключает дубликаты.  
- **Пароль**: Хранится в зашифрованном виде (рекомендуется использовать bcrypt).
- **OAuth**: Поддержка нескольких провайдеров через связь с [[OAuthProvider]].

> [!WARNING]  
> Не храните пароли в открытом виде в продакшене! Используйте хеширование.

---

## Примеры использования

### Создание пользователя
```prisma
const newUser = await prisma.user.create({
  data: {
    username: "john_doe",
    email: "john@example.com",
    password: "hashed_password",
    credentials: {
      create: {
        // данные для credentials
      }
    }
  }
});
```

### Получение пользователя с OAuth-провайдерами
```prisma
const user = await prisma.user.findUnique({
  where: { id: "some_user_id" },
  include: { oauthProviders: true }
});
```

---
## Связи с другими заметками

- **[[Credentials]]**: Модель учетных данных пользователя.  
- **[[OAuthProvider]]**: Модель провайдеров авторизации.  

---
