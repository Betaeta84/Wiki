
### Preferences

Contexts serve as to store persistent information. These are derived from the kernel preference, when the device is loaded.

Preferences are expected to be checked for existence prior to use. The `setting()` command provides a setting_type, a key, and a default value. Once called, `.key` is the preferred access method.

```python
self.context.setting(int, 'my_data', 0)
if my_data == 0:
     channel(_("Default Data Was Found.")
```

In the example, it is entirely possible that `my_data` is loaded from the persistent setting and has a non-zero value. These values are flushed during shutdown. This is the case for any variable that does not start with `_` and which is a `int`, `float`, `str`, or `bool`.
