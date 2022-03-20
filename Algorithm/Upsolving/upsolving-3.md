# 업솔빙

1. 30

```
n = list(map(str, input()))
n.sort(reverse=True)
r = int("".join(n))

if r % 30 == 0:
  print(r)
else:
  print(-1)
```

2. 수들의 합

```
n = int(input())
x = 0
t = 1

while True:
  x += t
  if x > n:
    t -= 1
    break
  t += 1

print(t)
```

3. 뒤집기

```
n = list(map(int, input()))
z = 0
o = 0

if n[0] == 0:
  z += 1
else:
  o += 1

for x in range(len(n) - 1):
  if n[x] != n[x + 1]:
    if n[x + 1] == 0:
      z += 1
    else:
      o += 1

print(min(z, o))
```

4. 캠핑

```
c = 0

while True:
  l, p, v = map(int, input().split())
  count = 0

  if l == 0 and p == 0 and v == 0:
    break
  else:
    count += (v // p) * l
    count += min(v % p, l)
    c += 1

    print("Case " + str(c) + ": " + str(count))
```
