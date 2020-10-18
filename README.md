Оглавление:
1. [About](https://github.com/denisssss43/realtor_api_wiki#about)
    1. [About](https://github.com/denisssss43/realtor_api_wiki#Description)
    1. [About](https://github.com/denisssss43/realtor_api_wiki#Install-and-run)
1. [Model](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#model)
1. [Sidekiq](https://github.com/denisssss43/realtor_api_wiki/blob/master/sidekiq.md#sidekiq)

# ABOUT
Краткое описание проэкта его установка и запуск.

## Description
Данное API предназначено для поиска риэлторских предложений.

## Install and run
Шаг 1. Получение репозитория:
> `$ git clone https://github.com/denisssss43/realtor_api.git ~/github/realtor_api`

Шаг 2. Установка всех необходимых gems:
> `~/github/realtor_api$ bundle install`

Шаг 3. Запуск всех необходимых контейнеров:
> `~/github/realtor_api$ sudo docker-compose up -d`

(доп.) Остановка контейнеров:
> `~/github/realtor_api$ sudo docker-compose stop`

Шаг 4. Запуск миграции в DB:
> `~/github/realtor_api$ rails db:migrate`

Шаг 5. Запуск rails-сервера (API):
> `~/github/realtor_api$ bundle exec rails s -p 3000 -b 0.0.0.0 -d`

Шаг 6. Запуск sidekiq:
> `~/github/realtor_api$ bundle exec sidekiq -d`