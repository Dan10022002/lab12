# Отчёт по lab12

## Tutorial

1. Создаём переменные, переходим в необходимые директории и активируем скрипты

_export GITHUB_USERNAME=<имя_пользователя><br/>
cd ${GITHUB_USERNAME}/workspace<br/>
pushd .<br/>
source scripts/activate_

2. Клонируем репозиторий и изменияем URL

_git clone https://github.com/${GITHUB_USERNAME}/lab07 projects/lab12<br/>
cd projects/lab08<br/>
git submodule update --init<br/>
git remote remove origin<br/>
git remote add origin https://github.com/${GITHUB_USERNAME}/lab12_

3. Создаём файл Dockerfile и записываем в него информацию об образе, который будет использоваться, и командах, которые будут выполнены:

```sh
$ cat > Dockerfile <<EOF
FROM ubuntu:18.04
EOF
```

```sh
cat >> Dockerfile <<EOF

RUN apt update -y
RUN apt install -yy gcc g++ cmake
EOF
```

```sh
cat >> Dockerfile <<EOF

COPY . print/
WORKDIR print
EOF
```

```sh
cat >> Dockerfile <<EOF

RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
RUN cmake --build _build
RUN cmake --build _build --target install
EOF
```

```sh
cat >> Dockerfile <<EOF

ENV LOG_PATH /home/logs/log.txt
EOF
```

```sh
cat >> Dockerfile <<EOF

VOLUME /home/logs
EOF
```

```sh
cat >> Dockerfile <<EOF

WORKDIR _install/bin
EOF
```

```sh
cat >> Dockerfile <<EOF

CMD ./demo
EOF
```

4. Собираем опраз

_docker build -t logger ._

5. Выводим информацию об образах

_docker images_

6. Создаём директорию, куда будут записываться логи, и запускаем докер в интерактивном режиме

_mkdir logs_
```sh
docker run -it -v "$(pwd)/logs/:/home/logs/" logger
text1
text2
text3
<C-D>
```

7. Смотрим внутреннее состояние контейнера

_docker inspect logger_

8. Смотрим логи

_cat logs/log.txt_

9. Редактируем .travis.yml, чтобы мы могли создавать на тревисе контейнер

_vim .travis.yml_

```sh
language: generic
services:
- docker

script:
- docker build -t logger .
```
10. Заливаем на гитхаб

_git add .<br/>
git commit -m"added cpack config"<br/>
git push origin master --tags_

11. Авторизируемся на  https://travis-ci.org и запускаем тест сборки

![travis](https://api.travis-ci.org/Dan10022002/lab08.svg?branch=master&status=passed)
