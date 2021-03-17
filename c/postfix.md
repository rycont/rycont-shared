
작성자 : [한우영](https://github.com/cokia/)
## 후위표기식 (Stack 02) - 자료구조 A

### 후위 표기식이란?

우리가 일반적으로 사용하는 사칙연산은 피연산자(숫자)사이에 연산자(+-*/)가 들어가는 형태로 **‘중위표기식(infix expression)’** 이라고 한다.  (예를 들면 `4+3` 의 형태이다)

그러나 후위표기식은 **피연산자가 먼저쓰이고, 그 뒤로 연산자가 나오는 형태**를 말한다. 

예를들어, `4+3` 의 중위표기식을 후위표기식으로 바꾼다면 `4 3 +` 으로 표현할 수 있다.

### 그럼 후위 표기식은 왜 쓰는걸까? 

일반적으로 우리가 사용하는 방법이 아니라서 익숙하지 않을뿐, 
실제로 후위 표기식은 괄호나 사칙연산의 우선순위를 생각하지 않아 훨씬 직관적인 표기법이다. 

중위표기식에서 4*7+2라는 연산을 진행할 때, 7+2를 먼저 연산하고 싶다면, 괄호를 사용할 수 밖에 없다. => `4*(7+2)`

하지만 후위표기식으로 표현한다면 `4 7 2 + *` 로 표현할 수있다. 그냥 나와있는 순서대로 계산만 하면 되는것이다 ! 

### 후위식으로의 변환

1. (A+B) * (C-D)를 후위식으로 바꾼다.
    1. 괄호를 다 친다. 연산의 우선순위에 맞아야 한다.

        ((A+B) * (C-D))

    2. 닫는 괄호 바로 앞으로 연산자들을 옮겨준다.

        ((A B +) (C D -) * )

    3. 괄호를 제거한다.

        A B + C D - *

*전위식은 연산자를 옮길 때 여는 괄호 바로 뒤로 옮긴다.

여담으로, 보통 우리가 프로그래밍을 할때도 `exec()` 나 `eval()` 함수를 사용해서 수식 계산을 하는 경우가 많은데, 사실 이러한 명령어들을 통해서 계산하는것도 중위식을 사용할때랑, 전위 / 후위표기식을 사용해서 할때랑 아주 작은 차이이긴 하지만 전/후위 표기식이 조금 빠르다. 이 이유를 찾아보니 intel 아키텍쳐를 기준으로 OoOE(Out-of-Order-Execution) 에 따라 순차적으로 명령을 실행하지 않는데, 이 과정에서 괄호 연산을 위한 조건 분기가 분기예측을 지연시키고, 이에 따른 성능 저하가 발생한다는 것이다. 

그런데 왜 우리는 프로그래밍을 할때 중위표현식을 사용할까?
사실 위에서 말한 분기예측으로 인한 실행시간 차이는 정말로 미미하고,  개발자가 쓰기 귀찮은 부분이 가장 큰 이유라고 생각한다. 

<img style="width: 150px;" alt="대충 이런 생각으로 그냥 쓰는거 아닐까(디시콘)" src="https://user-images.githubusercontent.com/24792377/102461714-2416b880-408c-11eb-9514-f403ae62d96e.png">

각설하고, 본론으로 돌아와 코드를 분석해보자.

```c
typedef struct{
	int item;
	struct StackNode* link;
} StackNode; 
// 먼저 StackNode 라는 이름을 가진 구조체를 선언한다.

typedef struct{
	StackNode* top;
} LinkedStackType;
// 마찬가지로 LinkedStackType 이라는 이름을 가진 구조체를 선언한다.


// 스택 초기화 함수
void init(LinkedStackType* s){
	s->top = NULL; 
	//stack 의 탑을 Null로 지정한다
}

// 스택이 비어있는지 검사하는 함수
int is_empty(LinkedStackType* s){
	return (s->top == NULL); 
	// Stack 의 top 이 NULL인지 여부를 반환한다 
}

// 스택에 노드를 삽입하는 함수
int push(LinkedStackType* s, int item){

	StackNode* temp = (StackNode*)malloc(sizeof(StackNode)); 
	// StackNode의 형태를 가지는 구조체 temp를 선언한다.
	if (temp == NULL)	// 메모리 할당이 되지 않았으면 에러를 발생시킴.
		return -1;

	else {	// 할당이 됐으면 기존 노드와 연결시켜준다.
		temp->item = item;//s->stk[s->ptr] = x;
		temp->link = s->top; 
		s->top = temp;
		return 0;
	}
}

// 스택에 마지막 값을 리턴하고 제거하는 함수
int pop(LinkedStackType* s){
	if (is_empty(s)){	
		// 노드가 비었으면 실행시키지 않는다.
		printf("스택이 비어있음.\n");
		exit(1);
	}
	else{	// 노드가 존재하면 최상위 노드를 불러내어 값을 리턴하고 제거함.
		StackNode* temp = s->top;
		int item = temp->item;

		s->top = s->top->link; //스택의 탑을 기존에 있던 탑의 링킹되어 있던 노드로 변경함 (기존 탑의 이전 노드)
		free(temp);  
		return item;
	}
}

// 스택 마지막 값을 확인, 리턴하는 함수
int peek(LinkedStackType* s)
{
	if (is_empty(s)){	// 노드가 비었으면 실행시키지 않는다.
		printf("스택이 비어있음.\n");
		exit(1);
	}
	else{	// 최상위 노드의 값을 리턴함
		return s->top->item;
	}
}

// 연산자의 우선순위를 반환하는 함수
int prec(char op){
	switch (op){	// 각각의 우선순위를 0,1,2로 리턴시킴.
		case '(': case ')':
			return 0; // 괄호의 경우 0
		case '+': case '-':
			return 1; // 더하기, 빼기 는 1
		case '*': case '/':
			return 2; // 곱하기, 나누기는 2
	} //우선순위가 
	return -1;
}

// 입력받은 중위표기된 배열을 후위표기로 바꿔주는 함수. 
//첫번째 인자인 exp 배열은 변환할 중위표현식이고, 두번째 인자인 str은 변환된 후위식을 반환할 배열
void infix_to_postfix(char exp[], char str[]) {
	int i = 0, k = 0;	// i : loop문 제어 변수, k : 배열 위치에 관한 변수
	char ch, top_op;	// ch : 배열문자가 저장될 임시변수, top_op : 스택값이 저장될 임시변수
	int len = strlen(exp);	// 배열의 길이를 저장할 변수
	StackNode* s; //스택 선언 

	init(&s);	// 스택 초기화
	for (i = 0; i < len; i++) {
		ch = exp[i];	// 입력받은 배열을 1칸씩 전진시키면서 문자를 확인하고
		switch (ch){	// 아래의 case문대로 조건실행, 혹은 default에 따라 처리함.
			case '+': 
			case '-': 
			case '*': 
			case '/':	// 연산자 (위에 있는 +,-,*,/ 에 대해 동일하게 실행)
			// 스택에 있는 연산자의 우선순위가 더 크거나 같으면 출력
				while (!is_empty(&s) && (prec(ch) <= prec(peek(&s)))) { 
					str[k++] = pop(&s); 
					//printf("%c", pop(&s));
				}
				push(&s, ch);
				break;
			case '(':
				push(&s, ch); // 괄호인 경우 스택에 푸시 
				break;
			case ')':
				top_op = pop(&s); // ) 삭제 
				while (top_op != '(') { // 왼쪽 괄호를 만날 때까지 출력
					str[k++] = top_op; // str에 괄호를 만나기 전까지 삽입
					//printf("%c", top_op); 
					top_op = pop(&s); // 팝 (삭제)
				}
				break;
			default: //숫자인 경우 
				str[k++] = ch;  // str에 삽입
				//printf("%c", ch); 
				break;
		}
	}
	while (!is_empty(&s)){ // 스택이 빌때까지 반복 
		str[k++] = pop(&s); // 스택에 들어있는 내용으로 str로 이동
		//printf("%c", pop(&s));
	}
}

// 후위표기식을 연산하는 함수
int eval(char exp[]){
	int op1, op2, value, i = 0;	
	// op1,2 : 연산될 숫자를 pop한 변수, value : 연산될 숫자를 push할 변수, i : loop문 제어 변수
	char ch;	// 배열 문자가 저장될 임시 변수
	StackNode* s; // 스택 선언

	init(&s);	// 스택 초기화
	for (i = 0; exp[i] != '\0'; i++){
		// 여기서 for문의 두번째 인자는 for문이 계속 도는 조건으로, char 형태의 배열의 마지막인 
		// '\0' 이 str[i]의 값이 아닐때 까지 반복해서 실행 ⇒ 결론적으로 str의 갯수만큼 출력.
		ch = exp[i]; // 임시변수에 저장 
		if (ch != '+' && ch != '-' && ch != '*' && ch != '/'){ 
		//만약 ch에 들어간 exp[i] 값이 숫자일 경우 (부호가 아닐경우)
			value = ch - '0';		// int형으로 변환
			push(&s, value); 		// 스택에 푸시
		}
		else{ // ch(exp[i]) 가 부호일 경우
			op2 = pop(&s); // 제일 마지막으로 저장된 숫자 호출 
			op1 = pop(&s); // 제일 마지막에서 다음으로 저장된 숫자 호출
			switch (ch){	// 연산자를 조사하여 그에 맞게 처리함.
			case '+': push(&s, op1 + op2); break;
			case '-': push(&s, op1 - op2); break;
			case '*': push(&s, op1 * op2); break;
			case '/': push(&s, op1 / op2);break;
			}
		}
	}
	return pop(&s);
}


int main()
{
	char* data;	// 입력받을 중위표기식이 저장될 char형 포인터변수
	char str[100] = { '\0' };	// 바뀐 후위표기식을 저장할 char형 배열
	int result;	// 연산값이 저장될 변수
	int i = 0;	// loop문 제어변수
	data = (char*)calloc(100, sizeof(int));	// 배열의 크기 할당
	printf("후위표기식으로 바꿀 중위표기식을 입력하세요. : ");
	gets(data);


	infix_to_postfix(data, str);	// 후위표기전환함수 호출

	printf("\n%s\n", str);
	for (i = 0; str[i] != '\0'; i++)	// str배열에 제대로 들어갔는지 확인.
	//여기서 for문의 두번째 인자는 for문이 계속 도는 조건으로, char 형태의 배열의 마지막인 
	// '\0' 이 str[i]의 값이 아닐때 까지 반복해서 실행 ⇒ 결론적으로 str의 갯수만큼 출력.
	// i <= strlen(str) 로 바꿀 수 있음.
	{
		printf("str[%d] = %c\n", i, str[i]);
	}
	result = eval(str);	// 후위표기식 연산함수 호출
	printf("\n계산 결과 : %d\n", result);

	return 0;
}
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUyNTk1Mzk4OF19
-->