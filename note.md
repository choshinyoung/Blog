https://cppreference.com/
https://docs.python.org/3/reference/index.html
https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/
## 약수 개수 구하기
```java
int counterFactors(int n) {
    int count = 0;

    for (int i = 1; i <= n; i++) {
        if (n % i == 0) {
            count++;
        }
    }

    return count;
}
```
## 소수 확인하기
```java
boolean isPrime(int n) {
    for (int i = 2; i <= Math.sqrt(n); i++) {
        if (n % i == 0) {
            return false;
        }
    }

    return true;
}
```
## 에라토스테네스의 체
```java
// 1부터 N까지의 수에서 소수 판별

Boolean[] isPrime = new Boolean[N + 1];
Arrays.fill(isPrime, true);
isPrime[0] = false;
isPrime[1] = false;

for (int i = 2; i <= Math.sqrt(N); i++) {
    if (!isPrime[i]) {
        continue;
    }

    for (int j = i * 2; j <= N; j += i) {
        isPrime[j] = false;
    }
}
```
## 마루의 팩토리얼 채점기
```java
// N을 입력받아 N!의 오른쪽 끝의 0의 개수(10이 곱해진 횟수)를 계산
int N = sc.nextInt();
int count = 0;

for (int i = 5; i <= N; i++) {
    // 10이 곱해지면 0을 하나씩 추가. 2의 배수는 5보다 빈도수가 높으니 고려하지 않아도 됨
    for (int j = i; j % 5 == 0; j /= 5) {
        count++;
    }
}

System.out.println(count);
```
## 최대공약수, 최소공배수
```java
// 최대공약수
static int gcd(int a, int b) {
    int r = a % b;

    if (r == 0) {
        return b;
    } else {
        return gcd(b, r);
    }
}

// 최소공배수
static int lcm(int a, int b) {
    return a * b / gcd(a, b);
}
```
## 배낭 문제
```cpp
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

int main() {
  ios_base::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);

  int N, M;
  cin >> N >> M;

  int P[N + 1], C[N + 1];
  for (int i = 1; i <= N; i++) {
    cin >> P[i] >> C[i];
  }

  int dp[N + 1][M + 1];
  for (int i = 0; i < M + 1; i++) {
    dp[0][i] = 0;
  }

  for (int i = 1; i < N + 1; i++) {
    for (int w = 0; w < M + 1; w++) {
      if (C[i] <= w) {
        dp[i][w] = max(dp[i - 1][w], dp[i - 1][w - C[i]] + P[i]);
      } else {
        dp[i][w] = dp[i - 1][w];
      }
    }
  }

  // cout << dp[N][M] << endl;

  // backtracking
  vector<int> result;
  int w = M;
  for (int i = N; i > 0; i--) {
    if (dp[i][w] != dp[i - 1][w]) {
      result.push_back(i);
      w -= C[i];
    }
  }
  reverse(result.begin(), result.end());

  if (result.empty()) {
    cout << "-1\n";
  } else {
    cout << result.size() << endl;
    for (int i : result) {
      cout << i << " ";
    }
    cout << endl;
  }
}
```
## N-Queen
```cpp
#include <iostream>
#include <vector>

using namespace std;

int N;
int result = 0;
vector<int> colPos;

void dfs(int row) {
  if (row == N) {
    result++;
    return;
  }

  for (int col = 0; col < N; col++) {
    bool avail = true;
    for (int i = 0; i < row; i++) {
      if (colPos[i] == col || abs(colPos[i] - col) == row - i) {
        avail = false;
        break;
      }
    }

    if (avail) {
      colPos[row] = col;
      dfs(row + 1);
    }
  }
}

int main() {
  ios_base::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);

  cin >> N;
  colPos.resize(N);
  dfs(0);

  cout << result << endl;
}
```
## 최소 스패닝 트리
```cpp
#include <iostream>
#include <queue>
#include <utility>
#include <vector>

using namespace std;

int main() {
  ios_base::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);

  int N;
  cin >> N;

  vector<pair<int, int>> adj[N];

  for (int i = 0; i < N; i++) {
    for (int j = 0; j < N; j++) {
      int a;
      cin >> a;

      if (i == j) {
        continue;
      }

      adj[i].emplace_back(pair{-a, j});
    }
  }

  bool visited[100] = {false};

  priority_queue<pair<int, int>> q;
  q.push(pair{0, 0});

  int mst = 0;

  while (!q.empty()) {
    int u = q.top().second, w = -q.top().first;
    q.pop();

    if (!visited[u]) {
      visited[u] = true;
      mst += w;

      for (pair<int, int> next : adj[u]) {
        q.push(next);
      }
    }
  }

  cout << mst << endl;
}
```
## 벨먼-포드
[나무위키](https://namu.wiki/w/%EB%B2%A8%EB%A8%BC-%ED%8F%AC%EB%93%9C%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
```java
BellmanFord(G,w,s):
//초기화 과정
for each u in G.V:     //노드를 초기화 하기
      distance[v] = inf      //모든 노드의 최단거리를 무한으로 지정
      parent[v] = null       //모든 노드의 부모 노드를 널값으로 지정
distance[s] = 0 //출발점의 최단거리는 0으로 지정한다
//거리측정 과정
for i from 1 to len(G.V):   //노드의 숫자만큼
     for each (u,v) in G.E:   //모든 변을 체크해 최단 거리를 찾아본다.
          if distance[u] + w[(u,v)] < distance[v]:   
          //만약 u를 경유하여 v로 가는 거리가 현재 v의 최단 거리보다 짧으면
               distance[v] = distance[u] + w[(u,v)]  //그 거리를 v의 최단거리로 지정
               parent[v] = u   //u를 v의 부모 노드로 지정
//음수 사이클 체크 과정
for each (u,v) in G.E:
     if distance[u] + w[(u,v)] < distance[v]:
          return false //음수 사이클을 확인하고 알고리즘을 정지
return distance[], parent[]
```
## 다익스트라
```fs
let [| s; e |] = Console.ReadLine().Split() |> Array.map int

let dist = Array.create (n + 1) Int32.MaxValue
dist[s] <- 0

let queue = PriorityQueue<int, int>()
queue.Enqueue(s, 0)

while queue.Count > 0 do
    let s' = queue.Dequeue()

    for e', x in vertexes[s'] do
        if dist[e'] > dist[s'] + x then
            dist[e'] <- dist[s'] + x
            queue.Enqueue(e', dist[e'])
```
## 위상 정렬
```fs
let rec dfs u =
    if not (visited[u]) then
        graph[u] |> List.iter dfs

        visited[u] <- true
        result <- u :: result

[ 1..n ] |> List.iter dfs
```
## Union-find
```fs
let parents = Array.init (n + 1) id

let rec find a =
    if parents[a] = a then
        a
    else
        parents[a] <- find parents[a]
        parents[a]

let union a b =
    let a' = find a
    let b' = find b

    if a' <> b' then
        parents[b'] <- a'
```
