---
layout: post
title: "1011번_Fly me to the Alpha Centauri"
categories:
  - Algorithm Problem
tags:
  - Algorithm Problem
---

[1011](https://www.acmicpc.net/problem/1011)

요즘은 자료구조를 활용하는 문제보다 이런 발상의 전환을 필요로하는 문제를 많이 접하고 있네요
조금만 생각을 다르게하면 쉽게 풀리는 문제입니다.

중요한건 처음과 마지막이 무조건 1광년만 움직일수 있다는점과
다음 이동은 이전이동에서 + 1또는 - 1 차이나는 거리만큼 이동할수 있다는 점이다.

이동을 최소한으로 하기 위해서는 매 이동마다 이동거리를 +1씩 해줘야되고
마지막에는 1광년을 이동하기때문에
오른쪽과 왼쪽에서 같이 1, 2, 3, 이런식으로 빼주면 된다.

그리고 출력하기전에 남은거리에 대한 경우의수를 처리해주면 끝
1. 다음이동거리로 한번에 갈수있는경우 +1
2. 다음이동거리와 그보다 적은수로 갈수 있는경우 +2
3. 다음이동거리의 두배보다 큰경우 --->반복문 계속

```c++
#include <iostream>

using namespace std;

int main()
{
    int T = 0;

    cin >> T;

    for (int i = 0; i < T; i++)
    {
        int start = 0;
        int end = 0;
        cin >> start;
        cin >> end;

        int count = 0;
        int distance = end - start;

        for (int j = 1; ; j++)
        {
            if (distance - j <= 0) {
                count++;
                break;
            }
            else if(distance <= 2*j){
                count += 2;
                break;
            }
            else {
                distance -= 2 * j;
                count += 2;
            }
        }
        cout << count << endl;
    }
}
```

n(n+1)/2 를 이용해서 이동할수 있는 최대합을 찾아서
거기서 나머지 조건들을 처리해줄려고 했는데
우선 답이 나오는건 둘쨰치고 (위 코드로 정답은 안나온다 위의 3가지 경우의수를 잘못처리하기때문)
시간초과가 걸리는데 1의 합 2의합 이런식으로 합을 계산하고 비교하는 방식때문이라고 추정된다.

문제는 이미 첫번째 코드로 맞았지만 오기가 생겨서 시간을 줄일수 있는 방법이 없을까 생각하다가
어짜피 공식이 있고 도달하고자 하는 값 (이동하는 거리의 절반) 이 있으니 이것을 식으로 만들어 보았다

distance/2 > n(n+1)/2

를 만족하는 n값이 내가 원하는 최대합을 만드는 수가 된다.
2차 부등식 중에서 실수 근만 사용하면 되기 때문에
간단하게 근의공식으로 값을 구할 수 있다.


```c++
#include <iostream>
#include <cmath>
using namespace std;

int isInteger(float num)
{
    if (num / 1.00 == (int)num) {
        return 1;
    }
    else {
        return 0;
    }
}
int main()
{
    int T = 0;

    cin >> T;

    for (int i = 0; i < T; i++)
    {
        int start = 0;
        int end = 0;
        cin >> start;
        cin >> end;

        int count = 0;
        float distance = end - start;

        float root = (-1 + sqrt(1 - 4 * 1 * (-1 * distance))) / 2;
        //cout << "root : " << root << endl;
        if (isInteger(root)) {
            cout << root*2 << endl;
        }
        else {
            int j = floor(root);
            int sum = j*(j + 1);
            distance -= sum;
            if (distance > j + 1) {
                cout << j * 2 + 2 << endl;
            }
            else {
                cout << j * 2 + 1 << endl;
            }
        }
    }
}

```
