# 나무 자르기
상근이는 나무 M미터가 필요하다. 근처에 나무를 구입할 곳이 모두 망해버렸기 때문에, 정부에 벌목 허가를 요청했다. 정부는 상근이네 집 근처의 나무 한 줄에 대한 벌목 허가를 내주었고, 상근이는 새로 구입한 목재절단기를 이용해서 나무를 구할것이다.

목재절단기는 다음과 같이 동작한다. 먼저, 상근이는 절단기에 높이 H를 지정해야 한다. 높이를 지정하면 톱날이 땅으로부터 H미터 위로 올라간다. 그 다음, 한 줄에 연속해있는 나무를 모두 절단해버린다. 따라서, 높이가 H보다 큰 나무는 H 위의 부분이 잘릴 것이고, 낮은 나무는 잘리지 않을 것이다. 예를 들어, 한 줄에 연속해있는 나무의 높이가 20, 15, 10, 17이라고 하자. 상근이가 높이를 15로 지정했다면, 나무를 자른 뒤의 높이는 15, 15, 10, 15가 될 것이고, 상근이는 길이가 5인 나무와 2인 나무를 들고 집에 갈 것이다. (총 7미터를 집에 들고 간다)

상근이는 환경에 매우 관심이 많기 때문에, 나무를 필요한 만큼만 집으로 가져가려고 한다. 이때, 적어도 M미터의 나무를 집에 가져가기 위해서 절단기에 설정할 수 있는 높이의 최댓값을 구하는 프로그램을 작성하시오.  

입력
첫째 줄에 나무의 수 N과 상근이가 집으로 가져가려고 하는 나무의 길이 M이 주어진다. (1 ≤ N ≤ 1,000,000, 1 ≤ M ≤ 2,000,000,000)

둘째 줄에는 나무의 높이가 주어진다. 나무의 높이의 합은 항상 M을 넘기 때문에, 상근이는 집에 필요한 나무를 항상 가져갈 수 있다. 높이는 1,000,000,000보다 작거나 같은 양의 정수 또는 0이다.

출력
적어도 M미터의 나무를 집에 가져가기 위해서 절단기에 설정할 수 있는 높이의 최댓값을 출력한다.

## Example1

```
Input: 
4 7
20 15 10 17

Output: 
15
```
## Example2

```
Input: 
4 0
1 4 5 7


Output: 
7
```


## trial1
### Intuition
```
공유기 문제와 비슷하게 이분탐색을 사용하여 문제를 해결하였다. 처음에는 while문에 lt와 rt가 같은경우에 빠져나오도록
이분탐색을 진행하였더니 rt값의 처음 초기화한 값(maximum값)이 답으로 나와야 하는경우 제대로된 값이 나오지
않게 되었다.
만약에 mid로 나무길이를 정해준 후에 해당 나무길이로 자른 나무개수를 모두 더한 후 그 sum한 값이 M보다
큰경우 잘라야하는 나무길이(mid)는 답이 될 가능성이 있다. 하지만 잘라야하는 나무길이는 최대한 덜자르도록 해야 하기 때문에 mid값은 최소가 되도록 해야한다.
그러므로 sum으로 자르고 난 후에 모을 수 있는 나무길이를 모두 더해서 저장해놓고 M과 비교하여 sum이 크가나
같다면 res보다 mid값이 더 큰경우 res를 mid로 업데이트 시켜주고
mid가 res보다 작은 경우에는 남겨둬야하는 나무 길이가 더 길어야하므로 
```
### Codes  
```cpp
int main() {
    freopen("나무자르기.txt", "r", stdin);
    long long N, M;
    cin >> N >> M;
    long long mx;
    vector<long long> v;
    for (int i = 0; i < N; i++) {
        long long tmp;
        cin >> tmp;
        v.push_back(tmp);
        mx = max(mx, tmp);
    }
    long long lt=0, rt=mx;
    long long mid;
    long long res=0;
    while (lt <= rt) {
        mid = (lt + rt) / 2;
        long long sum = 0;
        for (int i = 0; i < N; i++) {
        if (v[i] >= mid) {//나무 길이가 mid보다 큰경우
            sum += v[i] - mid;//뺀거 더하기
            }
        }
        if (sum >= M) {//나무가 더 높아져도 될 경우
            if(res<mid) res = mid;
            lt = mid + 1;
        }
        else {//나무가 부족한경우
            rt = mid-1;
        }
    }
    cout << res;
    return 0;
}
```

### Results (Performance)  
**Runtime:** 668 ms 
**Memory Usage:** 	14284 kb 

<p align="center"> 
<img src="./capture.JPG">
</p>


### 문제 URL (백준)  
https://www.acmicpc.net/problem/2805