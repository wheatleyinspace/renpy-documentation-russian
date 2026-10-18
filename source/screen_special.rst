=========================
Специальные имена экранов
=========================

В Ren'Py существует два типа специальных имен экранов. Первый тип — это экраны,
которые отображаются автоматически при выполнении команд языка сценариев Ren'Py
(или их программных эквивалентов). Второй тип — экраны меню. Они имеют стандартные
имена для общепринятой функциональности, однако эти экраны можно исключать или
изменять по своему усмотрению.

На этой странице приведены примеры таких экранов. Важно понимать, что хотя некоторые
экраны должны обладать минимальным набором функций, система экранов позволяет легко
добавлять к ним любые дополнительные возможности. Например, в то время как стандартный
экран say только отображает текст, система экранов позволяет без труда добавить на него
кнопки пропуска, авточтения или отключения звука.

Некоторые специальные экраны принимают параметры. К этим параметрам можно обращаться
как к переменным в области видимости экрана.

С некоторыми экранами также связаны специальные идентификаторы (id). Специальный id
должен присваиваться отображаемому объекту определенного типа. Это может приводить
к назначению свойств данному объекту и делать его доступным той части Ren'Py,
которая отображает этот экран.

.. note::

    Примеры экранов на этой странице могут отличаться от экранов, создаваемых
    по умолчанию в новом проекте.

Внутриигровые экраны
====================

Эти экраны отображаются автоматически при выполнении определенных операторов Ren'Py.

.. _say-screen:

Say
---

Экран ``say`` вызывается оператором say при отображении диалога в режиме ADV.
Он отображается со следующими параметрами:

`who`
    Текст имени говорящего персонажа.
`what`
    Текст реплики, произносимой персонажем.

Предполагается, что на экране будут объявлены отображаемые объекты со следующими идентификаторами:

"who"
    Текстовый отображаемый объект, выводящий имя говорящего персонажа. Объекту
    Character могут быть переданы аргументы, стилизующие этот объект.

"what"
    Текстовый отображаемый объект, выводящий реплику персонажа. Объекту Character
    могут быть переданы аргументы, стилизующие этот объект.
    **Отображаемый объект с этим идентификатором обязательно должен быть определен**,
    поскольку Ren'Py использует его для расчета времени авточтения (auto-forward-mode),
    клика для продолжения (click-to-continue) и других системных функций.

"window"
    Окно или фрейм. Традиционно содержит текст who и what. Объекту Character
    могут быть переданы аргументы для стилизации этого объекта.

::

    screen say(who, what):

        window id "window":
            has vbox

            if who:
                text who id "who"

            text what id "what"


.. _choice-screen:

Choice
------

Экран ``choice`` используется для отображения внутриигровых вариантов выбора,
создаваемых оператором menu. Он принимает следующий параметр:

`items`
    Список объектов пунктов меню, представляющих каждый из вариантов выбора.
    Каждый объект имеет следующие поля:

    .. attribute:: caption

        Строка с текстом варианта выбора.

    .. attribute:: action

        Действие, вызываемое при выборе этого пункта меню. Может быть None, если
        это заголовок меню, а :var:`config.narrator_menu` равно False.

    .. attribute:: chosen

        True, если этот выбор был выбран хотя бы один раз за все прохождения игры.

    .. attribute:: args

        Кортеж, содержащий любые позиционные аргументы, переданные пункту меню.

    .. attribute:: kwargs

        Словарь, содержащий любые именованные аргументы, переданные пункту меню.

    Эти элементы и содержащиеся в них действия становятся недействительными по окончании
    оператора menu.

Кроме того, любые аргументы, переданные оператору menu, передаются при вызове экрана.

::

    screen choice(items):

        window:
            style "menu_window"

            vbox:
                style "menu"

                for i in items:

                    if i.action:

                        button:
                            action i.action
                            style "menu_choice_button"

                            text i.caption style "menu_choice"

                    else:
                        text i.caption style "menu_caption"


.. _input-screen:

Input
-----

Экран ``input`` используется для отображения функции :func:`renpy.input`. Он принимает один
параметр:

`prompt`
    Текст приглашения ко вводу, переданный в renpy.input.

Предполагается, что экран объявляет отображаемый объект со следующим идентификатором:

"input"
    Отображаемый объект input, который обязательно должен присутствовать. Ему передаются
    все параметры, указанные в renpy.input.

::

    screen input(prompt):

        window:
            has vbox

            text prompt
            input id "input"


.. _nvl-screen:

NVL
---

Экран ``nvl`` используется для отображения диалога в режиме NVL. Он принимает
следующий параметр:

