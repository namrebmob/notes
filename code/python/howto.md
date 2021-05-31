# Useful Python Code Snippets

## Autocompletion for your python shell|console|interpreter

```py
import rlcompleter, readline
readline.parse_and_bind('tab:complete')
```

## run test SMTP server

```sh
python -m smtpd -n -c DebuggingServer localhost:8025
```

## Iterable convertion

- enumerate list

```py
STATUS_LIST = ['draft', 'published', 'new', 'old', 'edited']
print(list(enumerate(STATUS_LIST)))
```
