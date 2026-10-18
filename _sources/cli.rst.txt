.. _cli:

================================
Интерфейс командной строки (CLI)
================================

Ren'Py включает в себя интерфейс командной строки (CLI), который можно использовать
для автоматизации задач разработки, настройки систем непрерывной интеграции (CI)
и скриптов публикации релизов. Для большинства повседневных задач использовать CLI
не обязательно — все эти действия доступны через графический интерфейс лаунчера Ren'Py.

Примеры на этой странице предполагают, что команды выполняются из корневого каталога SDK
Ren'Py (содержащего файлы :file:`renpy.py`, :file:`renpy.sh` и :file:`renpy.exe`).
Приведены примеры как для Linux/macOS, так и для Windows.

CLI не является строго фиксированным API и может эволюционировать между версиями Ren'Py.
Данная страница актуальна для Ren'Py 8.3+.


Быстрые задачи
==============

Используйте эти готовые сценарии как шаблоны для копирования. Замените пути и коды языков на свои.

Запуск проекта
--------------

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            cd /path/to/renpy
            ./renpy.sh /path/to/project run

    .. tab:: Windows

        .. code-block:: bat

            cd C:\path\to\renpy
            .\lib\py3-windows-x86_64\python.exe renpy.py C:\path\to\project run


Сборка дистрибутивов
--------------------

Сборка настольных дистрибутивов игры (Windows, macOS, Linux) через утилиту лаунчера.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            cd /path/to/renpy
            ./renpy.sh launcher distribute /path/to/project --destination /path/to/output

    .. tab:: Windows

        .. code-block:: bat

            cd C:\path\to\renpy
            .\lib\py3-windows-x86_64\python.exe renpy.py launcher distribute C:\path\to\project --destination C:\path\to\output


Создание и обновление переводов
-------------------------------

Создание или обновление файлов перевода с последующим экспортом диалогов и строк для переводчиков.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            cd /path/to/renpy
            ./renpy.sh /path/to/project translate french
            ./renpy.sh /path/to/project dialogue french --strings

    .. tab:: Windows

        .. code-block:: bat

            cd C:\path\to\renpy
            .\lib\py3-windows-x86_64\python.exe renpy.py C:\path\to\project translate french
            .\lib\py3-windows-x86_64\python.exe renpy.py C:\path\to\project dialogue french --strings


Синтаксис
=========

