---
layout: post
category: "notes"
title:  "数据结构"
tags: [c]
---



# 链表

## 参考链表实现

```c
#include <stdio.h>
#include <stdlib.h>

#define OK 1
#define ERROR 0
typedef int ElemType, Status;

/* 线性表的单链表存储结构 */
typedef struct Lnode {
	ElemType data;
	struct Lnode *next;
} Lnode, *LinkList;
/* 定义LinkList */
//typedef struct Lnode *LinkList;


/* 初始化-头插法 */
Status CreateList_H(LinkList &L, int n)
{
	L = (LinkList)malloc(sizeof(Lnode));
	Lnode *s;
	L->next = NULL; //最后的结点指针域永远为NULL

	int i;
	ElemType ele;
	for(i = 0; i < n; i++) {
		scanf("%d", &ele);
		s = (Lnode*)malloc(sizeof(Lnode));
		s->data = ele;
		s->next = L->next; //将头指针指向下一个结点的地址，赋给新创建结点的next
		L->next = s; //将新创建结点的地址赋给头指针的下一个结点
	}

	return OK;
}


/* 初始化-尾插法 */
Status CreateList_T(LinkList &L, int n)
{
	L = (LinkList)malloc(sizeof(Lnode));
	L->next = NULL;

	Lnode *s;
	Lnode *r = L;

	int i;
	ElemType ele;
	for (i = 0; i < n; i++) {
		scanf("%d", &ele);
		s = (Lnode *)malloc(sizeof(Lnode));
		s->data = ele;
		r->next = s;
		r = s;
	}
	r->next = NULL;
	return OK;
}



/* 按照值查找 */
Lnode *GetList_Elem(LinkList L, ElemType find_ele)
{
	Lnode *node = L->next;
	while(node != NULL && node->data != find_ele) {
		node = node->next;
	}

	return node;
}


/* 按照位置查找 */
Lnode *GetList_Pos(LinkList L, int pos)
{
	if (pos == 0)
		return L;

	if (pos < 1)
		return NULL;

	Lnode *node = L;
	int j = 0;

	while (node && j < pos) {
		node = node->next;
		j++;
	}

	if (node == NULL) {
		return NULL;
	}

	return node;
}


/* 插入 */
Status InserElem(LinkList &L, int pos, ElemType inser_ele)
{
	Lnode *node;
	Lnode *new_node = (Lnode *)malloc(sizeof(Lnode));

	new_node->data = inser_ele;
	node = GetList_Pos(L, pos-1);

	new_node->next = node->next;
	node->next = new_node;

	return OK;
}


/* 删除 */
Status DeleteElem(LinkList &L, int pos, ElemType *del_ele)
{
	Lnode *node;
	Lnode *del_node;

	node = GetList_Pos(L, pos-1);
	del_node = node->next;
	node->next = del_node->next;

	*del_ele = del_node->data;
	free(del_node);

	return OK;
}



/* 输出 */
void PrintList(LinkList L)
{
	Lnode *node = L->next;
	printf("单链表元素为: ");
	while (node != NULL) {
		printf("%d\t", node->data);
		node = node->next;
	}
	printf("\n");
}



/* 合并 */
void MergeList(LinkList &LA, LinkList &LB, LinkList &LC)
{
	Lnode *anode = LA->next;
	Lnode *bnode = LB->next;
	Lnode *cnode;
	LC = cnode = LA; //用LA的头结点作为LC的头结点

	while(anode != NULL && bnode != NULL) {
		if (anode->data <= bnode->data) {
			cnode->next = anode;
			cnode = anode;
			anode = anode->next;
		} else {
			cnode->next = bnode;
			cnode = bnode;
			bnode = bnode->next;
		}
	}

	cnode->next = anode ? anode:bnode; //插入剩余段

	free(LB);
}



int main()
{
	LinkList L;
	ElemType inser_ele;
	Lnode *find_node;
	ElemType del_ele;
	int pos;

	printf("初始化链表L，请输出5个数字: ");
	CreateList_T(L, 5);
	PrintList(L);

	printf("请输入要插入元素及插入位置: ");
	scanf("%d %d", &inser_ele, &pos);
	InserElem(L, pos, inser_ele);
	PrintList(L);

	printf("请输入要删除元素的位置: ");
	scanf("%d", &pos);
	DeleteElem(L, pos, &del_ele);
	printf("所删除的元素为: %d\n", del_ele);
	PrintList(L);

	printf("请输入要查找的元素位置: ");
	scanf("%d", &pos);
	find_node = GetList_Pos(L, pos);
	printf("所查找的元素为:%d\n",find_node->data);

	printf("------接下来为单链表的合并---------\n");
	LinkList LA;
	LinkList LB;
	LinkList LC;
	printf("初始化链表LA，请输入5个数字：");
	CreateList_T(LA, 5);
	printf("初始化链表LB，请输入5个数字：");
	CreateList_T(LB, 5);
	MergeList(LA, LB, LC);
	PrintList(LC);

	return 0;
}
```



