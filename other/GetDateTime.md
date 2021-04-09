### C语言获取编译的时间
  
  其中星期由泰勒公式计算得到：
  
  泰勒公式：**`w=y+[y/4]+[c/4]-2c+[26(m+1）/10]+d-1`** 
  
   + w：星期； w对7取模得：0-星期日，1-星期一，2-星期二，3-星期三，4-星期四，5-星期五，6-星期六
   + c：世纪（注：一般情况下，在公式中取值为已经过的世纪数，也就是年份除以一百的结果，而非正在进行的世纪，也就是现在常用的年份除以一百加一；不过如果年份是公元前的年份且非整百数的话，c应该等于所在世纪的编号，如公元前253年，是公元前3世纪，c就等于-3）
   + y：年（一般情况下是后两位数，如果是公元前的年份且非整百数，y应该等于cMOD100+100）
   + m：月（m大于等于3，小于等于14，即在蔡勒公式中，某年的1、2月要看作上一年的13、14月来计算，比如2003年1月1日要看作2002年的13月1日来计算）
   + d：日
   + [ ]代表取整，即只要整数部分。
   
```C++
#include <stdio.h>
#include <stdint.h>
#include <string.h>
typedef struct time_struct {
	uint16_t year;
	uint8_t  month;
	uint8_t  day;
	uint8_t  week;
	uint8_t  hour;
	uint8_t  minute;
	uint8_t  second;
}TimeStructType;



void GetDateTime(TimeStructType *dt)
{
	char *date = __DATE__;
	char *time = __TIME__;

	dt->year = (date[7] - 48) * 1000 + \
		(date[8] - 48) * 100 + \
		(date[9] - 48) * 10 + \
		(date[10] - 48);

	if (0 == memcmp("Jan", date, 3))
		dt->month = 1;
	else if (0 == memcmp("Feb", date, 3))
		dt->month = 2;
	else if (0 == memcmp("Mar", date, 3))
		dt->month = 3;
	else if (0 == memcmp("Apr", date, 3))
		dt->month = 4;
	else if (0 == memcmp("May", date, 3))
		dt->month = 5;
	else if (0 == memcmp("Jun", date, 3))
		dt->month = 6;
	else if (0 == memcmp("Jul", date, 3))
		dt->month = 7;
	else if (0 == memcmp("Aug", date, 3))
		dt->month = 8;
	else if (0 == memcmp("Sep", date, 3))
		dt->month = 9;
	else if (0 == memcmp("Oct", date, 3))
		dt->month = 10;
	else if (0 == memcmp("Nov", date, 3))
		dt->month = 11;
	else if (0 == memcmp("Dec", date, 3))
		dt->month = 12;


	if (' ' != date[4])
		dt->day = (date[4] - 48) *10 ;
	else
		dt->day = 0;
	dt->day += (date[5] - 48);

	dt->week = dt->year + (dt->year / 4) + (26 * (dt->month + 1) / 10) + dt->day - 36;
	dt->week %= 7;

	dt->hour = (time[0] - 48) * 10 + \
		(time[1] - 48);
	dt->minute = (time[3] - 48) * 10 + \
		(time[4] - 48);
	dt->second = (time[6] - 48) * 10 + \
		(time[7] - 48);
}


int main(void)
{
	TimeStructType udt;
	GetDateTime(&udt);
	printf("%d-%d-%d %d %d:%d:%d", udt.year, udt.month, udt.day, udt.week, udt.hour, udt.minute, udt.second);
	return 0;
}
```
