# 공부한 것들

## python 프로젝트 구성하기

### 파일의 구성

- __init__.py
- [dir]docs
- [dir]modules
- [dir]test
- dependencies.txt or setup.py

<br>

### 의존성을 처리하는 방법들

- setup.py 를 이용하는 방법
- setup.py 를 이용하지 않고, dependencies 파일을 별도로 관리하는 방법

<br>

### __name__ 의 의미

Special Variables
When the Python interpeter reads a source file, it first defines a few special variables. In this case, we care about the __name__ variable.

When Your Module Is the Main Program

If you are running your module (the source file) as the main program, e.g.
```pyhon
python foo.py
```
the interpreter will assign the hard-coded string "__main__" to the __name__ variable, i.e.

```python
# It's as if the interpreter inserts this at the top
# of your module when run as the main program.
__name__ = "__main__" 
```
When Your Module Is Imported By Another

On the other hand, suppose some other module is the main program and it imports your module. This means there's a statement like this in the main program, or in some other module the main program imports:
```python
# Suppose this is in some other main program.
import foo
In this case, the interpreter will look at the filename of your module, foo.py, strip off the .py, and assign that string to your module's __name__ variable, i.e.

# It's as if the interpreter inserts this at the top
# of your module when it's imported from another module.
__name__ = "foo"
```
