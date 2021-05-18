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
改用malloc。
PLAYER *pl1 = (PLAYER*)malloc(sizeof(PLAYER));
然后用结构体指针进行赋值。
注意用strcpy(pl1->name,"狗血魔神");
还要注意动态内存申请是否成功。
