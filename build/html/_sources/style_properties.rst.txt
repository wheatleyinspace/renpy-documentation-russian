===============
Свойства стилей
===============

Свойства стилей, связанные со :doc:`стилями <style>`, управляют отображением
отображаемых объектов (:doc:`displayables`). Свойства стиля могут задаваться без
префикса (в этом случае они применяются ко всем состояниям объекта) либо с префиксом,
ограничивающим применение свойства определенными состояниями.


Префиксы свойств стилей
=======================

Применение префикса к свойству стиля позволяет объекту менять свой внешний вид в
зависимости от наличия фокуса или выбора. Например, кнопка может менять свой цвет
при наведении курсора мыши или подсвечиваться, если представленный ею вариант активен.

Существует пять основных состояний отображаемого объекта:

:dfn:`insensitive`
    Используется, когда объект неактивен и пользователь не может взаимодействовать с ним.

:dfn:`idle`
    Используется, когда объект доступен, но не находится под фокусом и не выбран.

:dfn:`hover`
    Используется, когда объект находится под фокусом (под курсором), но не выбран.

:dfn:`selected_idle`
    Используется, когда объект выбран, но не находится под фокусом.

:dfn:`selected_hover`
    Используется, когда объект и выбран, и находится под фокусом одновременно.

Кнопки и полосы (а также их разновидности) обновляют собственное состояние и состояние
своих дочерних элементов в ответ на события. Например, при наведении курсора на невыбранную
кнопку она сама и все ее дочерние элементы переходят в состояние hover.

Префиксы свойств стилей позволяют настраивать свойства для различных состояний.
Действует система наследования (импликации), при которой префикс может задавать свойство
сразу для нескольких состояний.

Правила импликации:

.. list-table::
 :header-rows: 1

 * - префикс
   - состояния, подразумеваемые префиксом

 * - (без префикса)
   - insensitive, idle, hover, selected\_idle, selected\_hover

 * - ``idle_``
   - idle, selected\_idle

 * - ``hover_``
   - hover, selected\_hover

 * - ``selected_``
   - selected\_idle, selected\_hover

 * - ``insensitive_``
   - insensitive

 * - ``selected_idle_``
   - selected\_idle

 * - ``selected_hover_``
   - selected\_hover

 * - ``selected_insensitive_``
   - selected\_insensitive

Рассмотрим это на примере текстовой кнопки (textbutton). Текстовые кнопки по умолчанию
используют два стиля: ``button`` для самой кнопки и ``button_text`` для текста внутри нее.
Свойство стиля :propref:`background` задает фон кнопки, а свойство :propref:`color` — цвет текста.::

    # Фон кнопки серый, когда она неактивна, светло-синий при наведении
    # и темно-синий в остальных случаях.
    style button:
        background "#006"
        insensitive_background "#444"
        hover_background "#00a"

    # Текст кнопки желтый, когда кнопка выбрана, и белый в остальных случаях.
    style button_text:
        color "#fff"
        selected_color "#ff0"


Значения свойств стилей
=======================

Каждое свойство стиля ожидает определенный тип данных. Многие из них являются стандартными
типами Python, но некоторые уникальны для Ren'Py. Ниже приведено описание этих типов.

.. glossary::

    position
        Позиции (координаты) используются для задания положения относительно верхнего
        левого угла содержащей области. (Для размещения и размера содержащая область
        определяется макетом, в котором находится объект, либо экраном; для точек
        привязки anchor содержащей областью является размер самого отображаемого объекта).

        Интерпретация значения позиции зависит от типа переданного значения:

        int (целое число: 0, 1, 37, 42)
            Число пикселей от левого или верхнего края содержащей области.
        float (число с плавающей точкой: 0.0, 0.5, 1.0)
            Доля от размера содержащей области. Например, 0.5 — середина стороны
            содержащей области, а 1.0 — правый или нижний край.

        .. function:: absolute(value, /)

            Например, ``absolute(100.25)`` или ``absolute(a+b)``.

            Число типа ``absolute`` интерпретируется как точное количество пикселей от
            левого или верхнего края экрана при субпиксельном рендеринге.

        .. function:: position(absolute, relative, /)

            Например, ``position(-10, .5)``.

            Комбинация абсолютного смещения `absolute` и относительного смещения `relative`.
            Два компонента складываются для вычисления итоговой координаты.

    displayable
        Любой отображаемый объект. Если путь содержит подстановку "[prefix\_]",
        выполняется поиск префикса, как описано ниже.

    color
        Цвета в Ren'Py могут выражаться в виде строк, начинающихся со знака решетки (#),
        за которым следует шестнадцатеричная тройка или четверка значений.

        В тройке компоненты задают красный, зеленый и синий цвета (RGB). В четверке —
        красный, зеленый, синий и альфа-канал непрозрачности (RGBA). Например:

        * ``"#f00"`` и ``"#ff0000"`` задают непрозрачный красный цвет.
        * ``"#0f08"`` и ``"#00ff0080"`` задают полупрозрачный зеленый цвет.

        Шестнадцатеричные коды цветов аналогичны используемым в HTML.

        Цвет также может быть представлен в виде кортежа из 4 целых чисел от 0 до 255
        в формате (R, G, B, A).

        * ``(0, 0, 255, 255)`` задает непрозрачный синий цвет.

        Наконец, цвет может быть экземпляром класса :class:`Color`.


