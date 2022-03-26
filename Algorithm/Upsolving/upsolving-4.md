# 업솔빙

1. 사과 담기 게임

```
n, m = map(int, input().split())
j = int(input())

start = 1
end = m
count = 0

for _ in range(j):
  target = int(input())

  if target < start:
    count += start - target
    start = target
    end = target + m - 1
  elif target > end:
    count += target - end
    end = target
    start = end - m + 1

print(count)
```

이 풀이는 다시한번 익혀서 이해하도록 하자.

2. 스네이크 버드

```
n, l = map(int, input().split())
r = list(map(int, input().split()))
r.sort()

for x in r:
  if x <= l:
    l += 1
print(l)
```

별거 없음

3. 알바생 강호

```
n = int(input())
r = []
for _ in range(n):
  r.append(int(input()))
r.sort(reverse=True)

c = 0
s = 0
for x in r:
  if x - s > 0:
    c += x - s
  s += 1

print(c)
```

0보다 커야한다는 것을 명심 / 별거 없음

4. UCPC는 무엇의 약자일까 ?

```
n = input()
target = ['U', 'C', 'P', 'C']
u = ''
l = ''

for x in n:
  if x == 'U' or x == 'C' or x == 'P':
    u += x
  elif x == 'u' or x == 'c' or x =='p':
    l += x

ur = ''
lr = ''
i = 0

for x in u:
  if ur == 'UCPC':
    break
  elif x == target[i]:
    ur += x
    i += 1

i = 0
for x in l:
  if lr == 'ucpc':
    break
  elif x.upper() == target[i]:
    lr += x
    i += 1

if ur == "UCPC" or lr.upper() == 'UCPC':
  print('I love UCPC')
else:
  print('I hate UCPC')
```

이전 정답코드는 대문자기준으로만 확인하였는데 더 정확하게 확인하기 위해 소문자 검증도 같이 넣어두었다.

5. 오셀로 재배치

```
t = int(input())
for x in range(t):
  n = int(input())
  r = list(map(str, input()))
  s = list(map(str, input()))

  l = []
  for x in range(n):
    if r[x] != s[x]:
      l.append(s[x])

  print(max(l.count('B'), l.count('W')))
```

W와 B의 다른 값을 뽑아 새로운 배열을 만든 후 그 중에서 많은 갯수를 가진 카운트를 뽑는다. 6. 2+1세일

```
n = int(input())
r = []
for _ in range(n):
  r.append(int(input()))

r.sort(reverse=True)

s = 0
l = []
for x in range(len(r)):
  if s > len(r):
    break
  l.append(r[s : s + 3])
  s += 3

c = 0
for x in range(len(l)):
  if len(l[x]) >= 3:
    c += sum(l[x]) - min(l[x])
  else:
    c += sum(l[x])

print(c)
```

내림차순 정렬 후 3개씩 끊어서 배열을 재생성 하고 3개면 전체값에서 가장 작은 값을 빼주고 3개이하면 모든 수를 더해주었다.

7. 소가 길을 건너간 이유 3

```
n = int(input())
r = []
for _ in range(n):
  r.append(list(map(int, input().split())))

r.sort()
c = sum(r[0])
for x in r[1:]:
  if x[0] > c:
    c = sum(x)
  else:
    c += x[1]

print(c)
```

별거없었다. 오름차순 정렬 후 첫번째 값의 합을 가지고 배열을 순회하면서 첫번째 값의 합과 두번째 값비교 후 첫번째 값이 크면 뒤에 초만 더해주면 되고 아니면 전체 값을 그 배열의 합으로 바꿔주면 된다.

8. 온라인 판매

```
n, m = map(int, input().split())
s = []
for x in range(m):
  s.append(int(input()))

s.sort()

t = 0
c = 0

for x in range(m):
  low = min(m - x, n)
  if c < s[x] * low:
    t = s[x]
    c = s[x] * low

print(t, c)
```

계란의 총 갯수와 사람 수 중 낮은 숫자를 골라야 한다.  
만약 계란은 6개 인데 사람은 8명이고 오름차순으로 1 ~ 8 까지 가격으로 산다고 가정하면 제일 싸게 사는 1원의 사람도 6개밖에 판매 못한다, 그래서 계란 갯수와 사람수 - 카운트 수 의 갯수중 작은 것을 골라서 곱해야한다.

9. 국회의원선거

```
n = int(input())
r = []
for _ in range(n):
  r.append(int(input()))

dasom = r[0]
other = r[1:]
count = 0

if n != 1:
  while True:
    if dasom > max(other):
      break
    else:
      dasom += 1
      other[other.index(max(other))] -= 1
      count += 1

  print(count)
else:
  print(0)
```

첫번쨰 값을 다솜이의 값으로 설정 해 두고 반복하면서 가장 높은 숫자의 값을 -1 다솜이의 값을 +1 하면서 카운트 하고 다솜이 의 값이 제일 큰 숫자가 되면 반복 종류 후 카운트를 출력하면 된다.
