======================
Свойства трансформаций
======================

Свойства трансформаций используются в :doc:`трансформациях <transforms>` для управления
тем, как отображается графический объект. Обычно они задаются с помощью :ref:`ATL <atl>`
или класса :class:`Transform`.

.. _transform-properties:

Список свойств трансформаций
============================

Доступны следующие свойства трансформаций.

Когда тип указан как :term:`position`, его относительный компонент интерпретируется как
доля от размера содержащей области (для :tpref:`pos`) или от размера самого отображаемого
объекта (для :tpref:`anchor`).

Обратите внимание, что не все свойства независимы. Например, :tpref:`xalign` и :tpref:`xpos`
изменяют одни и те же базовые параметры. В операторе ``parallel`` не следует изменять
взаимосвязанные свойства в нескольких параллельных ветках. Свойства угла и радиуса задают
одновременно горизонтальную и вертикальную координаты.

Позиционирование
----------------

.. transform-property:: pos

    :type: (position, position)
    :default: (0, 0)

    Положение относительно верхнего левого угла содержащей области.

.. transform-property:: xpos

    :type: position
    :default: 0

    Положение по горизонтали относительно левого края содержащей области.

.. transform-property:: ypos

    :type: position
    :default: 0

    Положение по вертикали относительно верхнего края содержащей области.

.. transform-property:: anchor

    :type: (position, position)
    :default: (0, 0)

    Точка привязки относительно верхнего левого угла отображаемого объекта.

.. transform-property:: xanchor

    :type: position
    :default: 0

    Положение точки привязки по горизонтали относительно левого края отображаемого объекта.

.. transform-property:: yanchor

    :type: position
    :default: 0

    Положение точки привязки по вертикали относительно верхнего края отображаемого объекта.

.. transform-property:: align

    :type: (float, float)
    :default: (0.0, 0.0)

    Эквивалентно установке одинаковых значений для pos и anchor.

.. transform-property:: xalign

    :type: float
    :default: 0.0

    Эквивалентно установке этого значения для xpos и xanchor.

.. transform-property:: yalign

    :type: float
    :default: 0.0

    Эквивалентно установке этого значения для ypos и yanchor.

.. transform-property:: offset

    :type: (absolute, absolute)
    :default: (0, 0)

    Смещение отображаемого объекта в пикселях по обоим направлениям. Положительные
    значения сдвигают объект вправо и вниз.

.. transform-property:: xoffset

    :type: absolute
    :default: 0

    Смещение отображаемого объекта в пикселях по горизонтали. Положительные значения
    сдвигают объект вправо.

.. transform-property:: yoffset

    :type: absolute
    :default: 0

    Смещение отображаемого объекта в пикселях по вертикали. Положительные значения
    сдвигают объект вниз.

.. transform-property:: xycenter

    :type: (position, position)
    :default: (0.0, 0.0)

    Эквивалентно установке pos в переданное значение, а anchor в (0.5, 0.5).

.. transform-property:: xcenter

    :type: position
    :default: 0.0

    Эквивалентно установке xpos в переданное значение, а xanchor в 0.5.

.. transform-property:: ycenter

    :type: position
    :default: 0.0

    Эквивалентно установке ypos в переданное значение, а yanchor в 0.5.

.. transform-property:: subpixel

    :type: boolean
    :default: False

    Если True, дочерний объект позиционируется с субпиксельной точностью.

    Субпиксельное позиционирование влияет на сглаживание цветов и прозрачность пикселей
    на границах при движении, предотвращая дискретное подергивание.


Вращение
--------

.. transform-property:: rotate

    :type: float or None
    :default: None

    Если не None, поворачивает изображение по часовой стрелке на указанное число градусов.
    Поворот изменяет размер контейнера согласно настройке rotate_pad.

