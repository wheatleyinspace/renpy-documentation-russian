.. _keymap:

=========================
Настройка привязок клавиш
=========================

Переменная :var:`config.keymap` содержит словарь, сопоставляющий имена событий со списками
символических имён клавиш (keysyms), вызывающих эти события.

.. note::

    Многие игроки привыкли к стандартным комбинациям клавиш Ren'Py и ожидают одинакового управления
    в различных играх.

В Ren'Py keysyms представляют собой строки, обозначающие кнопки мыши, кнопки геймпада или клавиши клавиатуры.

Кнопки мыши используют имена формата 'mouseup_#' или 'mousedown_#', где # — номер кнопки.
Ren'Py поддерживает 5-кнопочную мышь: кнопки 1, 2 и 3 — это левая, средняя и правая кнопки,
а кнопки 4 и 5 генерируются вращением колеса мыши вверх и вниз. Например, "mousedown_1" —
нажатие левой кнопки мыши, "mouseup_1" — её отпускание, а "mousedown_4" — прокрутка колеса вверх.

Существуют два вида символических имён клавиш клавиатуры:

1. Строка с символом, генерируемым при нажатии клавиши (удобно для букв и цифр, например "a", "A", "7").
   Они чувствительны к регистру: "a" не совпадает с "A". Этот вид keysym работает только при вводе текста
   (например, отпускание клавиши 'a' не сработает с ``keyup_a``, так как текст при отпускании не создаётся).

2. Символическое имя клавиши из констант pygame.constants (вида "K\_BACKSPACE", "K\_RETURN", "K\_TAB").
   Полный список можно найти в `документации Pygame <http://www.pygame.org/docs/ref/key.html>`_.

Перед именами клавиш клавиатуры и мыши можно указывать следующие префиксы, разделённые знаками подчёркивания:

alt
    Срабатывает при зажатой клавише Alt. Keysyms без этого префикса срабатывают, когда Alt не нажат.
meta
    Срабатывает при зажатой клавише Meta, Command (на macOS) или Windows.
ctrl
    Срабатывает при зажатой клавише Ctrl. (Сам по себе Ctrl обычно включает режим пропуска).
osctrl
    Клавиша Alt на macOS и Ctrl на других операционных системах.
anymod
    Срабатывает независимо от состояния клавиш Alt, Meta или Ctrl.
shift
    Срабатывает, когда зажата клавиша Shift.
noshift
    Срабатывает, когда Shift не зажат.
caps
    Срабатывает при включённом Caps Lock.
nocaps
    Срабатывает при выключенном Caps Lock.
num
    Срабатывает при включённом Num Lock.
nonum
    Срабатывает при выключенном Num Lock.
repeat
    Срабатывает при автоповторе клавиши от длительного удержания (не работает с мышью).
anyrepeat
    Срабатывает как при первом нажатии, так и при последующих автоповторах.
keydown
    Срабатывает в момент нажатия клавиши (по умолчанию).
keyup
    Срабатывает в момент отпускания клавиши.

Например, "shift_alt_K_F5" сработает при нажатии F5 с одновременно зажатыми Shift и Alt.
А "shift_mouse_1" — при клике левой кнопкой мыши с зажатым Shift.

Чтобы изменить привязку, измените соответствующий список в словаре :var:`config.keymap`.
Следующий код добавляет клавишу 't' для продолжения диалога и убирает пробел::

    init python:
        config.keymap['dismiss'].append('K_t')
        config.keymap['dismiss'].remove('K_SPACE')

