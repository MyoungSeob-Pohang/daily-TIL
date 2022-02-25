# 문자열 재정렬

알파벡 대문자와 숫자(0 ~ 9)로만 구성된 문자열이 입력으로 주어집니다. 이때 모든 알파벳을 오름차순으로 정렬하여 이어서 출력한 뒤 그 뒤에 모든 숫자를 더한 값을 이어서 출력합니다.

```
import re

i = input()

w = list(filter(str.isalpha, i))
w.sort()
w = ''.join(w)
n = re.findall(r'\d', i)

sum = 0

for x in n:
  sum += int(x)

print(w + str(sum))
```

이 코드는 re모듈을 사용하여 작성한 코드입니다.

```
i = input()
word = []
num = 0;

for x in i:
  if x.isalpha():
    word.append(x)
  else:
    num += int(x)

word.sort()

print(''.join(word) + str(num))
```

이 코드는 re모듈 없이 작성한 코드입니다.

기본적으로 입력받은 글자에 하나씩 접근하여 문자이면 따로 모아둔 뒤 정렬시키고, 숫자는 모아서 더한 후 문자로 변환하여 문자열 뒤에 붙여주면 된다.
