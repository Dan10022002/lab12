# Отчёт по lab08

## Tutorial

1. Создаём переменные, переходим в необходимые директории и активируем скрипты

_export GITHUB_USERNAME=<имя_пользователя><br/>
cd ${GITHUB_USERNAME}/workspace<br/>
pushd .<br/>
source scripts/activate_

2. Клонируем репозиторий и изменияем URL

_git clone https://github.com/${GITHUB_USERNAME}/lab07 projects/lab08<br/>
cd projects/lab08<br/>
git submodule update --init<br/>
git remote remove origin<br/>
git remote add origin https://github.com/${GITHUB_USERNAME}/lab08_

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

6. 
11. Заливаем на гитхаб

_git add .<br/>
git commit -m"added cpack config"<br/>
git push origin master --tags_

12. Авторизируемся на  https://travis-ci.org и запускаем тест сборки

![travis](https://api.travis-ci.org/Dan10022002/lab07.svg?branch=master&status=passed)
