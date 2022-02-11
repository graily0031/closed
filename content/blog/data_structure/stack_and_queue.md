---
title: [자료구조] 스택(stack) & 큐(queue)
date: "2022-02-12T00:00:00.000Z"
template: "blog"
draft: false
slug: "/blog/stack_and_queue/"
category: "data_structure"
description: ""
---

## 스택 (stack)
스택이란 쌓아올린다는 것을 의미하며, 원반 던지기 하듯 차곡차곡 쌓아올린 형태의 자료 구조를 의미한다.   
같은 구조와 크기의 자료를 정해진 방향으로만 쌓을 수 있고 top으로 정한 곳을 통해서만 접근할 수 있다. (쉽게 말해 입구가 하나밖에 없다 생각하면 쉽다.)   
top에는 가장 최근에 들어온 자료를 가리키고 있고 새 자료를 넣을 때 top이 가리키는 자료의 위에 쌓이게 된다. 삭제할 때 마찬가지로 top을 통해 가장 최근에 넣은 자료 먼저 삭제된다.   
top을 통해 삽입하는 연산을 `push`, 마찬가지로 top을 통해 삭제하는 연산을 `pop`이라 한다. 이러한 스택의 구조를 `후입 선출` 구조라 하며 줄여서 `LIFO(last in first out)`이라 부른다.   
만일 스택 저장공간에 데이터가 없는데 프로그램이 스택에서 데이터를 꺼내려고 할 경우 `stack underflow` 현상이 일어나게 되며, 반대로 스택 저장공간이 가득찼을 때 하나의 데이터를 더 넣고자 할 경우 `stack overflow` 현상이 일어나게 된다. 이 두 현상은 프로그램에 오류를 야기하게 된다.

### 스택 활용 예
* 웹 브라우저 뒤로가기
* 역순 문자열 만들기
* 후위 표기법 계산
* 실행 취소(undo)...등등

### 스택 예시
``` c++
#include <stdio.h>
#define MAX_SIZE 5


int stack[MAX_SIZE];
int top = 0;


void push(int val) {
	if(top >= MAX_SIZE)
	{
		printf("----overflow----");
	}
	else
	{
		stack[top] = val;
		top ++;
	}
}

void pop() {
	if (top == 0)
	{
		printf("----underflow");
	}
	else
	{
		printf("pop_%d\n", stack[top - 1]);
		stack[top - 1] = 0;
		top--;
	}
}

void show() {
	int n;
	printf("스택----------\n");
	for(n = 0; n < MAX_SIZE; n++)printf("%d\t", stack[n]);
	
	printf("\n");
}

int main() {
	int act = 0;
	int data;
	
	printf("명령어를 입력하세요\n");
	printf("1.push\t 2.pop\t 3.exit \n");
	scanf("%d", &act);
	
	switch(act)
	{
		case 1:
			printf("insert push val : ");
			scanf("%d", &data);
			push(data);
			break;
		
		case 2:
			pop();
			break;
			
		case 3:
			act = 4;
			break;
			
		default:
			break;
	}
	show();
}
```

## 큐 (queue)
큐는 예를 들어 은행의 번호표를 생각하면 쉽다. 창구에선 가장 먼저 번호표를 뽑은 사람의 업무를 처리해주는 것 처럼 선입선출 방식 즉 줄여서 `FIFO(first in first out)`방식의 자료구조이다.   
쉽게 생각해서 큐의 입구는 두개가 있다고 가정하면 된다. 한 쪽 끝에선 삽입 작업을 하고 다른 한 쪽 끝에선 삭제 작업이 이루어 지고 있다.   
삭제 연산만 수행되는 곳을 `front`, 삽입 연산만 수행되는 곳을 `rear`로 정해 각각의 연산작업만 수행된다.   
`rear`에서 이루어지는 삽입연산을 `enQueue`, `front`에서 이루어지는 삭제연산을 `deQueue`라 부른다.   
접근 방법은 위에 설명했다 시피 가장 첫 원소와 끝 원소로만 가능하다. 즉 `front`에서는 가장 먼저 큐에 들어온 `첫 번째 원소`가 되며, `rear`에서는 가장 늦게 큐에 들어온 `마지막 원소`가 되는 것 이다.

### 큐 활용 예
* 은행 업무
* 프로세스 관리
* 너비 우선 탐색(BFS / breadth first search) 구현
* 캐시(cache) 구현
* 우선순위가 같은 작업의 예약 구현

### 큐 예시
``` c++
#include <stdio.h>
#define MAX_SIZE 5


int queue[MAX_SIZE];
int front = -1;
int rear = -1;



int isEmpty(void) {
	if (front == rear)
	{
		return true;	
	 } 
	else
	{
		return false;
	}
}

void push(int val) {
	if ((rear + 1) % MAX_SIZE == front)
	{
		printf("----isFull----\n");
	}
	else
	{
		queue[++rear] = val;
	}
}

int pop() {
	if(isEmpty())
	{
		printf("----isEmpty---\n");
	}
	else
	{
		front = (front + 1) % MAX_SIZE;
		return queue[front];
	}
}


int main() {
	int act = 0;
	int data;
	
	printf("명령어를 입력하세요\n");
	printf("1.push\t 2.pop\t 3.exit \n");
	scanf("%d", &act);
	
	switch(act)
	{
		case 1:
			printf("insert push val : ");
			scanf("%d", &data);
			push(data);
			break;
		case 2:
			printf("pop()----->");
			printf("%d\n", pop());
			break;
			
		case 3:
			act = 4;
			break;
			
		default:
			break;
	}
}
```