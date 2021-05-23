# Отчёт по lab07

## Tutorial

1. Создаём переменные, задаём алисы, переходим в необходимые директории и активируем скрипты

_export GITHUB_USERNAME=<имя_пользователя><br/>
alias gsed=sed<br/>
cd ${GITHUB_USERNAME}/workspace<br/>
pushd .<br/>
source scripts/activate_

2. Клонируем репозиторий и изменияем URL

_git clone https://github.com/${GITHUB_USERNAME}/lab06 projects/lab07<br/>
cd projects/lab07<br/>
git remote remove origin<br/>
git remote add origin https://github.com/${GITHUB_USERNAME}/lab07_

3. Прописываем команды, необходимые для установки hunter

_mkdir -p cmake<br/>
wget https://raw.githubusercontent.com/cpp-pm/gate/master/cmake/HunterGate.cmake -O cmake/HunterGate.cmake_
```sh
gsed -i '/cmake_minimum_required(VERSION 3.4)/a\
\
include("cmake/HunterGate.cmake")\
HunterGate(\
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.251.tar.gz"\
    SHA1 "5659b15dc0884d4b03dbd95710e6a1fa0fc3258d"\
)\
' CMakeLists.txt
```

4. Удаляем, добавленный в предыдущейй работе фреймфорк и добавляем работу с хантером в CMakeLists.txt

_git rm -rf third-party/gtest
```sh
gsed -i '/set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")/a\
\
hunter_add_package(GTest)\
find_package(GTest CONFIG REQUIRED)\
' CMakeLists.txt
gsed -i 's|add_subdirectory(third-party/gtest)||' CMakeLists.txt
gsed -i 's/gtest_main/GTest::main/' CMakeLists.txt
```

5.Запустим компиляцию и сборку проекта

_cmake -H. -B_builds -DBUILD_TESTS=ON<br/>
cmake --build _builds<br/>
cmake --build _builds --target test<br/>
ls -la $HOME/.hunter_

6. Склонируем пакетный менеджер hunter в домашнюю папку

_git clone https://github.com/cpp-pm/hunter $HOME/projects/hunter<br/>
export HUNTER_ROOT=$HOME/projects/hunter<br/>
rm -rf _builds<br/>
cmake -H. -B_builds -DBUILD_TESTS=ON<br/>
cmake --build _builds<br/>
cmake --build _builds --target test_

![travis](https://api.travis-ci.org/Dan10022002/lab07.svg?branch=master&status=passed)
