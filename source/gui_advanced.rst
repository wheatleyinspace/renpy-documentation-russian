.. _gui-advanced:

===============
Продвинутый GUI
===============

В этом разделе собраны различные сведения о продвинутом использовании GUI.


Функции Python
==============

Для поддержки работы GUI существует несколько функций Python.

.. include:: inc/gui

.. function:: gui.button_text_properties(kind=None, accent=False):

    Устаревший псевдоним для :func:`gui.text_properties`.

Подробнее о gui.rebuild
-----------------------

Функция gui.rebuild — это довольно медленная функция, которая обновляет GUI
в соответствии с текущим состоянием Ren'Py. Она выполняет следующие действия:

* Повторно выполняет все операторы ``define``, определяющие переменные в пространстве
  имен gui.
* Повторно выполняет все блоки ``translate python`` для текущего языка.
* Повторно выполняет все операторы ``style``.
* Пересобирает все стили в системе.

Обратите внимание, что блоки ``init python`` не выполняются повторно при вызове ``gui.rebuild``. Поэтому ::

    define gui.text_size = persistent.text_size

и::

    init python:
        gui.text_size = persistent.text_size

работают по-разному.


.. _gui-default:

Оператор default, пространство имен gui и gui.rebuild
-----------------------------------------------------

Оператор ``default`` имеет измененную семантику при применении к пространству
имен ``gui``. При применении к переменной в пространстве имен ``gui`` оператор
default выполняется вперемежку с оператором define, и операторы default
не выполняются повторно при вызове :func:`gui.rebuild`.

Это означает, что если у нас есть::

    default gui.accent_color = "#c04040"
    define gui.hover_color = gui.accent_color

При первом запуске игры будет установлен цвет акцента, а затем цвет наведения
будет установлен равным цвету акцента. (Оба они затем используются для установки
различных цветов стиля.)

Однако если в ходе сценария игры выполняется::

    $ gui.accent_color = "#4040c0"
    $ gui.rebuild()

Ren'Py перезапустит только define, поэтому он установит цвет наведения
в соответствии с цветом акцента, а затем обновит стили. Это позволяет
создавать элементы GUI, которые меняются по ходу игры.

При установке переменных gui во время выполнения может потребоваться вызвать gui.rebuild в
метке ``after_load``::

    label after_load:
        $ gui.rebuild()
        return

Это относится только к переменным, заданным с помощью ``default``. Все переменные в пространстве имен gui должны задаваться
с помощью ``default`` или ``define``.


.. _gui-preferences:

Настройки GUI
=============

Ren'Py также поддерживает систему настроек GUI, состоящую из одной функции
и пары действий.

.. include:: inc/gui_preference

Пример
------

Система настроек GUI используется путем вызова :func:`gui.preference` при определении
переменных, с указанием имени настройки и значения по умолчанию.
Например, настройки GUI можно использовать для задания шрифта и
размера текста. ::

    define gui.text_font = gui.preference("font", "DejaVuSans.ttf")
    define gui.text_size = gui.preference("size", 22)

Затем можно использовать действия :class:`gui.SetPreference` и :class:`gui.TogglePreference`
для изменения значений настроек. Вот несколько примеров, которые
можно добавить на экран настроек (preferences). ::

    vbox:
        style_prefix "check"
        label _("Опции")
        textbutton _("OpenDyslexic") action gui.TogglePreference("font", "OpenDyslexic-Regular.otf", "DejaVuSans.ttf")

    vbox:
        style_prefix "radio"
        label _("Размер текста")
        textbutton _("Мелкий") action gui.SetPreference("size", 20)
        textbutton _("Средний") action gui.SetPreference("size", 22)
        textbutton _("Крупный") action gui.SetPreference("size", 24)