`dialogue`
    Список объектов NVL Entry, каждый из которых соответствует отображаемой строке
    диалога. Каждая запись содержит следующие поля:

    .. attribute:: current

        True, если это текущая строка диалога. Текст what текущей строки диалога
        должен отображаться с идентификатором "what".

    .. attribute:: who

        Имя говорящего персонажа или None, если имени нет.

    .. attribute:: what

        Произносимый текст диалога.

    .. attribute:: who_id, what_id, window_id

        Предпочтительные идентификаторы для имени, диалога и окна, связанных с записью.

    .. attribute:: who_args, what_args, window_args

        Свойства, связанные с именем, диалогом и окном. Они автоматически применяются,
        если задан соответствующий id, но также доступны отдельно.

    .. attribute:: multiple

        В случае :doc:`диалога нескольких персонажей <multiple>` это кортеж из двух
        элементов. Первый элемент — номер блока диалога (начиная с 1), а второй —
        общее количество блоков диалога в операторе multiple.

`items`
    Тот же список элементов, который передается в :ref:`экран choice <choice-screen>`.
    Если этот список пуст, меню не должно отображаться.

Когда параметр `items` отсутствует, экран NVL всегда должен назначать текстовому
виджету идентификатор "what". Ren'Py использует его для расчета времени авточтения,
индикатора клика для продолжения и других функций. (Это выполняется автоматически
при использовании стандартного what_id.)

Ren'Py также поддерживает экран ``nvl_choice``, который принимает те же параметры,
что и ``nvl``, и при своем наличии используется вместо ``nvl``, когда игроку
предоставляется выбор в режиме NVL.

::

    screen nvl(dialogue, items=None):

        window:
            style "nvl_window"

            has vbox:
                style "nvl_vbox"

            # Отображение диалога.
            for d in dialogue:
                window:
                    id d.window_id

                    has hbox:
                        spacing 10

                    if d.who is not None:
                        text d.who id d.who_id

                    text d.what id d.what_id

            # Отображение меню, если передано.
            if items:

                vbox:
                    id "menu"

                    for i in items:

                        if action:

                            button:
                                style "nvl_menu_choice_button"
                                action i.action

                                text i.caption style "nvl_menu_choice"

                        else:

                            text i.caption style "nvl_dialogue"


.. _notify-screen:

Notify
------

Экран ``notify`` используется функцией :func:`renpy.notify` для отображения всплывающих
уведомлений пользователю. Обычно он используется в сочетании с трансформацией, управляющей
появлением и исчезновением уведомления. Принимает один параметр:

`message`
    Отображаемое сообщение.

Пример экрана уведомлений::

    screen notify(message):
        zorder 100

        text "[message!tq]" at _notify_transform

        # Управляет временем между первым показом экрана и началом его скрытия.
        timer 3.25 action Hide('notify')

    transform _notify_transform:
        # Управление положением.
        xalign .02 yalign .015

        # Управление появлением и исчезновением.
        on show:
            alpha 0
            linear .25 alpha 1.0
        on hide:
            linear .5 alpha 0.0


.. _skip-indicator:

Индикатор пропуска (Skip Indicator)
-----------------------------------

При своем наличии экран ``skip_indicator`` отображается во время выполнения пропуска
и скрывается при завершении пропуска. Не принимает параметров.

Пример простого экрана индикатора пропуска::

    screen skip_indicator():

        zorder 100

        text _("Пропуск")


.. _ctc-screen:

CTC (Клик для продолжения)
--------------------------

При своем наличии экран ``ctc`` отображается после завершения вывода диалога на экран,
сигнализируя игроку о необходимости клика для перехода к следующей реплике. Он может
принимать один позиционный параметр и несколько именованных аргументов:

`arg`
    Отображаемый объект CTC, выбранный объектом :func:`Character`. Соответствует
    одному из аргументов `ctc`, `ctc_pause` или `ctc_timedpause` персонажа. Если объекту
    Character не был передан CTC, этот аргумент вообще не передается.

Кроме того, передаются некоторые параметры, если экран ожидает их в своем объявлении:

`ctc_kind`
    Тип отображаемого CTC: "last" (для последнего CTC в реплике), "pause" или "timedpause".

`ctc_last`
    Аргумент `ctc`, переданный в :func:`Character`.

`ctc_pause`
    Аргумент `ctc_pause`, переданный в :func:`Character`.

`ctc_timedpause`
    Аргумент `ctc_timedpause`, переданный в :func:`Character`.


Пример простого экрана CTC::

    screen ctc(arg=None):

        zorder 100

        hbox:
            xalign 0.98
            yalign 0.98

            add arg

            text _("Нажмите для продолжения"):
                size 12



Внеигровые экраны меню
======================

