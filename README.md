![example workflow](https://github.com/Ermaviv/kittygram_final/actions/workflows/main.yml/badge.svg "Название")

# Терминология
Kittygram — название текущего проекта

# О Проекте
Kittygram - социальная сеть.

## Возможности
- Регистрация/авторизация пользователей
- Публикация постов, где
  - необходимы:
    - Имя кота
    - Год рождения кота
  - можно добавить:
    - цвет кота
    - его картинку
    - достижения кота
- Доступ к просмотру имеют все пользователи
- Полный доступ к посту только авторы самих постов
# Для разработчиков
## Стек технологий
* Django 3.2.3
* djangorestframework 3.12.4
* djoser 2.1.0
* psycopg2-binary 2.9.3
* Pillow 9.0.0
* pytest 6.2.4
* pytest-django 4.4.0
* pytest-pythonpath 0.7.3
* PyYAML 6.0
* webcolors 1.11.1

## Как развернуть
* Выберите и создайте директорию, где будет размещен Kittygram.
* Перенесите в корень директории проекта файл `docker-compose.production.yml`
* Настройте запуск Kittygram в контейнерах и CI/CD с помощью GitHub Actions
  - установить и настроить Docker
  - форкнуть проект на GitHub
  - Присвоить перечень значений в `secrets`
    - DOCKER_USERNAME (наименование вашего аккаунта Docker)
    - DOCKER_PASSWORD (пароль от вашего аккаунта Docker)
    - HOST (IP адрес удаленного сервера)
    - USER (имя пользователя удаленного сервера)
    - SSH_PASSPHRASE (пароль от аккаунта пользователя удаленного сервера)
    - SSH_KEY (закрытый SSH-ключ для подключения в удаленному серверу)
    - TAG_BACKEND (ваше наименование контейнера для backend-а)
    - TAG_FRONTEND (ваше наименование контейнера для frontend-а)
    - TAG_GATEWAY (ваше наименование контейнера для gateway-а)
    - TELEGRAM_TO (наименование канала, куда отправлять инфу о состоянии деплоя)
    - TELEGRAM_TOKEN (токен от Telegram-канала, куда отправлять инфу о состоянии деплоя)
- Создайте файл и директории, чтобы расположить 
содержимое файла `kittygram_workflow.yml` в `/.github/workflows/main.yml`
- Запустите последовательно команды
```bash
# Производит сборку контейнеров в корне проекта
docker compose -f docker-compose.production.yml up
# Производит миграции БД (без нее будет ошибка связывания разных полей)
docker compose -f docker-compose.production.yml exec backend python manage.py migrate
# Перенесите статику для корректного отображения данных
docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic --no-input
```


- Для локального запуска тестов:
  - создайте виртуальное окружение
  - установите в него зависимости из `backend/requirements.txt`
  - запустите виртуальное окружение.

## Файл `.env`
- Его необходимо создать и расположить в корне KIttygram
- Его содержимое:
```bash
POSTGRES_USER=<имя пользователя БД>
POSTGRES_PASSWORD=<пароль БД, обятальное поле>
POSTGRES_DB=<наименование БД>
DB_HOST=<IP адрес БД, если локальная - 127.0.0.1>
DB_PORT=<Порт для БД от хоста, стандартный - 5433>

SECRET_KEY = '<ключ, указываемый в одноименном поле в файле>'
'<backend/kittygram_backend/settings.py>'

ALLOWED_HOSTS = '<имя домена, где размещаете проект> 127.0.0.1 localhost'
# Поменяйте ниже на "True", чтобы увидеть ошибки, если проект работает некорректно
DEBUG = False 
```
## Авторы проекта
- Yandex Practicum
- ermaviv