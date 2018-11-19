# Scripts

Scripts are just text files that contain commands you run with an interpreter.

Common scripting languages:

- Perl
- Python
- Bash
- R
- Ruby
- Batch (windows)
- Power Shell (windows)

### Bash

[Bash](https://www.gnu.org/software/bash/) is mainly found in Linux and is the default shell in most versions.

It can be used in windows through the use of:

- [cygwin](https://www.cygwin.com/)
- [Containers](https://docs.docker.com/docker-for-windows/)
- [Windows Subsystem for Linux](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/)

##### SHEBANG

The shebang `#!` followed by the path `/bin/bash -l` to the interpreter that will be use to execute the script. This needs to be first line of the script. 

```bash
#!/bin/bash -l
```

You can also use a more portable version of this with `/usr/bin/env bash -l ` .  `/usr/bin/env` is a application used to run the first `bash` found in your `PATH`. 

```bash
#!/usr/bin/env bash -l
```


