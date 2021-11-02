# docker-laravel-nuxt
 
Команды
--

 - docker-compose up --build (сборка проекта для продакшена)
 - docker-compose logs название_контейнера (посмотреть логи запущеного контейнера)
 - docker-compose -f docker-compose.yml -f docker-compose.dev.yml up --build (запускаем режим разработки)
 - docker run --rm -v $(pwd):/app composer install (установка vendora без композера, не забыть перейти в папочку laravel)
 - docker-compose exec name_services (позволяет запускать конкретные команды в контейнерах) bash (запускается терминал bash)


        docker-compose exec app php artisan key:generate - запускаем php artisan key:generate в сервисе app
        docker-compose exec db bash - запускаем bash терминал в контейнере db. Выход из него - команда 'exit' 
        docker-compose exec app php artisan migrate
        docker run -it docker-laravel-nuxt_laravel /bin/sh - для пакетов alpine

- docker image prune --all (удалить все images) или docker rmi -f $(docker images -a -q)
- docker rm -vf $(docker ps -a -q) (удалить все контейнеры)
- docker volume rm $(docker volume ls -q) (удалить все переменные)
- docker system prune - удаляет все и вся (containers, volumes, images, networks, build cache)
- docker exec -u 0 -it id_container bash - заходим в bash
- docker images | grep udemy - команда отображает образы и показывает нам только
  те, что начинаются на udemy
- docker-compose -f docker-compose.yml -f docker-compose.dev.yml up --build - команда выполняет объединение двух фалов заменяю значения прод на дев


Конфиги файлов с пояснением
--

- [docker-compose.yml](./readme/docker-compose.description.yml)
- [laravel](./readme/laravel/Dockerfile)
- [nuxt](./readme/nuxt/Dockerfile)
- [nginx](./readme/nginx/nginx.conf.test)
- [php7.3-7.4](./readme/php/Dockerfile)

Заметки
--
- 
- С поддоменами полная беда, пока не знаю как решить, http://project.local/api/hi http://project.local работают
- https://github.com/nevadskiy/laravel-nuxt-docker - интересный реозиторий
- https://titanwolf.org/Network/Articles/Article?AID=a4ef5ad1-fdb4-47ca-8414-3766a6f4e224 - как еще можно организовать структуру
- для alpine необходимо использовать apk update (вместое apt-get update) && apk add (вместо apt-get install)
- разница между ports и expose


    Порты Этот раздел используется для определения сопоставления между хост - сервером и контейнером Docker.
      ports: - 10005:80
    Это означает, что приложение, запущенное внутри контейнера, открыто в порту 80. 
    Но внешняя система/объект не может получить к нему доступ, поэтому его необходимо сопоставить с портом хост-сервера.


    Примечание: 
    вы должны открыть порт хоста 10005 и изменить правила брандмауэра, 
    чтобы разрешить внешним сущностям доступ к приложению.
    Они могут использовать

    http://{хост IP}:10005
    что-то вроде этого

    EXPOSE Это используется исключительно для определения порта, на котором приложение работает внутри контейнера docker.
    Вы также можете определить его в dockerfile. Как правило, это хорошая и широко используемая практика для определения 
    EXPOSE внутри dockerfile, потому что очень редко кто-либо запускает их на другом порту, кроме порта по умолчанию 80

---

    Я полностью согласен с предыдущими ответами. Я просто хотел бы упомянуть, что разница между expose и 
    портами является частью концепции безопасности в docker. 
    Это идет рука об руку с сетью docker. Например:

<blockquote>
Я полностью согласен с предыдущими ответами. Я просто хотел бы упомянуть, 
что разница между expose и портами является частью концепции безопасности в docker. 
Это идет рука об руку с сетью docker. 

Например:

    Представьте себе приложение с веб-интерфейсом и базой данных. 
    Внешний мир нуждается в доступе к веб-интерфейсу (возможно, через порт 80), 
    но только сам сервер должен иметь доступ к хосту и порту базы данных. И
    спользуя пользовательский мост, только веб-порт должен быть открыт, и приложению базы данных не нужны открытые порты, 
    так как веб-интерфейс может связаться с ним по определяемому пользователем мосту.

Это распространенный случай использования при настройке сетевой архитектуры в docker.
Так, например, в сети моста по умолчанию порты недоступны из внешнего мира.
Для этого вы можете открыть входную точку с помощью "ports".
С помощью "expose" вы определяете связь внутри сети. Е
сли вы хотите предоставить порты по умолчанию, вам не нужно определять "expose" в файле docker-compose.
</blockquote>