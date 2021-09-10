Meerk40t's native locale language is US English. The UI can be displayed in other languages providing a translation of all the necessary strings has been provided. **If a translation is not available for your own language, or if you think the existing translation can be improved, we encourage you to contribute improvements by editing the existing messages.po file.**

## Editing the language files
The `messages.po` file is stored in [/locale/](https://github.com/meerk40t/meerk40t/blob/master/locale/) and inside that are `meerk40t.po` and `meerk40t.mo` files for the various languages..

You can either edit the file in the github download the [messages.po](https://github.com/meerk40t/meerk40t/blob/master/locale/messages.po) file or use any po editor. There is also an online project page at loco: https://localise.biz/tatarize/meerk40t

### Editing or translating the About message
When you translate the About message text, it is a [legal licensing requirement](https://icons8.com/license) that [icons8](https://icons8.com/) be thanked in there.

## Regenerating the message.po file
Changes in gui, et al, will create new language strings and those new language strings will need translating. Generating a messages.po file is essential to this process. We use pybabel for this process. 

`pip install Babel`

Then:

`pybabel extract . -o messages.po`

Because I have a virtual environment I moved all the files to a directory and ran the script there, since it tried to do all the sub directories.

## Updating the meerk40t.po file

If your meerk40t.po is old you can use pybabel to update it:

For Chinese (zh, use the correct locale) this would be:
`pybabel update -l zh -i messages.po -o meerk40t.po`

## Acknowledgements
Please edit the Acknowledgements section of the Wiki home page and the About window /meerk40t/gui/about.py to add your name to the list of contributors.