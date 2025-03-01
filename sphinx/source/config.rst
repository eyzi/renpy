=======================
Configuration Variables
=======================

Configuration variables control the behavior of Ren'Py's implementation,
allowing Ren'Py itself to be customized in a myriad of ways. These range from
the common (such as changing the screen size) to the obscure (adding new
kinds of archive files).

Ren'Py's implementation makes the assumption that, once the GUI system has
initialized, configuration variables will not change. Changing configuration
variables outside of ``init`` blocks can lead to undefined behavior.
Configuration variables are not part of the save data.

Configuration variables are often changed in ``init python`` blocks::

    init python:

        # Use a widescreen resolution.
        config.screen_width = 1024
        config.screen_height = 600


Commonly Used
-------------

.. var:: config.name = ""

    This should be a string giving the name of the game. This is included
    as part of tracebacks and other log files, helping to identify the
    version of the game being used.

.. var:: config.save_directory = "..."

    This is used to generate the directory in which games and
    persistent information are saved. The name generated depends on
    the platform:

    Windows
        %APPDATA%/RenPy/`save_directory`

    Mac OS X
        ~/Library/RenPy/`save_directory`

    Linux/Other
        ~/.renpy/`save_directory`

    Setting this to None creates a "saves" directory underneath the
    game directory. This is not recommended, as it prevents the game
    from being shared between multiple users on a system. It can also
    lead to problems when a game is installed as Administrator, but run
    as a user.

    This must be set with either the define statement, or in a ``python
    early`` block. In either case, this will be run before any other
    statement, and so it should be set to a string, not an expression.

    To locate the save directory, read :var:`config.savedir` instead of
    this variable.

.. var:: config.version = ""

    This should be a string giving the version of the game. This is included
    as part of tracebacks and other log files, helping to identify the
    version of the game being used.

.. var:: config.window = None

    This controls the default method of dialogue window management. If
    not None, this should be one of "show", "hide", or "auto".

    When set to "show", the dialogue window is shown at all times.
    When set to "hide", the dialogue window is hidden when not in a
    say statement or other statement that displays dialogue. When set
    to "auto", the dialogue window is hidden before scene statements,
    and shown again when dialogue is shown.

    This sets the default. Once set, the default can be changed using the
    ``window show``, ``window hide`` and ``window auto`` statements. See
    :ref:`dialogue-window-management` for more information.


Transitions
-----------

These control transitions between various screens.

.. var:: config.adv_nvl_transition = None

    A transition that is used when showing NVL-mode text directly
    after ADV-mode text.

.. var:: config.after_load_transition = None

    A transition that is used after loading, when entering the loaded
    game.

.. var:: config.end_game_transition = None

    The transition that is used to display the main menu after the
    game ends normally, either by invoking return with no place to
    return to, or by calling :func:`renpy.full_restart`.

.. var:: config.end_splash_transition = None

    The transition that is used to display the main menu after the end
    of the splashscreen.

.. var:: config.enter_replay_transition = None

    If not None, a transition that is used when entering a replay.

.. var:: config.enter_transition = None

    If not None, this variable should give a transition that will be
    used when entering the game menu.

.. var:: config.enter_yesno_transition = None

    If not None, a transition that is used when entering the yes/no
    prompt screen.

.. var:: config.exit_replay_transition = None

    If not None, a transition that is used when exiting a replay.

.. var:: config.exit_transition = None

    If not None, this variable should give a transition that will be
    performed when exiting the game menu.

.. var:: config.exit_yesno_transition = None

    If not None, a transition that is used when exiting the yes/no
    prompt screen.

.. var:: config.game_main_transition = None

    The transition that is used to display the main menu after leaving
    the game menu. This is used when the load and preferences screens
    are invoked from the main menu, and it's also used when the user
    picks "Main Menu" from the game menu.

.. var:: config.intra_transition = None

    The transition that is used between screens of the game menu.

.. var:: config.main_game_transition = None

    The transition used when entering the game menu from the main
    menu, as is done when clicking "Load Game" or "Preferences".

.. var:: config.nvl_adv_transition = None

    A transition that is used when showing ADV-mode text directly
    after NVL-mode text.

.. var:: config.say_attribute_transition = None

    If not None, a transition to use when the image is changed by a
    say statement with image attributes.

.. var:: config.say_attribute_transition_callback = ...

    This is a function that return a transition to apply and a layer to
    apply it on

    This should be a function that takes four arguments, the image tag
    being shown, a `mode` parameter, a `set` containing pre-transition tags
    and a `set` containing post-transition tags. Where the value of the
    `mode` parameter is one of:

    * "permanent", for permanent attribute change (one that lasts longer
      than the current say statement).
    * "temporary", for a temporary attribute change (one that is restored
      at the end of the current say statement).
    * "both", for a simultaneous permanent and temporary attribute change
      (one that in part lasts longer than the current say statement, and in
      part is restored at the end of the current say statement).
    * "restore", for when a temporary (or both) change is being restored.

    This should return a 2-component tuple, consisting of:

    * The transition to use, or None if no transition should occur.
    * The layer the transition should be on, either a string or None. This is
      almost always None.

    The default implementation of this returns (config.say_attribute_transition,
    config.say_attribute_transition_layer).

.. var:: config.say_attribute_transition_layer = None

    If not None, this must be a string giving the name of a layer. (Almost always
    "master".) The say attribute is applied to the named layer, and Ren'Py
    will not pause to wait for the transition to occur. This will have the
    effect of transitioning in the attribute as dialogue is shown.

.. var:: config.window_hide_transition = None

    The transition used by the window hide statement when no
    transition has been explicitly specified.

.. var:: config.window_show_transition = None

    The transition used by the window show statement when no
    transition has been explicitly specified.


Preference Defaults
-------------------

Ren'Py has a number of variables that set the default values of
preferences. Please see the section on :ref:`preference variables <preference-variables>`
for more information on how to set defaults for various preferences.

Occasionally Used
-----------------

.. var:: config.adjust_attributes = { }

    If not None, this is a dictionary. When a statement or function that
    contains image attributes executes or is predicted, the tag is
    looked up in this dictionary. If it is not found, the None key
    is looked up in this dictionary.

    If either is found, they're expected to be a function. The function
    is given an image name, a tuple consisting of the tag and any
    attributes. It should return an adjusted tuple, which contains
    and a potential new set of attributes.

    As this function may be called during prediction, it must not rely
    on any state.

.. var:: config.after_load_callbacks = [ ... ]

    A list of functions that are called (with no arguments) when a load
    occurs.

