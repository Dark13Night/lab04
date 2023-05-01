# report lab04

## Задание
Вы продолжаете проходить стажировку в "Formatter Inc." (см подробности).

В прошлый раз ваше задание заключалось в настройке автоматизированной системы CMake.

Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в прошлый раз. Настройте сборочные процедуры на различных платформах:

используйте TravisCI для сборки на операционной системе Linux с использованием компиляторов gcc и clang;
используйте AppVeyor для сборки на операционной системе Windows.

для обоих случаев используем .github/actions

клонируем репозиторий предыдущей лабораторной

```
$ git clone https://github.com/Dark13Night/lab03 lab04
$ cd ~/lab04
$ git remote remove origin
$ git remote add origin https://github.com/Dark13Night/lab04.git
```

создаём директорию workflows 

```
$ mkdir .github
$ cd ~/lab04/.github
$ mkdir workflows
$ cd ~/lab04/.github/workflows
```


![VirtualBox_kali-linux-2022 4-virtualbox-amd64_18_04_2023_19_59_33](https://user-images.githubusercontent.com/112771541/235414081-b4cd7db3-ea46-428d-a3b8-691039e62a95.png)

Создаём файл Linux.yml

```
name: CMake

on:
 push:
  branches: [main]
 pull_request:
  branches: [main]

jobs: 
 build_Linux:

  runs-on: ubuntu-latest

  steps:
  - uses: actions/checkout@v3

  - name: Configure Solver
    run: cmake ${{github.workspace}}/solver_application/ -B ${{github.workspace}}/solver_application/build

  - name: Build Solver
    run: cmake --build ${{github.workspace}}/solver_application/build

  - name: Configure HelloWorld
    run: cmake -S ${{github.workspace}}/hello_world_application/ -B ${{github.workspace}}/hello_world_application/build

  - name: Build HelloWorld
    run: cmake --build ${{github.workspace}}/hello_world_application/build
```

'''
$ git add Linux.yml
$ git commit -m "Linux.yml - 0"
$ git push origin main
'''

(В процессе пришлось делать несколько попыток поэтому итоговый commit отличается от коммита на скрине)

![VirtualBox_kali-linux-2022 4-virtualbox-amd64_18_04_2023_20_08_47](https://user-images.githubusercontent.com/112771541/235414453-0c1fd781-08fc-4c90-84b8-76397fd07f9c.png)

Создаём файл Windows.yml

```
name: CMake

on:
 push:
  branches: [main]
 pull_request:
  branches: [main]

jobs: 
 build_Windows:
  runs-on: windows-latest
  steps:
  - uses: actions/checkout@v3

  - name: Configure Solver
    run: cmake ${{github.workspace}}/solver_application/ -B ${{github.workspace}}/solver_application/build

  - name: Build Solver
    run: cmake --build ${{github.workspace}}/solver_application/build
    
  - name: Configure HelloWorld
    run: cmake -S ${{github.workspace}}/hello_world_application/ -B ${{github.workspace}}/hello_world_application/build

  - name: Build HelloWorld
    run: cmake --build ${{github.workspace}}/hello_world_application/build
```

![VirtualBox_kali-linux-2022 4-virtualbox-amd64_18_04_2023_20_10_17](https://user-images.githubusercontent.com/112771541/235414727-03e98d6e-bf29-4f49-bde8-3bd4a21e7144.png)

```
$ git add Windows.yml
$ git commit -m "changes"
$ git push origin main
```

(В процессе пришлось делать несколько попыток поэтому итоговый commit отличается от коммита на скрине)

![VirtualBox_kali-linux-2022 4-virtualbox-amd64_18_04_2023_20_10_17](https://user-images.githubusercontent.com/112771541/235414815-9ca813a5-31f7-4f5a-8fcc-dc72ad4cf322.png)

Проверяем .github/actions(все неудачные попытки запуска удалены)

![VirtualBox_kali-linux-2022 4-virtualbox-amd64_01_05_2023_08_53_44](https://user-images.githubusercontent.com/112771541/235414970-cf746e31-808f-4108-b3c6-58f0daf28ca1.png)
