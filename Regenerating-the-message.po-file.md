Changes in gui, et al, will create new language strings and those new language strings will need translating. Generating a messages.po file is essential to this process.

So as to not forget here's how I did that:

http://svn.python.org/projects/python/trunk/Tools/i18n/pygettext.py

Got `pygettext.py` and ran python27 on `python pygettext.py *.py`

Renamed .pot file to be .po, put it in the locale directory.