.. var:: config.after_replay_callback = None

    If not None, a function that is called with no arguments after a
    replay completes.

.. var:: config.allow_underfull_grids = False

    If True, Ren'Py will not require grids to be full in order to display.

.. var:: config.audio_filename_callback = None

    If not None, this is a function that is called with an audio filename,
    and is expected to return a second audio filename, the latter of which
    will be played.

    This is intended for use when an a games has audio file formats changed,
    but it's not destired to update the game script.

.. var:: config.auto_channels = { "audio" : ( "sfx", "", ""  ) }

    This is used to define automatic audio channels. It's a map the
    channel name to a tuple containing 3 components:

    * The mixer the channel uses.
    * A prefix that is given to files played on the channel.
    * A suffix that is given to files played on the channel.

.. var:: config.auto_movie_channel = True

    If True, and the `play` argument is give to :func:`Movie`, an
    audio channel name is automatically generated for each movie.

    :var:`config.single_movie_channel` takes precendece over this
    variable.

.. var:: config.auto_load = None

    If not None, the name of a save file to automatically load when
    Ren'Py starts up. This is intended for developer use, rather than
    for end users. Setting this to "1" will automatically load the
    game in save slot 1.

.. var:: config.auto_voice = None

    This may be a string, a function, or None. If None, auto-voice is
    disabled.

    If a string, this is formatted with the ``id`` variable bound to the
    identifier of the current line of dialogue. If this gives an existing
    file, that file is played as voice audio.

    If a function, the function is called with a single argument, the
    identifier of the current line of dialogue. The function is expected to
    return a string. If this gives an existing file, that file is played as
    voice audio.

    See :ref:`Automatic Voice <automatic-voice>` for more details.

.. var:: config.autosave_slots = 10

    The number of slots used by autosaves.

.. var:: config.cache_surfaces = False

    If True, the underlying data of an image is stored in RAM, allowing
    image manipulators to be applied to that image without reloading it
    from disk. If False, the data is dropped from the cache, but kept as
    a texture in video memory, reducing RAM usage.

.. var:: config.character_id_prefixes = [ ]

    This specifies a list of style property prefixes that can be given
    to a :func:`Character`. When a style prefixed with one of the given
    prefix is given, it is applied to the displayable with that prefix
    as its ID.

    For example, the default GUI adds "namebox" to this. When a Character
    is given the `namebox_background` property, it sets :propref:`background`
    on the displayable in the say screen with the id "namebox".

.. var:: config.conditionswitch_predict_all = False

    The default value of the predict_all argument for :func:`ConditionSwitch`
    and :func:`ShowingSwitch`, which determines if all possible displayables
    are shown.

.. var:: config.context_callback = None

    This is a callback that is called when Ren'Py enters a new context,
    such as a menu context.

.. var:: config.context_copy_remove_screens = [ 'notify' ]

    Contains a list of screens that are removed when a context is copied
    for rollback or saving.

.. var:: config.context_fadein_music = 0

    The amount of time in seconds Ren'Py spends fading in music when the music is
    played due to a context change. (Usually, when the game is loaded.)

.. var:: config.context_fadeout_music = 0

    The amount of time in seconds Ren'Py spends fading out music when the music is
    played due to a context change. (Usually, when the game is loaded.)

.. var:: config.debug_image_cache = False

    If True, Ren'Py will write information about the :ref:`image cache <images>`
    to image_cache.txt.

.. var:: config.debug_prediction = False

    If True, Ren'Py will will write information about and errors that
    occur during prediction (of execution flow, images, and screens) to
    log.txt and the console.

.. var:: config.debug_sound = False

    Enables debugging of sound functionality. This disables the
    suppression of errors when generating sound. However, if a sound
    card is missing or flawed, then such errors are normal, and
    enabling this may prevent Ren'Py from functioning normally. This
    should always be False in a released game.

.. var:: config.debug_text_overflow = False

    When true, Ren'Py will log text overflows to text_overflow.txt. A text
    overflow occurs when a :class:`Text` displayable renders to a size
    larger than that allocated to it. By setting this to True and setting
    the :propref:`xmaximum` and :propref:`ymaximum` style properties of the dialogue
    window to the window size, this can be used to report cases where the
    dialogue is too large for its window.

.. var:: config.default_attribute_callbacks = { }

    When a statement or function that contains image attributes executes or is
    predicted, and the tag is not currently being shown, it's looked up in this
    dictionary. If it is not found, the None key is looked up instead.

    If either is found, they're expected to be a function. The function is
    given an image name, a tuple consisting of the tag and any attributes. It
    should return an iterable which contains any additional attributes to be
    applied when an image is first shown.

    The results of the function are treated as additive-only, and any explicit
    conflicting or negative attributes will still take precedence.

    As this function may be called during prediction, it must not rely on any
    state.

.. var:: config.default_tag_layer = "master"

    The layer an image is shown on if its tag is not found in :var:`config.tag_layer`.

.. var:: config.default_transform = ...

    When a displayable is shown using the show or scene statements,
    the transform properties are taken from this transform and used to
    initialize the values of the displayable's transform.

    The default transform is :var:`center`.

.. var:: config.defer_styles = False

    When true, the execution of style statements is deferred until after
    all ``translate python`` blocks have executed. This lets a ``translate
    python`` block update variables that are then used in style (not
    translate style) statements.

    While this defaults to False, it's set to True when :func:`gui.init`
    is called.

.. var:: config.developer = "auto"

    If set to True, developer mode is enabled. Developer mode gives
    access to the shift+D developer menu, shift+R reloading, and
    various other features that are not intended for end users.

    This can be True, False, or "auto". If "auto", Ren'Py will
    detect if the game has been packaged into a distribution, and
    set config.developer as appropriate.

.. var:: config.disable_input = False

    When true, :func:`renpy.input` terminates immediately and returns its
    `default` argument.

.. var:: config.displayable_prefix = { }

    See :ref:`Displayable prefixes <displayable-prefix>`.

.. var:: config.emphasize_audio_channels = [ 'voice' ]

    A list of strings giving audio channel names.

    If the "emphasize audio" preference is enabled, when one of the audio
    channels listed starts playing a sound, all channels that are not
    listed in this variable have their secondary audio volume reduced
    to :var:`config.emphasize_audio_volume` over :var:`config.emphasize_audio_time`
    seconds.

    When no channels listed in this variable are playing audio, all channels
    that are not listed have their secondary audio volume raised to 1.0 over
    :var:`config.emphasize_audio_time` seconds.

    For example, setting this to ``[ 'voice' ]]`` will lower the volume of all
    non-voice channels when a voice is played.

