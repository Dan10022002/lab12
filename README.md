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

_git rm -rf third-party/gtest_
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

```sh
cmake -H. -B_builds -DBUILD_TESTS=ON<br/>
cmake --build _builds<br/>
cmake --build _builds --target test<br/>
ls -la $HOME/.hunter
```

6. Склонируем пакетный менеджер hunter в домашнюю папку

```sh
git clone https://github.com/cpp-pm/hunter $HOME/projects/hunter<br/>
export HUNTER_ROOT=$HOME/projects/hunter<br/>
rm -rf _builds<br/>
cmake -H. -B_builds -DBUILD_TESTS=ON<br/>
cmake --build _builds<br/>
cmake --build _builds --target test
```

7. Изменим версию паектного менеджера на 1.7.0
8. 
```sh
cat $HUNTER_ROOT/cmake/configs/default.cmake | grep GTest<br/>
cat $HUNTER_ROOT/cmake/projects/GTest/hunter.cmake<br/>
mkdir cmake/Hunter
cat > cmake/Hunter/config.cmake <<EOF
hunter_config(GTest VERSION 1.7.0-hunter-9)
EOF
```

8. Добавим файл для демонстрации работы библиотеки print

_mkdir demo_
```sh
cat > demo/main.cpp <<EOF
#include <print.hpp>

#include <cstdlib>

int main(int argc, char* argv[])
{
  const char* log_path = std::getenv("LOG_PATH");
  if (log_path == nullptr)
  {
    std::cerr << "undefined environment variable: LOG_PATH" << std::endl;
    return 1;
  }
  std::string text;
  while (std::cin >> text)
  {
    std::ofstream out{log_path, std::ios_base::app};
    print(text, out);
    out << std::endl;
  }
}
EOF
```

9. Добавляем изменения в файл сборки

```sh
gsed -i '/endif()/a\
\
add_executable(demo ${CMAKE_CURRENT_SOURCE_DIR}/demo/main.cpp)\
target_link_libraries(demo print)\
install(TARGETS demo RUNTIME DESTINATION bin)\
' CMakeLists.txt
```

10. Тестируем код с помощью утилиты polly

_mkdir tools<br/>
git submodule add https://github.com/ruslo/polly tools/polly<br/>
tools/polly/bin/polly.py --test_

![1](https://github.com/Dan10022002/lab07/blob/master/1.png)

_tools/polly/bin/polly.py --install_

![2](https://github.com/Dan10022002/lab07/blob/master/2.png)

11. Создаём метку и заливаем на гитхаб

_git add .<br/>
git commit -m"added cpack config"<br/>
git push origin master --tags_

12. Авторизируемся на  https://travis-ci.org и запускаем тест сборки

![travis](https://api.travis-ci.org/Dan10022002/lab07.svg?branch=master&status=passed)
