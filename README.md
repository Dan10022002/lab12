# Отчёт по lab12

1. Открываем учебное пособие по vim на русском языке

_vimtutor ru_

2. Клонируем репозиторий с предыдущей добораторной работы (lab08) и изменяем URL

_git clone https://github.com/${GITHUB_USERNAME}/lab08 Dan10022002/workspace/projects/lab12<br/>
cd Dan10022002/workspace/projects/lab12<br/>
git remote remove origin<br/>
git remote add origin https://github.com/Dan10022002/lab12_

3. Открываем файл README.md, добавляем в него строку, сохраняем файл

_vim README.md_

```sh
:s/lab11/lab12/g
/file<CR>wChaving the path environment variable value **LOG_PATH**<ESC>
:wq
```

4. Открываем файл demo.cpp, добавляем переменную окружения LOG_PATH

_vim sources/demo.cpp_

```sh
Yp3wct>cstdlib<ESC>
/while<CR>ostd::string log_path = std::getenv("LOG_PATH");<ESC>
/"log<CR>
cf"log_path<ESC>
k2dd2kpVj<
:wq
```

5. Создаём дополнительную ветку и делаем релиз

_pushd $HUNTER_ROOT<br/>
git config --global hub.protocol https<br/>
git fork<br/>
git branch -u Dan10022002/master master<br/>
git release create -m"${HUNTER_VERSION}.1" ${HUNTER_VERSION}.1<br/>
git release show ${HUNTER_VERSION}.1<br/>_

6. Скачиваем архив 

_wget https://github.com/Dan10022002/hunter/archive/${HUNTER_VERSION}.1.tar.gz<br/>
export MYHUNTER_SHA1=`openssl sha1 ${HUNTER_VERSION}.1.tar.gz | cut -d'=' -f2 | cut -c2-41`<br/>
echo $MYHUNTER_SHA1<br/>
rm -rf ${HUNTER_VERSION}.1.tar.gz_

7. Обновляем хеш сумму в файле CMakeLists.txt

_popd<br/>
echo $MYHUNTER_SHA1 | pbcopy<br/>
vim CMakeLists.txt_

```sh
/SHA1<CR>
wc2w<C-V><ESC>
:wq
```

8. Заливаем изменения на гитхаб

_git add .<br/>
git commit -m"refactoring"<br/>
git push origin master_
