---
title: c++ stringstream, time
author: HEECHANG
layout: post
---

# c++ stringstream, time의 사용예
### c++ Study(5.24)

알고리즘 문제를 풀다가 시간형식의 스트링 "00:00:00"을 처리하는데 애를 먹었다 java 에서 스트링에 지원하는 spilt메서드를 사용하면 편하지만 c++은 찾아봐도 stringstream을 조건을 줘서 사용하는 방법을 쓰더라 그외에 수상한 헤더를 사용하는 방법밖에 없다

stringstream이 공백을 기준으로 자료형에 맞게 찾아주니까 ':'를 공백으로 바꾸는 방법을 사용해서 시간을 파싱하자

추가
time헤더 변환방법
localtime(&now)
mktime(when)

**example**
```c++
#include <iostream>
#include <time.h>
#include <sstream>

using namespace std;

char input[51];

int main()
{
	//"01:02:03"을 받으면 for문으로 :를 공백으로 바꿔주자
	char date[9] = {"01:02:03"};
	for (int i = 0; i < 8; i++)
	{
		if (date[i] == ':')
			date[i] = ' ';
	}
	stringstream stream1(date);
	//stream1.str(date); // 값 넣는법

	int hh, mm, ss; //int형으로 가져올수 있다.
	stream1 >> hh;
	stream1 >> mm;
	stream1 >> ss;

	cout << hh << " "<< mm<< " " << ss << endl;

//time헤더 활용
	time_t now;
	struct tm *when;
	time(&now); // 현재시각
	cout << now << endl;

	when = localtime(&now); // time_t  형식으로 변환합니다.

	cout << when->tm_hour << endl; //시
	cout << when->tm_min << endl; //분
	cout << when->tm_sec << endl; //초

	now = mktime(when); // tm형식으로 변환
	cout << now << endl;

}
```
