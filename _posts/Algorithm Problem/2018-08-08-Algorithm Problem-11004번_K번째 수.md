---
layout: post
title: "11004번_K번째 수"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---
[11004](https://www.acmicpc.net/problem/11004)

시간초과가 나오길래 입력수가 너무 많아서 정렬하는데 오래걸리나보다 했는데
500만개의 입력을받을때 cin 함수가 너무 느려서 발생하는 문제였다.

다음 두줄을 추가해주면 간단하게 해결된다.
```c++
  cin.tie(NULL);
	ios::sync_with_stdio(false);
```

전체 소스
```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

vector<int> vec;

int main()
{
	cin.tie(NULL);
	ios::sync_with_stdio(false);

	int N, K;
	cin >> N >> K;

	int temp = 0;
	for (int i = 0; i < N; i++)
	{
		cin >> temp;
		vec.push_back(temp);
	}

	sort(vec.begin(), vec.end());

	cout << vec[K - 1] << endl;
}
```