.. transform-property:: rotate_pad

    :type: boolean
    :default: True

    Если True, вокруг повернутого объекта добавляются поля, равные гипотенузе исходных
    ширины и высоты, гарантируя постоянный размер контейнера при вращении. Если False,
    контейнер принимает минимальный описывающий размер.

.. transform-property:: transform_anchor

    :type: boolean
    :default: False

    Если True, точка привязки anchor масштабируется и поворачивается вместе с объектом,
    становясь центром вращения и масштабирования.


Масштаб и отражение
-------------------

.. transform-property:: zoom

    :type: float
    :default: 1.0

    Масштабирует отображаемый объект с указанным коэффициентом.

.. transform-property:: xzoom

    :type: float
    :default: 1.0

    Масштабирует объект по горизонтали. Отрицательное значение зеркально отражает объект по горизонтали.

.. transform-property:: yzoom

    :type: float
    :default: 1.0

    Масштабирует объект по вертикали. Отрицательное значение зеркально отражает объект по вертикали.


Пиксельные эффекты
------------------

.. transform-property:: nearest

    :type: boolean
    :default: None

    Если True, объект отрисовывается с фильтрацией по ближайшему соседу (nearest-neighbor).
    Если False — с билинейной фильтрацией. Если None — наследуется от родителя или :var:`config.nearest_neighbor`.

.. transform-property:: alpha

    :type: float
    :default: 1.0

    Управляет непрозрачностью отображаемого объекта от 0.0 (полностью прозрачный) до 1.0 (полностью непрозрачный).

.. transform-property:: additive

    :type: float
    :default: 0.0

    Степень аддитивного смешивания цветов. При 1.0 используется оператор ADD, при 0.0 — стандартный OVER.

.. transform-property:: matrixcolor

    :type: None or Matrix or MatrixColor
    :default: None

    Цветовая матрица для перекрашивания отображаемого объекта. Подробнее см. :doc:`matrixcolor`.

.. transform-property:: blur

    :type: None or float
    :default: None

    Размывает отображаемый объект на указанное число пикселей `blur`.


Полярное позиционирование
-------------------------

.. transform-property:: around

    :type: (position, position)
    :default: (0.0, 0.0)

    Задает точку отсчета, от которой откладывается полярный вектор (вычисленный из
    :tpref:`angle` и :tpref:`radius`). Сумма дает итоговую позицию :tpref:`pos`.

.. transform-property:: angle

    :type: float

    Угол полярных координат в градусах: 0 — вверх, 90 — вправо.

.. transform-property:: radius

    :type: position

    Радиус полярных координат. Число с плавающей точкой масштабируется к меньшему из
    доступных измерений ширины и высоты.


Полярное позиционирование точки привязки
----------------------------------------

.. note::

    Хотя полярные координаты точки привязки поддерживаются, часто удобнее установить
    :tpref:`anchor` в (0.5, 0.5) и позиционировать центр объекта.

.. transform-property:: anchoraround

    :type: (position, position)

    Точка отсчета полярных координат внутри отображаемого объекта.

.. transform-property:: anchorangle

    :type: (float)

    Угол полярных координат точки привязки в градусах.

.. transform-property:: anchorradius

    :type: (position)

    Радиус полярных координат точки привязки. Рекомендуется передавать целые числа или
    :func:`absolute` значения.


Обрезка и изменение размера
---------------------------

.. transform-property:: crop

    :type: None or (position, position, position, position)
    :default: None

    Обрезает объект до прямоугольника (`x`, `y`, `width`, `height`). Координаты отсчитываются
    от верхнего левого угла дочернего объекта.

.. transform-property:: corner1

    :type: None or (position, position)
    :default: None

    Верхний левый угол рамки обрезки.

.. transform-property:: corner2

    :type: None or (position, position)
    :default: None

    Нижний правый угол рамки обрезки.

.. transform-property:: xysize

    :type: None or (position, position)
    :default: None

    Масштабирует объект до указанных размеров (ширина, высота). Эквивалентно одновременной
    установке :tpref:`xsize` и :tpref:`ysize`.

