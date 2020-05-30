Changes in gui, et al, will create new language strings and those new language strings will need translating. Generating a messages.po file is essential to this process. We use pybabel for this process. 

`pip install Babel`

Then:

`pybabel extract -o messages.po`

Because I have a virtual environment I moved all the files to a directory and ran the script there, since it tried to do all the sub directories.

---

The messages file is stored in `/locale/` and inside that are `meerk40t.po` and `meerk40t.mo` files for the various languages.