.. var:: config.emphasize_audio_time = 0.5

    See above.

.. var:: config.emphasize_audio_volume = 0.5

    See above.

.. var:: config.empty_window = ...

    This is called when _window is True, and no window has been shown
    on the screen. (That is, no call to :func:`renpy.shown_window` has
    occurred.) It's expected to show an empty window on the screen, and
    return without causing an interaction.

    The default implementation of this uses the narrator character to
    display a blank line without interacting.

.. var:: config.enable_language_autodetect = False

    If true, Ren'Py will attempt to determine the name of the language
    to use based on the locale of the player's system. If successful,
    this language will be used as the default language.

.. var:: config.enter_sound = None

    If not None, this is a sound file that is played when entering the
    game menu.

.. var:: config.exit_sound = None

    If not None, this is a sound file that is played when exiting the
    game menu.

.. var:: config.fix_rollback_without_choice = False

    This option determines how the built in menus or imagemaps behave
    during fixed rollback. The default value is False, which means that
    menu only the previously selected option remains clickable. If set
    to True, the selected option is marked but no options are clickable.
    The user can progress forward through the rollback buffer by
    clicking.

.. var:: config.font_replacement_map = { }

    This is a map from (font, bold, italics) to (font, bold, italics),
    used to replace a font with one that's specialized as having bold
    and/or italics. For example, if you wanted to have everything
    using an italic version of "Vera.ttf" use "VeraIt.ttf" instead,
    you could write::

        init python:
            config.font_replacement_map["Vera.ttf", False, True] = ("VeraIt.ttf", False, False)

    Please note that these mappings only apply to specific variants of
    a font. In this case, requests for a bold italic version of vera
    will get a bold italic version of vera, rather than a bold version
    of the italic vera.

.. var:: config.game_menu_music = None

    If not None, a music file to play when at the game menu.

.. var:: config.gl_clear_color = "#000"

    The color that the window is cleared to before images are drawn.
    This is mainly seen as the color of the letterbox or pillarbox
    edges drawn when aspect ratio of the window or monitor in fullscreen
    mode) does not match the aspect ratio of the game.

.. var:: config.gl_lod_bias = -0.5

    The default value of the :ref:`u_lod_bias <u-lod-bias>` uniform,
    which controls the mipmap level Ren'Py uses.

.. var:: config.gl_test_image = "black"

    The name of the image that is used when running the OpenGL
    performance test. This image will be shown for 5 frames or .25
    seconds, on startup. It will then be automatically hidden.

.. var:: config.has_autosave = True

    If true, the game will autosave. If false, no autosaving will
    occur.

.. var:: config.history_callbacks = [ ... ]

    This contains a list of callbacks that are called before Ren'Py adds
    a new object to _history_list. The callbacks are called with the
    new HistoryEntry object as the first argument, and can add new fields
    to that object.

    Ren'Py uses history callbacks internally, so creators should append
    their own callbacks to this  list, rather than replacing it entirely.

.. var:: config.history_length = None

    The number of entries of dialogue history Ren'Py keeps. This is
    set to 250 by the default gui.

.. var:: config.hw_video = False

    If true, hardware video playback will be used on mobile platforms. This
    may be faster, but only some formats are supported and only fullscreen video
    is available. If false, software playback will be used.

.. var:: config.hyperlink_handlers = { ... }

    A dictionary mapping a hyperlink protocol to the handler for that
    protocol. A handler is a function that takes the value (everything after
    the :) and performs some action. If a value is returned, the interaction
    ends. Otherwise, the click is ignored and the interaction continues.

.. var:: config.hyperlink_protocol = "call_in_new_context"

    The protocol that is used for hyperlinks that do not have a protocol
    assigned to them. See :ref:`the a text tag <a-tag>` for a description
    as to what the possible protocols mean.

.. var:: config.image_cache_size = None

    If not None, this is used to set the size of the :ref:`image cache <images>`, as a
    multiple of the screen size. This number is multiplied by the size of
    the screen, in pixels, to get the size of the image cache in pixels.

    If set too large, this can waste memory. If set too small, images
    can be repeatedly loaded, hurting performance.

.. var:: config.image_cache_size_mb = 300

    This is used to set the size of the :ref:`image cache <images>`, in
    megabytes. If :var:`config.cache_surfaces` is False, an image takes
    4 bytes per pixel, otherwise it takes 8 bytes per pixel.

    If set too large, this can waste memory. If set too small, images
    can be repeatedly loaded, hurting performance. If not none,
    :var:`config.image_cache_size` is used instead of this variable.

.. var:: config.input_caret_blink = 1.0

    If not False, sets the blinking period of the default caret, in seconds.

.. var:: config.language = None

    If not None, this should be a string giving the default language
    that the game is translated into by the translation framework.

.. var:: config.load_failed_label = None

    If a string, this is a label that is jumped to when a load fails because
    the script has changed so much that Ren'Py can't recover.
    Before performing the load, Ren'Py will revert to the start of the
    last statement, then it will clear the call stack.

    This may also be a function. If it is, the function is called with
    no arguments, and is expected to return a string giving the label.

.. var:: config.locale_to_language_function = ...

    A function that determines the language the game should use,
    based on the the user's locale.
    It takes 2 string arguments that give the ISO code of the locale
    and the ISO code of the region.

    It should return a string giving the name of a translation to use, or
    None to use the default translation.

.. var:: config.main_menu_music = None

    If not None, a music file to play when at the main menu.

.. var:: config.main_menu_music_fadein = 0.0

    The number of seconds to take to fade in :var:`config.main_menu_music`.

.. var:: config.menu_arguments_callback = None

    If not None, this should be a function that takes positional and/or
    keyword arguments. It's called whenever a menu statement runs,
    with the arguments to that menu statement.

    This should return a pair, containing a tuple of positional arguments
    (almost always empty), and a dictionary of keyword arguments.

.. var:: config.menu_clear_layers = []

    A list of layer names (as strings) that are cleared when entering
    the game menu.

.. var:: config.menu_include_disabled = False

    When this variable is set, choices disables with the if statement are
    included as disabled buttons.

.. var:: config.menu_window_subtitle = ""

    The :var:`_window_subtitle` variable is set to this value when entering
    the main or game menus.

