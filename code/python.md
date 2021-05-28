# [inspect](https://docs.python.org/3/library/inspect.html) — Inspect live objects

```py
inspect.signature(Post.__init__)
inspect.getmembers(Post)
inspect.getclasstree(Post)
inspect.getargspec(Post.__init__)
```

## Pip install from github branch

```sh
pip install git+https://github.com/pallets/flask@32272da9ac6e16fa22d3401b2d995d5bf3db9492
```

## Autocompletion for your python shell|console|interpreter

```py
import rlcompleter, readline
readline.parse_and_bind('tab:complete')
```

## How to Use Python Lambda Functions

- <https://realpython.com/python-lambda>
- <https://www.guru99.com/python-lambda-function.html>
- <https://dev.to/suvhotta/python-lambda-and-list-comprehension-5128>
- <https://www.dataquest.io/m/355-list-comprehensions-and-lambda-functions/>
- <https://www.studytonight.com/post/find-a-given-elements-index-in-python-list>
- <https://www.guru99.com/python-list-index.html>
- <https://lerner.co.il/2014/05/11/creating-python-dictionaries-reduce/>

```py
list(map((lambda x:x[1]), records))
```

- <https://realpython.com/python-return-statement/>
- <https://realpython.com/python-map-function/>
- <https://www.bitdegree.org/learn/python-map>

## run test SMTP server

```sh
python -m smtpd -n -c DebuggingServer localhost:8025
```

## Iter convertion

- enumerate list

```py
STATUS_LIST = ['draft', 'published', 'new', 'old', 'edited']
print(list(enumerate(STATUS_LIST)))
```