.. _style-prefix-search:

Поиск префикса стиля
--------------------

Когда свойство стиля содержит подстановку "[prefix\_]", выполняется поиск префикса.
Поиск производится раздельно для каждого состояния объекта, включая состояния,
подразумеваемые исходным свойством.

Например, при наличии следующего правила::

    style button:
        hover_background "[prefix_]background.png"

раздельный поиск выполняется для состояний hover и selected\_hover. Набор проверяемых
префиксов зависит от состояния:

.. list-table::
 :header-rows: 1

 * - состояние
   - порядок поиска

 * - idle
   - "idle\_", ""

 * - hover
   - "hover\_", ""

 * - insensitive
   - "insensitive\_", "", "idle\_"

 * - selected\_idle
   - "selected\_idle\_", "idle\_", "selected\_", ""

 * - selected_hover
   - "selected\_hover\_", "hover\_", "selected\_", ""

 * - selected\_insensitive
   - "selected\_insensitive\_", "hover\_", "selected\_", "", "selected\_idle\_", "idle\_"

При поиске каждый префикс пробуется по порядку. В строке подстрока "[prefix\_]" заменяется
на префикс, после чего Ren'Py проверяет наличие загружаемого файла или изображения с таким
именем. Если файл найден, поиск завершается и используется найденный объект. Иначе проверка
переходит к следующему префиксу.

Префикс стиля также передается через контейнеры, трансформации и фреймы, не принимающие
пользовательский ввод.

Пример: если существуют файлы :file:`idle_button.png` и :file:`hover_button.png`::

    style button:
        background "[prefix_]button.png"

эквивалентно::

    style button:
        idle_background "idle_button.png"
        hover_background "hover_button.png"
        insensitive_background "idle_button.png"

        selected_idle_background "idle_button.png"
        selected_hover_background "hover_button.png"
        selected_insensitive_background "idle_button.png"


Список всех свойств стилей
==========================

Свойства стилей определяют визуальный вид различных отображаемых объектов. Поскольку не
все свойства применимы ко всем объектам, они разделены на смысловые группы.


.. _position-style-properties:

Свойства стиля позиционирования
-------------------------------

Эти свойства применимы ко всем отображаемым объектам и в основном управляют положением
объекта внутри области, выделенной ему макетом, либо на экране (если объект вне макета).

.. style-property:: alt string or None

    Альтернативный текст для объекта при включенном синтезе речи (self-voicing).
    Подробнее см. :doc:`self_voicing`.

.. style-property:: xpos position

    Положение отображаемого объекта относительно левого края содержащей области.

.. style-property:: ypos position

    Положение отображаемого объекта относительно верхнего края содержащей области.

.. style-property:: pos tuple of (position, position)

    Эквивалентно заданию xpos равным первому элементу кортежа, а ypos — второму.

.. style-property:: xanchor position

    Положение точки привязки (anchor) относительно левого края отображаемого объекта.

.. style-property:: yanchor position

    Положение точки привязки (anchor) относительно верхнего края отображаемого объекта.

    Если объект является :func:`Text`, поддерживается специальное значение ``renpy.BASELINE``,
    привязывающее точку к базовой линии первой строки текста.

.. style-property:: anchor tuple of (position, position)

    Эквивалентно заданию xanchor первому элементу кортежа, а yanchor — второму.