Это экраны меню. Экраны ``main_menu`` и ``yesno_prompt`` вызываются неявно.
Когда пользователь открывает игровое меню, отображается экран, указанный в
:data:`_game_menu_screen` (по умолчанию ``save``).

Помните, что экраны меню можно свободно комбинировать и модифицировать.

.. _main-menu-screen:

Главное меню (Main Menu)
------------------------

Экран ``main_menu`` — это первый экран, отображаемый при запуске игры.

::

    screen main_menu():

        # Гарантирует замену любого другого экрана меню.
        tag menu

        # Фон главного меню.
        window:
            style "mm_root"

        # Кнопки главного меню.
        frame:
            style_prefix "mm"
            xalign .98
            yalign .98

            has vbox

            textbutton _("Начать игру") action Start()
            textbutton _("Загрузить игру") action ShowMenu("load")
            textbutton _("Настройки") action ShowMenu("preferences")
            textbutton _("Помощь") action Help()
            textbutton _("Выход") action Quit(confirm=False)

    style mm_button:
        size_group "mm"

.. _navigation-screen:

Навигация (Navigation)
----------------------

Экран ``navigation`` не является системным для Ren'Py. Однако по соглашению
мы размещаем навигацию игрового меню в экране с именем ``navigation``, а затем
используем этот экран внутри экранов сохранения, загрузки и настроек.

::

    screen navigation():

        # Фон игрового меню.
        window:
            style "gm_root"

        # Кнопки меню.
        frame:
            style_prefix "gm_nav"
            xalign .98
            yalign .98

            has vbox

            textbutton _("Вернуться") action Return()
            textbutton _("Настройки") action ShowMenu("preferences")
            textbutton _("Сохранить игру") action ShowMenu("save")
            textbutton _("Загрузить игру") action ShowMenu("load")
            textbutton _("Главное меню") action MainMenu()
            textbutton _("Помощь") action Help()
            textbutton _("Выход") action Quit()

    style gm_nav_button:
        size_group "gm_nav"

.. _save-screen:

Сохранение (Save)
-----------------

Экран ``save`` используется для выбора файла для сохранения игры.

::

    screen save():

        # Гарантирует замену любого другого экрана меню.
        tag menu

        use navigation

        frame:
            has vbox

            # Кнопки сверху позволяют выбрать страницу файлов.
            hbox:
                textbutton _("Предыдущая") action FilePagePrevious()
                textbutton _("Авто") action FilePage("auto")

                for i in range(1, 9):
                    textbutton str(i) action FilePage(i)

                textbutton _("Следующая") action FilePageNext()

            # Отображение сетки слотов сохранения.
            grid 2 5:
                transpose True
                xfill True

                # Отображение 10 слотов, пронумерованных от 1 до 10.
                for i in range(1, 11):

                    # Каждый слот является кнопкой.
                    button:
                        action FileAction(i)
                        xfill True
                        style "large_button"

                        has hbox

                        # Добавление скриншота и описания к кнопке.
                        add FileScreenshot(i)
                        text ( " %2d. " % i
                                + FileTime(i, empty=_("Пустой слот."))
                                + "\n"
                                + FileSaveName(i)) style "large_button_text"

.. _load-screen:

Загрузка (Load)
---------------

Экран ``load`` используется для выбора файла для загрузки игры.

::

    screen load():

        # Гарантирует замену любого другого экрана меню.
        tag menu

        use navigation

        frame:
            has vbox

            # Кнопки сверху позволяют выбрать страницу файлов.
            hbox:
                textbutton _("Предыдущая") action FilePagePrevious()
                textbutton _("Авто") action FilePage("auto")

                for i in range(1, 9):
                    textbutton str(i) action FilePage(i)

                textbutton _("Следующая") action FilePageNext()

            # Отображение сетки слотов сохранения.
            grid 2 5:
                transpose True
                xfill True

                # Отображение 10 слотов, пронумерованных от 1 до 10.
                for i in range(1, 11):

                    # Каждый слот является кнопкой.
                    button:
                        action FileAction(i)
                        xfill True
                        style "large_button"

                        has hbox

                        # Добавление скриншота и описания к кнопке.
                        add FileScreenshot(i)
                        text ( " %2d. " % i
                                + FileTime(i, empty=_("Пустой слот."))
                                + "\n"
                                + FileSaveName(i)) style "large_button_text"

.. _preferences-screen:

Настройки (Preferences)
-----------------------

Экран ``preferences`` используется для выбора параметров, управляющих отображением
и поведением игры.

Как правило, настройки представляют собой действия или значения полос, возвращаемые
функцией :func:`Preference`.

