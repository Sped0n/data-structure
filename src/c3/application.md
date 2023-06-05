# 应用

## 验证括号的合法性

题目很简单就是输入一串字符，判断字符中的括号是否合法。直接上代码：

```c
#include <iostream>
#include <string.h>
using namespace std;

#define MAXSIZE 100

typedef struct CharStack {
    char data[MAXSIZE];
    int top;
}

void InitStack(CharStack &S) {
    S.top = -1;
}

bool StackEmpty(CharStack S) {
    return S.top == -1;
}

bool Push(CharStack &S, char e) {
    if (S.top == MAXSIZE - 1) {
        return false;
    }
    S.top += 1;
    S.data[S.top] = e;
    return true;
}

bool Pop(CharStack &S, char &e) {
    if (S.top == -1) {
        return false;
    }
    e = S.data[S.top];
    S.top -= 1;
    return true;
}

char GetTop(CharStack S) {
    if (StackEmpty(S)) {
        return NULL;
    }
    return S.data[S.top];
}

int main() {
    CharStack S;
    InitStack(S);
    string input;
    cin >> input;
    for (int i = 0; i < input.size(); i++) {
        if ((input[i] == '{') || 
            (input[i] == '[') || 
            (input[i] == '(')) {
            Push(&S, input[i]);
        } else if ((input[i] == '}') || 
                   (input[i] == ']') || 
                   (input[i] == ')')) {
            char c;
            if (!Pop(&S, &c)){
                exit(1);
            }
            if ((input[i] == ')' && c != '(') ||
                (input[i] == ']' && c != '[') || 
                (input[i] == '}' && c != '{')) {
                cout << "括号不匹配！" << endl;
                break;
            }
        }
    }
    return 0;
}
```

## 队列在计算机中的应用

1. 解决主机与外部设备之间速度不匹配的问题。
2. 解决由多用户引起的资源竞争问题。
