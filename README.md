# win32_timer
在win32控制台应用程序中实现定时器  


// timer_test.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <iostream>
#include <Windows.h>
using namespace std;


int counttime = 0;

//1. sleep(函数)实现简单的定时器
void timer(int sec)
{
	cout << "定时" << sec << "秒" << endl;
	Sleep(sec * 1000);
	cout << "时间到" << endl;
}

//3.利用windows的消息传递机制和settimer函数实现定时器
void CALLBACK TimeProc(
	HWND hwnd,
	UINT message,
	UINT idTimer,
	DWORD dwTime)
{
	cout << "触发定时器！" << counttime++ << endl;
}
//2.利用多线程实现定时器

DWORD CALLBACK Thread(LPVOID pvoid)
{
	cout << "线程开始执行" << endl;
	MSG msg;
	//PeekMessage(&msg, NULL, WM_USER, WM_USER, PM_NOREMOVE);
	UINT timerID = SetTimer(NULL, 1, 1000, TimeProc);
	bool bRet;
	int * p = (int*)pvoid;
	int time = *p;
	while ((bRet = GetMessage(&msg, NULL, 0, 0)) != 0)
	{
		if (bRet == -1)
		{
			//handle the error
		}
		else
		{
			//TranslateMessage(&msg);
			if (time>counttime)
			{
				DispatchMessage(&msg);
			}
			else
			{
				break;
			}

		}
	}
	KillTimer(NULL, timerID);
	cout << "线程结束" << endl;
	return 0;
}



//在创建的线程中执行的函数


int _tmain(int argc, _TCHAR* argv[])
{
#pragma region //1. 用sleep()函数实现简单的定时器
	/*int time;
	while (cin>>time)
	{
		if (time==0)
		{
			return 0;
		}
		timer(time);
	}*/

#pragma endregion

#pragma region //2. 利用多线程实现定时器
	DWORD dwThreadID;
	int time = 5;
	int* pint = &time;
	HANDLE hThread = CreateThread(NULL,0,Thread,pint,0,&dwThreadID);
	int a;
	cin >> a;
	return 0;

#pragma endregion

#pragma region //3. 利用window的消息传递机制和settimer函数实现定时器
	/*SetTimer(NULL, 1, 1000, TimeProc);
	MSG msg;
	while (GetMessage(&msg,NULL,0,0))
	{
		DispatchMessage(&msg);
	}
*/
#pragma endregion


	return 0;
}

