# Hello
#include <iostream>
#include <iomanip>
#include <cstdio>
#include <conio.h>
#include <windows.h>
using namespace std;

int num = 1, v, flag;
int top[3];
int panz[3][10];

/***************************************************************************
  函数名称：
  功    能：完成与system("cls")一样的功能，但效率高
  输入参数：
  返 回 值：
  说    明：清除整个屏幕缓冲区，不仅仅是可见窗口区域(使用当前颜色)
***************************************************************************/
void cls(const HANDLE hout)
{
    COORD coord = { 0, 0 };
    CONSOLE_SCREEN_BUFFER_INFO binfo; /* to get buffer info */
    DWORD num;

    /* 取当前缓冲区信息 */
    GetConsoleScreenBufferInfo(hout, &binfo);
    /* 填充字符 */
    FillConsoleOutputCharacter(hout, (TCHAR)' ', binfo.dwSize.X * binfo.dwSize.Y, coord, &num);
    /* 填充属性 */
    FillConsoleOutputAttribute(hout, binfo.wAttributes, binfo.dwSize.X * binfo.dwSize.Y, coord, &num);

    /* 光标回到(0,0) */
    SetConsoleCursorPosition(hout, coord);
    return;
}

/***************************************************************************
  函数名称：gotoxy
  功    能：将光标移动到指定位置
  输入参数：HANDLE hout：输出设备句柄
            int X      ：指定位置的x坐标
            int Y      ：指定位置的y坐标
  返 回 值：无
  说    明：此函数不准修改
***************************************************************************/
void gotoxy(const HANDLE hout, const int X, const int Y)
{
    COORD coord;
    coord.X = X;
    coord.Y = Y;
    SetConsoleCursorPosition(hout, coord);
}

//横向输出
void out()
{
    cout << "A:";
    for (int i = 0; i < top[0]; i++)
        cout << setw(2) << panz[0][i];
    cout << setw(23 - 2 * top[0]) << "B:";
    for (int i = 0; i < top[1]; i++)
        cout << setw(2) << panz[1][i];
    cout << setw(23 - 2 * top[1]) << "C:";
    for (int i = 0; i < top[2]; i++)
        cout << setw(2) << panz[2][i];
    cout << setw(22 - 2 * top[2]) << " ";
}
//纵向输出
void print(HANDLE hout)
{
    gotoxy(hout, 11, 11);
    int y = 11;
    for (int i = 0; i < top[0]; i++)
    {
        putchar(panz[0][i] + '0');
        y--;
        gotoxy(hout, 11, y);
    }
    gotoxy(hout, 21, 11);
    y = 11;
    for (int i = 0; i < top[1]; i++)
    {
        putchar(panz[1][i] + '0');
        y--;
        gotoxy(hout, 21, y);
    }
    gotoxy(hout, 31, 11);
    y = 11;
    for (int i = 0; i < top[2]; i++)
    {
        putchar(panz[2][i] + '0');
        y--;
        gotoxy(hout, 31, y);
    }
    gotoxy(hout, 9, 12);
    cout << setw(26) << setfill('=') << '\n';
    gotoxy(hout, 11, 13);
    cout << "A" << setw(10) << setfill(' ') << "B"
        << setw(10) << "C";
}

void hanoi(int n, char src, char tmp, char dst, HANDLE hout)
{
    if (n == 1)
    {
        if (!v)
        {
            top[src - 'A']--;
            panz[dst - 'A'][top[dst - 'A']] = panz[src - 'A'][top[src - 'A']];
            top[dst - 'A']++;
            char c = _getch();
            while (1)
            {
                if (c == 13)
                    break;
                c = _getch();
            }
            gotoxy(hout, 0, 17);
            cout << "第" << setw(4) << num << "步(" << n << "#: "
                << src << "-->" << dst << ") ";
            if (flag)
            {
                out();
                c = _getch();
                while (1)
                {
                    if (c == 13)
                        break;
                    c = _getch();
                }
            }
            gotoxy(hout, 10 * (src - 'A' + 1) + 1, 12 - top[src - 'A'] - 1);
            putchar(' ');
            gotoxy(hout, 10 * (dst - 'A' + 1) + 1, 12 - top[dst - 'A']);
            putchar(panz[src - 'A'][top[src - 'A']] + '0');
            num++;
        }
        else
        {
            top[src - 'A']--;
            panz[dst - 'A'][top[dst - 'A']] = panz[src - 'A'][top[src - 'A']];
            top[dst - 'A']++;
            Sleep(1250 - v * 250);
            gotoxy(hout, 0, 17);
            cout << "第" << setw(4) << num << "步(" << n << "#: "
                << src << "-->" << dst << ") ";
            if (flag)
                out();
            gotoxy(hout, 10 * (src - 'A' + 1) + 1, 12 - top[src - 'A'] - 1);
            putchar(' ');
            gotoxy(hout, 10 * (dst - 'A' + 1) + 1, 12 - top[dst - 'A']);
            putchar(panz[src - 'A'][top[src - 'A']] + '0');
            num++;
        }
    }
    else
    {
        hanoi(n - 1, src, dst, tmp, hout);
        if (!v)
        {
            top[src - 'A']--;
            panz[dst - 'A'][top[dst - 'A']] = panz[src - 'A'][top[src - 'A']];
            top[dst - 'A']++;
            char c = _getch();
            while (1)
            {
                if (c == 13)
                    break;
                c = _getch();
            }
            gotoxy(hout, 0, 17);
            cout << "第" << setw(4) << num << "步(" << n << "#: "
                << src << "-->" << dst << ") ";
            if (flag)
            {
                out();
                c = _getch();
                while (1)
                {
                    if (c == 13)
                        break;
                    c = _getch();
                }
            }
            gotoxy(hout, 10 * (src - 'A' + 1) + 1, 12 - top[src - 'A'] - 1);
            putchar(' ');
            gotoxy(hout, 10 * (dst - 'A' + 1) + 1, 12 - top[dst - 'A']);
            putchar(panz[src - 'A'][top[src - 'A']] + '0');
            num++;
        }
        else
        {
            top[src - 'A']--;
            panz[dst - 'A'][top[dst - 'A']] = panz[src - 'A'][top[src - 'A']];
            top[dst - 'A']++;
            Sleep(1250 - 250 * v);
            gotoxy(hout, 0, 17);
            cout << "第" << setw(4) << num << "步(" << n << "#: "
                << src << "-->" << dst << ") ";
            if (flag)
                out();
            gotoxy(hout, 10 * (src - 'A' + 1) + 1, 12 - top[src - 'A'] - 1);
            putchar(' ');
            gotoxy(hout, 10 * (dst - 'A' + 1) + 1, 12 - top[dst - 'A']);
            putchar(panz[src - 'A'][top[src - 'A']] + '0');
            num++;
        }
        hanoi(n - 1, tmp, src, dst, hout);
        return;
    }
}

