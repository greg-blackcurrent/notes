https://bugzilla.gnome.org/show_bug.cgi?id=663779

I don't like this approach. The keymap handling should be left alone.

Instead, the modifiers should be mangled as they come out of the GdkEvent, ie. in vte_terminal_read_modifiers(). There, map to VTE_META_MASK whatever modifiers should map to it; perhaps as simple as testing GDK_META_MASK | GD_MOD1_MASK, or possibly as sophisticated as comment 14 suggests.
Comment 38



Widget::read_modifiers_from_gdk


Now called:
gdk_event_get_modifier_state


Terminal::widget_key_press(vte::platform::KeyEvent const& event)

_vte_keymap_key_add_key_modifiers




        GdkEvent* m_platform_event;
        unsigned m_modifiers;
        unsigned m_keyval;
        unsigned m_keycode;
        unsigned m_group;
        bool m_is_modifier;

{event,
type,
gdk_event_modifier_state,
keyval,
scancode,
group,
is_modifier}

https://docs.gtk.org/gdk4/flags.ModifierType.html


https://gitlab.gnome.org/GNOME/vte/-/issues/276

https://gitlab.gnome.org/GNOME/gnome-terminal/-/issues/220


Add an option here:

class Options {
public:
    gboolean allow_window_ops{false};
    gboolean audible_bell{false};
    gboolean a11y{gtk_if(true, false)};
    gboolean backdrop{false};
    gboolean bidi{true};
    gboolean bold_is_bright{false};


Probably not this
void
_vte_keymap_map(guint keyval,



terminal_screen_new

terminal_screen_profile_changed_cb

TERMINAL_PROFILE_SCROLLBACK_LINES_KEY



_terminal_screen_update_kinetic_scrolling !

