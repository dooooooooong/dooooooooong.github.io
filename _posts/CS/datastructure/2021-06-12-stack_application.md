### **스택의 적용**

스택은 arithmetic expression을 계산하는데 자주 활용됩니다. 그 중 대표적인 케이스들을 살펴봅시다.



**Infix, Postfix, Prefix expression**

- 중위 표기법 (Infix): 피연산자(숫자) 사이에 연산자(덧셈, 곱셈, 뺄셈, 나눗셈)가 있는 식  ex) **(A+B) \* C-D**
- 전위 표기법 (Prefix): 연산자가 피연산자 앞에 나오는 방식. ex) **- * + A B C**
- 후위 표기법 (Postfix): 연산자가 뒤에 오는 수식 ex) **A B + C * D -**





#### 1. Postfix to Fully parenthesized Infix

후위 표기법으로 서술된 산술식을 우리가 알기 편한 중위 표현식으로 바꿔주는 작업입니다. 스택을 이용하여 효율적으로 구성할 수 있습니다. 



**알고리즘**

1. postfix 산술식을 문자열으로 저장합니다. 
2. 문자를 하나씩 검사합니다.
   - 문자가 숫자(피연산자)인 경우: 스택에 저장합니다.
   - 문자가 연산기호(연산자)인 경우: 스택에서 두개의 숫자를 꺼내고 연산자를 적용합니다.



**예시**

```
Postfix: 1 2 3 + /
```



![postfixtoinfix](../../../assets/images/markdown_images/CS/datastructure/2021-06-12-stack_application/postfixtoinfix.gif)

<br>

<br>

<br>





```
Postfix: a b c - d + / e a - * c *
```



![postfixtoinfixexample](../../../assets/images/markdown_images/CS/datastructure/2021-06-12-stack_application/postfixtoinfixexample.gif)







**코드**

```c++
// postfix.cpp

#include <iostream>
#include <stack>
#include <cassert>
using namespace std;

#ifdef DEBUG
#define DPRINT(func) func;
#else
#define DPRINT(func) ;
#endif

// 출력부
template <typename T>
void printStack(stack<T> orig) {
    stack<T> temp;
    while (!orig.empty()) {
        temp.push(orig.top());
        orig.pop();
    }

    while (!temp.empty()) {
		cout << temp.top() << ' ';
        orig.push(temp.top());
        temp.pop();
    }
}


// 실제계산부분
string evaluate(string tokens) {
	stack<string> st;

	for (char token : tokens) {
		if (isspace(token)) continue;  // if token is a whitespace, skip it.
		DPRINT(cout << "token: " << token << endl;);
		DPRINT(printStack(st););

		// current token is a value(or operand), push it to st.
		if (token == '+' || token == '-' || token == '*' || token == '/') {
			string expression;
			expression += "(";

			string op1 = st.top();
			st.pop();

			string op2 = st.top();
			st.pop();

			expression += op2;
			expression += token;
			expression += op1;

			expression += ")";

			st.push(expression);
		}

		else {  // push the operand
			DPRINT(cout << "  push: " << token << endl;);
			// convert token(char type) to a string type and push it to the stack
			string s;
			s.push_back(token);
			st.push(s);
		}
	}

	// 계산후에는 한개의 element만 있어야합니다.
	assert(st.size() == 1);
	string expr = st.top();

	return expr;
}

// returns true if the tokens consist of '+', '-', '*', '/', spaces, and 
// digits (0 ~ 9), false otherwise.
bool is_numeric(string tokens) {

	string numeric = "+-*/ 0123456789";

	for (char token : tokens) {
		if (numeric.find(token) == string::npos) return false;
	}

	return true;
}

// returns a numeric value after evaluating the postfix evaluation.
double evaluate_numeric(string tokens) {
	stack<double> st;

	for (char token : tokens) {
		if (isspace(token)) continue;  // if token is a whitespace, skip it.
		DPRINT(cout << "token: " << token << endl;);

		// if token is a operator, evaluate; if a digit(or operand), push it to st.
		if (token == '+' || token == '-' || token == '*' || token == '/') {
			// checking the stack top() for right operand 
			// checking the stack top() for left oprand 
			// compute the expression and push the result

			double n1 = st.top();
			assert(!st.empty());
			st.pop();

			double n2 = st.top();
			assert(!st.empty());
			st.pop();

			if (token == '+') st.push(n2+n1);
			if (token == '-') st.push(n2-n1);
			if (token == '*') st.push(n2*n1);
			if (token == '/') {
				st.push(n2/n1);
				assert(n1 != 0);
			}
		}
		else { // push the operand (digit) in a value to the stack
			// convert token to a numeric data type and push it the stack
			double digit = token - '0';
			st.push(digit);
		}
	}

	assert(st.size() == 1);
	double expr = 0;
	expr = st.top();

	DPRINT(cout << "<evaluate() returns " << expr << endl;);
	return expr;
}
```



```c++
// driver.cpp
// compile : g++ postfix.cpp driver.cpp -o postfix
// run: ./infix

#include <iostream>
#include <string>
using namespace std;

string evaluate(string tokens);
double evaluate_numeric(string tokens);
bool is_numeric(string tokens);

int main() {
	setvbuf(stdout, nullptr, _IONBF, 0); // prevents the output from buffered on console.
	string expr;
	while (true) {
		cout << "\nEnter a postfix expression w/ or w/o spaces."  << endl;
		cout << "e.g.:  a b +, 2 3 4 * +, ab/5+ (q to quit): ";
		getline(cin, expr);
		if (expr[0] == 'q') break;

		cout << "postfix expr: " << expr << endl;
		cout << "  infix expr: " << evaluate(expr) << endl;
		if (is_numeric(expr))
			cout << "   evaluated: " << evaluate_numeric(expr) << endl;
	};

	string postx[] = {"2 3 4 * +", "a b * 5 +", "1 2 + 7 *", "a b c - d + / e a - * c *"};
	string infix[] = {"(2 + (3 * 4))", "((a * b) + 5)", "((1 + 2) * 7)",
	                  "(((a / ((b - c) + d)) * (e - a)) * c)"};
	for (auto expr : postx) {
		cout << "postfix expr: " << expr << endl;
		cout << "  infix expr: " << evaluate(expr) << endl;
		if (is_numeric(expr)) 
			cout << "   evaluated: " << evaluate_numeric(expr) << endl;
	}

	return EXIT_SUCCESS;
}
```





