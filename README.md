## Laboratory work II

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab02** и с лиценцией **MIT**
- [ ] 2. Сгенирировать токен для доступа к сервису **GitHub** с правами **repo**
- [ ] 3. Ознакомиться со ссылками учебного материала
- [ ] 4. Выполнить инструкцию учебного материала
- [ ] 5. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_EMAIL=<адрес_почтового_ящика>
$ export GITHUB_TOKEN=<сгенирированный_токен>
$ alias edit=<nano|vi|vim|subl>


```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ source scripts/activate
```

```ShellSession
$ mkdir ~/.config создали ~/.config
$ cat > ~/.config/hub <<EOF открыли и записали текст в ~/.config
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
$ git config --global hub.protocol https
```

```ShellSession
$ mkdir projects/lab02 && cd projects/lab02 создали и открыли projects/lab02
$ git init инициализация пустого репозитория
$ git config --global user.name ${GITHUB_USERNAME}
$ git config --global user.email ${GITHUB_EMAIL}
# check your git global settings
$ git config -e --global
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git добавление нового удалённого Git-репозитория
$ git pull origin master загрузка последней версии кода ветки master в локальный репозиторий
$ touch README.md добавление файла README.md
$ git status проверка статуса изменения файлов в git
$ git add README.md добавление README.md для отслеживания
$ git commit -m"added README.md" добавление коммита "added README.md"
$ git push origin master загрузка измениний на удалённый репозиторий в ветку master
```

Добавить на сервисе **GitHub** в репозитории **lab02** файл **.gitignore**
со следующем содержимом:

```ShellSession
*build*/
*install*/
*.swp
.idea/
```

```ShellSession
$ git pull origin master
$ git log просмотр истории коммитов в git
```

```ShellSession
$ mkdir sources
$ mkdir include
$ mkdir examples создание файлов sources, include, examples
$ cat > sources/print.cpp <<EOF создали и записали в sources/print.cpp код
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF
```

```ShellSession
$ cat > include/print.hpp <<EOF создали и записали в include/print.hpp код
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF
```

```ShellSession
$ cat > examples/example1.cpp <<EOF создали и записали в examples/example1.cpp код
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF
```

```ShellSession
$ cat > examples/example2.cpp <<EOF создали и записали в examples/example2.cpp код
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

```ShellSession
$ edit README.md редактирование README.md
```

```ShellSession
$ git status
$ git add . добавление всех файлов в git для отслеживания
$ git commit -m"added sources" добавление коммита "added sources"
$ git push origin master загрузка измениний на удалённый репозиторий в ветку master
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=02
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER}.git tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

### Part I

1. Создайте пустой репозиторий на сервисе github.com (или gitlab.com, или bitbucket.com).
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02H.git
2. Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдещем шаге.
$ git pull origin master
$ touch README.md
$ git add README.md
$ git commit -m"first commit"
$ git push origin master
3. Создайте файл `hello_world.cpp` в локальной копии репозитория (который должен был появиться на шаге 2). Реализуйте программу **Hello world** на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку `using namespace std;`.
$ mkdir sources
$ mkdir examples
$ cat > sources/hello_world.cpp <<EOF
#include <hello_world.hpp>
using namespace std

int main()
{
    cout << "hello world" << endl;
}
EOF
4. Добавьте этот файл в локальную копию репозитория.
$ git add sources/hello_world.cpp
5. Закоммитьте изменения с *осмысленным* сообщением.
$ git commit -m"create hello_world.cpp"
6. Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение `Hello world from @name`, где `@name` имя пользователя.
$ edit sources/hello_world.cpp
#include <hello_world.hpp>
#include <string>
using namespace std

int main()
{
    string name;
    cout << "enter your name" << endl;
    cin >> name;
    cout << endl << "hello world" << endl;
}
7. Закоммитьте новую версию программы. Почему не надо добавлять файл повторно `git add`?
Так как после изменений он сохранится автоматически
8. Запуште изменения в удалёный репозиторий.
$ git push origin master
9. Проверьте, что история коммитов доступна в удалёный репозитории.

### Part II

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. В локальной копии репозитория создайте локальную ветку `patch1`.
$ git checkout -b patch1
2. Внесите изменения в ветке `patch1` по исправлению кода и избавления от `using namespace std;`.
$ edit sources/hello_world.cpp <<EOF
#include <hello_world.hpp>
#include <string>

int main()
{
    std::string name;
    std::cout << "enter your name" << std::endl;
    std::cin >> name;
    std::cout << std::endl << "hello world" << std::endl;
}
EOF
3. **commit**, **push** локальную ветку в удалённый репозиторий.
$ git commit -m"change hello_world.cpp"
$ git push origin patch1
4. Проверьте, что ветка `patch1` доступна в удалёный репозитории.
5. Создайте pull-request `patch1 -> master`.
6. В локальной копии в ветке `patch1` добавьте в исходный код комментарии.
$ edit sources/hello_world.cpp <<EOF
#include <hello_world.hpp>
#include <string>

int main()
{
    std::string name; // объявлена переменная типа string
    std::cout << "enter your name" << std::endl; // вывод строки "enter your name"
    std::cin >> name; // ввод строки в переменную name
    std::cout << std::endl << "hello world" << std::endl; // вывод строки "hello world"
}
EOF
7. **commit**, **push**.
$ git commit -m"add comments"
$ git push origin patch1
8. Проверьте, что новые изменения есть в созданном на **шаге 5** pull-request
9. В удалённый репозитории выполните  слияние PR `patch1 -> master` и удалите ветку `patch1` в удаленном репозитории.
$ git checkout master
$ git merge patch1
10. Локально выполните **pull**.
$ git pull origin master
11. С помощью команды **git log** просмотрите историю в локальной версии ветки `master`.
12. Удалите локальную ветку `patch1`.
### Part III

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. Создайте новую локальную ветку `patch2`.
2. Измените *code style* с помощью утилиты [**clang-format**](http://clang.llvm.org/docs/ClangFormat.html). Например, используя опцию `-style=Mozilla`.
$ sudo npm install -g clang-format
$ clang-format -i -style=Mozilla sources/hello_world.cpp 
3. **commit**, **push**, создайте pull-request `patch2 -> master`.
$ git checkout -b patch2
$ git commit -m"Mozilla style"
$ git push origin patch2
4. В ветке **master** в удаленном репозитории измените комментарии, например, расставьте знаки препинания, переведите комментарии на другой язык.
5. Убедитесь, что в pull-request появились *конфликтны*.
6. Для этого локально выполните **pull** + **rebase** (точную последовательность команд, следует узнать самостоятельно). **Исправьте конфликты**.
$ git rebase master
$ git rebase --continue
7. Сделайте *force push* в ветку `patch2`
$ git push origin feature --force
8. Убедитель, что в pull-request пропали конфликтны. 
9. Вмержите pull-request `patch2 -> master`.

## Links

- [hub](https://hub.github.com/)
- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)

```
Copyright (c) 2015-2019 The ISC Authors
```
