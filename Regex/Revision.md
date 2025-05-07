# Regex

# Special Characters

```python
. any character
\ escape character
```

# Anchors

```python
^ start of string
$ end of string
\A start of string
\Z end of string
\b word boundary
\B non-word boundary
```

# Quantifiers

```python
* 0 or more
+ 1 or more
? 0 or 1
{n} n times
{n,} n or more
{n,m} n to m times
*? 0 or more, non-greedy
+? 1 or more, non-greedy
?? 0 or 1, non-greedy
{n}? n times, non-greedy        # re.sub(r'l{2}?', 'x', 'hello world chillly') -> 'hexo world chixly'
{n,}? n or more, non-greedy     # re.sub(r'l{2,}?', 'x', 'hello world chillly') -> 'hexo world chixly'
{n,m}? n to m times, non-greedy # re.sub(r'l{2,4}?', 'x', 'hello world chillly') -> 'hexo world chixly'
*+ greedy
++ greedy
?+ greedy
{n}+ n times, greedy        # re.sub(r'l{2}+', 'x', 'hello world chillly') -> 'hexo world chixly'
{n,}+ n or more, greedy     # re.sub(r'l{2,}+', 'x', 'hello world chillly') -> 'hexo world chixy'
{n,m}+ n to m times, greedy # re.sub(r'l{2,4}+', 'x', 'hello world chillly') -> 'hexo world chixy'
```

# Character Classes

```python
\d or [0-9] any digit
\D or [^0-9] any non-digit
\s any whitespace character
\S any non-whitespace character
\w any word character
\W any non-word character
```

# Groups

```python
r = re.match(r'(he)l', "hello world chillly")
r.groups() # ('he',) 
```

## Group Reference

```python
# \1, \2, \3, ...

r = re.match(r'(he)l', "hello world chillly")
r.group(0) # 'hel'
r.group(1) # 'he'
```

## Named Group

```python
(?P`<name>`pattern)
r = re.match(r'(?P<name>he)l', "hello world chillly")
r.groups() # ('he',)
r.group('name') # 'he'
```

## Non-Capturing Group

```python
r = re.match(r'(?:he)l', "hello world chillly")
r.groups() # ()
```

# Lookaround

?<= positive lookbehind
?<! negative lookbehind
?= positive lookahead
?! negative lookahead
⚠️ Lookaheads are zero-width assertions; They are not consumed; they are only used to check if the pattern is present.


## Positive Lookbehind

```python
# Replace 'foo' only if it is preceded by 'bar'
re.sub(r' worl', ' worx', "hello world chillly,world")  # 'hello worxd chillly,world' Character consumed
re.sub(r'(?<=he)l', 'x', "hello world chillly")         # 'hexlo world chillly'
re.sub(r'(?<= wor)l', 'x', "hello world chillly,world") # 'hello worxd chillly,world' ⚠️ Zero-width assertion, no character consumed
re.sub(r'(?<=...)l', 'x', "hello world chillly,world")  # 'helxo worxd chixxxy,worxd'
re.sub(r'(?<=.*)l', 'x', "hello world chillly,world") # re.error: look-behind requires fixed-width pattern
```

## Negative Lookbehind

```python
# Replace 'foo' only if it is not preceded by 'bar'
re.sub(r'(?<!he)l', 'x', "hello world chillly")         # 'helxo worxd chixxxy'
re.sub(r'(?<! wor)l', 'x', "hello world chillly,world") # 'hexxo world chixxxy,worxd' ⚠️ Zero-width assertion, no character consumed
re.sub(r' worl', ' worx', "hello world chillly,world")  # 'hello worxd chillly,world' Character consumed
re.sub(r'(?<!...)l', 'x', "hello world chillly,world")  # 'hexlo world chillly,world'
re.sub(r'(?<!.*)l', 'x', "hello world chillly,world")   # re.error: look-behind requires fixed-width pattern
```

## Positive Lookahead

```python
# Replace 'foo' only if it is followed by 'bar'
re.sub(r'l(?=l)', 'x', "hello world chillly")         # 'hexlo world chixxly'
re.sub(r'l(?=o )', 'x', "hello world chillly,world") # 'helxo world chillly,world' ⚠️ Zero-width assertion, no character consumed
re.sub(r'l(?=...)', 'x', "hello world chillly,world")  # 'hexxo worxd chixxxy,world'
re.sub(r'l(?=.*)', 'x', "hello world chillly,world")   # 'hexxo worxd chixxxy,worxd'
```

## Negative Lookahead

```python
# Replace 'foo' only if it is not followed by 'bar'
re.sub(r'l(?!l)', 'x', "hello world chillly,world")   # 'helxo worxd chillxy,worxd'
re.sub(r'l(?!o )', 'x', "hello world chillly,world")  # 'hexlo worxd chixxxy,worxd' ⚠️ Zero-width assertion, no character consumed
re.sub(r'l(?!...)', 'x', "hello world chillly,world") # 'hello world chillly,worxd'
re.sub(r'l(?!.*)', 'x', "hello world chillly,world")  # 'hello world chillly,world'
```

# Flags

```python
re.IGNORECASE
re.MULTILINE
re.DOTALL
re.VERBOSE
```

# Functions

```python
r = re.compile(r'll') # Save compile pattern
r.match("hello world chillly,world") # <re.Match object; span=(2, 4), match='ll'>
r.search("hello world chillly,world") # <re.Match object; span=(2, 4), match='ll'>
r.findall("hello world chillly,world") # ['ll', 'll']
r.finditer("hello world chillly,world") # <callable_iterator of <re.Match object; span=(2, 4), match='ll'>, <re.Match object; span=(15, 17), match='ll'>>
r.sub("x", "hello world chillly,world") # 'hexo world chixly,world'
r.subn("x", "hello world chillly,world") # ('hexo world chixly,world', 2)
r.split("hello world chillly,world") # ['hello', 'world', 'chillly,world']
```