<br><br>



#### 2. Calculate Infix Expression

중위표현식으로 입력을 받았을 때 계산하는 것도 스택을 활용한 풀이법이 존재합니다. 

예를 들어 아래와 같은 식을 계산한다고 합시다.

$ 2^2 + 10 / 5 - 1 $



인간은 사칙연산에 대한 우선순위가 있기 때문에 손쉽게 5라는 것을 알 수 있지만 컴퓨터는 우선순위를 정해주고 규칙을 설정해주어야 합니다.



**알고리즘**

연산자 스택과 숫자 스택 2개를 사용합니다.



1. infix 산술식을 문자열으로 저장합니다. 

2. 문자를 하나씩 검사합니다.

   - 공백(whitespace): 무시합니다.

   - 왼쪽 괄호 (`(`): 연산자 스택에 `push`합니다.

   - 숫자: 여러 자리 숫자인지 아닌지 체크하고 숫자 스택에 `push`합니다.

   - 오른쪽 괄호 (`)`)

     - 연산자 스택의 맨 위가 왼쪽 왼쪽 괄호 (`(`)가 될 때까지
       - 연산자 스택에서 연산자를 `pop()` 하여 가져옵니다.
       - 숫자 스택에서 2개를 `pop()`하여 가져옵니다. 
       - 연산자를 올바른 순서로 숫자에 적용합니다.
       - 결과값을 숫자 스택에  `push`합니다. 
     - 연산자 스택에서 왼쪽 괄호를 `pop`합니다.

     

   - 연산자 (`+ - * / ^`)

     - 연산자 스택이 비워지고 연산자 스택의 맨 위의 항목이 **현재 연산자**보다 더 높거나 같은 우선순위를 가질 때까지
       - 연산자 스택에서 연산자를 `pop()` 하여 가져옵니다.
       - 숫자 스택에서 2개를 `pop()`하여 가져옵니다. 
       - 연산자를 올바른 순서로 숫자에 적용합니다.
       - 결과값을 숫자 스택에  `push`합니다. 
     - **현재 연산자**를 연산자 스택에 `push`합니다.



3. 연산자 스택이 비워질 때 까지

   1. 연산자 스택에서 연산자를 `pop()` 하여 가져옵니다.
   2. 숫자 스택에서 2개를 `pop()`하여 가져옵니다. 
   3. 연산자를 올바른 순서로 숫자에 적용합니다.
   4. 결과값을 숫자 스택에  `push`합니다. 

   

4. 숫자 스택의 맨 위 요소를 반환합니다. (이 시점에서 연산자 스택은 비어있어야하고 숫자 스택에는 단 1개의 값만 저장되어 있어야합니다. )





#### 3. Infix to Postfix

![image-20211103111313224](../../../assets/images/markdown_images/CS/datastructure/2021-06-12-stack_application/infixtopostfixtable.png)

<br>



**스택 활용** <br>연산자 스택, postfix string 사용



**1.** 피연산자가 나오면 posfix string에 추가한다.<br>**2.** 연산자가 나오면 <br>  \- opertor stack의 맨 위의 요소의 우선순위가 현재 입력된 연산자보다 낮을 때까지 pop하여 postfix string에 추가합니다. <br> \- 그다음 푸쉬 <br>

**3.** `)` 인 경우, `(` 를 만날 때 까지 모든 연산자를 postfix string에 추가한다. <br>**4.** infix string에 대해 검사가 끝났으면 operator stack이 빌때까지 pop하여 연산자를 postfix string에 추가한다.





```
infix: a – (b + c * d) / e
```

![image-20211103143943779](../../../assets/images/markdown_images/CS/datastructure/2021-06-12-stack_application/infixtopostfixexample-16428454164411.png) 



**code**

```c++
#include <iostream>
#include <stack>
#include <cctype>

using namespace std;

int priority (char oper) {
   if(oper == '+' || oper == '-') return 1;
   else if(oper == '*' || oper == '/') return 2;
   else return 0;

}

string infixTopostFix (string s) {
    stack <char> oper;
    oper.push('#'); // for underflow prevent
    string postfix;

    for (int i = 0; i < s.length(); i++) {

        if (isalnum(s[i])) postfix.push_back(s[i]); // operand
        if (s[i] == '(') oper.push(s[i]);
      
      	// operator pop
        if (s[i] == ')') {
            while (oper.top() != '(' && oper.top() != '#') {
                postfix.push_back(oper.top());
                oper.pop();
            }
            oper.pop();
        }
				
        if (s[i] == '+' || s[i] == '-' || s[i] == '*' || s[i] == '/') {
            if (priority(s[i]) > priority(oper.top())) oper.push(s[i]);
          
          	// operator prioirty caculation
            else {
                while (oper.top() != '#' && priority(s[i]) <= priority(oper.top())) {
                    postfix.push_back(oper.top());
                    oper.pop();
                }

                oper.push(s[i]);
            }
        }
    }

    while (oper.top() != '#') {
        postfix.push_back(oper.top());
        oper.pop();
    }

    return postfix;
}
```

