---
layout : post
title : 완전탐색 - 숨바꼭질(BFS)
CATEGORY: "Algorithm"
tag : [Algorithm]
---

> https://www.acmicpc.net/problem/1697

| 시간 제한 | 메모리 제한 | 제출  | 정답 | 맞은 사람 | 정답 비율 |
| --------- | ----------- | ----- | ---- | --------- | --------- |
| 2초       | 128MB       | 29306 | 8091 | 5079      | 24.925%   |

#### 문제

수빈이는 동생과 숨바꼭질을 하고 있다. 수빈이는 현재 점 N(0 ≤ N ≤ 100,000)에 있고, 동생은 점 K(0 ≤ K ≤ 100,000)에 있다. 수빈이는 걷거나 순간이동을 할 수 있다. 만약, 수빈이의 위치가 X일 때 걷는다면 1초 후에 X-1 또는 X+1로 이동하게 된다. 순간이동을 하는 경우에는 1초 후에 2*X의 위치로 이동하게 된다.

수빈이와 동생의 위치가 주어졌을 때, 수빈이가 동생을 찾을 수 있는 가장 빠른 시간이 몇 초 후인지 구하는 프로그램을 작성하시오.



#### 입력

첫 번째 줄에 수빈이가 있는 위치 N과 동생이 있는 위치 K가 주어진다.



#### 출력

수빈이가 동생을 찾는 가장 빠른 시간을 출력한다.

#### 예제 입력

```c++
5 17
```

#### 예제 출력

```c++
4
```



기본적인 BFS문제이다. Queue에 다음에 갈 수 있는 좌표를 저장하고, Vector을 이용하여 해당 Vector<좌표>에 값이 없을 경우 방문하지 않는 곳이므로 다음에 방문 하게 했다.

BFS를 이용했음으로 가장 빠른 시간을 위한 조건은 따로 필요하지 않았다.



```c++
#include<iostream>
#include<vector>
#include<queue>
#include<algorithm>
using namespace std;

vector<int> v(100001);

void BFS(int start, int end)
{
	queue<int> q;
	q.push(start);
	v[start] = 1;
	while (1)
	{
		if (q.empty()) break;
		int now = q.front();
		q.pop();

		if (now - 1 >= 0)
		{
			if (!v[now - 1])
			{
				v[now - 1] = v[now] + 1;
				q.push(now - 1);
			}
		}

		if (now + 1 <= 100000)
		{
			if (!v[now + 1])
			{
				v[now + 1] = v[now] + 1;
				q.push(now + 1);
			}
		}

		if (now * 2 <= 100000)
		{
			if (!v[now * 2])
			{
				v[now * 2] = v[now] + 1;
				q.push(now * 2);
			}
		}

		if (v[end])
		{
			cout << v[end] - 1 << '\n';
			break;
		}
	}
}

int main()
{
	int s, e;
	cin >> s >> e;

	BFS(s,e);

	return 0;
}
```



