1. 왜 해시값을 매번 빠꿔놨을까?

https://stackoverflow.com/questions/27522626/hash-function-in-python-3-3-returns-different-results-between-sessions

Python uses a random hash seed to prevent attackers from tar-pitting your application by sending you keys designed to collide. See the [original vulnerability disclosure](http://www.ocert.org/advisories/ocert-2011-003.html). By offsetting the hash with a random seed (set once at startup) attackers can no longer predict what keys will collide.

2. 해시는 진짜진짜로 항상빠를까요? 아니면 왜일까요?

   ## 해시는 진짜 빠른걸까?

   - https://www.acmicpc.net/board/view/24773

     > unordered_set이나 unordered_map은 해시를 사용합니다. 이들의 삽입/탐색/삭제가 모두 O(1)이라는 것은 어디까지나 평균이 그렇다는 것이고, 실제로 탐색이 소요하는 시간은 해시 탐색 과정에서 충돌이 얼마나 발생하는지에 달려있습니다. 여러 수가 같은 해시값을 가지고 있다면 그 중에서도 실제로 일치하는 값을 찾아낼 때까지 그 수들을 모조리 뒤져봐야 합니다.
     >
     > 이 문제는 입력이 굉장히 크기 때문에 웬만한 크기의 해시로는 상당한 양의 충돌을 피할 수 없고, 매우 큰 해시를 다루는 것 자체가 시간을 상당히 소요하는 일이 됩니다.