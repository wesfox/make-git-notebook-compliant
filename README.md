## Make Jupyter notebooks git compliant

These steps allow you to get read of all the "not-gitable" stuff in a jupyter notebook :)

### Install jq

Linux
```sh
sudo apt-get update && sudo apt-get install jq;
```

MacOS
```sh
brew install jq
```

### Create a .git/.gitattributes with the following content
```conf
*.ipynb filter=nbstrip_full
```

### Modify the .git/config file adding the following into [core] and [filter "nbstrip_full"] content
```conf
[core]
attributesfile = .gitattributes

[filter "nbstrip_full"]
clean = "jq --indent 1 \
        '(.cells[] | select(has(\"outputs\")) | .outputs) = []  \
        | (.cells[] | select(has(\"execution_count\")) | .execution_count) = null  \
        | .metadata = {\"language_info\": {\"name\": \"python\", \"pygments_lexer\": \"ipython3\"}} \
        | .cells[].metadata = {} \
        '"
smudge = cat
required = true
```


### (optional) Create the following alias (if it does not already exist) in you .bashrc or .zshrc

This alias allows you to execute the command used by git through the `nbstrip_jq` command.

```sh
alias nbstrip_jq="jq --indent 1 \
    '(.cells[] | select(has(\"outputs\")) | .outputs) = []  \
    | (.cells[] | select(has(\"execution_count\")) | .execution_count) = null  \
    | .metadata = {\"language_info\": {\"name\": \"python\", \"pygments_lexer\": \"ipython3\"}} \
    | .cells[].metadata = {} \
    '"
```
