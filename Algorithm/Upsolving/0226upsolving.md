# 2월 26일 업솔빙

생각보다 코드가 달라진 것도 많았다. 풀이가 달라진 것도 생각이 달라진 것도 많다. 실질적으로 한번 더 풀어본 것이 아주 큰 도움이 된 거같다.

1. 설탕배달

```
n = int(input())
sum = 0;

while(True):
  if n < 0 :
    sum = -1
    break

  if n % 5 == 0:
    sum += n // 5
    break
  else:
    n -= 3
    sum += 1

print(sum)
```

2. 거스름돈

```
n = 1000 - int(input())
count = 0;

coin = [500, 100, 50, 10, 5, 1]

for x in coin:
  if x > n:
    pass
  else:
    count += n // x
    n %= x

print(count)
```

3. 동전 0

```
n, k = map(int, input().split())
c = []
count = 0

for x in range(int(n)):
  c.append(int(input()))

c.reverse();

for x in c:
  if x > k:
    pass
  else:
    count += k // x
    k %= x

print(count)
```

4. ATM

```
n = int(input())
k = list(map(int, input().split()))

sum = 0
count = 0

k.sort()

for x in k:
  count += x
  sum += count

print(sum)
```

5. 잃어버린 괄호

```
k = input().split('-')
sum = 0

for x in k[0].split('+'):
  sum += int(x)

for x in k[1:]:
  for y in x.split('+'):
    sum -= int(y)

print(sum)
```

6. 로프

```
i = int(input())
k = []
sum = []

for x in range(i):
  k.append(int(input()))

k.sort(reverse=True)

for x in range(len(k)):
  sum.append(k[x] * (x+1))

print(max(sum))
```

7. 보물

```
n = int(input())

a = list(map(int, input().split()))
b = list(map(int, input().split()))

s = 0

for x in range(len(a)):
  s += max(a) * min(b)
  a.remove(max(a))
  b.remove(min(b))

print(s)
```
