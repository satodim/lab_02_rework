# lab_02

## Tutorial

### Проинициализируем нужные для работы переменные, перейдем в рабочее пространство и активируем ранее подготовленные скрипты.
```sh
export GITHUB_USERNAME=satodim
export GITHUB_EMAIL=<адрес_почтового_ящика>
export GITHUB_TOKEN=<сгенирированный_токен>
alias edit=nano

cd ${GITHUB_USERNAME}/workspace
source scripts/activate
```

### Теперь создадим папку `.config`, в ней файл конфигурации `hub` с необходимыми переменными:

```sh
mkdir ~/.config
cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
git config --global hub.protocol https
```
последней командой установим протокол `https` для работы с гитхабом.

Создаём папку для второй лабы и переходим туда
```sh
mkdir projects/lab_02_rework && cd projects/lab_02_rework
```
Создаем новый репозиторий
```sh
git init
```

Вывод предупреждает о том, что текущая ветка называется `master`:
```
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint: 
hint:  git config --global init.defaultBranch <name>
hint: 
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint: 
hint:  git branch -m <name>
Initialized empty Git repository in /home/vboxuser/satodim/workspace/workspace/projects/lab_02/.git/
```
На гитхабе главная ветка называется `main`, поэтому  переименуем локальную ветку `master` в `main`

```sh
git branch -m main
```

### Установим юзернейм и почту для гита:
```sh
git config --global user.name ${GITHUB_USERNAME}
git config --global user.email ${GITHUB_EMAIL}
```

И проверим, что все пошло по плану:
```sh
git config -e --global
```

```
[hub]
        protocol = https
[user]
        name = satodim
        email = ...@gmail.com
```
### Все установилось правильно, привяжем локальную директорию с созданным удаленно репозиторием:
```sh
git remote add origin https://github.com/${GITHUB_USERNAME}/lab_02.git
```

И загрузим все, что есть в этом репозитории, на наш компьютер
```sh
git pull origin main
```

Вывод предыдущей команды:
```sh
Username for 'https://github.com': satodim
Password for 'https://satodim@github.com': 
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 219 bytes | 109.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/satodim/lab_02_rework.git
 * [new branch]      main -> main

```
 
### Создадим `README.md`
```sh
touch README.md
```
 
### Теперь посмотрим на статусы файлов в папке
```sh
git status
```
 
```

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	README.md

nothing added to commit but untracked files present (use "git add" to track)

```

### Видно, что новосозданный README пока что никак не участвует в репозитории, поэтому мы можем его добавить и сделать новый коммит:
```sh
git add README.md
git commit -m "added README.md"
```

```
[main (root-commit) 47d8633] added README.md
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md

```

### Запушим локальный коммит на удаленный репозиторий
```sh
git push origin main
```

```
Username for 'https://github.com': satodim
Password for 'https://satodim@github.com': 
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 2 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 282 bytes | 282.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/satodim/lab_02.git
   9a76e6f..200709d  main -> main
```

На гитхабе появился README.

### Теперь удаленно создадим `.gitignore` и загрузим к нам.

На всякий случай убедимся, что все действительно загрузилось:
```sh
ls -a
```

```
.  ..  .git  .gitignore  README.md
```
Действительно, `.gitignore` загрузился

### Посмотрим на историю репозитория 
```sh
git log
```

```
commit 6d18420accea3fed9081f83583cfcd58ac0cfc1b (HEAD -> main, origin/main)
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 09:01:02 2026 +0300

    Create .gitignore

commit 47d86333afde59b4097550e9d33ac9f7c45996ca
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 05:55:46 2026 +0000

    added README.md

```
### Ну и теперь создадим несколько `cpp` файлов

```sh
mkdir sources
mkdir include
mkdir examples

cat > sources/print.cpp <<EOF
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

cat > include/print.hpp <<EOF
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF

cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF

cat > examples/example2.cpp <<EOF
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

### Коммитим и пушим их:
```sh
git add .
git commit -m"added sources"
git push origin master
```
# Homework

## Part I

### Создайте пустой репозиторий на сервисе github.com

Создали **этот** репозиторий

### Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдущем шаге.

```sh
git init
git branch -m main
git remote add origin https://github.com/satodim/lab_02_rework
git pull origin main
```

Вывод последней команды:

```
Username for 'https://github.com': satodim
Password for 'https://satodim@github.com': 
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 2 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (9/9), 929 bytes | 929.00 KiB/s, done.
Total 9 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/satodim/lab_02_rework.git
   6d18420..7fdb656  main -> main
