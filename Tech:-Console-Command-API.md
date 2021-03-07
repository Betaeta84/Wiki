
Console commands within the API are registered with several decorators. Here's an example used internally for the `thread` command:

```python
        @self.console_command("thread", help="show threads")
        def thread(command, channel, _, args=tuple(), **kwargs):
            """
            Display the currently registered threads within the Kernel.
            """
            channel(_("----------"))
            channel(_("Registered Threads:"))
            for i, thread_name in enumerate(list(self.threads)):
                thread = self.threads[thread_name]
                parts = list()
                parts.append("%d:" % (i + 1))
                parts.append(str(thread))
                if thread.is_alive:
                    parts.append(_("is alive."))
                channel(" ".join(parts))
            channel(_("----------"))
            return
```

# context.console_command
The decorator is being used to register the console command. This can be done *anywhere* in registration code.

## command
The command is either a string or tuple of strings (if this is reserving more than one namespace) which is the command that will be typed to trigger this command. It is acceptable to use the same command used elsewhere *only* if the namespace of the input_type is different. For example both `operations` and `clipboard` have a `cut` command. But, because `clipboard` must be given a `clipboard` type the namespaces do not overlap and both can be used in their respective contexts.

## hidden
Hidden prevents this command from showing up in help.

## help
Help is the quick help message of this particular command.


# context.console_argument

Registers required operands for the command, these must exist. If they do not the command is called with them set to `None`.

```python
        @self.console_argument("extended_help", type=str)
        @self.console_command(("help", "?"), hidden=True, help="help <help>")
```

For example, here's the argument for `help`/`?` it accepts an extended_help which is the command that should be looked up and presented to the user for extended help. In the signature of the function being decorated there an argument `extended_help`

`def help(command, channel, _, extended_help, output=None, input=None, args=tuple(), **kwargs):`

It does not need to be defaulted because it will be given a value, even if that value is `None`

## type
The type argument is used to define a given type for this console_argument. These can be anything and the string data will be used to initialize this type.

## input_type
The input_type is either a string or tuple of strings to be used to register the command in the kernel. This command will only be called if the data_type matches the data_type of the given input_type. If set to None then this should be the first command in a series, if set to a data_type *and* `None` then the command must be able to be both the first command in the sequence *and* able to take the requested data_type.

## output_type
The output_type will give the valid data types this command may return. Any console_command should return `"output_type", <data>` unless the result of the function in None. If the output type is `None` or omitted there is no chaining for the command `args` or `remainder` values will be given. With a given output_type, the expectation is this is a chained function and any command accepting the given output_type can be chained together with this function.

## regex
The regex is a bool that says whether the given command is a regex of a pattern that would match the given command. In some extreme cases, the `bindalias` captures `.*` values as an alias can be established dynamically. It is also common to use this for suffix values like `plan0` or `timer2` where the suffix is part of the command and given to the Console Function in the `command` value.

## help
The help data will provide the quick command explanation located in help. The extended help will be the docstring within the decorated command.

## default
The default is the value to be used if no value is given.

# context.command_option
Provides optional 

For example this option for path 
```
@self.console_option("path", "p", type=str, help="Path of variables to set.")
```

## action
The action can only be "store_true", works for bool values to simply store the value `true` if they exist.

## default
The default is the value to be used if no value is given.


# Console Function
The console function is the function being decorated

## command
The command is either a string or tuple of strings (if this is reserving more than one namespace) which is the command that will be typed to trigger this command.

## channel
The channel is the console channel. This is to assist in giving replies and responses to the console or providing data if that's the goal of this function.

## _ (translation)
The _ function is the translation function. If we are providing information we should do in a way that respects internationalization.

## <arguments>
The arguments are required arguments in the function. These will be defined with `console_argument` functions.

## <options>
The options are optional arguments provided with either a `-` prefix of a single letter or `--` prefix for the entire option name.

## args
The args function provides a tuple of string objects that were parsed, but not used. If no `output_type` is defined for the given function all remaining arguments are passed to this console function.

## remainder
The remainder is a single string of the remaining values in a non-parsed fashion.

