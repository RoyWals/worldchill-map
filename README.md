# Публикация Live-карты WorldChill на GitHub Pages + Gist (бесплатно, без открытых портов)

Этот способ работает даже если игровой хостинг (например, Play2Go) блокирует входящие HTTP-подключения. Плагин сам отправляет состояние карты в GitHub Gist, а сайт загружается с GitHub Pages.

## Шаг 1. Создайте сайт на GitHub Pages (1 раз)
1. Создайте публичный репозиторий на GitHub (например `worldchill-map`).
2. Загрузите в корень репозитория **3 файла** из этой папки (`webmap-site/`):
   - `index.html`
   - `terrain.png`
   - `biomes.bin`
3. В репозитории откройте **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `main` / `/ (root)` → Save**.
4. Через ~1 минуту сайт появится по адресу вида `https://<ваш-ник>.github.io/worldchill-map/`.

## Шаг 2. Создайте токен GitHub (только с правом `gist`)
1. Откройте **GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)** (или *Fine-grained tokens* с правом на Gists).
2. Нажмите **Generate new token (classic)**, поставьте галочку **ТОЛЬКО** `gist` и скопируйте токен (`ghp_...`).

## Шаг 3. Вставьте настройки в `plugins/WorldChillCountries/config.yml`
```yaml
web-map:
  enabled: true
  local-server: false
  show-players: true
  refresh-seconds: 2
  github-gist:
    enabled: true
    token: "ghp_ВАШ_ТОКЕН_СЮДА"
    gist-id: ""
    push-interval-seconds: 20
    pages-url: "https://<ваш-ник>.github.io/worldchill-map/"
```

Перезагрузите сервер или введите `/c admin reload` — плагин **сам создаст Gist**, запишет его `gist-id` в `config.yml`, а команда **`/c webmap`** сразу выдаст готовую кликабельную ссылку `https://<ваш-ник>.github.io/worldchill-map/?gist=...`!

*(Опционально, чтобы не передавать `?gist=...` в URL вообще, можете вставить выданный `gist-id` в строку `const DEFAULT_GIST_ID = "";` в файле `index.html` на GitHub.)*