```

### Создайте файл `hello_world.cpp` в локальной копии репозитория (который должен был появиться на шаге 2). Реализуйте программу Hello world на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку `using namespace std;`.

Этот файл:
```cpp
#include <iostream>
using namespace std;

int main() {
	cout << "Hello world!" << name << endl;
	return 0;
}
```

### Добавьте этот файл в локальную копию репозитория.
```sh
git add hello_world.cpp
```

### Закоммитьте изменения с осмысленным сообщением.
```sh
git commit -m "hello_world.cpp"
```

```
[main 83c293e] hello_world.cpp
 1 file changed, 7 insertions(+)
 create mode 100644 hello_world.cpp
```
### Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение Hello world from @name, где @name имя пользователя.

Новая версия программы
```cpp
#include <iostream>
using namespace std;

int main() {
	string name;
	cin >> name;
	cout << "Hello world from " << name << endl;
	return 0;
}
```

### Закоммитьте новую версию программы. Почему не надо добавлять файл повторно `git add`?

```sh
git commit -a -m "added hello_world.cpp"
```

`-a` означает коммит всех измененных файлов, чтобы этот файл смог закоммититься.

### Запуште изменения в удалёный репозиторий.

```sh
git push origin main
```

```
Username for 'https://github.com': satodim
Password for 'https://satodim@github.com': 
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 2 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 710 bytes | 710.00 KiB/s, done.
Total 6 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To https://github.com/satodim/lab_02_rework.git
   7fdb656..da64f08  main -> main

```

### Проверьте, что история коммитов доступна в удалённом репозитории.

```sh
git log
```

```
commit da64f0863055e8e698ab45eae44a9f93682e3b55 (HEAD -> main, origin/main)
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 06:10:11 2026 +0000

    added hello_world.cpp

commit 83c293ee46d965dedf8bfe8df46a39d96a5d9641
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 06:09:01 2026 +0000

    hello_world.cpp

commit 7fdb6566f1b29be58b3aa77c8cb027db3f24730a
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 06:04:33 2026 +0000

    added sources

commit 6d18420accea3fed9081f83583cfcd58ac0cfc1b
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 09:01:02 2026 +0300

    Create .gitignore

```
## Part II

### В локальной копии репозитория создайте локальную ветку `patch1`.

```sh
git checkout -b patch1
```

### Внесите изменения в ветке `patch1` по исправлению кода и избавления от `using namespace std;`.

Новый код:
```cpp
#include <iostream>

int main() {
	std::string name;
	std::cin >> name;
	std::cout << "Hello world from " << name << std::endl;
	return 0;
}
```

### **commit**, **push** локальную ветку в удалённый репозиторий.

``` sh
git commit -a -m "removed using namespace std"
```

```
[patch1 17308f9] removed using namespace std
 1 file changed, 3 insertions(+), 4 deletions(-)

```

```sh
git push origin patch1
```

```
Username for 'https://github.com': satodim
Password for 'https://satodim@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 2 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 375 bytes | 187.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: 
remote: Create a pull request for 'patch1' on GitHub by visiting:
remote:      https://github.com/satodim/lab_02_rework/pull/new/patch1
remote: 
To https://github.com/satodim/lab_02_rework.git
 * [new branch]      patch1 -> patch1

```

### Проверьте, что ветка `patch1` доступна в удалённом репозитории.

```sh
git log
```

```
commit 17308f944f55bb7adb4f15fc7885b183309b23e1 (HEAD -> patch1, origin/patch1)
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 06:13:21 2026 +0000

    removed using namespace std

commit da64f0863055e8e698ab45eae44a9f93682e3b55 (origin/main, main)
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 06:10:11 2026 +0000

    added hello_world.cpp

commit 83c293ee46d965dedf8bfe8df46a39d96a5d9641
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 06:09:01 2026 +0000

    hello_world.cpp