.. style-property:: xalign float

    Эквивалентно установке одинакового значения для xpos и xanchor. Размещает объект
    по горизонтали в долях экрана: 0.0 — левый край, 0.5 — центр, 1.0 — правый край.

.. style-property:: yalign float

    Эквивалентно установке одинакового значения для ypos и yanchor. Размещает объект
    по вертикали: 0.0 — верхний край, 0.5 — середина по вертикали, 1.0 — нижний край.

.. style-property:: align tuple of (float, float)

    Эквивалентно установке xalign первому элементу кортежа, а yalign — второму.

.. style-property:: xcenter position

    Эквивалентно установке xpos в переданное значение, а xanchor — в 0.5.

.. style-property:: ycenter position

    Эквивалентно установке ypos в переданное значение, а yanchor — в 0.5.

.. style-property:: xycenter tuple of (position, position)

    Эквивалентно установке xcenter первому элементу кортежа, а ycenter — второму.

.. style-property:: xoffset int

    Смещение в пикселях, добавляемое к горизонтальной позиции, вычисленной через xpos и xalign.

.. style-property:: yoffset int

    Смещение в пикселях, добавляемое к вертикальной позиции, вычисленной через ypos и yalign.

.. style-property:: offset tuple of (int, int)

    Эквивалентно установке xoffset первому элементу кортежа, а yoffset — второму.

.. style-property:: xmaximum int

    Максимальный горизонтальный размер отображаемого объекта в пикселях.

.. style-property:: ymaximum int

    Максимальный вертикальный размер отображаемого объекта в пикселях.

.. style-property:: maximum tuple of (int, int)

    Эквивалентно установке xmaximum первому элементу кортежа, а ymaximum — второму.

.. style-property:: xminimum int

    Минимальная ширина отображаемого объекта в пикселях. Действует только для объектов,
    способных изменять свой размер.

.. style-property:: yminimum int

    Минимальная высота отображаемого объекта в пикселях. Действует только для объектов,
    способных изменять свой размер.

.. style-property:: minimum tuple of (int, int)

    Эквивалентно установке xminimum первому элементу кортежа, а yminimum — второму.

.. style-property:: xsize int or float

    Эквивалентно установке xminimum и xmaximum в одинаковое значение. Задает фиксированную
    ширину объекта. Число с плавающей точкой задает долю ширины родительского контейнера.

.. style-property:: ysize int or float

    Эквивалентно установке yminimum и ymaximum в одинаковое значение. Задает фиксированную
    высоту объекта. Число с плавающей точкой задает долю высоты родительского контейнера.

.. style-property:: xysize tuple of (int or float, int or float)

    Эквивалентно установке xsize первому элементу кортежа, а ysize — второму.

.. style-property:: xfill boolean

    Если True, объект растягивается по всей доступной ширине контейнера. Если False,
    его размер достаточен лишь для вмещения дочерних элементов.

.. style-property:: yfill boolean

    Если True, объект растягивается по всей доступной высоте контейнера. Если False,
    его размер достаточен лишь для вмещения дочерних элементов.

.. style-property:: area tuple of (int, int, int, int)

    Кортеж вида (`xpos`, `ypos`, `width`, `height`). Размещает объект так, чтобы его
    верхний левый угол находился в точке `xpos`, `ypos`, а размеры составляли `width` и `height`.

.. style-property:: mipmap boolean or None

    Управляет генерацией mipmap-уровней текстур для данного отображаемого объекта
    (применимо к :func:`Text`, :func:`Movie` и переходам dissolve).
    Если None, берется из переменных конфигурации (:var:`config.mipmap_text` и др.).


.. _text-style-properties:

Свойства стиля текста
---------------------

.. style-property:: antialias boolean

    Если True (по умолчанию), текст шрифтов TrueType отрисовывается со сглаживанием (антиалиасингом).

.. style-property:: adjust_spacing boolean or str

    Если True, Ren'Py корректирует интервалы текста экранного разрешения под виртуальное
    разрешение, гарантируя неизменность границ фреймов и контейнеров при масштабировании.
    "horizontal" корректирует только горизонтальные интервалы, "vertical" — только вертикальные.
    По умолчанию True для большинства текстов и False для текста в ``input``.

.. style-property:: altruby_style style or None

    Объект стиля, используемый для альтернативного руби-текста (фуриганы).

