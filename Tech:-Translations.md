# Translations

The hooks into the translations API are plentiful and common. These often involves providing `_` as a function. The typical routines to perform translations use the `_("text")` formatting to extract the text. The actual translations are provided by the GUI or could be provided by other functionalities. They are expected to exist. The default no-operation `kernel.translation` function is `lambda e: e`, and will be used if nothing else replaces it.

The expectation is that this function will be replaced with the right function within the Kernel and this is done in the `wxMeerK40t` App class within MeerK40t.

All console commands provide a `_` value to the user. All channels have a `channel._` function available to the user, and `context._` should be equally provided.

If no translation routines are implemented, this performs no work. If a translation function is provided, this will give the user access to translations. The expectation is that these should be used rather consistently for any outward facing text, especially gui text and user messages. However, actual functional operations like commands should not be translated since this will give unusual results.

Most gui windows could access this function through the context or kernel. However currently most of them define this as a global `_ = wx.GetTranslation` and then use that function whenever text is used:

```python
        self.preview_menu.menu_prephysicalhome = wxglade_tmp_menu_sub.Append(
            wx.ID_ANY,
            _("Physical Home"),
            _("Automatically add a physical home command before all jobs"),
            wx.ITEM_CHECK,
        )
```

Here the `_("Physical Home")` function will typically within English reduce to `Physical Home` but could be translated if needed. Extraction programs that extract translation strings look for `_()` values and thus the function should used or defined in that form.

```python
        @kernel.console_command(
            "consoleserver", help=_("starts a console_server on port 23 (telnet)")
        )
        def server_console(
            command, channel, _, port=23, silent=False, quit=False, **kwargs
        ):
            root = kernel.root
```
This is part of the definition of `consoleserver` in `kernelserver.py` (0.7.1). We see not only is _ used for the help commands, but by default the commands which are flagged as console_command pass a `command`, a `channel` and a `_` value to the defined command. This is the `command` used to execute the this code, the `channel` we should use for reply messages and the `_` function to translate the reply messages.

We can perform `_ = kernel.translation` to get the translation function if we need to translate text located elsewhere.