commit 7fdb6566f1b29be58b3aa77c8cb027db3f24730a
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 06:04:33 2026 +0000

    added sources
```

### Я сделал pull-request удаленно на гитхабе. В этом убедимся с помощью утилиты `gh`.
```sh
gh pr view --json number,title,author
```
вывод:
```
{
  "author": {
    "id": "U_kgDOCjA-pg",
    "is_bot": false,
    "login": "satodim",
    "name": "satodim"
  },
  "number": 1,
  "title": "removed using namespace std"
}
```

### В локальной копии в ветке `patch1` добавьте в исходный код комментарии.

```cpp
// Подключаем библиотеку ввода-вывода
#include <iostream>

int main() {
	// Создаем строку для имени
	std::string name;
	// Принимаем имя
	std::cin >> name;
	// Печатаем
	std::cout << "Hello world from " << name << std::endl;
	return 0;
}
```

### commit, push

```sh
git commit -a -m "added comments"
```

```
it -a -m "added comments"
[patch1 ec63d76] added comments
 1 file changed, 6 insertions(+)

```

```sh
git push origin patch1
```

```
Username for 'https://github.com': satodim
Password for 'https://satodim@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 2 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 491 bytes | 491.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/satodim/lab_02_rework.git
   17308f9..ec63d76  patch1 -> patch1
```

### Проверьте, что новые изменения есть в созданном на шаге 5 pull-request

```sh
gh pr view --json number,title,author,commits
```

```
{
  "author": {
    "id": "U_kgDOCjA-pg",
    "is_bot": false,
    "login": "satodim",
    "name": "satodim"
  },
  "commits": [
    {
      "authoredDate": "2026-04-01T06:13:21Z",
      "authors": [
        {
          "email": "vovkakursk8@gmail.com",
          "id": "U_kgDOCjA-pg",
          "login": "satodim",
          "name": "satodim"
        }
      ],
      "committedDate": "2026-04-01T06:13:21Z",
      "messageBody": "",
      "messageHeadline": "removed using namespace std",
      "oid": "17308f944f55bb7adb4f15fc7885b183309b23e1"
    },
    {
      "authoredDate": "2026-04-01T06:16:53Z",
      "authors": [
        {
          "email": "vovkakursk8@gmail.com",
          "id": "U_kgDOCjA-pg",
          "login": "satodim",
          "name": "satodim"
        }
      ],
      "committedDate": "2026-04-01T06:16:53Z",
      "messageBody": "",
      "messageHeadline": "added comments",
      "oid": "ec63d76a70b41ab449700fecb50c6f5099986f96"
    }
  ],
  "number": 1,
  "title": "removed using namespace std"
}
```

### В удалённый репозитории выполните слияние PR `patch1 -> master` и удалите ветку `patch1` в удаленном репозитории.

Сделал это на гитхабе

### Локально выполните **pull**.

```sh
git checkout main
git pull origin main
```

```
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (1/1), 896 bytes | 896.00 KiB/s, done.
From https://github.com/satodim/lab_02_rework
 * branch            main       -> FETCH_HEAD
   da64f08..58b9617  main       -> origin/main
Updating da64f08..58b9617
Fast-forward
 hello_world.cpp | 13 +++++++++----
 1 file changed, 9 insertions(+), 4 deletions(-)
```

### С помощью команды `git log` просмотрите историю в локальной версии ветки **main**.

```
commit 58b96171292f6386d087a290c1ed15d3961b5326 (HEAD -> main, origin/main)
Merge: da64f08 ec63d76
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 09:17:50 2026 +0300

    Merge pull request #1 from satodim/patch1
    
    removed using namespace std

commit ec63d76a70b41ab449700fecb50c6f5099986f96 (origin/patch1, patch1)
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 06:16:53 2026 +0000

    added comments

commit 17308f944f55bb7adb4f15fc7885b183309b23e1
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 06:13:21 2026 +0000

    removed using namespace std