.. var:: config.minimum_presplash_time = 0.0

    The minimum amount of time, in seconds, a presplash, Android presplash,
    or iOS LaunchImage is displayed for. If Ren'Py initializes before this
    amount of time has been reached, it will sleep to ensure the image is
    shown for at least this amount of time. The image may be shown longer
    if Ren'Py takes longer to start up.

.. var:: config.missing_background = "black"

    This is the background that is used when :var:`config.developer` is True
    and an undefined image is used in a :ref:`scene statement
    <scene-statement>`. This should be an image name (a string), not a
    displayable.

.. var:: config.mode_callbacks = [ ... ]

    A list of callbacks called when entering a mode. For more documentation,
    see the section on :ref:`Modes`.

    The default value includes a callback that implements :var:`config.adv_nvl_transition`
    and :var:`config.nvl_adv_transition`.

.. var:: config.mouse = None

    This variable controls the use of user-defined mouse cursors. If
    None, the system mouse is used, which is usually a black-and-white
    mouse cursor.

    Otherwise, this should be a dictionary giving the
    mouse animations for various mouse types. Keys used by the default
    library include "default", "say", "with", "menu", "prompt",
    "imagemap", "pause", "mainmenu", and "gamemenu". The "default" key
    should always be present, as it is used when a more specific key
    is absent.

    Each value in the dictionary should be a list of (`image`,
    `xoffset`, `yoffset`) tuples, representing frames.

    `image`
        The mouse cursor image. The maximum size for this image
        varies based on the player's hardware. 32x32 is guaranteed
        to work everywhere, while 64x64 works on most hardware. Larger
        images may not work.

    `xoffset`
        The offset of the hotspot pixel from the left side of the
        cursor.

    `yoffset`
        The offset of the hotspot pixel from the top of the cursor.

    The frames are played back at 20Hz, and the animation loops after
    all frames have been shown.

.. var:: config.mouse_displayable = None

    If not None, this should either be a displayable, or a callable that
    returns a displayable. The callable may return None, in which case
    Ren'Py proceeds if the displayable is None.

    If a displayable is given, the mouse cursor is hidden, and the
    displayable is shown above anything else. This displayable is
    responsible for positioning and drawing a sythetic mouse
    cursor, and so should probably be a :func:`MouseDisplayable`
    or something very similar.

.. var:: config.narrator_menu = False

    (This is set to True by the default screens.rpy file.) If true,
    then narration inside a menu is displayed using the narrator
    character. Otherwise, narration is displayed as captions
    within the menu itself.

.. var:: config.nearest_neighbor = False

    Uses nearest-neighbor filtering by default, to support pixel art or
    melting players' eyes.

.. var:: config.notify = ...

    This is called by :func:`renpy.notify` or :func:`Notify` with a
    single  `message` argument, to display the notification. The default
    implementation is :func:`renpy.display_notify`. This is intended
    to allow creators to intercept notifications.

.. var:: config.optimize_texture_bounds = False

    When True, Ren'Py will scan images to find the bounding box of the
    non-transparent pixels, and only load those pixels into a texture.

.. var:: config.overlay_functions = [ ]

    A list of functions. When called, each function is expected to
    use ui functions to add displayables to the overlay layer.

.. var:: config.overlay_screens = [ ... ]

    A list of screens that are displayed when the overlay is enabled,
    and hidden when the overlay is suppressed. (The screens are shown
    on the screens layer, not the overlay layer.)

.. var:: config.pause_after_rollback = False

    If False, the default, rolling back will skip any pauses (timed or
    not) and stop only at other interactions such as dialogues, menus...
    If True, renpy will include timeless pauses to the valid places a
    rollback can take the user.

.. var:: config.preload_fonts = [ ]

    A list of the names of TrueType and OpenType fonts that Ren'Py should
    load when starting up. Including the name of a font here can prevent
    Ren'Py from pausing when introducing a new typeface.

.. var:: config.python_callbacks = [ ]

    A list of functions. The functions in this list are called, without
    any arguments, whenever a Python block is run outside of the init
    phase.

    One possible use of this would be to have a function limit a variable
    to within a range each time it is adjusted.

    The functions may be called while Ren'Py is starting up, before the start
    of the game proper, and  potentially before the variables the
    function depends on are initialized. The functions are required to deal
    with this, perhaps by using ``hasattr(store, 'varname')`` to check if
    a variable is defined.

.. var:: config.quicksave_slots = 10

    The number of slots used by quicksaves.

.. var:: config.quit_action = ...

    The action that is called when the user clicks the quit button on
    a window. The default action prompts the user to see if he wants
    to quit the game.

.. var:: config.reload_modules = [ ]

    A list of strings giving the names of python modules that should be
    reloaded along with the game. Any submodules of these modules
    will also be reloaded.

.. var:: config.replace_text = None

    If not None, a function that is called with a single argument, a text to
    be displayed to the user. The function can return the same text it was
    passed, or a replacement text that will be displayed instead.

    The function is called after substitutions have been performed and after
    the text has been split on tags, so its argument contains nothing but
    actual text. All displayed text passes through the function: not only
    dialogue text, but also user interface text.

    This can be used to replace specific ASCII sequences with corresponding
    Unicode characters, as demonstrated by the following::

        def replace_text(s):
            s = s.replace("'", u'\u2019') # apostrophe
            s = s.replace('--', u'\u2014') # em dash
            s = s.replace('...', u'\u2026') # ellipsis
            return s
        config.replace_text = replace_text

.. var:: config.replay_scope = { "_game_menu_screen" : "preferences" }

    A dictionary mapping variables in the default store to the values
    the variables will be given when entering a replay.

.. var:: config.return_not_found_label = None

    If not None, a label that is jumped to when a return site is not found.
    The call stack is cleared before this jump occurs.

.. var:: config.save_json_callbacks = [ ]

    A list of callback functions that are used to create the json object
    that is stored with each save and marked accessible through :func:`FileJson`
    and :func:`renpy.slot_json`.

    Each callback is called with a Python dictionary that will eventually be
    saved. Callbacks should modify that dictionary by adding JSON-compatible
    Python types, such as numbers, strings, lists, and dicts. The dictionary
    at the end of the last callback is then saved as part of the save slot.

    The dictionary passed to the callbacks may have already have keys
    beginning with an underscore ``_``. These keys are used by Ren'Py,
    and should not be changed.