Словарь привязок по умолчанию определён в renpy/common/00keymap.rpy и выглядит следующим образом::

    config.keymap = dict(

        # Привязки, действующие практически везде, если не отключены явно.
        rollback = [ 'anyrepeat_K_PAGEUP', 'anyrepeat_KP_PAGEUP', 'K_AC_BACK', 'mousedown_4' ],
        screenshot = [ 'alt_K_s', 'alt_shift_K_s', 'noshift_K_s' ],
        toggle_afm = [ ],
        toggle_fullscreen = [ 'alt_K_RETURN', 'alt_K_KP_ENTER', 'K_F11', 'noshift_K_f' ],
        game_menu = [ 'K_ESCAPE', 'K_MENU', 'K_PAUSE', 'mouseup_3' ],
        hide_windows = [ 'mouseup_2', 'noshift_K_h' ],
        launch_editor = [ 'shift_K_e' ],
        dump_styles = [ ],
        reload_game = [ 'alt_K_r', 'shift_K_r' ],
        inspector = [ 'alt_noshift_K_i', 'shift_K_i' ],
        full_inspector = [ 'alt_shift_K_i' ],
        developer = [ 'alt_K_d', 'shift_K_d', ],
        quit = [ ],
        iconify = [ ],
        help = [ 'K_F1', 'meta_shift_/' ],
        choose_renderer = ['alt_K_g', 'shift_K_g' ],
        progress_screen = [ 'alt_shift_K_p', 'meta_shift_K_p', 'K_F2' ],
        bubble_editor = [ 'alt_K_b', 'shift_K_b' ],

        # Специальные возможности (Accessibility).
        accessibility = [ 'shift_K_a' ],
        self_voicing = [ 'alt_K_v', 'K_v' ],
        clipboard_voicing = [ 'alt_shift_K_c', 'shift_K_c' ],
        debug_voicing = [ 'alt_shift_K_v', 'meta_shift_K_v' ],
        extra_voicing = [ '?' ],

        # Реплики и диалог.
        rollforward = [ 'anyrepeat_K_PAGEDOWN', 'anyrepeat_KP_PAGEDOWN', 'mousedown_5', ],
        dismiss = [ 'K_RETURN', 'K_SPACE', 'K_KP_ENTER', 'K_SELECT', 'mouseup_1' ],
        dismiss_unfocused = [ ],

        # Пауза.
        dismiss_hard_pause = [ ],

        # Навигация фокуса.
        focus_left = [ 'anyrepeat_K_LEFT', 'anyrepeat_KP_LEFT' ],
        focus_right = [ 'anyrepeat_K_RIGHT', 'anyrepeat_KP_RIGHT' ],
        focus_up = [ 'anyrepeat_K_UP', 'anyrepeat_KP_UP' ],
        focus_down = [ 'anyrepeat_K_DOWN', 'anyrepeat_KP_DOWN' ],

        # Кнопки.
        button_ignore = [ 'mousedown_1' ],
        button_select = [ 'K_RETURN', 'K_KP_ENTER', 'K_SELECT', 'mouseup_1',  ],
        button_alternate = [ 'mouseup_3' ],
        button_alternate_ignore = [ 'mousedown_3' ],

        # Поле ввода текста.
        input_backspace = [ 'anyrepeat_K_BACKSPACE' ],
        input_enter = [ 'K_RETURN', 'K_KP_ENTER' ],
        input_next_line = [ 'shift_K_RETURN', 'shift_K_KP_ENTER' ],
        input_left = [ 'anyrepeat_K_LEFT', 'anyrepeat_KP_LEFT' ],
        input_right = [ 'anyrepeat_K_RIGHT', 'anyrepeat_KP_RIGHT' ],
        input_up = [ 'anyrepeat_K_UP', 'anyrepeat_KP_UP' ],
        input_down = [ 'anyrepeat_K_DOWN', 'anyrepeat_KP_DOWN' ],
        input_delete = [ 'anyrepeat_K_DELETE', 'anyrepeat_KP_DELETE' ],
        input_home = [ 'K_HOME', 'KP_HOME', 'meta_K_LEFT' ],
        input_end = [ 'K_END', 'KP_END', 'meta_K_RIGHT' ],
        input_copy = [ 'ctrl_noshift_K_INSERT', 'ctrl_noshift_K_c', 'meta_noshift_K_c' ],
        input_paste = [ 'shift_K_INSERT', 'ctrl_noshift_K_v', 'meta_noshift_K_v' ],
        input_jump_word_left = [ 'osctrl_K_LEFT', 'osctrl_KP_LEFT' ],
        input_jump_word_right = [ 'osctrl_K_RIGHT', 'osctrl_KP_RIGHT' ],
        input_delete_word = [ 'osctrl_K_BACKSPACE' ],
        input_delete_full = [ 'meta_K_BACKSPACE' ],

        # Область просмотра (Viewport).
        viewport_leftarrow = [ 'anyrepeat_K_LEFT', 'anyrepeat_KP_LEFT' ],
        viewport_rightarrow = [ 'anyrepeat_K_RIGHT', 'anyrepeat_KP_RIGHT' ],
        viewport_uparrow = [ 'anyrepeat_K_UP', 'anyrepeat_KP_UP' ],
        viewport_downarrow = [ 'anyrepeat_K_DOWN', 'anyrepeat_KP_DOWN' ],
        viewport_wheelup = [ 'mousedown_4' ],
        viewport_wheeldown = [ 'mousedown_5' ],
        viewport_drag_start = [ 'mousedown_1' ],
        viewport_drag_end = [ 'mouseup_1' ],
        viewport_pageup = [ 'anyrepeat_K_PAGEUP', 'anyrepeat_KP_PAGEUP'],
        viewport_pagedown = [ 'anyrepeat_K_PAGEDOWN', 'anyrepeat_KP_PAGEDOWN' ],

        # Управление режимом пропуска.
        skip = [ 'anymod_K_LCTRL', 'anymod_K_RCTRL' ],
        stop_skipping = [ ],
        toggle_skip = [ 'K_TAB' ],
        fast_skip = [ '>', 'shift_K_PERIOD' ],

        # Полоса прокрутки (Bar).
        bar_activate = [ 'mousedown_1', 'K_RETURN', 'K_KP_ENTER', 'K_SELECT' ],
        bar_deactivate = [ 'mouseup_1', 'K_RETURN', 'K_KP_ENTER', 'K_SELECT' ],
        bar_left = [ 'anyrepeat_K_LEFT', 'anyrepeat_KP_LEFT' ],
        bar_right = [ 'anyrepeat_K_RIGHT', 'anyrepeat_KP_RIGHT' ],
        bar_up = [ 'anyrepeat_K_UP', 'anyrepeat_KP_UP' ],
        bar_down = [ 'anyrepeat_K_DOWN', 'anyrepeat_KP_DOWN' ],

        # Удаление файла сохранения.
        save_delete = [ 'K_DELETE', 'KP_DELETE' ],

        # Листание страниц меню сохранения/загрузки.
        save_page_prev = ['mousedown_4'],
        save_page_next = ['mousedown_5'],

        # Перетаскиваемые элементы (Draggable).
        drag_activate = [ 'mousedown_1' ],
        drag_deactivate = [ 'mouseup_1' ],

        # Консоль отладки.
        console = [ 'shift_K_o', 'alt_shift_K_o' ],
        console_exit = [ 'K_ESCAPE', 'K_MENU', 'K_PAUSE', 'mouseup_3' ],
        console_older = [ 'anyrepeat_K_UP', 'anyrepeat_KP_UP' ],
        console_newer = [ 'anyrepeat_K_DOWN', 'anyrepeat_KP_DOWN' ],

        # Инструмент режиссёра (Director).
        director = [ 'noshift_K_d' ],

        # Игнорируются (сохранены для обратной совместимости).
        toggle_music = [ ],
        viewport_up = [ ],
        viewport_down = [ ],

        # Профилирование производительности.
        performance = [ 'K_F3' ],
        image_load_log = [ 'K_F4' ],
        profile_once = [ 'K_F8' ],
        memory_profile = [ 'K_F7' ],
    )