.. note::

    Обозначения, принятые на этой странице:

    * ``<name>`` — значение, которое вы подставляете сами (путь к проекту, код языка и т. д.).
    * ``[item]`` — необязательный параметр, указывать который не обязательно.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh [global options...] [<basedir>] [<command>] [command options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py [global options...] [<basedir>] [command] [command options...]

.. describe:: <basedir>

    :default: Каталог с исполняемым файлом Ren'Py (лаунчер)

    В описании команд ниже <basedir> обозначает путь к базовому каталогу проекта (где расположена папка :file:`game/`).

.. describe:: <command>

    :default: ``run``

    Задаёт выполняемую команду.


Глобальные параметры
--------------------

Следующие флаги могут использоваться с любой командой:

.. option:: --savedir <directory>

    Задаёт каталог, в котором размещаются файлы сохранений и постоянные данные (:doc:`persistent`).

.. option:: --trace <level>

    Задаёт уровень детализации трассировки, записываемой в файл :file:`trace.txt` (1 = для каждого вызова, 2 = для каждой строки).

.. option:: --version

    Выводит текущую версию используемого Ren'Py и завершает работу.

.. option:: --compile

    Принудительно перекомпилирует все скрипты :file:`.rpy` перед выполнением команды.

.. option:: --compile-python

    Принудительно перекомпилирует весь код Python вместо чтения из байткода :file:`game/cache/bytecode-*.rpyb`.

.. option:: --keep-orphan-rpyc

    При компиляции предотвращает удаление файлов :file:`.rpyc`, для которых не найдено исходных файлов :file:`.rpy` или :file:`_ren.py` с тем же именем.

.. option:: --errors-in-editor

    Открывает сообщения об ошибках непосредственно в настроенном текстовом редакторе.

.. option:: --safe-mode

    Принудительно запускает Ren'Py в безопасном режиме (safe mode), позволяя настроить параметры графики.

.. option:: --help, -h

    Выводит справочное сообщение со списком команд и синтаксисом, после чего завершает работу. Возвращает наиболее актуальную информацию о доступных опциях.

Ren'Py умеет выгружать метаданные об игре в формат JSON.
Следующие параметры позволяют выбрать целевой файл и содержимое дампа:

.. option:: --json-dump <file>

    Задаёт имя и путь к JSON-файлу дампа.

.. option:: --json-dump-private

    Включает в дамп закрытые имена (начинающиеся с символа подчёркивания ``_``).

.. option:: --json-dump-common

    Включает в дамп имена, определённые в системном каталоге :file:`common/`.

.. note::

    Команды и флаги CLI могут обновляться в новых версиях. Чтобы увидеть актуальный список параметров, выполните:

    .. tabs::

        .. tab:: Linux / macOS

            .. code-block:: bash

                ./renpy.sh --help

        .. tab:: Windows

            .. code-block:: bash

                .\lib\py3-windows-x86_64\python.exe renpy.py --help

    Чтобы просмотреть актуальные параметры конкретной команды:

    .. tabs::

        .. tab:: Linux / macOS

            .. code-block:: bash

                ./renpy.sh <basedir> <command> --help

        .. tab:: Windows

            .. code-block:: bash

                .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> <command> --help


Основные команды рабочего процесса
==================================

.. _cli-run:

Запуск проекта (Run)
--------------------

Запускает проект в обычном режиме. Эта команда выполняется по умолчанию, если имя команды не указано.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> run [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> run [options...]

.. option:: --profile-display

    Выводит отчёт о времени, затрачиваемом на отрисовку кадров на экране.
    Эквивалентно установке :var:`config.profile` в True.

.. option:: --debug-image-cache

    Записывает диагностическую информацию о :ref:`кэше изображений <images>` в файл :file:`image_cache.txt`.
    Эквивалентно установке :var:`config.debug_image_cache` в True.

.. option:: --warp <filename:linenumber>

    Пытается переместить выполнение непосредственно к указанной строке сценария. Принимает аргумент вида ``имя_файла:номер_строки``.
    Для работы :ref:`варпинга к строке <warping_to_a_line>` требуется, чтобы переменная :var:`config.developer` была установлена в True.


.. _cli-quit:

Выход (Quit)
------------

Немедленно завершает работу игры (полезно в связке с компиляцией или формированием дампов).

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> quit

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> quit


.. _cli-compile:

Компиляция (Compile)
--------------------

Компилирует скрипты игры, создавая файлы :file:`.rpyc` из исходников :file:`.rpy`.
Эквивалентно нажатию кнопки «Принудительная перекомпиляция» (Force Recompile) в лаунчере Ren'Py.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> compile [--keep-orphan-rpyc]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> compile [--keep-orphan-rpyc]

.. option:: --keep-orphan-rpyc

    Предотвращает удаление файлов :file:`.rpyc`, для которых больше нет соответствующих исходных файлов :file:`.rpy` или :file:`_ren.py`.


.. _cli-director:

Интерактивный режиссёр (Director)
---------------------------------

Запускает :doc:`интерактивный режиссёр <director>` и затем начинает игру.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> director

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> director


.. _cli-rmpersistent:

Удаление постоянных данных (Remove Persistent Data)
---------------------------------------------------

Удаляет сохранённые :doc:`постоянные данные <persistent>` игры. Это полезно, поскольку данные persistent сохраняются в нескольких местах:

- в каталоге сохранений игры (:file:`game/saves/`)
- в системном каталоге, определяемом переменной :var:`config.save_directory`

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> rmpersistent

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> rmpersistent

.. warning::

    Эта операция полностью сбрасывает весь игровой прогресс, включая открытые концовки, флаги прочитанного текста и настройки пользователя.


.. _cli-update:

Обновление проекта (Update Project)
-----------------------------------

Скачивает и устанавливает обновления игры Ren'Py с веб-сервера.
Подробнее см. раздел :doc:`updater`.

Параметры идентичны аргументам метода :meth:`updater.update`.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> update <url> [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> update <url> [options...]

.. option:: <url>

    Задаёт URL к файлу :file:`updates.json` на веб-сервере.

.. option:: --base <directory>

    Задаёт базовый каталог обновляемой игры. По умолчанию — текущая игра.

.. option:: --force

    Принудительно выполняет обновление, даже если номера версий совпадают.

.. option:: --key <key>

    Файл с открытым ключом для криптографической проверки обновления.

.. option:: --simulate <option>

    Режим симуляции для тестирования интерфейса обновления без реального скачивания файлов. Одно из значений: ``available``, ``not_available`` или ``error``.

.. warning::

    Команда изменяет файлы вашей игры. Используйте с осторожностью.


Проверка и тестирование
=======================

.. _cli-lint:

Проверка скрипта (Lint)
-----------------------

Запускает проверку игры :ref:`линтером <lint>`. Скрипт проверяется на наличие синтаксических и логических ошибок, после чего выводится статистика по объёму текста. Эквивалентно кнопке «Проверка скрипта (Lint)» в лаунчере.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> lint [<filename>] [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> lint [<filename>] [options...]

.. option:: <filename>

    Если указано имя файла, отчёт линтера будет сохранён в этот файл вместо вывода в стандартный поток вывода (консоль).

.. option:: --error-code

    Завершает процесс с кодом ошибки 1 при обнаружении любых ошибок lint. Крайне полезно в скриптах CI для фиксации сбоя сборки.

.. option:: --no-orphan-tl

    Не выводит предупреждения об потерянных переводах (orphan translations). Потерянными переводами считаются :ref:`операторы перевода <translation_statement>`, не ссылающиеся ни на одну строку в основном языке.

.. option:: --reserved-parameters

    Проверяет параметры операторов Ren'Py на использование зарезервированных ключевых слов Python и Ren'Py.
    В частности, проверяются операторы :doc:`label <label>`, :doc:`screen <screens>` и :ref:`ATL <atl>`.

.. option:: --by-character

    Выводит детальную статистику по количеству блоков, слов и символов текста для каждого персонажа отдельно.

.. option:: --check-unclosed-tags

    Проверяет наличие незакрытых текстовых тегов в строках диалога.

.. option:: --all-problems

    Выводит полный список всех проблем каждого вида, а не только первые десять записей.


.. _cli-test:

Запуск автотестов (Run Testcases)
---------------------------------

Запускает :doc:`автоматизированные тесты <testcases>` для игры.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> test [<filters>] [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> test [<filters>] [options...]

Подробнее см. раздел :ref:`Автоматизированное тестирование <test-command-line>`.


Сборка и распространение
========================

.. note::

    В процессе сборки Ren'Py создаёт файлы :file:`.rpyc`, содержащие метаданные для загрузки игры. Системы непрерывной интеграции (CI) должны сохранять эти файлы :file:`.rpyc` после сборки и передавать их в следующую сборку (напрямую или помещая в каталог :file:`old-game/`). Игнорирование этого правила может привести к тому, что новая сборка не сможет загрузить сохранения игроков из предыдущих версий.


.. _cli-add-from:

Добавление меток from к вызовам call (Add From To Calls)
--------------------------------------------------------

Добавляет пункт ``from`` к каждому :ref:`оператору вызова call <call-statement>`, у которого он отсутствует. Как правило, эту процедуру рекомендуется выполнять перед публикацией релиза, чтобы Ren'Py мог однозначно находить точку возврата вызовов при последующих обновлениях игры.

Подразумевает параметр :option:`--compile`.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> add_from

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> add_from

.. warning::

    Команда перезаписывает и модифицирует файлы сценария вашей игры.


.. _cli-distribute:

Сборка пакетов (Distribute)
---------------------------

:doc:`Выполняет сборку дистрибутивов <build>` игры для платформ Windows, macOS и Linux.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh launcher distribute <basedir> [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py launcher distribute <basedir> [options...]

.. option:: --destination <directory>

    Задаёт каталог для сохранения готовых архивов дистрибутива. По умолчанию архивы сохраняются в каталог вида «`name`-`version`-dists» внутри текущей папки (имена формируются на основе :var:`build.name` и :var:`build.version`).

.. option:: --format <format>

    Принудительно задаёт формат упаковки дистрибутивов.

.. option:: --macapp <app>

    Задаёт путь к шаблону macapp, используемому для подписи пакетов macOS, вместо встроенного в Ren'Py.

.. option:: --no-archive

    Отключает упаковку файлов в :ref:`архивы RPA <archives>`.

.. option:: --no-update

    Отключает генерацию файлов обновления, необходимых для работы модуля :doc:`updater`.

.. option:: --package <package>

    Задаёт имя собираемого пакета (например, "pc", "mac" или "market"). Опцию можно указывать многократно для сборки нескольких конкретных пакетов. По умолчанию собираются все объявленные пакеты.

.. option:: --packagedest <package>

    Задаёт выходное имя для пакета (без расширения). Требует явного указания ровно одного параметра :option:`--package`.


.. _cli-android-build:

Сборка для Android (Android Build)
----------------------------------

Собирает релиз игры для :doc:`android`. Предполагается, что через лаунчер был предварительно установлен Android SDK, сгенерированы ключи подписи и настроен проект.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh launcher android_build <basedir> [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py launcher android_build <basedir> [options...]

.. option:: --destination <directory>

    Задаёт выходной каталог для сборки. По умолчанию используется папка «`name`-`version`-dists» в текущем каталоге на основе :var:`build.name` и :var:`build.version`.

.. option:: --bundle

    Формирует пакет формата :file:`.aab` (Android App Bundle). Если опция не указана, собирается стандартный файл :file:`.apk`.

.. option:: --install

    Автоматически устанавливает собранный файл :file:`.apk` или :file:`.aab` на подключённое Android-устройство.

.. option:: --launch

    Запускает игру на подключённом устройстве после сборки (подразумевает параметр :option:`--install`).

.. option:: --package <package>

    Задаёт имя собираемого пакета. По умолчанию собирается пакет 'android'.


.. _cli-ios-create:

Создание проекта iOS (iOS Create)
---------------------------------

Создаёт проект Xcode для последующей сборки игры под платформу :doc:`ios`. Предполагается, что поддержка iOS была предварительно установлена через лаунчер.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh launcher ios_create <basedir> <destination>

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py launcher ios_create <basedir> <destination>

.. describe:: <basedir>

    Путь к проекту игры Ren'Py.

.. describe:: <destination>

    Путь к создаваемому проекту iOS (папка проекта Xcode).


.. _cli-ios-populate:

Обновление проекта iOS (iOS Populate)
-------------------------------------

Копирует файлы игры в проект Xcode, созданный с помощью команды :ref:`ios_create <cli-ios-create>`. Используется для обновления данных игры в существующем проекте iOS, собранном на той же версии Ren'Py.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh launcher ios_populate <basedir> <destination>

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py launcher ios_populate <basedir> <destination>

.. describe:: <basedir>

    Путь к проекту игры Ren'Py.

.. describe:: <destination>

    Путь к обновляемому проекту Xcode для iOS.


.. _cli-web-build:

Сборка для Web (Web Build)
--------------------------

Выполняет веб-сборку игры (HTML5/WebAssembly). Предполагается, что поддержка веб-платформы была установлена через лаунчер, а необходимые конфигурационные файлы (например, :file:`progressive_download.txt`) уже настроены в проекте.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh launcher web_build <basedir> [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py launcher web_build <basedir> [options...]

.. option:: --destination <directory>

    Каталог, в который будут сохранены упакованные веб-файлы.

.. option:: --launch

    Запускает локальный веб-сервер и открывает игру в браузере по завершении сборки.


Перевод и локализация
=====================

.. seealso::

    * :doc:`translation`


.. _cli-translate:

Генерация переводов (Translate)
-------------------------------

Создаёт или обновляет файлы перевода в каталоге :file:`tl/<language>/` проекта. Записывает недостающие строки диалогов и текстовые переменные, оставляя уже переведённые фрагменты без изменений.
Используйте флаг :option:`--count` для подсчёта непереведённых строк без изменения файлов на диске.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> translate <language> [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> translate <language> [options...]

.. option:: <language>

    Код языка, для которого генерируются файлы перевода.

.. option:: --count

    Выводит количество недостающих строк перевода без записи файлов на диск.

.. option:: --rot13

    Применяет алгоритм rot13 к генерируемым строкам перевода (для псевдолокализации).

.. option:: --piglatin

    Преобразует строки по правилам поросячьей латыни (pig latin). Переопределяется флагом :option:`--rot13`.

.. option:: --empty

    Создаёт пустые строки для перевода. Переопределяется флагами :option:`--rot13` и :option:`--piglatin`.

.. option:: --min-priority <int>

    Генерирует перевод только для строк с приоритетом выше указанного значения.

.. option:: --max-priority <int>

    :default: 499, если :var:`config.translate_launcher` равно True, иначе 299.

    Генерирует перевод для строк с приоритетом ниже указанного значения.

.. option:: --strings-only

    Генерирует перевод только для текстовых строк (strings), пропуская диалоги.

.. option:: --common-only

    Генерирует перевод только для строк из системного каталога :file:`common/`.

.. option:: --no-todo

    Не добавляет комментарии с меткой TODO в начало создаваемых файлов перевода.

.. option:: --string <string>

    Генерирует перевод только для конкретной указанной строки. Опцию можно использовать многократно.


.. _cli-dialogue:

Экспорт диалогов (Dialogue)
---------------------------

Экспортирует диалоги (а также опционально переводимые строки интерфейса) в файл :file:`dialogue.tab` (по умолчанию с разделителями-табуляциями) или в текстовый файл :file:`dialogue.txt`, если указан флаг :option:`--text`.

Если указан аргумент языка, выводится переведённый текст (при его наличии), иначе экспортируется исходный оригинальный текст.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> dialogue <language> [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> dialogue <language> [options...]

.. note::

    Это команда экспорта и отчётности. Она не создаёт рабочие файлы сценариев перевода в :file:`tl/<language>/`.

.. describe:: <language>

    Язык, для которого извлекается диалоговый текст.

.. option:: --text

    Выводит диалоги в виде простого форматированного текста. Если не указано, формируется файл с разделителями-табуляциями (.tab).

.. option:: --strings

    Включает в экспорт все переводимые строки интерфейса, а не только фразы персонажей.
    Большинство таких строк объявляются через функции :func:`_` и :func:`__`, а также в :doc:`меню выборов <menus>`.

.. option:: --notags

    Удаляет все текстовые теги форматирования из реплик.

.. option:: --escape

    Экранирует кавычки и специальные символы.


.. _cli-extract-strings:

Экспорт строк (Extract Strings)
-------------------------------

Экспортирует существующие переводы из игры в файл JSON.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> extract_strings <language> <destination> [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> extract_strings <language> <destination> [options...]

.. option:: <language>

    Язык, из которого извлекаются переведённые строки.

.. option:: <destination>

    Путь к JSON-файлу, в который будут сохранены переведённые строки.

.. option:: --merge

    Объединяет извлекаемые данные с текущим содержимым JSON-файла, сохраняя существующие в нём переводы.

.. option:: --force

    Не возбуждает исключение, если указанный язык отсутствует в проекте.


.. _cli-merge-strings:

Импорт строк (Merge Strings)
----------------------------

Импортирует переведённые строки из JSON-файла обратно в проект игры.

Подразумевает параметр :option:`--compile`.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh <basedir> merge_strings <language> <source> [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py <basedir> merge_strings <language> <source> [options...]

.. option:: <language>

    Язык, в который внедряются переведённые строки.

.. option:: <source>

    JSON-файл, из которого загружаются переводы.

.. option:: --reverse

    Инвертирует направление языков в файле JSON.

.. option:: --replace

    Заменяет нетривиальные переводы. Тривиальным считается перевод, который отсутствует или совпадает со строкой на языке оригинала.


Команды лаунчера
================

Эти команды управляют функциями лаунчера Ren'Py непосредственно из консоли.


.. _cli-generate-gui:

Генерация GUI (Generate GUI)
----------------------------

Генерирует интерфейс GUI для существующей игры Ren'Py.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh launcher generate_gui <basedir> [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py launcher generate_gui <basedir> [options...]

.. option:: --width <width>

    :default: 1280

    Ширина генерируемого окна интерфейса в пикселях.

.. option:: --height <height>

    :default: 720

    Высота генерируемого окна интерфейса в пикселях.

.. option:: --accent <color>

    :default: #00B8C3

    Основной акцентный цвет, используемый в оформлении интерфейса.

.. option:: --boring <color>

    :default: #000000

    Фоновый (нейтральный) цвет интерфейса.

.. option:: --light

    Указывает, что создаётся светлая тема оформления.

.. option:: --template <directory>

    :default: "gui"

    Каталог шаблона оформления, содержащий исходные файлы интерфейса.

.. option:: --language <language>

    :default: None

    Язык для генерации переводов строк и комментариев в файлах GUI.

.. option:: --start

    Инициализирует новый проект, перезаписывая код и графику интерфейса.

.. option:: --replace-images

    Перезаписывает существующие файлы изображений интерфейса.

.. option:: --replace-code

    Полностью перезаписывает существующий файл :file:`gui.rpy`.

.. option:: --update-code

    Обновляет существующий файл :file:`gui.rpy` без полной перезаписи.

.. option:: --minimal

    Обновляет только файл :file:`options.rpy` и переводы.


.. _cli-gui-images:

Графика интерфейса (GUI Images)
-------------------------------

Генерирует растровые изображения (для кнопок, полос прокрутки, переключателей и т. д.) на основе :doc:`переменных GUI <gui>`.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh launcher gui_images <basedir> [options...]

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py launcher gui_images <basedir> [options...]


.. _cli-get-projects-directory:

Получение каталога проектов (Get Projects Directory)
----------------------------------------------------

Выводит путь к каталогу проектов, используемому лаунчером Ren'Py.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh launcher get_projects_directory

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py launcher get_projects_directory


.. _cli-set-projects-directory:

Установка каталога проектов (Set Projects Directory)
----------------------------------------------------

Задаёт путь к каталогу, в котором лаунчер Ren'Py ищет проекты игр. Предназначено для использования на минималистичных системах без GUI-диалогов выбора папок.

Команда может выполняться только тогда, когда графический лаунчер не запущен.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh launcher set_projects_directory <projects>

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py launcher set_projects_directory <projects>

.. describe:: <projects>

    Путь к каталогу проектов.


.. _cli-set-project:

Выбор проекта (Set Project)
---------------------------

Делает указанный проект активным в лаунчере, переключая текущий проект и при необходимости каталог проектов.

Команда может выполняться только тогда, когда графический лаунчер не запущен.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh launcher set_project <project>

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py launcher set_project <project>

.. describe:: <project>

    Полный путь к активируемому проекту.


.. _cli-update-old-game:

Обновление старой версии игры (Update Old Game)
-----------------------------------------------

Копирует скомпилированные файлы :file:`.rpyc` из каталога :file:`<basedir>/game` в каталог :file:`<basedir>/old-game`.

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            ./renpy.sh launcher update_old_game <basedir>

    .. tab:: Windows

        .. code-block:: bat

            .\lib\py3-windows-x86_64\python.exe renpy.py launcher update_old_game <basedir>

.. seealso::

    * :ref:`old-game`


Пользовательские команды
========================

Помимо стандартных команд, разработчик может создавать собственные команды CLI для конкретного проекта с помощью функции :func:`renpy.arguments.register_command`.

Перед запуском пользовательской команды :doc:`игра проходит фазу инициализации <lifecycle>`, после чего управление передаётся функции команды.

.. function:: renpy.arguments.register_command(name, function, uses_display=False)

    Регистрирует команду, доступную для вызова при запуске Ren'Py из командной строки. При вызове команды вызывается переданная функция `function` без аргументов.

    .. describe:: name

        :type: str

        Имя команды для вызова из командной строки.

    .. describe:: function

        :type: function

        Функция Python, вызываемая при запуске команды (вызывается без позиционных аргументов).

        Если функция должна принимать дополнительные аргументы командной строки, она должна создать экземпляр :class:`renpy.arguments.ArgumentParser` и вызвать на нём метод :func:`parse_args`. В противном случае следует вызвать :func:`renpy.arguments.takes_no_arguments`.

        Если функция возвращает True, стандартный запуск Ren'Py продолжается в обычном режиме. Если функция возвращает False (или None), Ren'Py немедленно завершает работу после возврата из ``function()``.

        .. seealso::

            Подробнее о разборе аргументов командной строки см. в
            `документации ArgumentParser <https://docs.python.org/3/library/argparse.html>`_.

    .. describe:: uses_display

        :type: bool

        Если True, Ren'Py инициализирует графическую подсистему и дисплей. Если False, используются фиктивные (dummy) видео- и аудиодрайверы без открытия графического окна.


Пример
------

.. code-block:: renpy

    init python:

        def compute_area_command():
            ap = renpy.arguments.ArgumentParser(description='Вычисление площади различных геометрических фигур.')
            ap.add_argument("dimensions", nargs="*", type=float, help="Размеры фигуры.")
            ap.add_argument("--square", action="store_true", help="Вычислить площадь квадрата.")
            ap.add_argument("--rectangle", action="store_true", help="Вычислить площадь прямоугольника.")
            ap.add_argument("--circle", action="store_true", help="Вычислить площадь круга.")

            args = ap.parse_args()

            if args.square:
                print(f"Квадрат: {args.dimensions[0] ** 2}")

            elif args.rectangle:
                print(f"Прямоугольник: {args.dimensions[0] * args.dimensions[1]}")

            elif args.circle:
                print(f"Круг: {3.14 * args.dimensions[0] ** 2}")

            # Завершаем работу и не запускаем саму игру
            return False

        renpy.arguments.register_command("compute_area", compute_area_command)

Зарегистрированную команду затем можно вызвать следующим образом:

.. tabs::

    .. tab:: Linux / macOS

        .. code-block:: bash

            > ./renpy.sh <basedir> compute_area 3 --square

            Квадрат: 9.0

            > ./renpy.sh <basedir> compute_area 3 2 --rectangle

            Прямоугольник: 6.0

            > ./renpy.sh <basedir> compute_area 1 --circle

            Круг: 3.14

            > ./renpy.sh <basedir> compute_area --help

    .. tab:: Windows

        .. code-block:: bat

            > .\lib\py3-windows-x86_64\python.exe renpy.py basedir compute_area 3 --square

            Квадрат: 9.0

            > .\lib\py3-windows-x86_64\python.exe renpy.py basedir compute_area 3 2 --rectangle

            Прямоугольник: 6.0

            > .\lib\py3-windows-x86_64\python.exe renpy.py basedir compute_area 1 --circle

            Круг: 3.14

            > .\lib\py3-windows-x86_64\python.exe renpy.py basedir compute_area --help
