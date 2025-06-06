The application implements no defenses against OS command injection, so an attacker can submit the following input to execute an arbitrary command:

`& echo aiwefwlguh &`

If this input is submitted in the `productID` parameter, the command executed by the application is:

`stockreport.pl & echo aiwefwlguh & 29`

The `echo` command causes the supplied string to be echoed in the output. This is a useful way to test for some types of OS command injection. The `&` character is a shell command separator. In this example, it causes three separate commands to execute, one after another. The output returned to the user is:

`Error - productID was not provided aiwefwlguh 29: command not found`

The three lines of output demonstrate that:

- The original `stockreport.pl` command was executed without its expected arguments, and so returned an error message.
- The injected `echo` command was executed, and the supplied string was echoed in the output.
- The original argument `29` was executed as a command, which caused an error.

Placing the additional command separator `&` after the injected command is useful because it separates the injected command from whatever follows the injection point. This reduces the chance that what follows will prevent the injected command from executing.