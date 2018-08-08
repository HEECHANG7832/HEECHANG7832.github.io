---
title: 1260번BFS_DFS
author: HEECHANG
layout: post
---

# 1260번BFS_DFS
### Algorithm Study(6.13)

[1260](https://www.acmicpc.net/problem/1260)

```c++
#include <iostream>
#include <queue>

using namespace std;

int adj_matrix[1001][1001] = { 0, };
int check[1001] = { 0, };
int N, M, V;

void dfs(int node)
{
    cout << node << " ";
    check[node] = 1; //visited!!!
    for (int i = 1; i <= N; i++)
    {
        if (adj_matrix[node][i] && check[i] == 0)
        {
            dfs(i); //go to next node
        }
    }
}
void bfs(int node)
{
    queue<int> q; //for bfs

    check[node] = 1; //visited!!!
    cout << node << " ";

    q.push(node);
    while (!q.empty())
    {
        int next_node = q.front();
        q.pop();
        for (int i = 1; i <= N; i++)
        {
            if (adj_matrix[next_node][i] && check[i] == 0)
            {
                check[i] = 1; //visited!!
                cout << i << " ";
                q.push(i);
            }
        }
    }

}
int main()
{



    cin >> N;
    cin >> M;
    cin >> V;

    //input and make adj_matrix
    for (int i = 0; i < M; i++)
    {
        int x; cin >> x;
        int y; cin >> y;
        adj_matrix[x][y] = 1;
        adj_matrix[y][x] = 1;
    }

    dfs(V);
    cout << endl;
    for (int i = 1; i <= N; i++)
        check[i] = 0;
    bfs(V);
    cout << endl;

}

```