commit da64f0863055e8e698ab45eae44a9f93682e3b55
Author: satodim <vovkakursk8@gmail.com>
```

### Удалите локальную ветку `patch1`.

```sh
git branch -d patch1
```

```
Deleted branch patch1 (was ec63d76).
```
## Part III

### Создайте новую локальную ветку `patch2`.

```sh
git checkout -b patch2
```

### Измените code style с помощью утилиты `clang-format`. Например, используя опцию `-style=Mozilla`.

```sh
clang-format -style=Mozilla -i hello_world.cpp 
```

код:
```cpp
// Подключаем библиотеку ввода-вывода
#include <iostream>

int
main()
{
  // Создаем строку для имени
  std::string name;
  // Принимаем имя
  std::cin >> name;
  // Печатаем
  std::cout << "Hello world from " << name << std::endl;
  return 0;
}
```

### commit, push, создайте pull-request `patch2 -> master`

```sh
git commit -a -m "changed code style"
```

```
[patch2 369623b] changed code style
 1 file changed, 10 insertions(+), 10 deletions(-)
```

```sh
git push origin patch2
```

```
Username for 'https://github.com': satodim
Password for 'https://satodim@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 2 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 396 bytes | 198.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
remote: 
remote: Create a pull request for 'patch2' on GitHub by visiting:
remote:      https://github.com/satodim/lab_02_rework/pull/new/patch2
remote: 
To https://github.com/satodim/lab_02_rework.git
 * [new branch]      patch2 -> patch2
```

Также сделаем pull-request и, как и в прошлый раз, проверим его создание с помощью утилиты `gh`:

```sh
gh pr view --json number,title,author,commits
```

```
{
  "author": {
    "id": "U_kgDOCjA-pg",
    "is_bot": false,
    "login": "satodim",
    "name": "satodim"
  },
  "commits": [
    {
      "authoredDate": "2026-04-01T06:20:37Z",
      "authors": [
        {
          "email": "vovkakursk8@gmail.com",
          "id": "U_kgDOCjA-pg",
          "login": "satodim",
          "name": "satodim"
        }
      ],
      "committedDate": "2026-04-01T06:20:37Z",
      "messageBody": "",
      "messageHeadline": "changed code style",
      "oid": "369623bb9d5a230c748a91990d1cabed385b5f82"
    }
  ],
  "number": 2,
  "title": "changed code style"
}
```

### В ветке `main` в удаленном репозитории измените комментарии, например, расставьте знаки препинания, переведите комментарии на другой язык.

Переключимся на векту `main`

```sh
git checkout main
```

И переведем комментарии на английский:
```cpp
// including standard IO library
#include <iostream>

int main() {
	// creating string for name
	std::string name;
	// recieving name
	std::cin >> name;
	// printing "hello world from name"
	std::cout << "Hello world from " << name << std::endl;
	return 0;
}
```

Ну и по классике:

```sh
git commit -a -m "translated comments"
```

```
[main a1836b2] translated comments
 1 file changed, 4 insertions(+), 6 deletions(-)
```

```sh
git push origin main
```

```
sUsername for 'https://github.com': satodim
Password for 'https://ssatodim@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 2 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 423 bytes | 423.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/satodim/lab_02_rework.git
   58b9617..a1836b2  main -> main

```

### Теперь переключимся на ветку `patch2`
```sh
git checkout patch2
```

И проверим наш pull-request, в этот раз посмотрим в том числе и на параметр `mergeable`
```sh
gh pr view --json number,title,author,commits,mergeable
```

```
{
  "author": {
    "id": "U_kgDOCjA-pg",
    "is_bot": false,
    "login": "satodim",
    "name": "satodim"
  },
  "commits": [
    {
      "authoredDate": "2026-04-01T06:20:37Z",
      "authors": [
        {
          "email": "vovkakursk8@gmail.com",
          "id": "U_kgDOCjA-pg",
          "login": "satodim",
          "name": "satodim"
        }
      ],
      "committedDate": "2026-04-01T06:20:37Z",
      "messageBody": "",
      "messageHeadline": "changed code style",
      "oid": "369623bb9d5a230c748a91990d1cabed385b5f82"
    }
  ],
  "mergeable": "CONFLICTING",
  "number": 2,
  "title": "changed code style"
}
```

И как можно заметить, этот параметр равен `CONFLICTING`, то есть действительно присутствуют конфликты.

### Для этого локально выполните `pull` + `rebase` (точную последовательность команд, следует узнать самостоятельно). Исправьте конфликты.

Для начала на всякий случай подтянем ветку `main`, чтобы она была синхронизированна с удалённым репозиторием.

```sh
git checkout main
git pull origin main
```

У меня оно и так уже было up-to-date, поэтому я получил следующий вывод:

```
From https://github.com/satodim/lab_02_rework
 * branch            main       -> FETCH_HEAD
