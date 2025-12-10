# ATOM.GAME - Полная документация проекта

## Содержание
1. [Описание проекта](#описание-проекта)
2. [Технологии](#технологии)
3. [Структура файлов](#структура-файлов)
4. [Инструкция по развёртыванию](#инструкция-по-развёртыванию)
5. [Переменные окружения](#переменные-окружения)
6. [Команды для работы](#команды-для-работы)
7. [Промт для ИИ-помощника](#промт-для-ии-помощника)

---

## Описание проекта

ATOM.GAME - это система управления киберспортивными мероприятиями с тремя ролями пользователей:
- **Super Admin** - управляет всеми городами, пользователями, одобряет заявки
- **City Organizer** - управляет событиями своего города, бюджетом
- **SMM** - создаёт отчёты о публикациях, загружает медиа

---

## Технологии

### Frontend
- **React 18** - библиотека для UI
- **TypeScript** - типизация
- **Vite** - сборщик
- **TailwindCSS** - стили
- **Shadcn/UI** - компоненты (Radix UI)
- **TanStack Query** - работа с API
- **Wouter** - роутинг

### Backend
- **Node.js + Express** - сервер
- **TypeScript** - типизация
- **Drizzle ORM** - работа с базой данных
- **PostgreSQL** - база данных

---

## Структура файлов

### Корневые файлы

```
/
├── package.json          # Зависимости и скрипты npm
├── tsconfig.json         # Настройки TypeScript
├── vite.config.ts        # Настройки сборщика Vite
├── tailwind.config.ts    # Настройки TailwindCSS
├── postcss.config.js     # Настройки PostCSS
├── drizzle.config.ts     # Настройки Drizzle ORM для миграций
├── components.json       # Конфигурация Shadcn UI
└── design_guidelines.md  # Гайдлайны по дизайну
```

### Папка /shared (Общий код)

```
/shared/
└── schema.ts             # ГЛАВНЫЙ ФАЙЛ: схема базы данных и типы
```

**schema.ts** - содержит:
- Определения таблиц БД (users, cities, events, smmReports)
- Zod-схемы для валидации (insertUserSchema, insertEventSchema и т.д.)
- TypeScript типы (User, City, Event, SmmReport)

### Папка /server (Backend)

```
/server/
├── index.ts              # Точка входа сервера
├── routes.ts             # ВСЕ API-эндпоинты
├── storage.ts            # Интерфейс работы с БД (CRUD операции)
├── db.ts                 # Подключение к PostgreSQL
├── seed.ts               # Начальные данные для БД
├── static.ts             # Раздача статических файлов
└── vite.ts               # Интеграция Vite для разработки
```

#### Детальное описание файлов сервера:

**index.ts** - Главный файл сервера
- Создаёт Express-приложение
- Подключает middleware (session, json parsing)
- Регистрирует роуты
- Запускает сервер на порту 5000

**routes.ts** - API-эндпоинты

Авторизация:
- `POST /api/auth/login` - вход (email, password)
- `POST /api/auth/logout` - выход
- `GET /api/auth/me` - текущий пользователь

CRUD:
- `GET /api/users` - список пользователей
- `POST /api/users` - создание пользователя
- `GET /api/cities` - список городов
- `GET /api/cities/:id` - один город
- `POST /api/cities` - создание города
- `GET /api/events` - список событий (фильтр ?cityId=xxx)
- `GET /api/events/pending` - события на утверждение
- `POST /api/events` - создание события
- `PATCH /api/events/:id` - обновление события
- `PATCH /api/events/:id/approve` - одобрение события
- `PATCH /api/events/:id/reject` - отклонение события
- `GET /api/smm-reports` - отчёты SMM (фильтр ?userId=xxx)
- `POST /api/smm-reports` - создание отчёта
- `PATCH /api/smm-reports/:id/submit` - отправка отчёта на проверку
- `GET /api/dashboard/stats` - статистика для дашборда

**storage.ts** - Работа с БД
- Класс `DatabaseStorage` реализует интерфейс `IStorage`
- Все CRUD операции для users, cities, events, smmReports
- Использует Drizzle ORM для запросов

**db.ts** - Подключение к БД
- Создаёт пул подключений PostgreSQL
- Экспортирует объект `db` для Drizzle

### Папка /client (Frontend)

```
/client/
├── index.html            # Главная HTML-страница
├── public/
│   └── favicon.png       # Иконка сайта
└── src/
    ├── main.tsx          # Точка входа React
    ├── App.tsx           # Главный компонент с роутингом
    ├── index.css         # Глобальные стили и тема
    ├── lib/
    │   ├── queryClient.ts # Настройка TanStack Query
    │   └── utils.ts       # Утилиты (cn для классов)
    ├── hooks/
    │   ├── use-toast.ts   # Хук для уведомлений
    │   ├── use-mobile.tsx # Хук для мобильного режима
    │   └── use-auth.tsx   # Хук авторизации (AuthContext)
    ├── pages/             # Страницы приложения
    └── components/        # UI-компоненты
```

### Страницы (/client/src/pages/)

```
/pages/
├── LoginPage.tsx         # Страница входа
├── CitiesPage.tsx        # Список городов
├── EventsPage.tsx        # Список событий
├── BudgetPage.tsx        # Управление бюджетом
├── UsersPage.tsx         # Управление пользователями
├── MediaPage.tsx         # Медиа-файлы
├── SocialAnalyticsPage.tsx # Аналитика соцсетей
├── NotificationsPage.tsx # Уведомления
├── SettingsPage.tsx      # Настройки
└── not-found.tsx         # Страница 404
```

### Компоненты (/client/src/components/)

```
/components/
├── dashboard/                  # Дашборды по ролям
│   ├── SuperAdminDashboard.tsx # Дашборд суперадмина
│   ├── CityOrgDashboard.tsx    # Дашборд организатора города
│   ├── SMMDashboard.tsx        # Дашборд SMM-специалиста
│   └── EventDetailPage.tsx     # Детали события
├── ui/                         # Базовые UI-компоненты (Shadcn)
│   ├── button.tsx
│   ├── card.tsx
│   ├── dialog.tsx
│   ├── form.tsx
│   ├── input.tsx
│   ├── select.tsx
│   ├── tabs.tsx
│   ├── toast.tsx
│   ├── sidebar.tsx
│   └── ... (и другие)
├── AppSidebar.tsx              # Боковое меню
├── ThemeToggle.tsx             # Переключатель темы
├── EventCard.tsx               # Карточка события
├── CityCard.tsx                # Карточка города
├── ApprovalCard.tsx            # Карточка заявки на одобрение
├── StatCard.tsx                # Карточка статистики
├── BudgetProgress.tsx          # Прогресс-бар бюджета
├── StatusBadge.tsx             # Бейдж статуса
├── ChecklistItem.tsx           # Элемент чеклиста
└── EventCreateDialog.tsx       # Диалог создания события
```

---

## Инструкция по развёртыванию

### Требования
- Node.js 18+ 
- PostgreSQL 14+
- npm или yarn

### Шаг 1: Клонирование и установка

```bash
# Клонируйте репозиторий
git clone <ваш-репозиторий>
cd atom-game

# Установите зависимости
npm install
```

### Шаг 2: Настройка базы данных

1. Создайте базу данных PostgreSQL:
```sql
CREATE DATABASE atomgame;
```

2. Создайте файл `.env` в корне проекта:
```env
DATABASE_URL=postgresql://user:password@localhost:5432/atomgame
SESSION_SECRET=ваш-секретный-ключ-для-сессий-минимум-32-символа
```

### Шаг 3: Применение схемы БД

```bash
# Применить схему к базе данных
npm run db:push
```

### Шаг 4: Заполнение начальными данными (опционально)

```bash
# Запустить seed-скрипт
npx tsx server/seed.ts
```

### Шаг 5: Сборка для продакшена

```bash
# Собрать frontend и backend
npm run build
```

### Шаг 6: Запуск

```bash
# Для разработки (с hot-reload)
npm run dev

# Для продакшена
npm start
```

Приложение будет доступно на http://localhost:5000

### Настройка Nginx (для продакшена)

```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### Запуск через PM2 (для продакшена)

```bash
# Установите PM2
npm install -g pm2

# Запустите приложение
pm2 start npm --name "atomgame" -- start

# Автозапуск при перезагрузке сервера
pm2 startup
pm2 save
```

---

## Переменные окружения

| Переменная | Описание | Пример |
|------------|----------|--------|
| `DATABASE_URL` | URL подключения к PostgreSQL | `postgresql://user:pass@localhost:5432/atomgame` |
| `SESSION_SECRET` | Секретный ключ для сессий | `your-secret-key-min-32-chars` |
| `PORT` | Порт сервера (опционально) | `5000` |

---

## Команды для работы

```bash
# Разработка
npm run dev           # Запуск в режиме разработки

# База данных
npm run db:push       # Применить схему к БД
npm run db:push --force # Принудительно применить (осторожно!)

# Сборка
npm run build         # Собрать проект
npm start             # Запустить собранный проект

# Проверка типов
npx tsc --noEmit      # Проверить TypeScript без сборки
```

---

## Промт для ИИ-помощника

Скопируйте этот промт и используйте его при работе с ИИ (ChatGPT, Claude и т.д.):

```
Я работаю над проектом ATOM.GAME - системой управления киберспортивными мероприятиями.

ТЕХНОЛОГИИ:
- Frontend: React 18, TypeScript, Vite, TailwindCSS, Shadcn/UI, TanStack Query, Wouter
- Backend: Node.js, Express, TypeScript, Drizzle ORM, PostgreSQL

СТРУКТУРА ПРОЕКТА:

1. /shared/schema.ts - ГЛАВНЫЙ ФАЙЛ для типов и схемы БД
   - Таблицы: users, cities, events, smmReports
   - Здесь определяются все поля и типы данных
   - Используй createInsertSchema из drizzle-zod для валидации

2. /server/routes.ts - ВСЕ API-эндпоинты
   - Тонкие роуты, логика в storage.ts
   - Валидация через Zod-схемы

3. /server/storage.ts - CRUD операции с БД
   - Класс DatabaseStorage реализует IStorage
   - Добавляй новые методы здесь

4. /client/src/App.tsx - Главный компонент с роутингом
   - Используется Wouter для навигации
   - SidebarProvider для бокового меню

5. /client/src/components/dashboard/ - Дашборды по ролям
   - SuperAdminDashboard.tsx - для админа
   - CityOrgDashboard.tsx - для организатора города
   - SMMDashboard.tsx - для SMM-специалиста

6. /client/src/pages/ - Страницы приложения
   - Добавляй новые страницы сюда
   - Регистрируй в App.tsx

ПРАВИЛА КОДА:

1. Query keys должны быть строками с полным URL:
   ПРАВИЛЬНО: queryKey: [`/api/events?cityId=${cityId}`]
   НЕПРАВИЛЬНО: queryKey: ["/api/events", { cityId }]

2. Для мутаций используй apiRequest из @/lib/queryClient
3. После мутации invalidate кэш: queryClient.invalidateQueries({ queryKey: [...] })
4. Компоненты UI берём из @/components/ui/
5. Иконки из lucide-react

КАК ДОБАВИТЬ НОВУЮ СУЩНОСТЬ:

1. Добавь таблицу в /shared/schema.ts
2. Создай insert-схему и типы
3. Добавь методы в /server/storage.ts
4. Добавь роуты в /server/routes.ts
5. Создай компоненты в /client/src/components/
6. После изменения схемы: npm run db:push

КАК ИСПРАВИТЬ ОШИБКУ:

1. Проверь консоль браузера (F12 -> Console)
2. Проверь логи сервера в терминале
3. Проверь соответствие query key и invalidation key
4. Убедись что типы совпадают между frontend и backend

ТЕКУЩЕЕ СОСТОЯНИЕ:
[Опиши здесь что ты хочешь сделать или какая проблема]
```

---

## Быстрый справочник по файлам

| Что хочешь сделать | Какой файл редактировать |
|-------------------|-------------------------|
| Добавить поле в таблицу | `/shared/schema.ts` |
| Добавить API-эндпоинт | `/server/routes.ts` |
| Добавить метод работы с БД | `/server/storage.ts` |
| Добавить страницу | `/client/src/pages/` + регистрация в `App.tsx` |
| Добавить компонент | `/client/src/components/` |
| Изменить стили/тему | `/client/src/index.css` |
| Добавить пункт меню | `/client/src/components/AppSidebar.tsx` |
| Изменить роутинг | `/client/src/App.tsx` |

---

## Типичные ошибки и решения

### Ошибка: Query не обновляется после мутации
**Решение:** Проверь что queryKey при invalidate точно совпадает с queryKey в useQuery

### Ошибка: Данные не загружаются
**Решение:** Проверь что enabled: true (или условие выполняется) в useQuery

### Ошибка: TypeScript ругается на типы
**Решение:** Проверь что типы в schema.ts актуальны и используешь правильный тип (Select vs Insert)

### Ошибка: База данных не подключается
**Решение:** Проверь DATABASE_URL в .env и доступность PostgreSQL

---

## Контакты и поддержка

При возникновении вопросов используйте промт выше для работы с ИИ-помощником.
