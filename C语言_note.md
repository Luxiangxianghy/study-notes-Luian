# 1. 链表

## 1.1 链表概念

链表是一种 **动态数据结构**，由若干节点（Node）组成。  
每个节点一般包含两个部分：

- **数据域（data）**：存储实际数据  
- **指针域（next / prev）**：指向下一个或前一个节点  

链表的特点：

- 内存可以不连续（相比数组）  
- 插入和删除操作效率高  
- 查找元素需要从头节点开始遍历  

---

## 1.2 链表类型

链表主要有以下几种类型：

- **单向链表**：每个节点只有一个 `next` 指针，只能从头到尾遍历。  
- **双向链表**：每个节点有 `next` 和 `prev` 指针，可以双向遍历。  
- **循环链表**：链表尾节点指向头节点，形成环形结构。  

---

## 1.3 单向链表示例（C语言）

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node* next;
} Node;

// 创建节点
Node* createNode(int val) {
    Node* node = (Node*)malloc(sizeof(Node));
    node->data = val;
    node->next = NULL;
    return node;
}

// 打印链表
void printList(Node* head) {
    Node* cur = head;
    while(cur) {
        printf("%d -> ", cur->data);
        cur = cur->next;
    }
    printf("NULL\n");
}

// 头插法
Node* insertAtHead(Node* head, int val) {
    Node* node = createNode(val);
    node->next = head;
    return node;
}

// 尾插法
Node* insertAtTail(Node* head, int val) {
    Node* node = createNode(val);
    if(!head) return node;
    Node* cur = head;
    while(cur->next) cur = cur->next;
    cur->next = node;
    return head;
}

// 删除节点
Node* deleteNode(Node* head, int val) {
    Node *cur = head, *prev = NULL;
    while(cur && cur->data != val) {
        prev = cur;
        cur = cur->next;
    }
    if(!cur) return head; // 未找到
    if(!prev) head = cur->next; // 删除头节点
    else prev->next = cur->next;
    free(cur);
    return head;
}

int main() {
    Node* head = NULL;
    head = insertAtHead(head, 10);
    head = insertAtTail(head, 20);
    head = insertAtTail(head, 30);

    printList(head); // 10 -> 20 -> 30 -> NULL
    head = deleteNode(head, 20);
    printList(head); // 10 -> 30 -> NULL
    return 0;
}
```

## 1.4 双向链表

## 1.5 双向链表
