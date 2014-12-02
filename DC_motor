/**************************************

定时器2用于产生11.0592MHz晶振的9600波特率
定时器0用于PWM产生方波

**************************************/
#include <reg52.h> 
typedef unsigned char uint8;
typedef unsigned int  uint16;
sbit YF  = P1^0;    // 右轮负向 
sbit YZ  = P1^1;    // 右轮正向 
sbit ZZ  = P1^2;    // 左轮正向
sbit ZF  = P1^3;    // 左轮负向
void right();
void left();
void speed();
void slow();
void start();
void stop();
void back();
bit back_flag;
uint8 dat,ZKB1,ZKB2;

void PWM_init()
{
	TMOD = 0x21;
	TH0 = 0xFF;
	TL0 = 0xA0;
	ET0 = 1;
	TR0 = 1;
	ES = 1;
	IP = 0x10; 
	SCON =0x50;
	T2CON=0x30;
	RCAP2H=0xFF;
	RCAP2L=0xDC;
	EA  = 1;
	TR2 = 1;
	ET2 = 1;
	back_flag = 0;
	ZZ = ZF = YZ = YF = 0;
}
void main() 
{ 
	PWM_init();
	while(1);
}

void start()
{ZKB1 = 50;ZKB2 = 50;}

void stop()
{PWM_init();ZKB1 = 0; ZKB2 = 0; }

void speed()
{ZKB1 = 85;ZKB2 = 85;}

void slow()
{ZKB1 = 30;ZKB2 = 30;}

void left()
{ZKB1 = 20;ZKB2 = 70;}

void right()
{ZKB1 = 70;ZKB2 = 20;}

void back()
{
	ZZ = YZ = 0;
	back_flag = 1;
}

void uart_ISR() interrupt 4 
{ 
	RI = 0;
	dat = SBUF;	
	switch(dat)
	{
		case 0xAA: speed();break;
		case 0xBB: slow(); break;
		case 0xCC: back(); break;
		case 0xDD: left(); break;
		case 0xEE: right();break;		
		case 0xFF: start();break;
		default  : stop(); break;
	}
}

void timer0() interrupt 1
{
	static uint8 count = 0;
	TH0 = 0xFF;
	TL0 = 0xA0;
	++count;
	if(count > 100){count = 0;}
  if(!back_flag)
	{
		if(count < ZKB1)ZZ = 1;else ZZ = 0;		
		if(count < ZKB2)YZ = 1;else YZ = 0;
	}
	if(back_flag)
	{
		if(count < 60)ZF = 1;else ZF = 0;		
		if(count < 60)YF = 1;else YF = 0;
	}
}
