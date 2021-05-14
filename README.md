# 2021-5-14
结构体训练

1、中国有句俗语叫“三天打鱼两天晒网”，某人从90年1月1日起开始“三天打鱼两天晒网”。问这个人在以后的某一天中是在“打渔”，还是在“晒网”.
**输入格式要求："%d%d%d" 提示信息："Enter year/month/day:"
**输出格式要求："He is fishing.\n" "He is sleeping.\n"
程序运行示例如下：
Enter year/month/day:1990 1 5
He is sleeping.

我的答案：
#include<stdio.h>

int Isleap(int year);

typedef struct date {
	int year;
	int month;
	int day;
} DATE; 

int main(int argc,char const*argv[])
{
	DATE dat1;
	printf("Enter year/month/day:");
	scanf("%d%d%d",&dat1.year ,&dat1.month ,&dat1.day );
	
	int sum = 0;
	int i;
	 
	for(i=1990 ; i<dat1.year ; i++)											//加上相差年份所隔的天数。 
	{
		if(Isleap(i))												//判断此年份是否是闰年 
		{
			sum += 366;
		}															//是闰年，则总天数加上366. 
		else
		{
			sum += 365;
		}															//是平年，总天数加上365 
	}
	
	int m;
	int a[13]={0,31,28,31,30,31,30,31,31,30,31,30,31};				//建立一个有平年各月份天数的数组
	for(m=1 ; m<dat1.month ; m++)											//加上所隔月份的总天数 
	{
		sum += a[m];
		if(m == 2 && Isleap(dat1.year) ) 
		{
			sum += 1;
		}															//判断月份是否为闰年2月，若是2月，则总天数加一。
	}
	sum += dat1.day ;														//总天数加上日 
	
	switch(sum % 5)													//余数定输出 
	{
		case 1:
		case 2:
		case 3:printf("He is fishing.\n");break;
		case 0:
		case 4:printf("He is sleeping.\n");break;		
	}
	
	return 0;
}

int Isleap(int year)													//判断闰年的函数 
{
	int ret;
	if ( year % 4 == 0 && year % 100 != 0 || year % 400 == 0 )
	{
		ret = 1;
	}
	else  
	{
		ret = 0;
	}
	return ret;
}

2、计算日期的差值 用结构体完成
（1）编写一函数，计算两个日期之间的时间差，并将其值返回。
日期以年、月、日表示。
“时间差”以天数表示。
注意考虑日期之间的闰年。
函数的输入参数为日期1和日期2，
函数的返回值为时间差，单位为天数。
（2）编写一程序，在主函数中输入两个日期,调用上述函数计算两个日期之间的时间差，并
将结果输出。
为了计算简便，假设用户输入的日期1总是早于日期2。
结构体形式如下：
typedef struct date
{
int year;
int month;
int day;
}Date;

输入提示信息："请输入日期1（空格分隔，年月日）:\n"
输入数据格式："%d%d%d"
输入提示信息："请输入日期2（空格分隔，年月日，晚于日期1）:\n"
输入数据格式："%d%d%d"
输出提示信息： "相差天数为:"
输出数据格式："\t%d天\n"

运行样例1：
请输入日期1（空格分隔，年月日）:
2016 12 25↙
请输入日期2（空格分隔，年月日，晚于日期1）:
2017 12 20↙
相差天数为: 360 天

我的答案：
#include<stdio.h>

typedef struct date
{
	int year;
	int month;
	int day;
} Date;

int Isleap(int year);
int CountDays(int a[],Date dat);

