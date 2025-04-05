---
title: OAuthProvider Model
draft: false
---
---

Модель `OAuthProvider` описывает провайдеров авторизации, связанных с пользователем в проекте. Она используется для хранения информации о подключенных OAuth-сервисах (например, Google, GitHub) и их связи с моделью [[User]].

## Описание полей

| Поле       | Тип         | Описание                                                     | Ограничения            |
|------------|-------------|--------------------------------------------------------------|------------------------|
| `provider` | `OAuthProviderEnum` | Тип OAuth-провайдера (например, LOCAL, GOOGLE, GITHUB) | -                      |
| `userId`   | `String`    | Идентификатор пользователя, связанного с провайдером         | -                      |
| `createdAt`| `DateTime`  | Дата создания записи                                         | `@default(now())`      |
| `updatedAt`| `DateTime`  | Дата последнего обновления записи                            | `@updatedAt`           |

## Отношения

### [[User]]
- **Тип связи**: Многие-к-одному  
- **Описание**: Связывает провайдера с моделью [[User]] через поле `userId`. Один пользователь может иметь несколько OAuth-провайдеров.  
- **Поле связи**: `userId` ссылается на `id` в [[User]].  
- **Действие при удалении**: `onDelete: Cascade` — при удалении пользователя удаляются все связанные OAuth-провайдеры.  
- **Пример**: `@relation(fields: [userId], references: [id], onDelete: Cascade)`.

## Уникальные ограничения

- **`@@unique([userId, provider])`**: Гарантирует, что один пользователь не может иметь дублирующихся записей для одного и того же провайдера (например, дважды Google).

## Код модели

```prisma
model OAuthProvider {
  provider  OAuthProviderEnum
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId    String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@unique([userId, provider])
}
```

## Перечисление OAuthProviderEnum

```prisma
enum OAuthProviderEnum {
  LOCAL 
  GOOGLE 
  GITHUB 
}
```

---

## Особенности реализации

- **Провайдеры**: Поле `provider` использует перечисление `OAuthProviderEnum` для строгого определения доступных провайдеров.  
- **Каскадное удаление**: При удалении пользователя ([[User]]) все связанные записи в `OAuthProvider` автоматически удаляются благодаря `onDelete: Cascade`.  
- **Временные метки**: `createdAt` и `updatedAt` автоматически заполняются и обновляются Prisma для отслеживания изменений.

> [!NOTE]  
> Перечисление `OAuthProviderEnum` можно расширить, добавив новых провайдеров (например, `TWITTER`, `FACEBOOK`).

---

## Примеры использования

### Создание OAuth-провайдера для пользователя
```prisma
const oauthProvider = await prisma.oAuthProvider.create({
  data: {
    provider: "GOOGLE",
    userId: "some_user_id"
  }
});
```

### Получение всех провайдеров пользователя
```prisma
const providers = await prisma.oAuthProvider.findMany({
  where: { userId: "some_user_id" }
});
```


---

## Рекомендации

> [!WARNING]  
> Убедитесь, что `userId` существует в [[User]] перед созданием записи, чтобы избежать ошибок целостности данных.

- **Расширение провайдеров**: При добавлении нового провайдера в `OAuthProviderEnum` обновите логику авторизации в приложении.  
- **Логирование**: Логируйте создание и удаление записей для отладки проблем с OAuth.  
- **Тестирование**: Проверьте работу каждого провайдера (LOCAL, GOOGLE, GITHUB) в тестовой среде.

---

Эта заметка описывает модель `OAuthProvider`, её структуру, отношения и примеры использования. Она интегрируется в документацию Obsidian через вики-ссылки и может быть дополнена ссылками на реальные файлы или новые заметки.