.. var:: config.say_arguments_callback = None

    If not None, this should be a function that takes the speaking character,
    followed by positional and keyword arguments. It's called whenever a
    say statement occurs with the arguments to that say statement. This
    always includes an interact argument, and can include others provided
    in the say statement.

    This should return a pair, containing a tuple of positional arguments
    (almost always empty), and a dictionary of keyword arguments (almost
    always with at least interact in it).

    For example::

        def say_arguments_callback(who, interact=True, color="#fff"):
            return (), { "interact" : interact, "what_color" : color }

        config.say_arguments_callback = say_arguments_callback

.. var:: config.screen_height = 600

    The height of the screen. Usually set by :func:`gui.init` to
    a much larger size.

.. var:: config.screen_width = 800

    The width of the screen. Usually set by :func:`gui.init` to a much
    larger size.

.. var:: config.single_movie_channel = None

    If not None, and the `play` argument is give to :func:`Movie`,
    this is the name used for the channel the movie is played on.
    This should not be "movie", as that name is reserved for
    Ren'Py's internal use.

.. var:: config.skip_sounds = False

    If False, non-looping audio will not be played when Ren'Py is
    skipping.

.. var:: config.speaking_attribute = None

    If not None, this should be a string giving the name of an image
    attribute. The image attribute is added to the image when the
    character's image tag when the character is speaking, and removed
    when the character stops.

    This is applied to the image on the default layer for the tag,
    which can be set using :var:`config.tag_layer`.

.. var:: config.tag_layer = { }

    A dictionary mapping image tag strings to layer name strings. When
    an image is shown without a specific layer name, the image's tag is
    looked up in this dictionary to get the layer to show it on. If the
    tag is not found here, :var:`config.default_tag_layer` is used.

.. var:: config.tag_transform = { }

    A dictionary mapping image tag strings to transforms or lists of
    transforms. When an image is newly-shown without an at clause,
    the image's tag is looked up in this dictionary to find a transform
    or list of transforms to use.

.. var:: config.tag_zorder = { }

    A dictionary mapping image tag strings to zorders. When an image is
    newly-shown without a zorder clause, the image's tag is looked up
    in this dictionary to find a zorder to use. If no zorder is found,
    0 is used.

.. var:: config.thumbnail_height = 75

    The height of the thumbnails that are taken when the game is
    saved. These thumbnails are shown when the game is loaded. Please
    note that the thumbnail is shown at the size it was taken at,
    rather than the value of this setting when the thumbnail is shown
    to the user.

    This is changed by the default GUI.

.. var:: config.thumbnail_width = 100

    The width of the thumbnails that are taken when the game is
    saved. These thumbnails are shown when the game is loaded. Please
    note that the thumbnail is shown at the size it was taken at,
    rather than the value of this setting when the thumbnail is shown
    to the user.

    This is changed by the default GUI.

.. var:: config.tts_voice = None

    If not None, a string giving a non-default voice that is used to
    play back text-to-speech for self voicing. The possible choices are
    platform specific, and so this should be set in a platform-specific
    manner. (It may make sense to change this in translations, as well.)

.. var:: config.window_auto_hide = [ 'scene', 'call screen', 'menu', "say-centered" ]

    A list of statements that cause ``window auto`` to hide the empty
    dialogue window.

.. var:: config.window_auto_show = [ 'say', 'menu-with-caption' ]

    A list of statements that cause ``window auto`` to show the empty
    dialogue window.

.. var:: config.window_icon = None

    If not None, this is expected to be the filename of an image
    giving an icon that is used for the game's main window. This does
    not set the icon used by windows executables and mac apps, as
    those are controlled by :ref:`special-files`.

.. var:: config.window_overlay_functions = []

    A list of overlay functions that are only called when the window
    is shown.

.. var:: config.window_title = None

    The static portion of the title of the window containing the
    Ren'Py game. :var:`_window_subtitle` is appended to this to get
    the full title of the window.

    If None, the default, this defaults to the value of :var:`config.name`.



Rarely or Internally Used
-------------------------

.. var:: config.adjust_view_size = None

    If not None, this should be a function taking two arguments, the width
    and height of the physical window. It is expected to return a tuple
    giving the width and height of the OpenGL viewport, the portion of the
    screen that Ren'Py will draw pictures to.

    This can be used to configure Ren'Py to only allow certain sizes of
    screen. For example, the following allows only integer multiples
    of the original screen size::

        init python:

            def force_integer_multiplier(width, height):
                multiplier = min(width / config.screen_width, height / config.screen_height)
                multiplier = max(int(multiplier), 1)
                return (multiplier * config.screen_width, multiplier * config.screen_height)

            config.adjust_view_size = force_integer_multiplier

.. var:: config.afm_bonus = 25

    The number of bonus characters added to every string when
    auto-forward mode is in effect.

.. var:: config.afm_callback = None

    If not None, a Python function that is called to determine if it
    is safe to auto-forward. The intent is that this can be used by a
    voice system to disable auto-forwarding when a voice is playing.

.. var:: config.afm_characters = 250

    The number of characters in a string it takes to cause the amount
    of time specified in the auto forward mode preference to be
    delayed before auto-forward mode takes effect.

.. var:: config.afm_voice_delay = .5

    The number of seconds after a voice file finishes playing
    before AFM can advance text.

.. var:: config.all_character_callbacks = [ ]

    A list of callbacks that are called by all characters. This list
    is prepended to the list of character-specific callbacks.

.. var:: config.allow_skipping = True

    If set to False, the user is not able to skip over the text of the
    game. See :var:`_skipping`.

.. var:: config.allow_screensaver = True

    If True, the screensaver may activite while the game is running. If
    False, the screensaver is disabled.

.. var:: config.archives = [ ]

    A list of archive files that will be searched for images and other
    data. The entries in this should consist of strings giving the
    base names of archive files, without the .rpa extension.

    The archives are searched in the order they are found in this list.
    A file is taken from the first archive it is found in.

    At startup, Ren'Py will automatically populate this variable with
    the names of all archives found in the game directory, sorted in
    reverse ascii order. For example, if Ren'Py finds the files
    data.rpa, patch01.rpa, and patch02.rpa, this variable will be
    populated with ``['patch02', 'patch01', 'data']``.

.. var:: config.at_exit_callbacks = [ ]

    A list of callbacks that are called when Ren'Py quits or restarts
    the game. These callbacks should not interact with the user.

.. var:: config.auto_choice_delay = None

    If not None, this variable gives a number of seconds that Ren'Py
    will pause at an in-game menu before picking a random choice from
    that menu. We'd expect this variable to always be set to None in
    released games, but setting it to a number will allow for
    automated demonstrations of games without much human interaction.

