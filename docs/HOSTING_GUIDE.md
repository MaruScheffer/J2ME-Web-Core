# 🌐 Руководство по хостингу J2ME-Web-Core

> **Важно:** Для работы freej2me-web требуется сервер с поддержкой **Range header** (HTTP 206 Partial Content).

---

## 📋 Сравнение хостингов

| Хостинг | Range Header | Бесплатно | Лимиты | Сложность |
|---------|:------------:|:---------:|--------|:---------:|
| **Cloudflare Pages** | ✅ Да | ✅ Да | Безлимитный трафик | ⭐ Легко |
| **Netlify** | ✅ Да | ✅ Да | 100 GB/мес | ⭐ Легко |
| **Vercel** | ✅ Да | ✅ Да | 100 GB/мес | ⭐ Легко |
| **GitHub Pages** | ❌ Нет | ✅ Да | 100 GB/мес | ⭐ Легко |
| Свой сервер (Nginx) | ✅ Да | — | Безлимит | ⭐⭐⭐ Сложно |

---

## 🏆 Рекомендация: Cloudflare Pages

**Лучший выбор для J2ME-Web-Core:**
- ✅ Безлимитный бесплатный трафик
- ✅ Поддержка Range header
- ✅ Быстрый CDN по всему миру
- ✅ Автодеплой из GitHub
- ✅ Бесплатный SSL сертификат

---

## 📖 Инструкции по настройке

---

# 1. Cloudflare Pages (Рекомендуется)

## Регистрация