.. style-property:: axis dict or None

    Словарь настройки осей :ref:`вариативного шрифта <variable-fonts>`. Пример::

        style default:
            font "VariableFont.ttf"
            axis { "weight" : 500, "width" : 95 }

.. style-property:: black_color color

    При отрисовке растровых шрифтов черный цвет отображается этим цветом (для TrueType не действует).

.. style-property:: bold boolean

    Если True, текст выводится полужирным начертанием.

.. style-property:: caret displayable or None

    Отображаемый объект, используемый в качестве текстового курсора в виджете ввода.
    По умолчанию мигающая вертикальная линия толщиной 1 пиксель.

.. style-property:: color color

    Цвет отображаемого текста.

.. style-property:: emoji_font string

    Шрифт для отображения символов Emoji.

.. style-property:: first_indent int

    Отступ первой строки абзаца (красная строка) в пикселях.

.. style-property:: font string

    Имя файла или зарегистрированное имя шрифта (например, ``"DejaVuSans.ttf"``).

.. style-property:: font_features dict or None

    Словарь параметров типографики OpenType Layout::

        style default:
            font "SomeFont.ttf"
            font_features { "liga" : True, "smcp" : True, "calt" : False }

.. style-property:: hinting str

    Режим хинтинга шрифта: "auto", "auto-light", "bytecode", "none". Может также
    быть True для выборки из :var:`config.font_hinting`.

.. style-property:: hyperlink_functions tuple of (function, function, function)

    Кортеж из трех функций для гиперссылок: (стиль ссылки, клик по ссылке, фокус ссылки).

.. style-property:: instance string or None

    Имя именованного начертания для вариативного шрифта (например, "Bold").

.. style-property:: italic boolean

    Если True, текст отображается курсивным начертанием.

.. style-property:: justify boolean

    Если True, текст выравнивается по ширине (за исключением последней строки абзаца).

.. style-property:: kerning float

    Кернинг — дополнительное расстояние в пикселях между парами символов.

.. style-property:: language string

    Языковые правила переноса строк: ``"unicode"`` (по умолчанию), ``"japanese-strict"``,
    ``"japanese-normal"``, ``"japanese-loose"``, ``"korean-with-spaces"``, ``"thaic90"``,
    ``"western"``, ``"anywhere"``.


.. style-property:: layout string

    Алгоритм компоновки слов по строкам:
    ``"tex"`` (по умолчанию, алгоритм Кнута-Пласса для минимизации разницы длин строк),
    ``"subtitle"`` (равномерное распределение строк), ``"greedy"`` (жадный перенос на новую строку),
    ``"nobreak"`` (запрет переносов).

.. style-property:: line_leading int

    Отступ над строкой текста в пикселях.

.. style-property:: line_overlap_split int

    Количество пикселей перекрытия строк при медленном выводе текста, относящихся к верхней строке.

.. style-property:: line_spacing int

    Отступ под строкой текста в пикселях (межстрочный интервал).

.. style-property:: min_width int

    Минимальная ширина каждой строки текста в пикселях.

.. style-property:: newline_indent boolean

    Если True, отступ :propref:`first_indent` применяется после каждого символа новой строки.

.. style-property:: outlines list of tuple of (int, color, int, int)

    Список контуров (обводок), рисуемых позади текста. Каждый кортеж задает
    (`толщина`, `цвет`, `xoffset`, `yoffset`). Отрисовываются от задних к передним.

.. style-property:: outline_scaling string

    Режим масштабирования обводок: ``"linear"`` (по умолчанию) или ``"step"``.

.. style-property:: prefer_emoji boolean

    Определяет предпочтительный рендеринг символов с двойной природой (Emoji или стандартный текст).

.. style-property:: reading_order string

    Направление чтения текста: ``None`` (автоопределение), ``"ltr"``, ``"rtl"``, ``"wltr"``, ``"wrtl"``.

.. style-property:: rest_indent int

    Отступ для второй и последующих строк абзаца в пикселях.

.. style-property:: ruby_line_leading int

    Дополнительное расстояние над строкой с руби-текстом в пикселях (в дополнение к :propref:`line_leading`).

.. style-property:: ruby_style style or None

    Объект стиля для отображения руби-текста (фуриганы).

.. style-property:: shaper "harfbuzz" or "freetype"

    Шейпер текста: "harfbuzz" (поддерживает сложные лигатуры, индийские языки, комбинированные эмодзи)
    или "freetype".