Already up to date.
```

Дальше переключаемся на другую ветку и пытаемся ее перебазировать:

```sh
git checkout patch2
git rebase main
```

На что гит выведет следующее сообщение:
```
Auto-merging hello_world.cpp
CONFLICT (content): Merge conflict in hello_world.cpp
error: could not apply 369623b... changed code style
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 369623b... changed code style

```

А `hello_world.cpp` стал вот таким:

```cpp
// including standard IO library
#include <iostream>

<<<<<<< HEAD
int main() {
	// creating string for name
	std::string name;
	// recieving name
	std::cin >> name;
	// printing "hello world from name"
	std::cout << "Hello world from " << name << std::endl;
	return 0;
=======
int
main()
{
  // Создаем строку для имени
  std::string name;
  // Принимаем имя
  std::cin >> name;
  // Печатаем
  std::cout << "Hello world from " << name << std::endl;
  return 0;
>>>>>>> 0c61fbe (changed code style)
}
```

Исправим конфликт, совместив английские комментарии с другим стилем кода:
```sh
// including standard IO library
#include <iostream>

int
main()
{
  // creating string for name
  std::string name;
  // recieving name
  std::cin >> name;
  // printing "hello world from name"
  std::cout << "Hello world from " << name << std::endl;
  return 0;
}
```

Теперь пометим этот файл как переделанный с помощью команды 
```sh
git add hello_world.cpp
```

И продолжим перебазирование:
```sh
git rebase --continue
```

Других конфликтов не возникло. Гит предложил ввести название этого перебазирования и потом вывел это:
```
[detached HEAD a345626] 	changed code style
 1 file changed, 10 insertions(+), 8 deletions(-)
Successfully rebased and updated refs/heads/patch2.
```

### Сделайте force push в ветку `patch2`

```sh
git push --force origin patch2
```

```
Username for 'https://github.com': satodim
Password for 'https://satodim@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 2 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 431 bytes | 431.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/satodim/lab_02_rework.git
 + 369623b...a345626 patch2 -> patch2 (forced update)

```

### Убедитесь, что в pull-request пропали конфликтны.

Делаем это уже привычным для нас образом

```sh
gh pr view --json number,title,author,commits,mergeable
```

```
{
  "author": {
    "id": "U_kgDOCjA-pg",
    "is_bot": false,
    "login": "satodim",
    "name": "satodim"
  },
  "commits": [
    {
      "authoredDate": "2026-04-01T06:20:37Z",
      "authors": [
        {
          "email": "vovkakursk8@gmail.com",
          "id": "U_kgDOCjA-pg",
          "login": "satodim",
          "name": "satodim"
        }
      ],
      "committedDate": "2026-04-01T06:26:07Z",
      "messageBody": "",
      "messageHeadline": "\tchanged code style",
      "oid": "a34562688e3090be863424752eaa384e55d35066"
    }
  ],
  "mergeable": "MERGEABLE",
  "number": 2,
  "title": "changed code style"
}
```

Теперь переменная `mergeable` равна `MERGEABLE`.

### Вмержите pull-request `patch2` -> `master`.

Сделал это на гитхабе.

Теперь можно посмотреть последние 2 коммита и убедиться, что слияние прошло успешно:
```sh
git checkout main
git log -2
```

```
commit a1836b223775d299535dff15546307f454838efe (HEAD -> main, origin/main)
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 06:23:20 2026 +0000

    translated comments

commit 58b96171292f6386d087a290c1ed15d3961b5326
Merge: da64f08 ec63d76
Author: satodim <vovkakursk8@gmail.com>
Date:   Wed Apr 1 09:17:50 2026 +0300

    Merge pull request #1 from satodim/patch1
    
    removed using namespace std

