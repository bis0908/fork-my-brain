
---

- 비선형 데이터 구조.
- 루트 노드와 자식 노드들로 구성 되어 있다.

#### 이진 트리 (Binary tree)
- 각 노드들은 최대 2개의 자식을 가질 수 있다.
- 이진 탐색 트리
	- 일종의 순서를 가진 정렬된 트리
	- 부모의 왼쪽 노드: 부모보다 작아야 함.
	- 오른쪽 노드: 부모보다 커야 함.
	- 즉 비교가 가능한 데이터 종류에만 사용할 수 있음.

#### 너비 우선 탐색
- 큐 자료구조 사용

- 큐(배열일 수 있음)와 방문한 노드의 값을 저장할 변수를 만듭니다.
- 루트 노드를 큐에 배치합니다.
- 큐에 항목이 있는 한 반복합니다.
- 큐에서 노드를 대기열에서 해제하고 노드 값을 노드를 저장하는 변수에 푸시합니다.
- 큐에 대기 중인 노드에 왼쪽 프로퍼티가 있는 경우 - 해당 프로퍼티를 큐에 추가합니다.
- 큐에 대기 중인 노드에 오른쪽 프로퍼티가 있는 경우 - 해당 프로퍼티를 큐에 추가합니다.
- 값을 저장하는 변수를 반환합니다.
- code
```js
bfs() {
    const queue = []; // FIFO. 1. push() -> 2. shift();
    const visited = [];
    let current = this.root;

    queue.push(current);

    while (queue.length !== 0) {
      const dequeued = queue.shift();
      visited.push(dequeued.value);

			if (dequeued.left) {
        queue.push(dequeued.left);
      }
      if (dequeued.right) {
        queue.push(dequeued.right);
      }
    }
    return visited;
  }
```


#### 깊이 우선 탐색
- 스택 자료구조 사용 (재귀함수)
- 깊이 우선의 종류
	- 정위
	```js
	dfsInOrder() {
    const data = [];

    function traverse(node) {
      if (node.left) {
        traverse(node.left);
      }

      data.push(node.value);

      if (node.right) {
        traverse(node.right);
      }
    }
    traverse(this.root);

    return data;
  }
```
	- 후위
	```js
	dfsPostOrder() {
    const data = [];

	function traverse(node) {
      if (node.left) {
        traverse(node.left);
      }
      if (node.right) {
        traverse(node.right);
      }
      data.push(node.value);
    }
    traverse(this.root);

		return data;
  }
```
	- 전위
```js
dfsPreOrder() {
    const data = [];
    function traverse(node) {
      data.push(node.value);
      if (node.left) {
        traverse(node.left);
      }
      if (node.right) {
        traverse(node.right);
      }
    }
    traverse(this.root);

    return data;
  }
```
- 

