---
title: List逆序的递归和迭代
date: 2019-10-01 23:24:24
tags: c++
categories: c++
---

List的逆序可以有递归和迭代两种方法, 这两种方法的C++实现如下.
<!--more-->

```c++
#include <iostream>

struct Node {
    int data;
    Node *next;
};

void printNode(Node *node) {
    Node *p = node;
    while (p != nullptr) {
        std::cout << p->data << ",";
        p = p->next;
    }
    std::cout << "\n";
}

Node * reverseNode(Node *node) {
    Node *next = nullptr;
    Node *prev = nullptr;

    while (node != nullptr) {
        next = node->next;
        node->next = prev;

        prev = node;
        node = next;
    }

    return prev;
}

Node * reverseNodeByRecursion(Node *node) {
    if (node == nullptr || node->next == nullptr) {
        return node;
    }

    Node *head = reverseNodeByRecursion(node->next);
    node->next->next = node;
    node->next = nullptr;

    return head;
};

int main() {
    Node *head = new Node;
    head->data = 0;
    head->next = nullptr;

    Node *p = head;
    for (int i = 1; i < 10; ++i) {
        p->next = new Node;
        p = p->next;
        p->data = i;
        p->next = nullptr;
    }
    printNode(head);

    head = reverseNode(head);
    printNode(head);

    head = reverseNodeByRecursion(head);
    printNode(head);

    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```
