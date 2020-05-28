# Regular Expressions
## Table of contents
__1. Meta characters__

__2. Usage and meaning of meta characters__

---

## 1. Meta characters

\* 메타 문자: 원래 그 문자가 가진 뜻이 아닌 특별한 용도로 사용되는 문자를 말한다.

\* 종류 : .  ^  $  *  +  ?  {  }  [  ]  \   | (  )


.
## 2. Usage and meaning of meta characters
### __2.1. 문자 클래스 [ ]__

\* 문자 클래스로 만들어진 정규식은 "[와 ] 사이의 문자들과 매치"라는 의미를 갖는다 ([와 ] 사이에는 어떤 문자도 들어갈 수 있다.)

__예시) [abc]__
>"a"는 정규식과 일치하는 문자인 "a"가 있으므로 매치
>"before"는 정규식과 일치하는 문자인 "b"가 있으므로 매치
>"dude"는 정규식과 일치하는 문자인 a, b, c 중 어느 하나도 포함하고 있지 않으므로 매치되지 않음

.
\* [ ] 안의 두 문자 사이에 하이픈(-)을 사용하게 되면 두 문자 사이의 범위(From - To)를 의미한다.

__예시) [a-c] = [abc] , [1-9] = [123456789]__
>[a-zA-Z] : 알파벳 모두
>[0-9] : 숫자

.
\*  문자 클래스 내에 ^ 메타 문자가 사용될 경우에는 반대(not)라는 의미를 갖는다.

__예시) [^0-9] -> 문자__


__코드 예시__
```python
import re
p = re.compile('[^0-9]')
m = p.match('adssds')
if m:
     print('Match found: ', m.group())
else:
    print('No match')
```
>__Match found:  a__


### __2.2. Dot( . )__

\* 정규 표현식의 Dot(.) 메타 문자는 줄바꿈 문자인 \n를 제외한 모든 문자와 매치됨을 의미한다.

__예시) a.b -> "a + 모든문자 + b"__
>"aab"는 가운데 문자 "a"가 모든 문자를 의미하는 .과 일치하므로 정규식과 매치
>"a0b"는 가운데 문자 "0"가 모든 문자를 의미하는 .과 일치하므로 정규식과 매치
>"abc"는 "a"문자와 "b"문자 사이에 어떤 문자라도 하나는있어야 하는 이 정규식과 일치하지 않으므로 매치되지 않음

__코드 예시__
```python
p = re.compile('a.b')
m = p.match('a!b')

if m:
     print('Match found: ', m.group())
else:
    print('No match')
```
>__Match found:  a!b__

```python
p = re.compile('a..b')
m = p.match('a!sb')

if m:
     print('Match found: ', m.group())
else:
    print('No match')
```
>__Match found:  a!sb__

.

\* 문자 클래스([]) 내에 Dot(.) 메타 문자가 사용된다면 이것은 "모든 문자"라는 의미가 아닌 문자 . 그대로를 의미한다. (__"a + Dot(.)문자 + b"__)

__코드 예시__
```python
p = re.compile('a[.]b')
m = p.match('a.b')

if m:
     print('Match found: ', m.group())
else:
    print('No match')
```
>__Match found:  a.b__

.

\* _정규식 작성시 옵션으로 re.DOTALL 이라는 옵션을 주면 \n문자와도 매치되게 할 수 있다. (re.DOTALL affects what the . pattern can match.)_

__코드 예시__
```python
p = re.compile('a.b', flags = re.DOTALL)

m = p.match('a\nb')

if m:
     print('Match found: ', m.group())
else:
    print('No match')
```
>__Match found:  a
b__


\* _re.MULTILINE (re.MULTILINE affects where ^ and $ anchors match.)_

__코드 예시__

```python
p = re.compile('^python', flags = re.MULTILINE)

data = """python one\nlife is too short\n python two\nyou need python\npython three"""

print(p.findall(data))
```
>__['python', 'python', 'python']__


.
### __2.3. 반복 ( * )__

\* "*"의 의미는 *바로 앞에 있는 문자가 0부터 무한대로 반복될 수 있다는 의미이다.(사실 메모리 제한으로 2억개 정도만 가능)

__예시) ca*t__

![center](D:/asd3.png)


### __2.4. 반복 ( + )__

\* +는 최소 1번 이상 반복될 때 사용한다. 즉, *가 반복 횟수 0부터라면 +는 반복 횟수 1부터인 것이다.

__예시) ca+t -> "c + a(1번 이상 반복) + t"__.

![center](D:/asd2.png)

### __2.5. 반복 ( {m,n} )__

\* {m, n} 정규식을 사용하면 반복 횟수가 m부터 n까지인 것을 매치할 수 있다. 또한 m 또는 n을 생략할 수도 있다. ( {1,}은 +와 동일하며 {0,}은 *와 동일하다. )

__예시) ca{2}t -> "c + a(반드시 2번 반복) + t"__.

![center](D:/asd4.png)

__예시) ca{2,5}t -> "c + a(2~5회 반복) + t"__.

![center](D:/asd5.png)

### __2.6. 반복 ( ? )__

\* ? 메타문자가 의미하는 것은 {0, 1} 이다.

__예시) ab?c -> "a + b(있어도 되고 없어도 된다) + c"__.

![center](D:/asd6.png)

### __2.7. |__

\* 메타문자는 "or"의 의미와 동일하다. A|B 라는 정규식이 있다면 이것은 A 또는 B라는 의미가 된다.

```python
p = re.compile('Crow|Servo')
m = p.match('CrowHello')

if m:
     print('Match found: ', m.group())
else:
    print('No match')
```
>__Match found:  Crow__

### __2.8. ^__

\* 메타문자는 문자열의 맨 처음과 일치함을 의미한다.

```python
print(re.search('^Life', 'Life is too short'))
```
>__<sre.SRE_Match object at 0x01FCF3D8>__

```python
print(re.search('^Life', 'My Life'))
```
>__None__

### __2.9. $__

\* $ 메타문자는 ^ 메타문자의 반대의 경우이다. $는 문자열의 끝과 매치함을 의미한다.

```python
print(re.search('short$', 'Life is too short'))
```
>__<sre.SRE_Match object; span=(12, 17), match='short'>__

```python
print(re.search('short$', 'Life is too short, you need python'))
```
>__None__


## 3. Reference
https://wikidocs.net/4308
http://stackoverflow.com/questions/41620093/whats-the-difference-between-re-dotall-and-re-multiline