.. var:: config.autoreload = True

    If True, Shift+R will toggle automatic reloading. When automatic
    reloading is enabled, Ren'Py will reload the game whenever a used
    file is modified.

    If False, Ren'Py will reload the game once per press of Shift+R.

.. var:: config.autosave_frequency = 200

    Roughly, the number of interactions that will occur before an
    autosave occurs. To disable autosaving, set :var:`config.has_autosave` to
    False, don't change this variable.

.. var:: config.autosave_on_choice = True

    If True, Ren'Py will autosave upon encountering an in-game choice.
    (When :func:`renpy.choice_for_skipping` is called.)

.. var:: config.autosave_on_quit = True

    If True, Ren'Py will attempt to autosave when the user attempts to quit,
    return to the main menu, or load a game over the existing game. (To
    save time, the autosave occurs while the user is being prompted to confirm
    his or her decision.)

.. var:: config.autosave_on_input = True

    If True, Ren'Py will autosave when the user inputs text.
    (When :func:`renpy.input` is called.)

.. var:: config.character_callback = None

    The default value of the callback parameter of Character.

.. var:: config.choice_layer = "screens"

    The layer the choice screen (used by the menu statement) is shown on.

.. var:: config.clear_layers = []

    A list of names of layers to clear when entering the main and game
    menus.

.. var:: config.context_clear_layers = [ 'screens' ]

    A list of layers that are cleared when entering a new context.

.. var:: config.controller_blocklist = [ ... ]

    A list of strings, where each string is matched against the GUID
    of a game controller. These strings are mached as a prefix to the
    controller GUID (which cand be found in log.txt), and if matched,
    prevent the controller from being initialized.

.. var:: config.exception_handler = None

    If not None, this should be a function that takes three arguments:

    * A string giving the text of a traceback, abbreviated so that it only includes
      creator-written files.
    * The full text of the traceback, including both creator-written and Ren'Py
      files.
    * The path to a file containing a traceback method.

    This function can present the error to a user in any way fit. If it returns True,
    the exception is ignored and control is transferred to the next statement. If it
    returns False, the built-in exception handler is use. This function may also call
    :func:`renpy.jump` to transfer control to some other label.

.. var:: config.fade_music = 0.0

    This is the amount of time in seconds to spend fading the old
    track out before a new music track starts. This should probably be
    fairly short, so the wrong music doesn't play for too long.

.. var:: config.fast_skipping = False

    Set this to True to allow fast skipping outside of developer mode.

.. var:: config.file_open_callback = None

    If not None, this is a function that is called with the file name
    when a file needs to be opened. It should return a file-like
    object, or None to load the file using the usual Ren'Py
    mechanisms. Your file-like object must implement at least the
    read, seek, tell, and close methods.

    One may want to also define a :var:`config.loadable_callback` that
    matches this.

.. var:: config.focus_crossrange_penalty = 1024

    This is the amount of penalty to apply to moves perpendicular to
    the selected direction of motion, when moving focus with the
    keyboard.

.. var:: config.gl_resize = True

    Determines if the user is allowed to resize an OpenGL-drawn window.

.. var:: config.hard_rollback_limit = 100

    This is the number of steps that Ren'Py will let the user
    interactively rollback. Set this to 0 to disable rollback
    entirely, although we don't recommend that, as rollback is useful
    to let the user see text he skipped by mistake.

.. var:: config.help_screen = "help"

    The name of the screen shown by the :func:`Help` action, or by pressing
    f1 on the keyboard.

.. var:: config.hide = renpy.hide

    A function that is called when the :ref:`hide statement <hide-statement>`
    is executed. This should take the same arguments as renpy.hide.

.. var:: config.imagemap_auto_function = ...

    A function that expands the `auto` property of a screen language
    :ref:`imagebutton <sl-imagebutton>` or :ref:`imagemap <sl-imagemap>`
    statement into a displayable. It takes the value of the auto property,
    and the desired image, one of: "insensitive", "idle", "hover",
    "selected_idle", "selected_hover", or "ground". It should return a
    displayable or None.

    The default implementation formats the `auto` property with
    the desired image, and then checks if the computed filename exists.

.. var:: config.keep_side_render_order = True

    If True, the order of substrings in the Side positions will be
    determine the order of children render.

.. var:: config.implicit_with_none = True

    If True, then by default the equivalent of a :ref:`with None <with-none>`
    statement will be performed after interactions caused by dialogue, menus
    input, and imagemaps. This ensures that old screens will not show
    up in transitions.

.. var:: config.interact_callbacks = ...

    A list of functions that are called (without any arguments) when
    an interaction is started or restarted.

.. var:: config.keep_running_transform = True

    If True, showing an image without supplying a transform or ATL
    block will cause the image to continue the previous transform
    an image with that tag was using, if any. If False, the transform
    is stopped.

.. var:: config.keymap = dict(...)

    This variable contains a keymap giving the keys and mouse buttons
    assigned to each possible operation. Please see the section on
    Keymaps for more information.

.. var:: config.label_callback = None

    If not None, this is a function that is called whenever a label is
    reached. It is called with two parameters. The first is the name
    of the label. The second is True if the label was reached through
    jumping, calling, or creating a new context, and False
    otherwise.

.. var:: config.label_overrides = { }

    This variable gives a way of causing jumps and calls of labels in
    Ren'Py script to be redirected to other labels. For example, if you
    add a mapping from "start" to "mystart", all jumps and calls to
    "start" will go to "mystart" instead.

.. var:: config.layer_clipping = { }

    Controls layer clipping. This is a map from layer names to (x, y,
    height, width) tuples, where x and y are the coordinates of the
    upper-left corner of the layer, with height and width giving the
    layer size.

    If a layer is not mentioned in config.layer_clipping, then it is
    assumed to take up the full screen.

.. var:: config.layers = [ 'master', 'transient', 'screens', 'overlay' ]

    This variable gives a list of all of the layers that Ren'Py knows
    about, in the order that they will be displayed to the
    screen. (The lowest layer is the first entry in the list.) Ren'Py
    uses the layers "master", "transient", "screens", and "overlay"
    internally, so they should always be in this list.

    The :func:`renpy.add_layer` can add layers to this variable without
    needing to know the original contents.

.. var:: config.lint_hooks = ...

    This is a list of functions that are called, with no arguments,
    when lint is run. The functions are expected to check the script
    data for errors, and print any they find to standard output (using
    the Python ``print`` statement is fine in this case).

.. var:: config.load_before_transition = True

    If True, the start of an interaction will be delayed until all
    images used by that interaction have loaded. (Yeah, it's a lousy
    name.)

