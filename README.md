# Лабораторная работа №03: Автоматизация сборки проектов с помощью CMake


## Ход выполнения работы

### Шаг 1. Настройка окружения и создание структуры проекта
Создание рабочей директории для лабораторной работы №3, инициализация локального Git-репозитория и связывание его с удаленным репозиторием на GitHub.

**Команда:**
```bash
export GITHUB_TOKEN="my_token"
mkdir -p ~/workspace/projects/lab03 && cd ~/workspace/projects/lab03
git init -b main
git remote add origin https://${GITHUB_TOKEN}@[github.com/g4l0p3r1d0l/lab03.git](https://github.com/g4l0p3r1d0l/lab03.git)
mkdir include sources examples

```

**Вывод:**

```bash
Initialized empty Git repository in /Users/mac/workspace/projects/lab03/.git/

```

---

### Шаг 2. Создание исходного кода проекта

Создание заголовочного файла, файла реализации функции вывода текста и основного файла примера.

**Команды:**

```bash
# 1. Создание заголовочного файла include/print.hpp
cat << 'EOF' > include/print.hpp
#pragma once
#include <string>
#include <ostream>
void print(const std::string& text, std::ostream& out);
EOF

# 2. Создание файла реализации sources/print.cpp
cat << 'EOF' > sources/print.cpp
#include <print.hpp>
#include <iostream>
void print(const std::string& text, std::ostream& out) {
    out << text;
}
EOF

# 3. Создание файла с примером использования examples/example1.cpp
cat << 'EOF' > examples/example1.cpp
#include <print.hpp>
#include <iostream>
int main() {
    print("Hello from Lab 03!", std::cout);
    std::cout << std::endl;
    return 0;
}
EOF

```

---

### Шаг 3. Создание конфигурационного файла CMakeLists.txt

Написание сценария сборки для CMake с определением статической библиотеки `print` и исполняемого файла `example1`.

**Команда:**

```bash
cat << 'EOF' > CMakeLists.txt
cmake_minimum_required(VERSION 3.10)
project(lab03 VERSION 1.0.0)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(print STATIC sources/print.cpp)
target_include_directories(print PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
add_executable(example1 examples/example1.cpp)
target_link_libraries(example1 print)
EOF

```

---

### Шаг 4. Генерация файлов сборки (Configure)

Запуск утилиты CMake для генерации файлов сборки в директорию `_build` с использованием компилятора AppleClang.

**Команда:**

```bash
cmake -H. -B_build

```

**Вывод:**

```bash
-- The C compiler identification is AppleClang 17.0.0.17000013
-- The CXX compiler identification is AppleClang 17.0.0.17000013
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.5s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/mac/workspace/projects/lab03/_build

```

---

### Шаг 5. Сборка проекта (Build)

Компиляция исходного кода, создание статической библиотеки и линковка исполняемого файла.

**Команда:**

```bash
cmake --build _build

```

**Вывод:**

```bash
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
[ 75%] Building CXX object CMakeFiles/example1.dir/examples/example1.cpp.o
[100%] Linking CXX executable example1
[100%] Built target example1

```

---

### Шаг 6. Тестирование работоспособности программы

Запуск скомпилированного бинарного файла для проверки корректности работы библиотеки.

**Команда:**

```bash
./_build/example1

```

**Вывод:**

```bash
Hello from Lab 03!

```

---

### Шаг 7. Настройка Git и публикация на GitHub

Создание файла `.gitignore` для исключения артефактов сборки из индекса репозитория, фиксация изменений (коммит) и отправка кода на удаленный сервер.

**Команды:**

```bash
echo "_build/" > .gitignore
git add .
git commit -m "feat: complete lab03 basic cmake setup"
git push origin main --force

```

**Вывод:**

```bash
[main (root-commit) bb383ba] feat: complete lab03 basic cmake setup
 6 files changed, 27 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 CMakeLists.txt
 create mode 100644 README.md
 create mode 100644 examples/example1.cpp
 create mode 100644 include/print.hpp
 create mode 100644 sources/print.cpp
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 10 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (11/11), 1.22 KiB | 1.22 MiB/s, done.
Total 11 (delta 0), reused 0 (delta 0), pack-reused 0
To [https://github.com/g4l0p3r1d0l/lab03.git](https://github.com/g4l0p3r1d0l/lab03.git)
 + 464b8c0...bb383ba main -> main (forced update)

```

---

## Вывод

В ходе лабораторной работы была успешно освоена автоматизация процесса сборки C++ проектов с помощью инструмента **CMake**. Были созданы статическая библиотека `print` и исполняемый файл `example1`, настроены пути заголовочных файлов и успешно выполнена сборка проекта с использованием компилятора **AppleClang** на платформе macOS. Итоговый проект опубликован в удаленном репозитории GitHub.
