# Authoring a Plan Command
```python
def plugin(kernel, lifecycle):
    if lifecycle == 'register':
        def example():
            yield COMMAND_WAIT_FINISH
            def print_hello():
                print("Hello World.")
            yield COMMAND_FUNCTION, print_hello()
        kernel.register('plan/example', example)
```

This would then be run by `plan command --op example` and that would add the plan command to the stuff being prepared for the spooler.

# Authoring a Console Command
todo

# Authoring a Tree Menu Option
todo

# Authoring a Webhelp
todo

# Authoring a Window
todo

# Authoring a RasterWizard script.
todo