1. Перейдите на [dash.cloudflare.com](https://dash.cloudflare.com)
2. Нажмите **Sign Up** (можно войти через GitHub)
3. Подтвердите email

## Создание проекта

1. В панели управления выберите **Pages** в левом меню
2. Нажмите **Create a project**
3. Выберите **Connect to Git**
4. Авторизуйте GitHub и выберите репозиторий `QuadDarv1ne/J2ME-Web-Core`

## Настройка сборки

| Параметр | Значение |
|----------|----------|
| **Project name** | `j2me-web-core` (или любое) |
| **Production branch** | `main` |
| **Build command** | Оставьте пустым |
| **Build output directory** | `/` (или `.`) |

5. Нажмите **Save and Deploy**

## Результат

Через 1-2 минуты сайт будет доступен:
```
https://j2me-web-core.pages.dev
```

Игры:
```
https://j2me-web-core.pages.dev/games/Asphalt4_SamsungSGHX820.jar
```

## Кастомный домен (опционально)

1. **Pages** → ваш проект → **Custom domains**
2. Нажмите **Set up a custom domain**
3. Введите ваш домен (например, `j2me.example.com`)
4. Добавьте CNAME запись в DNS вашего домена:
   ```
   j2me CNAME j2me-web-core.pages.dev
   ```

---

# 2. Netlify

## Регистрация

1. Перейдите на [netlify.com](https://www.netlify.com)
2. Нажмите **Sign Up** (можно войти через GitHub)
3. Подтвердите email

## Создание проекта

### Вариант A: Из GitHub

1. Нажмите **Add new site** → **Import an existing project**
2. Выберите **GitHub** и авторизуйтесь
3. Выберите репозиторий `QuadDarv1ne/J2ME-Web-Core`
4. Настройки:
   - **Build command:** Оставьте пустым
   - **Publish directory:** `/` или `.` 
5. Нажмите **Deploy site**

### Вариант B: Drag & Drop

1. Скачайте содержимое репозитория как ZIP
2. Перейдите на [app.netlify.com/drop](https://app.netlify.com/drop)
3. Перетащите папку с файлами

## Результат

Сайт будет доступен:
```
https://random-name-12345.netlify.app
```

Можно изменить имя в **Site settings** → **Change site name**

## Лимиты бесплатного тарифа

- 100 GB трафика в месяц
- 300 минут сборки
- 1 concurrent build

---

# 3. Vercel

## Регистрация

1. Перейдите на [vercel.com](https://vercel.com)
2. Нажмите **Sign Up** (можно войти через GitHub)
3. Подтвердите email

## Создание проекта

1. Нажмите **Add New** → **Project**
2. Выберите репозиторий `QuadDarv1ne/J2ME-Web-Core`
3. Настройки:
   - **Framework Preset:** Other
   - **Root Directory:** `./`
   - **Build Command:** Оставьте пустым
   - **Output Directory:** `./`
4. Нажмите **Deploy**

## Результат

Сайт будет доступен:
```
https://j2me-web-core.vercel.app
```

## Настройка vercel.json (опционально)

Создайте файл `vercel.json` в корне репозитория:

```json
{
  "headers": [
    {
      "source": "/games/(.*)",
      "headers": [
        { "key": "Accept-Ranges", "value": "bytes" },
        { "key": "Access-Control-Allow-Origin", "value": "*" },
        { "key": "Cache-Control", "value": "public, max-age=31536000" }
      ]
    }
  ]
}
```

## Лимиты бесплатного тарифа

- 100 GB трафика в месяц
- 6000 минут сборки
- Unlimited deployments

---

# 4. GitHub Pages (Не рекомендуется)

## ⚠️ Проблема

GitHub Pages **НЕ поддерживает Range header**. Это означает:

```
Запрос:  Range: bytes=0-1023
Ответ:   HTTP 200 OK (весь файл)
Ожидается: HTTP 206 Partial Content ❌
```

freej2me-web не может загружать JAR файлы напрямую.

## Возможное использование

GitHub Pages подходит только для:
- Хостинга HTML/CSS/JS файлов
- Скачивания JAR файлов пользователями вручную
- Демо-страниц

## Настройка (если нужно)

1. Перейдите в репозиторий → **Settings**
2. **Pages** → **Source**: Deploy from a branch
3. Выберите ветку `main` и папку `/ (root)`
4. Нажмите **Save**

Сайт будет доступен:
```
https://quaddarv1ne.github.io/J2ME-Web-Core/
```

---

# 5. Собственный сервер (Nginx)

## Требования

- VPS или выделенный сервер
- Nginx установлен
- Домен (опционально)

## Конфигурация Nginx

Файл `/etc/nginx/sites-available/j2me`:

```nginx
server {
    listen 80;
    server_name j2me.example.com;
    
    root /var/www/j2me-web-core;
    index index.html;
    
    # Включить Range header
    location / {
        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;
    }
    
    # Специальные заголовки для JAR файлов
    location ~* \.jar$ {
        add_header Accept-Ranges bytes;
        add_header Access-Control-Allow-Origin *;
        add_header Content-Type application/java-archive;
        
        # Включить partial content
        sendfile on;
        tcp_nopush on;
    }
    
    # Кэширование статических файлов
    location ~* \.(jar|jad|zip|html|css|js|png|jpg|gif)$ {
        expires 30d;
        add_header Cache-Control "public, immutable";
        add_header Access-Control-Allow-Origin *;
    }
}
```

## Активация

```bash
# Создать симлинк
sudo ln -s /etc/nginx/sites-available/j2me /etc/nginx/sites-enabled/

# Проверить конфигурацию
sudo nginx -t

# Перезапустить Nginx
sudo systemctl restart nginx
```

## SSL с Certbot

```bash
# Установить Certbot
sudo apt install certbot python3-certbot-nginx

# Получить сертификат
sudo certbot --nginx -d j2me.example.com

# Автообновление
sudo systemctl enable certbot.timer
```

---

## 🔗 Полезные ссылки

| Ресурс | URL |
|--------|-----|
| Cloudflare Pages | https://pages.cloudflare.com |
| Netlify | https://www.netlify.com |
| Vercel | https://vercel.com |
| freej2me-web | https://zb3.me/freej2me-web |
| Репозиторий | https://github.com/QuadDarv1ne/J2ME-Web-Core |

---

## 🎮 Как проверить работоспособность

### Проверка Range header

```bash
curl -I -H "Range: bytes=0-99" https://your-domain.com/games/game.jar
```

**Правильный ответ:**
```
HTTP/2 206 Partial Content
Accept-Ranges: bytes
Content-Range: bytes 0-99/1234567
```

**Неправильный ответ (GitHub Pages):**
```
HTTP/2 200 OK
```

### Проверка CORS

```bash
curl -I https://your-domain.com/games/game.jar
```

**Должен быть заголовок:**
```
Access-Control-Allow-Origin: *
```

---

## 📝 Чек-лист перед деплоем

- [ ] Выбран хостинг с поддержкой Range header
- [ ] Загружены все файлы из репозитория
- [ ] JAR файлы доступны по прямым ссылкам
- [ ] Проверен Range header (HTTP 206)
- [ ] Настроен SSL (HTTPS)
- [ ] Проверена работа в freej2me-web

---

*Документация создана для проекта [J2ME-Web-Core](https://github.com/QuadDarv1ne/J2ME-Web-Core)*
