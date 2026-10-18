.. _achievement:

==========
Достижения
==========

Модуль достижений (Achievement) позволяет разработчику выдавать достижения
игроку, сбрасывать достижения и проверять, было ли получено то или иное достижение.
Он также позволяет отслеживать прогресс выполнения достижения.

По умолчанию модуль достижений сохраняет информацию в файле постоянных данных (persistent).
Если доступна и включена поддержка Steam, данные о достижениях автоматически
синхронизируются со Steam.

Поддержку Steam необходимо отдельно добавить в Ren'Py, чтобы гарантировать, что она
распространяется только разработчиками, принятыми в партнёрскую программу Steam.
Чтобы установить её, в лаунчере выберите «Настройки» (preferences) -> «Установить библиотеки»
(Install libraries) -> «Установить поддержку Steam» (Install Steam Support).


.. include:: inc/achievement


Переменные, управляющие достижениями:

.. var:: achievement.steam_position = None

    Если не None, задаёт позицию всплывающего уведомления Steam.
    Должно быть строкой, одной из следующих: "top left", "top right", "bottom left"
    или "bottom right".

.. var:: config.steam_appid = None

    Если не None, задаёт appid игры в Steam. Ren'Py автоматически установит этот
    appid при запуске. Задаётся с помощью инструкции define::

        define config.steam_appid = 12345

.. var:: config.automatic_steam_timeline = True

    Если True, при запуске под Steam игра будет автоматически обновлять временную шкалу Steam Timeline.

    В настоящее время это включает в себя:

    * Обновление фазы игры в соответствии с :var:`save_name`, если эта переменная задана.
    * Обновление режима игры для отражения нахождения игрока внутри меню.


API Steamworks
--------------

Когда Steam доступен, привязка на основе ctypes к API Steamworks доступна
как ``achievement.steamapi``. Привязка представляет собой экземпляр модуля steamapi,
расположенного `здесь <https://github.com/renpy/renpy-build/blob/master/steamapi/steamapi.py>`_,
и представляет собой машинный перевод C++ API Steamworks на Python.

Кроме того, большое количество функций доступно в объекте achievement.steam, если
и только если доступен API Steamworks.

.. var:: achievement.steam

    Если Steam успешно инициализирован, представляет собой пространство имён с высокоуровневыми
    методами Steam. Если Steam не инициализирован, значение равно None. Всегда проверяйте,
    что это значение не равно None, перед вызовом методов.

Приложения Steam (Steam Apps)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. include:: inc/steam_apps

Оверлей Steam (Steam Overlay)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. include:: inc/steam_overlay

Статистика Steam (Steam Stats)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. include:: inc/steam_stats

Временная шкала Steam (Steam Timeline)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. include:: inc/steam_timeline

Пользователь Steam (Steam User)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. include:: inc/steam_user

Мастерская Steam (Steam Workshop)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. include:: inc/steam_ugc
