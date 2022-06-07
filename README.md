#include<iostream>
#include<conio.h>

using namespace std;
const int x_max = 40; // границы 
const int y_max = 20;

char field[y_max][x_max]; // массив для поля 

int y_fruit = rand() % (y_max-2); // случайные начальные координаты фрукта c поправкой на границы
int x_fruit = rand() % (x_max-3);
enum  Direction { RIGHT, LEFT, UP, DOWN } Dir; // набор направлений 
bool is_play = true; //условие выхода из игры

int worm_size = 1;
const int max_size_worm = 100;

struct Coordinates { int y; int x; };

Coordinates worm_xy[max_size_worm]; // массив для хранения координат змейки

void to_the_right();
void to_the_left();
void to_up();
void to_down();


void fill_field()
{
	for (int y = 0; y < y_max; y++)
	{
		for (int x = 0; x < x_max; x++)
		{
			if (x == x_max - 1)
			{
				field[y][x] = '\n';
			}
			else if (y == 0 || y == y_max - 1 || x == 0 || x == x_max - 2)
			{
				field[y][x] = '#';
			}
			else
			{
				field[y][x] = ' ';
			}
		}
	}
	field[y_fruit][x_fruit] = 'F'; //первый фрукт
	worm_xy[0].y = 1; //начало змейки
	worm_xy[0].x = 1;
	field[worm_xy[0].y][worm_xy[0].x] = '8';
}
void show_all()
{
	for (int y = 0; y < y_max; y++)
		for (int x = 0; x < x_max; x++)
			cout << field[y][x];
	cout << " Score is " << worm_size * 10 << endl;
}
void input() 
{
	if (_kbhit())
		switch (_getch())
		{
		case 'a':
			Dir = LEFT;
			break;
		case 'w':
			Dir = UP;
			break;
		case 's':
			Dir = DOWN;
			break;
		case 'd':
			Dir = RIGHT;
			break;
		}
}
void move_logic()
{
	if (0 == worm_xy[0].y || y_max - 1 == worm_xy[0].y || 0 == worm_xy[0].x || x_max - 2 == worm_xy[0].x)
		is_play = false;
	

	if (worm_xy[0].y == y_fruit && worm_xy[0].x == x_fruit) //проверка, съел ли фрукт
	{
		++worm_size;
		y_fruit = rand() % (y_max - 2);
		x_fruit = rand() % (x_max - 3);
		field[y_fruit][x_fruit] = 'F';
	}

	switch (Dir)
	{
	case RIGHT:
		to_the_right();
		break;
	case LEFT:
		to_the_left();
		break;
	case UP:
		to_up();
		break;
	case DOWN:
		to_down();
		break;
	}
}

void to_the_right()
{
	if (2 > worm_size)
	{
		field[worm_xy[0].y][worm_xy[0].x] = ' ';
		worm_xy[0].x += 1;
		field[worm_xy[0].y][worm_xy[0].x] = '8';
	}
	
	if (2 <= worm_size)
	{
		for (int f = (worm_size - 1); f != -1; f--)
		{
			if (f == worm_size - 1)
			{
				field[worm_xy[f].y][worm_xy[f].x] = ' ';

				worm_xy[f].y = worm_xy[f - 1].y;
				worm_xy[f].x = worm_xy[f - 1].x;
				continue;
			}
			else if (f < (worm_size - 1) && f> 0)
			{
				worm_xy[f].y = worm_xy[f - 1].y;
				worm_xy[f].x = worm_xy[f - 1].x;
				continue;
			}
			else if (f == 0)
			{
				worm_xy[f].x += 1;
				field[worm_xy[f].y][worm_xy[f].x] = '8';
				continue;
			}
		}

	}
}
void to_the_left()
{
	if (2 > worm_size)
	{
		field[worm_xy[0].y][worm_xy[0].x] = ' ';
		worm_xy[0].x -= 1;
		field[worm_xy[0].y][worm_xy[0].x] = '8';
	}

	if (2 <= worm_size)
	{
		for (int f = (worm_size - 1); f != -1; f--)
		{
			if (f == worm_size - 1)
			{
				field[worm_xy[f].y][worm_xy[f].x] = ' ';

				worm_xy[f].y = worm_xy[f - 1].y;
				worm_xy[f].x = worm_xy[f - 1].x;
				continue;
			}
			else if (f < (worm_size - 1) && f> 0)
			{
				worm_xy[f].y = worm_xy[f - 1].y;
				worm_xy[f].x = worm_xy[f - 1].x;
				continue;
			}
			else if (f == 0)
			{
				worm_xy[f].x -= 1;
				field[worm_xy[f].y][worm_xy[f].x] = '8';
				continue;
			}
		}

	}
}
void to_up()
{
	if (2 > worm_size)
	{
		field[worm_xy[0].y][worm_xy[0].x] = ' ';
		worm_xy[0].y -= 1;
		field[worm_xy[0].y][worm_xy[0].x] = '8';
	}

	if (2 <= worm_size)
	{
		for (int f = (worm_size - 1); f != -1; f--)
		{
			if (f == worm_size - 1)
			{
				field[worm_xy[f].y][worm_xy[f].x] = ' ';

				worm_xy[f].y = worm_xy[f - 1].y;
				worm_xy[f].x = worm_xy[f - 1].x;
				continue;
			}
			else if (f < (worm_size - 1) && f> 0)
			{
				worm_xy[f].y = worm_xy[f - 1].y;
				worm_xy[f].x = worm_xy[f - 1].x;
				continue;
			}
			else if (f == 0)
			{
				worm_xy[f].y -= 1;
				field[worm_xy[f].y][worm_xy[f].x] = '8';
				continue;
			}
		}

	}
}
void to_down()
{

	if (2 > worm_size)
	{
		field[worm_xy[0].y][worm_xy[0].x] = ' ';
		worm_xy[0].y += 1;
		field[worm_xy[0].y][worm_xy[0].x] = '8';
	}

	if (2 <= worm_size)
	{
		for (int f = (worm_size - 1); f != -1; f--)
		{
			if (f == worm_size - 1)
			{
				field[worm_xy[f].y][worm_xy[f].x] = ' ';

				worm_xy[f].y = worm_xy[f - 1].y;
				worm_xy[f].x = worm_xy[f - 1].x;
				continue;
			}
			else if (f < (worm_size - 1) && f> 0)
			{
				worm_xy[f].y = worm_xy[f - 1].y;
				worm_xy[f].x = worm_xy[f - 1].x;
				continue;
			}
			else if (f == 0)
			{
				worm_xy[f].y += 1;
				field[worm_xy[f].y][worm_xy[f].x] = '8';
				continue;
			}
		}

	}
}


int main()
{
	fill_field();
	while (is_play)
	{
		show_all();
		input();
		move_logic();
		system("cls");
	}
	cout << " You lose" << endl;
	cout << "Worm size is " << worm_size;
	return 0;
}
