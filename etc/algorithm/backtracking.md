[🏠 처음으로](/README.md)

# BackTracking

> 1. 모든 가능한 해를 탐색하는 알고리즘의 한 종류
> 1. 재귀적으로 가능한 모든 경우의 수를 탐섹
>     1. 각 단계에서 조건에 따라
>          1. 계속 진행
>          1. 탐색을 중지하고 다음 분기로 넘어가서(되돌아가서) 다시 탐색

<br>

## 특징

- 재귀적 접근: 재귀적으로 탐색
- 가지치기
    1. 가능성이 없는 경로는 중간에 탐색을 중단한다
    2. 다른 경로로 넘아간다
    - 불필요한 탐색을 줄이고 효율성을 높임
- 상태 공간 트리
    - 문제를 트리 구조로 생각
    - 각 노드는 가능한 해의 상태를 나타냄
    - 백 트래킹은 이 트리에서 DFS로 동작

## 예시: N-Queens 문제

> N*N 체스판에서 N개의 퀸이 서로 공격할 수 없도록 배치하는 방법을 찾는 문제

### 단계

1. 첫번째 행부터 시작해서, 각 행에 퀸을 놓는다고 가정
1. 퀸을 놓을 때, 이전에 놓인 퀸과 경로가 겹치는지 확인
    1. 만약 겹친다면, 다른 위치에 시도
1. 만약 특정행에서 더이상 퀸을 놓을 수 있는 위치가 없다면, 이전 행으로 돌아가(백트랙) 퀸을 다른 위치에 놓고 다시 시도
1. 가능한 모든 경우의 수를 탐색해서 조건에 만족하는 해를 찾음

        def is_safe(board, row, col, N):
            # 같은 열에 다른 퀸이 있는지 확인
            for i in range(row):
                if board[i][col] == 1:
                    return False

            # 좌측 대각선 위쪽에 다른 퀸이 있는지 확인
            for i, j in zip(range(row, -1, -1), range(col, -1, -1)):
                if board[i][j] == 1:
                    return False

            # 우측 대각선 위쪽에 다른 퀸이 있는지 확인
            for i, j in zip(range(row, -1, -1), range(col, N)):
                if board[i][j] == 1:
                    return False

            return True

        def solve_n_queens_util(board, row, N):
            # 모든 퀸을 다 놓은 경우 (기저 조건)
            if row >= N:
                print_board(board, N)
                print()  # 새로운 답안 사이에 빈 줄을 넣기 위함
                return

            # 모든 열에 퀸을 놓는 시도
            for i in range(N):
                if is_safe(board, row, i, N):
                    # 현재 위치에 퀸을 놓음
                    board[row][i] = 1

                    # 재귀적으로 다음 행에 퀸을 놓음
                    solve_n_queens_util(board, row + 1, N)

                    # 백트래킹: 퀸을 제거하고 다른 위치에 놓는 것을 시도
                    board[row][i] = 0

        def solve_n_queens(N):
            board = [[0 for _ in range(N)] for _ in range(N)]

            solve_n_queens_util(board, 0, N)

        def print_board(board, N):
            for i in range(N):
                for j in range(N):
                    print("Q" if board[i][j] == 1 else ".", end=" ")
                print()

        # 예시로 N=4인 경우 실행
        solve_n_queens(4)


## 예시: K개의 장애물이 있는 미로에서 최단 경로 찾기

- 입력:
    - `0`, `1`, `-1`로 이루어진 2차원 배열의 미로
        - `0`은 이동할 수 있는 통로
        - `1`은 이동할 수 없는 장애물
        - `-1`은 출구
  - `(0, 0)`에서 시작
  - 최대 K개의 장애물을 제거할 수 있음
  
- 출력: 
    - 시작점에서 출구까지의 최단 경로를 찾고, 그 길이를 출력
    - 경로가 존재하지 않으면 `-1`을 출력

### 접근 방식:

- 미로 탐색 문제와는 다르게, 장애물을 최대 K개까지 제거할 수 있다는 조건이 있음
- BFS(너비 우선 탐색)를 기본으로 사용
- 각 경로에서 제거된 장애물의 수를 함께 추적하여 탐색을 진행

### 문제 해결을 위한 Python 코드:

```python
from collections import deque

def is_within_bounds(x, y, n, m):
    return 0 <= x < n and 0 <= y < m

def shortest_path_with_obstacles(maze, k):
    n = len(maze)
    m = len(maze[0])

    # 방향벡터: 상, 하, 좌, 우
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    # BFS 큐: (x, y, 현재까지 제거한 장애물 수, 경로 길이)
    queue = deque([(0, 0, 0, 0)])

    # 방문 기록: (x, y)에서 k개 이하의 장애물을 제거하고 도달한 최소 경로 길이
    visited = [[[float('inf')] * (k + 1) for _ in range(m)] for _ in range(n)]
    visited[0][0][0] = 0

    while queue:
        x, y, obstacles_removed, steps = queue.popleft()

        # 출구에 도달한 경우
        if maze[x][y] == -1:
            return steps

        # 4가지 방향으로 이동
        for dx, dy in directions:
            nx, ny = x + dx, y + dy

            if is_within_bounds(nx, ny, n, m):
                if maze[nx][ny] == 0 and steps + 1 < visited[nx][ny][obstacles_removed]:
                    visited[nx][ny][obstacles_removed] = steps + 1
                    queue.append((nx, ny, obstacles_removed, steps + 1))

                elif maze[nx][ny] == 1 and obstacles_removed < k and steps + 1 < visited[nx][ny][obstacles_removed + 1]:
                    visited[nx][ny][obstacles_removed + 1] = steps + 1
                    queue.append((nx, ny, obstacles_removed + 1, steps + 1))

    # 출구에 도달할 수 없는 경우
    return -1

# 예시 미로
maze = [
    [0, 1, 0, 0, 0],
    [0, 1, 1, 1, 0],
    [0, 0, 0, 1, 0],
    [1, 1, 0, 0, 0],
    [0, 0, 0, 1, -1]
]

k = 2  # 최대 제거할 수 있는 장애물의 수

# 미로 문제 해결
result = shortest_path_with_obstacles(maze, k)
print(f"최단 경로 길이: {result}")
```

<br>

### 백트래킹과 BFS의 조합:

- BFS ➞ 너비 우선 탐색은 최단 경로를 찾는 데 매우 적합함  
- 방문 기록
    - `visited` 배열을 사용
        - 각 지점에 대해 장애물을 몇 개 제거하고 도달했는지를 기록
        - 더 나은 경로가 발견되면 기존 경로를 덮어쓰는 방식으로 중복 계산을 회피