int main(int argc,char const*argv[])
{
	Date dat1,dat2;
	int a[13] = {0,31,28,31,30,31,30,31,31,30,31,30,31};				//建立一个有平年各月份天数的数组
	
	printf("请输入日期1（空格分隔，年月日）:\n");
	scanf("%d%d%d",&dat1.year ,&dat1.month ,&dat1.day );
	
	printf("请输入日期2（空格分隔，年月日，晚于日期1）:\n");
	scanf("%d%d%d",&dat2.year ,&dat2.month ,&dat2.day );	

	int totalgap = 0;
	int i;
	 
	for(i=dat1.year  ; i<dat2.year-1 ; i++)											
	{
		if(Isleap(i+1))												
		{
			totalgap += 366;
		}														
		else
		{
			totalgap += 365;
		}													
	}
	
	int gap1,gap2;
	gap1 = CountDays(a,dat1);
	gap2 = CountDays(a,dat2);	
	
	if (dat1.year < dat2.year )
	{
		int Gap1;
		if (Isleap(dat1.year ))
		{
			Gap1 = 366 - gap1; 
		}
		else
		{
			Gap1 = 365 - gap1;
		}
		totalgap += (Gap1 + gap2);
	}
	else
	{
		totalgap = gap2 - gap1;
	}												 
	
	printf("相差天数为:");
	printf("\t%d天\n",totalgap);
	
	return 0;
}

int Isleap(int year)													 
{
	int ret;
	if ( year % 4 == 0 && year % 100 != 0 || year % 400 == 0 )
	{
		ret = 1;
	}
	else  
	{
		ret = 0;
	}
	return ret;
}

int CountDays(int a[],Date dat)
{
	int i;
	int sum = 0;
	
	for(i=1 ; i<dat.month ; i++)
  {
		sum += a[i];
		if(i == 2 && Isleap(dat.year) ) 
		{
			sum += 1;
		}															
	}
	sum += dat.day ;
	
	return sum;
}

标准答案：
#include<stdio.h>
typedef struct date
{                    
int year;
int month;
int day;
}                    Date;
int isLeap(int year);
int dif(Date a, Date b);
void main()
{                    
Date a, b;
printf("请输入日期1（空格分隔，年月日）:\n");
scanf("%d%d%d", &a.year, &a.month, &a.day);
printf("请输入日期2（空格分隔，年月日，晚于日期1）:\n");
scanf("%d%d%d", &b.year, &b.month, &b.day);
printf("相差天数为:");
printf("\t%d天\n", dif(a, b));
}                    
int isLeap(int year) //判断一个年份是否是闰年的函数
{                    
if(year%400==0 || (year%4==0 && year%100!=0))
return 1;
else
return 0;
}                    
int dif(Date a, Date b)
{                    
int i;
long day=0, day1=0, day2=0;
int d[2][13]={{0,31,28,31,30,31,30,31,31,30,31,30,31},{0,31,29,31,30,31,30,31,31,30,31,30,31}};// day变量为年份a到年份b前一年的年份总天数
for(i=a.year; i<b.year; i++)
if(isLeap(i))
day += 366;
else
day += 365;
// day1变量为年份a年初到当天的年内总天数
for(i=1; i<a.month; ++i)
day1 += d[isLeap(a.year)][i];
day1 += a.day;
// day1变量为年份b年初到当天的年内总天数
for(i=1; i<b.month; ++i)
day2 += d[isLeap(b.year)][i];
day2 += b.day;
return day + day2 - day1;
}

3、写一个函数将以秒计数的时间转换为以时、分、秒计数的时间。
函数原型为：char *seconds_to(int seconds)。
编写main调用测试它。

**输入格式要求："%d" 提示信息："请输入时间（秒）：\n"
**输出格式要求："%d小时%d分钟%d秒\n"

答案：
#include <stdio.h>

char *seconds_to(int seconds);

int main(int argc,char const *argv[])
{
    int t;
    char *t1;
    
    printf("请输入时间（秒）：\n");
    scanf("%d", &t);
    
	t1 = seconds_to(t);
    printf("%d小时%d分钟%d秒\n", *(t1 + 2), *(t1 + 1), *(t1 + 0));
    
	return 0;
}

char *seconds_to(int seconds)
{
    static char time[3];
    time[0] = seconds % 60;
    time[1] = ( (seconds - time[0]) / 60) % 60;	//分秒之间是60进位，可以根据进位原则求出分
    time[2] = ( (seconds / 60) - time[1]) / 60;	// 时分之间也是60进位
    return &time[0];
}