## 自实现

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

typedef struct node {
	int data;
	struct node *next;
} node;
typedef struct node *linklist;

int Createlist_H(linklist &L, int n)
{
	node *p;
	int i;

	L = (linklist)malloc(sizeof(node));
	L->next = NULL;

	for (i = 0; i < n; i++) {
		p = (node *)malloc(sizeof(node));
		p->data = rand()%100;

		p->next = L->next;
		L->next = p;
	}

	return 0;
}

int Createlist_T(linklist &L, int n)
{
	node *p, *r;
	int i;

	L = (linklist)malloc(sizeof(node));
	r = L;

	for(i = 0; i < n; i++) {
		p = (node *)malloc(sizeof(node));
		p->data = rand()%100;

		r->next = p;
		r = p;
	}
	r->next = NULL;

	return 0;
}

int GetElem(linklist L, int pos, int *ele)
{
	int i = 1;
	node *p = L->next; /**/

	while(p && i < pos) {
		p = p->next;
		i++;
	}

	if (!p || i > pos) /**/
		return 1;

	*ele = p->data;

	return 0;
}

int InserElem(linklist &L, int pos, int ele)
{
	int i = 1;
	node *p = L; /**/

	node *q = (node *)malloc(sizeof(node));
	q->data = ele;

	while (p && i < pos) {
		p = p->next;
		i++;
	}

	if (!p || i > pos) /**/
		return 1;

	q->next = p->next;
	p->next = q;

	return 0;
}

int DeleteElem(linklist &L, int pos, int *del_ele)
{
	int i = 1;
	node *p = L;
	node *q;

	if (!p)
		return 1;

	while(p && i < pos) {
		p = p->next;
		i++;
	}

	if (!(p->next) || i > pos)
		return 1;

	q = p->next;
	p->next = q->next;
	free(q);

	return 0;
}

int Printlist(linklist L)
{
	node *p = L->next;
	int i = 0;

	if (p == NULL)
		printf("null list\n");

	while(p) {
		printf("%d\t", p->data);
		p = p->next;

		i++;
	}
	printf("\n");

	return i;
}

void Deletelist(linklist &L)
{
	node *p = L->next;
	node *q;

	while(p) {
		q = p->next;
		free(p);
		p = q;
	}
	p->next = NULL;
}

int main()
{
	linklist LL;
	int listlength;
	int getelem;
	int delelem;
	srand(time(0));

	Createlist_T(LL, 10);
	listlength = Printlist(LL);
	printf("init list length: %d\n", listlength);

	GetElem(LL, 1, &getelem);
	printf("get elem: %d\n", getelem);

	InserElem(LL, 10, 55);
	listlength = Printlist(LL);
	printf("inser list length: %d\n", listlength);

	DeleteElem(LL, 5, &delelem);
	listlength = Printlist(LL);
	printf("dele list length: %d\n", listlength);

	Deletelist(LL);
	//listlength = Printlist(LL);
	//printf("list length: %d\n", listlength);
}
```



# 其他