.. style-property:: size int

    Размер шрифта в пикселях.

.. style-property:: slow_abortable boolean

    Если True, клик по тексту прерывает эффект медленного появления и мгновенно выводит остаток текста.

.. style-property:: slow_cps int or True

    Скорость вывода текста (число символов в секунду либо True для использования скорости из настроек).

.. style-property:: slow_cps_multiplier float

    Множитель скорости вывода текста.

.. style-property:: strikethrough boolean

    Если True, текст отображается зачеркнутым.

.. style-property:: textalign float

    Выравнивание текста в пределах строки: 0.0 — по левому краю, 0.5 — по центру, 1.0 — по правому краю.

.. style-property:: underline boolean

    Если True, текст отображается с подчеркиванием.

.. style-property:: vertical boolean

    Если True, текст отображается вертикально.


.. _window-style-properties:

Свойства стиля окон
-------------------

Свойства окон задают вид окон, фреймов и кнопок. Раздел :ref:`margin-style-properties`
также входит в эту категорию.

.. style-property:: background displayable or None

    Фон окна (часто :func:`Frame`, позволяющий масштабировать рамку фона под размер окна).
    Если None, фон не рисуется.

.. style-property:: foreground displayable or None

    Если не None, отображается поверх содержимого окна.

.. style-property:: left_padding int

    Внутренний отступ слева в пикселях.

.. style-property:: right_padding int

    Внутренний отступ справа в пикселях.

.. style-property:: xpadding int

    Эквивалентно установке одинакового значения для left_padding и right_padding.

.. style-property:: top_padding int

    Внутренний отступ сверху в пикселях.

.. style-property:: bottom_padding int

    Внутренний отступ снизу в пикселях.

.. style-property:: ypadding int

    Эквивалентно установке одинакового значения для top_padding и bottom_padding.

.. style-property:: padding tuple

    Кортеж из двух (x, y) или четырех (left, top, right, bottom) внутренних отступов.

.. style-property:: size_group string or None

    Строковый идентификатор группы размеров. Ren'Py выравнивает размеры всех окон с одинаковым
    size_group до единого размера.

.. style-property:: modal boolean or callable

    Если True, окно является модальным (события не проходят сквозь него к нижним слоям).
    Может быть функцией проверки условий.


.. _button-style-properties:

Свойства стиля кнопок
---------------------

.. style-property:: child displayable or None

    Отображаемый объект, заменяющий дочерний элемент кнопки (например, `insensitive_child`
    может отображать значок замка на заблокированной кнопке).

.. style-property:: hover_sound string

    Звук при получении кнопкой фокуса (наведении курсора).

.. style-property:: activate_sound string

    Звук при активации (клике) кнопки. Также воспроизводится при взаимодействии с полосой bar.

.. style-property:: mouse string

    Курсор мыши при наведении на кнопку (ключ из :var:`config.mouse`).

.. style-property:: focus_mask multiple

    Маска фокуса кнопки:
    - Displayable: непрозрачные области определяют область кликабельности.
    - True: сама кнопка используется как маска.
    - callable: функция проверки координат точки клика.
    - None: вся прямоугольная область кнопки активна.

.. style-property:: keyboard_focus boolean

    Если True, кнопка может выбираться с помощью клавиатуры или геймпада.

.. style-property:: keyboard_focus_insets (int, int, int, int) or None

    Кортеж сужения прямоугольника фокуса (left, top, right, bottom) в пикселях для перекрывающихся кнопок.

.. style-property:: key_events boolean

    Если True, клавиатурные события передаются дочерним элементам кнопки.


.. _bar-style-properties:

Свойства стиля полос (Bar)
--------------------------

Полосы (bars) используются для индикаторов, полос прокрутки и слайдеров.

.. style-property:: bar_vertical boolean

    Если True, полоса ориентирована вертикально. Если False — горизонтально.

.. style-property:: bar_invert boolean

    Если True, значение полосы заполняется справа налево или сверху вниз.

.. style-property:: bar_resizing boolean

    Если True, стороны полосы масштабируются. Если False — обрезаются (crop).

.. style-property:: left_gutter int

    Размер левого желоба в пикселях.

.. style-property:: right_gutter int

    Размер правого желоба в пикселях.

.. style-property:: top_gutter int

    Размер верхнего желоба в пикселях.

.. style-property:: bottom_gutter int

    Размер нижнего желоба в пикселях.