.. var:: config.loadable_callback = None

    When not None, a function that's called with a filename. It should return
    True if the file is loadable, and False if not. This can be used with
    :var:`config.file_open_callback` or :var:`config.missing_image_callback`.

.. var:: config.log_width = 78

    The width of lines logged when :var:`config.log` is used.

.. var:: config.longpress_duration = 0.5

    The amount of time the player must press the screen for a longpress
    to be recognized on a touch device.

.. var:: config.longpress_radius = 15

    The number of pixels the touch must remain within for a press to be
    recognized as a longpress.

.. var:: config.longpress_vibrate = .1

    The amount of time the device will vibrate for after a longpress.

.. var:: config.log = None

    If not None, this is expected to be a filename. Much of the text
    shown to the user by :ref:`say <say-statement>` or :ref:`menu
    <menu-statement>` statements will be logged to this file.

.. var:: config.mipmap_dissolves = False

    The default value of the mipmap argument to :func:`Dissolve`,
    :func:`ImageDissolve`, :func:`AlphaDissolve`, and :func:`AlphaMask`.

.. var:: config.mipmap_movies = False

    The default value of the mipmap argument to :func:`Movie`.

.. var:: config.mipmap_text = False

    The default value of the mipmap argument to :func:`Text`, including
    text used in screen statements.

.. var:: config.missing_image_callback = None

    If not None, this function is called when an attempt to load an
    image fails. It may return None, or it may return an image
    manipulator. If an image manipulator is returned, that image
    manipulator is loaded in the place of the missing image.

    One may want to also define a :var:`config.loadable_callback`,
    especially if this is used with a :func:`DynamicImage`.

.. var:: config.missing_label_callback = None

    If not None, this function is called when Ren'Py attempts to access
    a label that does not exist in the game. The callback should take a
    single parameter, the name of the missing label. It should return the
    name of a label to use as a replacement for the missing label, or None
    to cause Ren'Py to raise an exception.

.. var:: config.mouse_focus_clickthrough = False

    If true, clicks that cause a window to be focused will be processed
    normally. If false, such clicks will be ignored.

.. var:: config.mouse_hide_time = 30

    The mouse is hidden after this number of seconds has elapsed
    without any mouse input. This should be set to longer than the
    expected time it will take to read a single screen, so mouse users
    will not experience the mouse appearing then disappearing between
    clicks.

    If None, the mouse will never be hidden.

.. var:: config.movie_mixer = "music"

    The mixer that is used when a :func:`Movie` automatically defines
    a channel for video playback.

.. var:: config.new_translate_order = True

    Enables the new order of style and translate statements introduced in
    :ref:`Ren'Py 6.99.11 <renpy-6.99.11>`.

.. var:: config.new_substitutions = True

    If True, Ren'Py will apply new-style (square-bracket)
    substitutions to all text displayed.

.. var:: config.old_substitutions = True

    If True, Ren'Py will apply old-style (percent) substitutions to
    text displayed by the :ref:`say <say-statement>` and :ref:`menu
    <menu-statement>` statements.

.. var:: config.overlay_during_with = True

    True if we want overlays to be shown during :ref:`with statements
    <with-statement>`, or False if we'd prefer that they be hidden during
    the with statements.

.. var:: config.overlay_layers = [ 'overlay' ]

    This is a list of all of the overlay layers. Overlay layers are
    cleared before the overlay functions are called. "overlay" should
    always be in this list.

.. var:: config.pause_with_transition = False

    If false, :func:`renpy.pause` is always, used by the ``pause`` statement.
    If true, when given a delay, ``pause`` is equivalent to ``with Pause(...)``.

.. var:: config.per_frame_screens = [ ... ]

    This is a list of strings giving the name of screens that are updated
    once per frame, rather than once per interaction. Ren'Py uses this internally,
    so if you add a screen, append the name rather than replacing the list in
    its entirety.

.. var:: config.periodic_callback = None

    If not None, this should be a function. The function is called,
    with no arguments, at around 20Hz.

.. var:: config.play_channel = "audio"

    The name of the audio channel used by :func:`renpy.play`,
    :propref:`hover_sound`, and :propref:`activate_sound`.

.. var:: config.predict_statements = 32

    This is the number of statements, including the current one, to
    consider when doing predictive image loading. A breadth-first
    search from the current statement is performed until this number
    of statements is considered, and any image referenced in those
    statements is potentially predictively loaded. Setting this to 0
    will disable predictive loading of images.

.. var:: config.profile = False

    If set to True, some profiling information will be output to
    stdout.

.. var:: config.profile_init = 0.25

    ``init`` and ``init python`` blocks taking longer than this amount of time
    to run are reported to log file.

.. var:: config.quit_on_mobile_background = False

    If True, the mobile app will quit when it loses focus, rather than
    saving and restoring its state. (See also :var:`config.save_on_mobile_background`,
    which controls this behavior.)

.. var:: config.rollback_enabled = True

    Should the user be allowed to rollback the game? If set to False,
    the user cannot interactively rollback.

.. var:: config.rollback_length = 128

    When there are more than this many statements in the rollback log,
    Ren'Py will consider trimming the log. This also covers how many
    steps Ren'Py will rollback when trying to load a save when the script
    has changed.

    Decreasing this below the default value may cause Ren'Py to become
    unstable.

.. var:: config.rollback_side_size = .2

    If the rollback side is enabled, the fraction of the screen on the
    rollback side that, when clicked or touched, causes a rollback to
    occur.

.. var:: config.say_allow_dismiss = None

    If not None, this should be a function. The function is called
    with no arguments when the user attempts to dismiss a :ref:`say
    statement <say-statement>`. If this function returns True, the
    dismissal is allowed, otherwise it is ignored.

.. var:: config.say_layer = "screens"

    The layer the say screen is shown on.

.. var:: config.say_menu_text_filter = None

    If not None, then this is a function that is given the text found
    in strings in the :ref:`say <say-statement>` and :ref:`menu
    <menu-statement>` statements. It is expected to return new
    (or the same) strings to replace them.

.. var:: config.say_sustain_callbacks = ...

    A list of functions that are called, without arguments, before the
    second and later interactions caused by a line of dialogue with
    pauses in it. Used to sustain voice through pauses.

.. var:: config.save_dump = False

    If set to True, Ren'Py will create the file save_dump.txt whenever it
    saves a game. This file contains information about the objects contained
    in the save file. Each line consists of a relative size estimate, the path
    to the object, information about if the object is an alias, and a
    representation of the object.