::

    screen preferences():

        tag menu

        # Включение панели навигации.
        use navigation

        # Размещение столбцов настроек в сетку шириной в 3 ячейки.
        grid 3 1:
            style_prefix "prefs"
            xfill True

            # Левый столбец.
            vbox:
                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Экран")
                    textbutton _("Оконный") action Preference("display", "window")
                    textbutton _("Полноэкранный") action Preference("display", "fullscreen")

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Переходы")
                    textbutton _("Все") action Preference("transitions", "all")
                    textbutton _("Нет") action Preference("transitions", "none")

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Скорость текста")
                    bar value Preference("text speed")

                frame:
                    style_prefix "pref"
                    has vbox

                    textbutton _("Джойстик...") action ShowMenu("joystick_preferences")

            vbox:

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Пропуск")
                    textbutton _("Прочитанное") action Preference("skip", "seen")
                    textbutton _("Все сообщения") action Preference("skip", "all")

                frame:
                    style_prefix "pref"
                    has vbox

                    textbutton _("Начать пропуск") action Skip()

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("После выборов")
                    textbutton _("Останавливать") action Preference("after choices", "stop")
                    textbutton _("Продолжать") action Preference("after choices", "skip")

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Время авточтения")
                    bar value Preference("auto-forward time")

            vbox:

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Громкость музыки")
                    bar value Preference("music volume")

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Громкость звуков")
                    bar value Preference("sound volume")
                    textbutton "Тест" action Play("sound", "sound_test.ogg") style "soundtest_button"

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Громкость голоса")
                    bar value Preference("voice volume")
                    textbutton "Тест" action Play("voice", "voice_test.ogg") style "soundtest_button"

    style pref_frame:
        xfill True
        xmargin 5
        top_margin 5

    style pref_vbox:
        xfill True

    style pref_button:
        size_group "pref"
        xalign 1.0

    style pref_slider:
        xmaximum 192
        xalign 1.0

    style soundtest_button:
        xalign 1.0

.. _yesno-prompt-screen:
.. _confirm-screen:

Подтверждение (Confirm)
-----------------------

Экран ``confirm`` используется для запроса подтверждения пользователя («Да/Нет»).
Он принимает следующие параметры:

`message`
    Сообщение, отображаемое пользователю. В Ren'Py используются как минимум следующие сообщения:

    * gui.ARE_YOU_SURE - "Вы уверены?" Используется по умолчанию при неизвестном сообщении.
    * gui.DELETE_SAVE - "Вы уверены, что хотите удалить это сохранение?"
    * gui.OVERWRITE_SAVE - "Вы уверены, что хотите перезаписать сохранение?"
    * gui.LOADING - "Загрузка приведет к потере несохраненного прогресса.\nВы уверены, что хотите сделать это?"
    * gui.QUIT - "Вы уверены, что хотите выйти?"
    * gui.MAIN_MENU - "Вы уверены, что хотите вернуться в главное меню?\nНесохраненный прогресс будет потерян."
    * gui.CONTINUE - "Вы уверены, что хотите продолжить с места, где остановились?"
    * gui.END_REPLAY - "Вы уверены, что хотите завершить повтор?"
    * gui.SLOW_SKIP - "Вы уверены, что хотите начать пропуск?"
    * gui.FAST_SKIP_SEEN - "Вы уверены, что хотите пропустить до следующего выбора?"
    * gui.FAST_SKIP_UNSEEN - "Вы уверены, что хотите пропустить непрочитанный диалог до следующего выбора?"
    * UNKNOWN_TOKEN - Этот файл сохранения был создан на другом устройстве. Злонамеренно созданные файлы сохранений могут нанести вред вашему компьютеру. Доверяете ли вы создателю этого сохранения и всем, кто мог изменить этот файл?
    * TRUST_TOKEN - Доверяете ли вы устройству, на котором было создано сохранение? Выбирайте «Да» только в том случае, если вы являетесь единственным пользователем этого устройства.

    Значения переменных являются строками, что позволяет отображать их с помощью текстового объекта.

`yes_action`
    Действие, выполняемое при выборе ответа «Да».

`no_action`
    Действие, выполняемое при выборе ответа «Нет».

До Ren'Py 6.99.10 этот экран назывался ``yesno_prompt``. Если экран ``confirm`` отсутствует,
вместо него используется ``yesno_prompt``.

Этот экран также вызывается функцией :func:`renpy.confirm` и действием :func:`Confirm`.

::

    screen confirm(message, yes_action, no_action):

        modal True

        window:
            style "gm_root"

        frame:
            style_prefix "confirm"

            xfill True
            xmargin 50
            ypadding 25
            yalign .25

            vbox:
                xfill True
                spacing 25

                text _(message):
                    textalign 0.5
                    xalign 0.5

                hbox:
                    spacing 100
                    xalign .5
                    textbutton _("Да") action yes_action
                    textbutton _("Нет") action no_action