.. style-property:: left_bar displayable

    Отображаемый объект для левой (заполненной) стороны полосы.

.. style-property:: right_bar displayable

    Отображаемый объект для правой стороны полосы.

.. style-property:: top_bar displayable

    Отображаемый объект для верхней стороны вертикальной полосы.

.. style-property:: bottom_bar displayable

    Отображаемый объект для нижней стороны вертикальной полосы.

.. style-property:: base_bar displayable

    Базовый отображаемый объект для всей полосы.

.. style-property:: thumb displayable or None

    Бегунок (ползунок), отображаемый на границе между сторонами полосы.

.. style-property:: thumb_align float

    Выравнивание бегунка относительно полосы (0.5 для центрирования бегунка по толщине).

.. style-property:: thumb_shadow displayable or None

    Тень ползунка.

.. style-property:: thumb_offset int or tuple of (int, int)

    Смещение перекрытия бегунка и полосы в пикселях.

.. style-property:: mouse string

    Стиль курсора мыши при наведении на полосу.

.. style-property:: unscrollable string or None

    Поведение при неактивной прокрутке: None (обычный вид), "insensitive" (неактивный вид),
    "hide" (скрыть полосу с сохранением места).

.. style-property:: keyboard_focus boolean

    Доступность полосы для управления с клавиатуры или геймпада.


.. _box-style-properties:

Свойства стиля блоков (Box)
---------------------------

Используются контейнерами горизонтального (hbox) и вертикального (vbox) размещения.

.. style-property:: spacing int

    Расстояние между элементами блока в пикселях.

.. style-property:: first_spacing int

    Расстояние между первым и вторым элементами блока в пикселях.

.. style-property:: box_reverse boolean

    Если True, порядок размещения элементов обратный (справа налево для hbox, снизу вверх для vbox).

.. style-property:: box_wrap boolean

    Если True, элементы переносятся на новую строку/колонку при достижении границы контейнера.

.. style-property:: box_wrap_spacing int

    Расстояние между строками или столбцами при включенном box_wrap.

.. style-property:: box_align float or None

    Выравнивание рядов или колонок внутри блока (0.0 — по левому/верхнему краю, 0.5 — по центру).

.. style-property:: order_reverse boolean

    Если True, порядок отрисовки элементов обратный (первый элемент рисуется поверх остальных).

.. style-property:: box_justify boolean or "first" or "all"

    Выравнивание элементов по ширине блока с подгонкой расстояний между ними.


.. _grid-style-properties:

Свойства стиля сеток (Grid)
---------------------------

Используются отображаемыми объектами grid и vpgrid. Раздел :ref:`margin-style-properties`
также применим к сеткам.

.. style-property:: spacing int

    Расстояние между ячейками сетки по горизонтали и вертикали в пикселях.

.. style-property:: xspacing int or None

    Расстояние между ячейками по горизонтали в пикселях.

.. style-property:: yspacing int or None

    Расстояние между ячейками по вертикали в пикселях.


.. _fixed-style-properties:

Свойства стиля Fixed
--------------------

Используются макетом fixed.

.. style-property:: fit_first boolean or "width" or "height"

    Если True, размер контейнера fixed подгоняется под размер его первого дочернего элемента.

.. style-property:: xfit boolean

    Если True, ширина fixed подгоняется под правый край самого правого дочернего элемента.

.. style-property:: yfit boolean

    Если True, высота fixed подгоняется под нижний край самого нижнего дочернего элемента.


.. _margin-style-properties:

Свойства стиля внешних отступов (Margin)
----------------------------------------

Свойства внешних отступов добавляют прозрачное пространство вокруг окон, рамок,
кнопок, а также снаружи сеток grid и vpgrid.

.. style-property:: left_margin int

    Внешний отступ слева в пикселях.

.. style-property:: right_margin int

    Внешний отступ справа в пикселях.

.. style-property:: xmargin int

    Эквивалентно установке одинакового значения для left_margin и right_margin.

.. style-property:: top_margin int

    Внешний отступ сверху в пикселях.

.. style-property:: bottom_margin int

    Внешний отступ снизу в пикселях.

.. style-property:: ymargin int

    Эквивалентно установке одинакового значения для top_margin и bottom_margin.

.. style-property:: margin tuple

    Кортеж из двух (x, y) или четырех (left, top, right, bottom) внешних отступов.