/***************************************************************************
  函数名称：
  功    能：
  输入参数：
  返 回 值：
  说    明：
***************************************************************************/
int main()
{
    HANDLE hout = GetStdHandle(STD_OUTPUT_HANDLE); //取标准输出设备对应的句柄
    int n;
    char src, tmp, dst;
    while (1)
    {
        cout << "请输入汉诺塔的层数(1-10)" << endl;
        cin >> n;
        cin.clear();
        cin.ignore(1024, '\n');
        if (n >= 1 && n <= 10)
            break;
    }
    while (1)
    {
        cout << "请输入起始柱(A-C)" << endl;
        cin >> src;
        cin.clear();
        cin.ignore(1024, '\n');
        if (src >= 'A' && src <= 'C')
            break;
        else if (src >= 'a' && src <= 'c')
        {
            src -= 32;
            break;
        }
    }
    while (1)
    {
        cout << "请输入目标柱(A-C)" << endl;
        cin >> dst;
        cin.clear();
        cin.ignore(1024, '\n');
        if (dst >= 'A' && dst <= 'C')
        {
            if (dst == src)
            {
                cout << "目标柱(" << dst << ")不能与起始柱(" << src << ")相同" << endl;
                continue;
            }
            else
                break;
        }
        else if (dst >= 'a' && dst <= 'c')
        {
            dst -= 32;
            if (dst == src)
            {
                cout << "目标柱(" << dst << ")不能与起始柱(" << src << ")相同" << endl;
                continue;
            }
            else
                break;
        }
    }
    if ((src == 'A' && dst == 'B') || (src == 'B' && dst == 'A'))
        tmp = 'C';
    else if ((src == 'A' && dst == 'C') || (src == 'C' && dst == 'A'))
        tmp = 'B';
    else if ((src == 'B' && dst == 'C') || (src == 'C' && dst == 'B'))
        tmp = 'A';
    //初始化全局变量
    for (int i = 0; i < n; i++)
        panz[src - 'A'][i] = n - i;
    top[0] = top[1] = top[2] = 0;
    top[src - 'A'] = n;

    while (1)
    {
        cout << "请输入移动速度(0-5：0-按回车单步演示 1-延时最长 5-延时最短)：" << endl;
        cin >> v;
        if (cin.fail())
        {
            cin.clear();
            cin.ignore(1024, '\n');
            continue;
        }
        else if (v >= 0 && v <= 5)
            break;
    }
    while (1)
    {
        cout << "请输入是否显示内部数组值(0-不显示 1-显示)：" << endl;
        cin >> flag;
        if (cin.fail())
        {
            cin.clear();
            cin.ignore(1024, '\n');
            continue;
        }
        else if (flag == 0 || flag == 1)
            break;
    }
    cls(hout);
    cout << "从 " << src << " 移动到 " << dst << ", 共 " << n
        << " 层, 延时设置为 " << v << ", ";
    if (flag)
        cout << "显示内部数组值" << endl;
    else
        cout << "不显示内部数组值" << endl;

    gotoxy(hout, 0, 17);
    cout << "初始:" << setw(2) << " ";
    if (flag)
        out();
    cin.ignore();
    if (!v)
    {
        char c = _getch();
        while (1)
        {
            if (c == 13)
                break;
            c = _getch();
        }
        print(hout);
    }
    else
    {
        print(hout);
    }
    hanoi(n, src, tmp, dst, hout);
    if (!v)
    {
        char c = _getch();
        while (1)
        {
            if (c == 13)
                break;
            c = _getch();
        }
    }
    gotoxy(hout, 0, 18);
    return 0;
}