.. transform-property:: xsize

    :type: None or position
    :default: None

    Задает ширину отображаемого объекта.

.. transform-property:: ysize

    :type: None or position
    :default: None

    Задает высоту отображаемого объекта.

.. transform-property:: fit

    :type: None or string
    :default: None

    Управляет режимом подгонки размеров:

    .. list-table::
       :widths: 15 85
       :header-rows: 1

       * - Значение
         - Описание
       * - ``contain``
         - Максимально возможный размер без выхода за границы контейнера с сохранением пропорций.
       * - ``cover``
         - Минимально возможный размер, покрывающий всю площадь контейнера с сохранением пропорций.
       * - None или ``fill``
         - Растягивает объект для точного совпадения с размерами без сохранения пропорций.
       * - ``scale-down``
         - Как ``contain``, но никогда не увеличивает исходный размер объекта.
       * - ``scale-up``
         - Как ``cover``, но никогда не уменьшает исходный размер объекта.


Панорамирование и замощение
---------------------------

.. transform-property:: xpan

    :type: None or float
    :default: None

    Угол горизонтального панорамирования по круговой панораме 360 градусов (от -180 до 180).

.. transform-property:: ypan

    :type: None or float
    :default: None

    Угол вертикального панорамирования.

.. transform-property:: xtile

    :type: int
    :default: 1

    Количество повторений (замощений) изображения по горизонтали.

.. transform-property:: ytile

    :type: int
    :default: 1

    Количество повторений (замощений) изображения по вертикали.


Переходы
--------

См. раздел :ref:`atl-transitions`.

.. transform-property:: delay

    :type: float
    :default: 0.0

    Длительность перехода в секундах, если трансформация используется в качестве перехода.

.. transform-property:: events

    :type: boolean
    :default: True

    Если True, события пользовательского ввода передаются дочернему элементу трансформации.
    Если False — блокируются (полезно для старого виджета `old_widget` при переходах).


Другие свойства
---------------

.. transform-property:: fps

    :type: float or None
    :default: None

    Дискретизирует время внутри трансформации (например, fps=10 имитирует 10 кадров в секунду).

.. transform-property:: show_cancels_hide

    :type: boolean
    :default: True

    Если True, появление нового объекта с тем же тегом отменяет трансформацию скрытия (hide) старого.


Порядок применения свойств
==========================

Свойства трансформаций применяются в следующем строгом порядке:

#. fps
#. mesh, blur
#. tile
#. pan
#. crop, corner1, corner2
#. xysize, size, maxsize
#. zoom, xzoom, yzoom
#. point_to
#. orientation
#. xrotate, yrotate, zrotate
#. rotate
#. zpos
#. matrixtransform, matrixanchor
#. zzoom
#. perspective
#. nearest, blend, alpha, additive, shader
#. matrixcolor
#. Свойства GL, Uniform-переменные
#. Свойства позиционирования
#. show_cancels_hide


Устаревшие свойства трансформаций
=================================

.. warning::

    Следующие свойства не рекомендуется использовать в современных проектах. Они сохранены
    исключительно для обратной совместимости.

.. transform-property:: alignaround

    :type: (float, float)

    Устанавливает одинаковые значения для :tpref:`anchor`, :tpref:`around` и :tpref:`anchoraround`.

.. transform-property:: crop_relative

    :type: boolean
    :default: True

    Если False, числа с плавающей точкой в crop интерпретируются как абсолютные пиксели.
    Вместо этого рекомендуется использовать тип :func:`absolute`.

.. transform-property:: size

    :type: None or (int, int)
    :default: None

    Устаревшая версия :tpref:`xysize`.

.. transform-property:: maxsize

    :type: None or (int, int)
    :default: None

    Вместо этого используйте :tpref:`xysize` со свойством :tpref:`fit` равным "contain".