Привязки кнопок геймпада работают аналогично, сопоставляя события геймпада со списками событий Ren'Py::

    config.pad_bindings = {
        "pad_leftshoulder_press" : [ "rollback", ],
        "pad_lefttrigger_pos" : [ "rollback", ],
        "pad_back_press" : [ "rollback", ],

        "repeat_pad_leftshoulder_press" : [ "rollback", ],
        "repeat_pad_lefttrigger_pos" : [ "rollback", ],
        "repeat_pad_back_press" : [ "rollback", ],

        "pad_guide_press" : [ "game_menu", ],
        "pad_start_press" : [ "game_menu", ],

        "pad_y_press" : [ "hide_windows", ],
        "pad_x_press" : [ "button_alternate" ],

        "pad_rightshoulder_press" : [ "rollforward", ],
        "repeat_pad_rightshoulder_press" : [ "rollforward", ],

        "pad_righttrigger_pos" : [ "dismiss", "button_select", "bar_activate", "bar_deactivate" ],
        "pad_a_press" : [ "dismiss", "button_select", "bar_activate", "bar_deactivate"],
        "pad_b_press" : [ "game_menu" ],

        "pad_dpleft_press" : [ "focus_left", "bar_left", "viewport_leftarrow" ],
        "pad_leftx_neg" : [ "focus_left", "bar_left", "viewport_leftarrow" ],
        "pad_rightx_neg" : [ "focus_left", "bar_left", "viewport_leftarrow" ],

        "pad_dpright_press" : [ "focus_right", "bar_right", "viewport_rightarrow" ],
        "pad_leftx_pos" : [ "focus_right", "bar_right", "viewport_rightarrow" ],
        "pad_rightx_pos" : [ "focus_right", "bar_right", "viewport_rightarrow" ],

        "pad_dpup_press" : [ "focus_up", "bar_up", "viewport_uparrow" ],
        "pad_lefty_neg" : [ "focus_up", "bar_up", "viewport_uparrow" ],
        "pad_righty_neg" : [ "focus_up", "bar_up", "viewport_uparrow" ],

        "pad_dpdown_press" : [ "focus_down", "bar_down", "viewport_downarrow" ],
        "pad_lefty_pos" : [ "focus_down", "bar_down", "viewport_downarrow" ],
        "pad_righty_pos" : [ "focus_down", "bar_down", "viewport_downarrow" ],

        "repeat_pad_dpleft_press" : [ "focus_left", "bar_left", "viewport_leftarrow" ],
        "repeat_pad_leftx_neg" : [ "focus_left", "bar_left", "viewport_leftarrow" ],
        "repeat_pad_rightx_neg" : [ "focus_left", "bar_left", "viewport_leftarrow" ],

        "repeat_pad_dpright_press" : [ "focus_right", "bar_right", "viewport_rightarrow" ],
        "repeat_pad_leftx_pos" : [ "focus_right", "bar_right", "viewport_rightarrow" ],
        "repeat_pad_rightx_pos" : [ "focus_right", "bar_right", "viewport_rightarrow" ],

        "repeat_pad_dpup_press" : [ "focus_up", "bar_up", "viewport_uparrow" ],
        "repeat_pad_lefty_neg" : [ "focus_up", "bar_up", "viewport_uparrow" ],
        "repeat_pad_righty_neg" : [ "focus_up", "bar_up", "viewport_uparrow" ],

        "repeat_pad_dpdown_press" : [ "focus_down", "bar_down", "viewport_downarrow" ],
        "repeat_pad_lefty_pos" : [ "focus_down", "bar_down", "viewport_downarrow" ],
        "repeat_pad_righty_pos" : [ "focus_down", "bar_down", "viewport_downarrow" ],
    }

События кнопок геймпада имеют формат "pad_*кнопка*_press" или "pad_*кнопка*_release".
События аналоговых осей имеют формат "pad_*ось*_pos", "pad_*ось*_neg" или "pad_*ось*_zero".
При удержании генерируются события автоповтора с префиксом "repeat\_".

Геймпады, требующие специальной инициализации для работы на ПК (например, Nintendo Switch Pro Controller),
по умолчанию отключены через чёрный список :var:`config.controller_blocklist`.

.. include:: inc/keymap
