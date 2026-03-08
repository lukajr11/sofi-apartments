# Sofi Apartments — Инструкция по запуску

## Что у вас в папке

```
sofi-apartments/
├── index.html      ← Основное приложение
├── manifest.json   ← PWA-манифест (для установки на телефон)
├── sw.js           ← Service Worker (офлайн-режим)
├── icon-192.png    ← Иконка приложения (нужно создать)
├── icon-512.png    ← Иконка приложения (нужно создать)
└── README.md       ← Этот файл
```

---

## Шаг 1 — Запуск на Mac (офлайн, без Firebase)

Просто дважды кликните на `index.html` — откроется в Safari или Chrome.
Данные сохраняются в браузере (localStorage). Работает без интернета.

---

## Шаг 2 — Подключение Firebase (синхронизация Mac + iPhone + iPad)

### 2.1 Создайте проект Firebase (бесплатно)

1. Откройте https://console.firebase.google.com
2. Нажмите **«Добавить проект»**
3. Введите имя: `sofi-apartments`
4. Отключите Google Analytics (не нужно) → **Создать проект**

### 2.2 Создайте Realtime Database

1. В меню слева: **Build → Realtime Database**
2. Нажмите **«Создать базу данных»**
3. Выберите регион: **Europe-West (Belgium)** — ближайший к России
4. В правилах выберите **«Тестовый режим»** (доступ для вас одного, можно закрыть позже)
5. Нажмите **Активировать**

### 2.3 Получите конфигурацию

1. В меню слева нажмите на шестерёнку ⚙️ → **Настройки проекта**
2. Прокрутите вниз до раздела **«Ваши приложения»**
3. Нажмите иконку **«</>»** (Веб)
4. Введите название: `sofi-web` → **Зарегистрировать**
5. Скопируйте блок `firebaseConfig`

### 2.4 Вставьте конфигурацию в index.html

Откройте `index.html` в любом текстовом редакторе (TextEdit, VS Code и т.д.).

Найдите этот блок (~120 строка):

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  ...
};
```

Замените его на ваш скопированный конфиг. Пример:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyBxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  authDomain: "sofi-apartments-12345.firebaseapp.com",
  databaseURL: "https://sofi-apartments-12345-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "sofi-apartments-12345",
  storageBucket: "sofi-apartments-12345.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdefabcdefabcdef"
};
```

> ⚠️ Убедитесь, что строка `databaseURL` присутствует — она нужна для Realtime Database.

---

## Шаг 3 — Публикация (чтобы открывать с iPhone/iPad)

### Вариант A — GitHub Pages (бесплатно, рекомендуется)

1. Создайте аккаунт на https://github.com
2. Создайте новый репозиторий: **New repository** → `sofi-apartments` → Public
3. Загрузите файлы: нажмите **«uploading an existing file»**, перетащите все 5 файлов
4. Перейдите в **Settings → Pages**
5. Source: **Deploy from a branch** → Branch: `main` → папка: `/ (root)` → **Save**
6. Через 2 минуты сайт будет доступен по адресу: `https://ВАШ_ЛОГИН.github.io/sofi-apartments`

### Вариант B — Netlify (ещё проще, drag & drop)

1. Откройте https://netlify.com → войдите через GitHub или email
2. Перетащите папку `sofi-apartments` прямо на сайт
3. Готово! Получите ссылку вида `https://random-name-12345.netlify.app`

---

## Шаг 4 — Установка на iPhone / iPad как приложение

1. Откройте ссылку вашего сайта в **Safari** (обязательно Safari, не Chrome)
2. Нажмите кнопку **«Поделиться»** (квадрат со стрелкой вверх)
3. Прокрутите вниз → **«На экран «Домой»»**
4. Нажмите **«Добавить»**

Приложение появится на рабочем столе, открывается без адресной строки — как нативное.

---

## Шаг 5 — Иконки (необязательно, но красиво)

Создайте два PNG файла:
- `icon-192.png` — 192×192 пикселей
- `icon-512.png` — 512×512 пикселей

Можно использовать любой онлайн-сервис, например https://favicon.io

---

## Правила безопасности Firebase (после настройки)

Когда всё работает, закройте доступ к базе — замените правила в Firebase Console:

**Build → Realtime Database → Rules:**

```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

Или, если используете только вы без логина, можно оставить тестовый режим и просто не делиться ссылкой.

---

## Итого — стоимость

| Сервис | Стоимость |
|--------|-----------|
| Firebase Realtime Database | Бесплатно (до 1 ГБ данных, 10 ГБ трафика/месяц) |
| GitHub Pages | Бесплатно |
| Netlify (стартовый план) | Бесплатно |
| **Итого** | **₽0 / месяц** |

---

## Вопросы?

Если что-то не работает — напишите Claude и опишите на каком шаге возникла проблема.
