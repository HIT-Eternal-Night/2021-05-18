# 2021-05-18
学习学校课程里的链表

首先，学习建立一个单向链表，以有限个节点为例，代码如下：
#include<stdio.h>

typedef struct player{
	int account;
	char name[20];
	int HP;
	int power;
	int intelligence;
	struct player *next;//此处不能PLAYER *next；
} PLAYER;

int main(int argc,char const *argv[])
{
	PLAYER pl1 = {1,"狗血魔神",26446,827,499};
	PLAYER pl2 = {2,"宇宙第一",99999,999,999};
	PLAYER pl3 = {3,"火云邪神",30889,973,120};
	
	PLAYER *head = &pl1;
	pl1.next = &pl2;
	pl2.next = &pl3;
	pl3.next = NULL;
	
	printf("The first player is:%s",head->name );
	
	return 0;
}

然后，将代码封装为函数。
#include<stdio.h>

typedef struct player{
	int account;
	char name[20];
	int HP;
	int power;
	int intelligence;
	struct player *next;
} PLAYER;

PLAYER * Build(void);

int main(int argc,char const *argv[])
{
	PLAYER *head;
	head = Build();
	printf("The first player is:%s",head->name );
	return 0;
}

PLAYER * Build(void)
{
	PLAYER pl1 = {1,"狗血魔神",26446,827,499};
	PLAYER pl2 = {2,"宇宙第一",99999,999,999};
	PLAYER pl3 = {3,"火云邪神",30889,973,120};
	
	PLAYER *head = &pl1;
	pl1.next = &pl2;
	pl2.next = &pl3;
	pl3.next = NULL;
	
	return head;
}
如果采用上述方式，输出会是乱码。因为pl1,pl2,pl3的作用域不是全局的，哪怕返回了指向pl1的指针，在回到main函数里时，
属于pl1的那块内存空间也已经被释放了。

解决方案：
改用malloc。（记得#include<stdlib.h>）
PLAYER *pl1 = (PLAYER*)malloc(sizeof(PLAYER));
然后用结构体指针进行赋值。
注意用strcpy(pl1->name,"狗血魔神");
还要注意动态内存申请是否成功。

接下来研究存储多组数据的情况
#include <stdio.h>
#include <stdlib.h>

typedef struct player{
	int account;
	char name[20];
	int HP;
	int power;
	int intelligence;
	struct player *next;
} PLAYER;

PLAYER * Build(int num);

int main(int argc,char const *argv[])
{
	PLAYER *head;
	head = Build(100);
	printf("The first player is:%s",head->name );
	return 0;
}

PLAYER * Build(int num)
{
	PLAYER *Pt,*prePt,*head;
	
	Pt = (PLAYER*)malloc(sizeof(PLAYER));
	if (Pt != NULL)
	{
		scanf("%d%s%d%d%d",&Pt->account ,Pt->name ,&Pt->HP ,&Pt->power ,&Pt->intelligence );
		head = Pt;
		prePt = Pt;
	}
	else
	{
		printf("Failed.\n");
		exit(0);
	}
	
	int i;
	for (i=0 ; i<num ; i++)
	{
		Pt = (PLAYER*)malloc(sizeof(PLAYER));
		if (Pt != NULL)
		{
			scanf("%d%s%d%d%d",&Pt->account ,Pt->name ,&Pt->HP ,&Pt->power ,&Pt->intelligence );
			prePt->next = Pt;
			prePt = Pt;
		}
		else
		{
			printf("Failed.\n");
			exit(0);
		}
	}
	Pt->next = NULL;
	return head;
}

再介绍应用：
1、在单向链表中查找查找节点————查找尾节点
PLAYER *FindLast(PLAYER* head)
{
	PLAYER *pr;
	pr = head;
	while (pr->next != NULL)
	{
		pr = pr->next ;
	}
	return pr;
}

2、在单向链表中查找查找节点————查找第n个节点
PLAYER *FindNode(PLAYER* head, int index)
{
	PLAYER *Pt = head;
	int i = 1;
	while (i<index && Pt != NULL)
	{
		Pt = Pt->next ;
		i++;
	}
	return Pt;
}

3、向单向链表的尾部插入一个节点
PLAYER *Insert(PLAYER *head)
{
	PLAYER *prePt,*Pt;
	Pt = (PLAYER*)malloc(sizeof(PLAYER));
	scanf("%d%s%d%d%d",&Pt->account ,Pt->name ,&Pt->HP ,&Pt->power ,&Pt->intelligence );
	
	prePt = FindLast(head);
	prePt->next = Pt;
	Pt->next = NULL;
	
	return head;
}

4、向单向链表的某个位置插入一个节点
PLAYER *Insert(PLAYER *head, int index)
{
	PLAYER *prePt,*Pt;
	Pt = (PLAYER*)malloc(sizeof(PLAYER));
	scanf("%d%s%d%d%d",&Pt->account ,Pt->name ,&Pt->HP ,&Pt->power ,&Pt->intelligence );
	
	prePt = FindNode(head,index);
	
	if (prePt != NULL)
	{
		Pt->next = prePt->next ;
		prePt->next = Pt;		
	}
	else
	{
		printf("There is no such node\n");
	}
	
	return head;
}

5、在单向链表中删除某个节点
PLAYER *Delete(PLAYER *head, int index)
{
	PLAYER *Pt = NULL;
	PLAYER *prePt = NULL;
	
	prePt = FindNode(head,index-1);
	
	Pt = prePt->next ; 
	if (Pt != NULL)
	{
		prePt->next = Pt->next ;
		free(Pt);
	}
	else
	{
		printf("There is no such node\n");
	}
	return head;
}