.. var:: config.save_on_mobile_background = True

    If True, the mobile app will save its state when it loses focus. The state
    is saved in a way that allows it to be automatically loaded (and the game
    to resume its place) when the app starts again.

.. var:: config.save_physical_size = True

    If True, the physical size of the window will be saved in the
    preferences, and restored when the game resumes.

.. var:: config.savedir = ...

    The complete path to the directory in which the game is
    saved. This should only be set in a ``python early`` block. See also
    config.save_directory, which generates the default value for this
    if it is not set during a ``python early`` block.

.. var:: config.scene = renpy.scene

    A function that's used in place of renpy.scene by the :ref:`scene
    statement <scene-statement>`. Note that this is used to clear the screen,
    and config.show is used to show a new image. This should have the same
    signature as renpy.scene.

.. var:: config.screenshot_callback = ...

    A function that is called when a screenshot is taken. The function
    is called with a single parameter, the full filename the screenshot
    was saved as.

.. var:: config.screenshot_crop = None

    If not None, this should be a (`x`, `y`, `height`, `width`)
    tuple. Screenshots are cropped to this rectangle before being
    saved.

.. var:: config.screenshot_pattern = "screenshot%04d.png"

    The pattern used to create screenshot files. This pattern is applied (using
    Python's %-formatting rules) to the natural numbers to generate a sequence
    of filenames. The filenames may be absolute, or relative to
    config.renpy_base. The first filename that does not exist is used as the
    name of the screenshot.

    Directories are created if they do not exist.

    See also :var:`_screenshot_pattern`, which is used in preference to this
    variable if not None.

.. var:: config.script_version = None

    If not None, this is interpreted as a script version. The library
    will use this script version to enable some compatibility
    features, if necessary. If None, we assume this is a
    latest-version script.

    This is normally set in a file added by the Ren'Py launcher when
    distributions are built.

.. var:: config.searchpath = [ 'common', 'game' ]

    A list of directories that are searched for images, music,
    archives, and other media, but not scripts. This is initialized to
    a list containing "common" and the name of the game directory.

.. var:: config.search_prefixes = [ "", "images/" ]

    A list of prefixes that are prepended to filenames that are searched
    for.

.. var:: config.show = renpy.show

    A function that is used in place of renpy.show by the :ref:`show
    <show-statement>` and :ref:`scene <scene-statement>` statements. This
    should have the same signature as renpy.show.

.. var:: config.skip_delay = 75

    The amount of time that dialogue will be shown for, when skipping
    statements using ctrl, in milliseconds. (Although it's nowhere
    near that precise in practice.)

.. var:: config.skip_indicator = True

    If True, the library will display a skip indicator when skipping
    through the script.

.. var:: config.sound = True

    If True, sound works. If False, the sound/mixer subsystem is
    completely disabled.

.. var:: config.sound_sample_rate = 48000

    The sample rate that the sound card will be run at. If all of your
    wav files are of a lower rate, changing this to that rate may make
    things more efficient.

.. var:: config.start_callbacks = [ ... ]

    A list of callbacks functions that are called with no arguments
    after the init phase, but before the game (including the
    splashscreen) starts. This is intended to be used by frameworks
    to initialize variables that will be saved.

    The default value of this variable includes callbacks that Ren'Py
    uses internally to implement features such as nvl-mode. New
    callbacks can be appended to this list, but the existing callbacks
    should not be removed.

.. var:: config.start_interact_callbacks = [ ... ]

    A list of functions that are called (without any arguments) when
    an interaction is started. These callbacks are not called when an
    interaction is restarted.

.. var:: config.quit_callbacks = [ ... ]

    A list of functions that are called (without any arguments) when
    Ren'Py terminates. This is intended to free resources, such as
    opened files or started threads.

.. var:: config.top_layers = [ ]

    This is a list of names of layers that are displayed above all
    other layers, and do not participate in a transition that is
    applied to all layers. If a layer name is listed here, it should
    not be listed in config.layers.

.. var:: config.transient_layers = [ 'transient' ]

    This variable gives a list of all of the transient
    layers. Transient layers are layers that are cleared after each
    interaction. "transient" should always be in this list.

.. var:: config.transform_uses_child_position = True

    If True, transforms will inherit :ref:`position properties
    <position-style-properties>` from their child. If not, they won't.

.. var:: config.transition_screens = True

    If True, screens will participate in transitions, dissolving from the
    old state of the screen to the new state of the screen. If False, only
    the latest state of the screen will be shown.

.. var:: config.translate_clean_stores = [ "gui" ]

    A list of named stores that are cleaned to their state at the end of
    the init phase when the translation language changes.

.. var:: config.variants = [ ... ]

    A list of screen variants that are searched when choosing a screen to
    display to the user. This should always end with None, to ensure
    that the default screens are chosen. See :ref:`screen-variants`.

.. var:: config.voice_filename_format = "{filename}"

    A string that is formatted with the string argument to the voice
    statement to produce the filename that is played to the user. For
    example, if this is "{filename}.ogg", the ``voice "test"`` statement
    will play test.ogg.

.. var:: config.with_callback = None

    If not None, this should be a function that is called when a :ref:`with
    statement <with-statement>` occurs. This function can be responsible for
    putting up transient things on the screen during the transition. The
    function is called with a single argument, which is the transition that
    is occurring. It is expected to return a transition, which may or may not
    be the transition supplied as its argument.


Garbage Collection
------------------

These variables allow you to tune the Python garbage collector and the
Ren'Py management of the Python garbage collector.

.. var:: config.manage_gc = True

    If True, Ren'Py will manage the GC itself. This means that it will apply
    the settings below.

.. var:: config.gc_thresholds = (25000, 10, 10)

    The GC thresholds that Ren'Py uses when not idle. These are set to try
    to ensure that garbage collection doesn't happen. The three numbers are:

    * The net number of objects that need to be allocated before a level-0
      collection.
    * The number of level-0 collections that trigger a level-1 collection.
    * The number of level-1 collections that trigger a level-2 collection.

    (Level-0 collections should be fast enough to not cause a frame drop,
    level-1 collections might, level-2 will.)

.. var:: config.idle_gc_count = 2500

    The net number of objects that triggers a collection when Ren'Py has
    reached a steady state. (The fourth frame or later after the screen has been
    updated.)

.. var:: config.gc_print_unreachable = False

    If True, Ren'Py will print to its console and logs information about the
    objects that are triggering collections.
