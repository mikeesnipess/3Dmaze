# 3Dmaze
This is 3D maze in console.

#include "pch.h"
#include <iostream>
#include <Windows.h>
#include <chrono>
#include <vector>
#include <algorithm>
#include <array>
#include <stack>
#include <math.h>
#include <random>
#include<conio.h>


//Config
int screenWidth = 210;
int screenHeight = 140;
int fontSizeWidth = 7;
int fontSizeHeight = 4;

//Maze Size
const int mapHeight = 21;
const int mapWidth = 41;
			


//Player Info
float playerX = 1.0f;
float playerY = 1.0f;
float playerLookAngle = 0.0f;


//Player config
float fieldOfView = 3.14159 / 3.0;
float depthOfView = 16.0f;
float walkSpeed = 1.5f;

float distanceToWall;


//Game
int* maze;
bool showMinimap = true;


HANDLE hConsole;
HANDLE m_hConsoleIn;
DWORD dwBytesWritten;
wchar_t *screen;


int m_mousePosX;
int m_mousePosY;
bool m_mouseOldState[5] = { 0 };
bool m_mouseNewState[5] = { 0 };
bool m_bConsoleInFocus = true;
int posOld = 0;
struct sKeyState
{
	bool bPressed;
	bool bReleased;
	bool bHeld;
} m_keys[256], m_mouse[5];

//Forward declaration
void resetGrid();
void generateMaze(int selection);
void setupCommandConsole();
void configureCommandConsole();
int XYToIndex(int x, int y);
void HandleInputs(float deltaTime, int* maze);
void Render(int* maze, wchar_t *screen, float deltaTime);
void clear() {
	COORD topLeft = { 0, 0 };
	HANDLE console = GetStdHandle(STD_OUTPUT_HANDLE);
	CONSOLE_SCREEN_BUFFER_INFO screen;
	DWORD written;

	GetConsoleScreenBufferInfo(console, &screen);
	FillConsoleOutputCharacterA(
		console, ' ', screen.dwSize.X * screen.dwSize.Y, topLeft, &written
	);
	FillConsoleOutputAttribute(
		console, FOREGROUND_GREEN | FOREGROUND_RED | FOREGROUND_BLUE,
		screen.dwSize.X * screen.dwSize.Y, topLeft, &written
	);
	SetConsoleCursorPosition(console, topLeft);
}
int menu();

int main()
{


	//srand(time(0));
	srand(time(NULL));
	int i=21;
	int j=41;
	int aleator=0;
	
	//aleator = rand() % 21;
	//std::cout << "\nAleator = " << aleator << "\n";
	/*for (int i = rand()<mapHeight; i < mapHeight;i++)
		for (int j = rand()<mapHeight; j < mapWidth; j++)
		{

			aleator[i][j] == rand() < mapHeight;
			goto a1;
		}
	a1:*/


	//GenerateMAZE
	resetGrid();

	int selection = menu();
	generateMaze(selection);

	int l = 10;
	int k = 21;
	int lol= 37;

				maze[lol] = '1';
	for (size_t y = 0; y < mapHeight; y++) {
		for (size_t x = 0; x < mapWidth; x++)
		{

			//std::cout << maze[aleator[y][x]];
		//maze[x] = '#';

			
			a1:
			if (maze[XYToIndex(x, y)] != 0)
			{
				std::cout << " ";
				
				//if(lol == 37)
				//{
				//	std::cout << "-";
				//	maze[lol] = '-';
				//	lol = 0;
				//	//aleator = lol;
				//}
			}
			else
				std::cout << "@";

			
				
				
			
				


		}
		std::cout << std::endl;
				
	}
					
	std::cout << "Apasa orice pentru a juca..." << std::endl;
	std::cin.ignore(256, '\n');
	std::cin.get();
	clear();
	setupCommandConsole();
	configureCommandConsole();

	auto timeFlag1 = std::chrono::system_clock::now();
	auto timeFlag2 = std::chrono::system_clock::now();
	
	//screen[(((int)playerY) * 4 + 1 + (screenHeight / 2 - mapHeight * 4 / 2)) * screenWidth + (((int)playerX) * 4 + 1 + screenWidth / 2 - mapWidth * 4 / 2)] = 'L';
	//if ((((int)playerY) * 4 + 1 + (screenHeight / 2 - mapHeight * 4 / 2)) * screenWidth + (((int)playerX) * 4 + 1 + screenWidth / 2 - mapWidth * 4 / 2)) = maze[2];
	
	while (true) {
		timeFlag2 = std::chrono::system_clock::now();
		std::chrono::duration<float> chronoElapsedtime = timeFlag2 - timeFlag1;
		timeFlag1 = timeFlag2;
		float deltaTime = chronoElapsedtime.count();

		HandleInputs(deltaTime, maze);
		Render(maze, screen, deltaTime);
	if (maze[(int)playerY * mapWidth + (int)playerX] == maze[lol])
	{
		system("cls");		
		std::cout << "\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFINFINFIN\tFINFINFINFINFINFIN\tFINFINFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFINFINFIN\tFINFINFINFINFINFIN\tFINFINFINFIN\t\t\t    FINFIN"; 
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFINFINFIN\tFINFINFINFINFINFIN\tFINFINFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFINFINFIN\tFINFINFINFINFINFIN\tFINFINFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFINFINFIN\tFINFINFINFINFINFIN\tFINFINFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFINFINFIN\tFINFINFINFINFINFIN\tFINFINFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFINFINFIN\tFINFINFINFINFINFIN\tFINFINFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\tFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\tFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\tFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\tFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\tFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\tFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\tFINFIN\t\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t   FINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t   FINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t   FINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t   FINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t   FINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t   FINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFIN\t\t\tFIN\t\tFINFIN\t\tFINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFIN\t\t\tFIN\t\tFINFIN\t\tFINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFIN\t\t\tFIN\t\tFINFIN\t\tFINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFIN\t\t\tFIN\t\tFINFIN\t\tFINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFIN\t\t\tFIN\t\tFINFIN\t\tFINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFIN\t\t\tFIN\t\tFINFIN\t\tFINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFIN\t\t\tFIN\t\tFINFIN\t\tFINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFIN\t\t\tFIN\t\tFINFIN\t\tFINFIN\t\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFINFINFINFIN\t\t\tFIN\t\tFINFIN\t\t   FINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t   FINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t   FINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t   FINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t   FINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t   FINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t   FINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t   FINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\tFINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\tFINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\tFINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\tFINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\tFINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\tFINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\tFINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\tFINFIN\t    FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\t   FINFIN   FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\t   FINFIN   FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\t   FINFIN   FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\t   FINFIN   FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\t   FINFIN   FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\t   FINFIN   FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\t   FINFIN   FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\t\tFIN\t\tFINFIN\t\t\t   FINFIN   FINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\tFINFINFINFINFINFIN\tFINFIN\t\t\t      FINFINFINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\tFINFINFINFINFINFIN\tFINFIN\t\t\t      FINFINFINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\tFINFINFINFINFINFIN\tFINFIN\t\t\t      FINFINFINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\tFINFINFINFINFINFIN\tFINFIN\t\t\t      FINFINFINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\tFINFINFINFINFINFIN\tFINFIN\t\t\t      FINFINFINFIN";
		std::cout << "\n\t\t\t\t\t\t\tFIN\t\t\tFINFINFINFINFINFIN\tFINFIN\t\t\t      FINFINFINFIN";
		std::cout << "\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n";
		_getch();
		exit(0);
	}
	}

	delete[] maze;
	
}

int menu()
{
	std::cout << std::endl;
	int selection = 0;
	std::cout << " Alegeti algoritmul care doriti sa-l utilizati. \n";
	std::cout << " 1. Backtracking recursiv\n";
	std::cout << " 2. Hunt-and-Kill\n";
	std::cout << "\n\tAlegeti: ";
	std::cin >> selection;

	return selection - 1;

}

void HandleInputs(float deltaTime, int* maze) {
	if (GetAsyncKeyState((VK_LEFT)) & 0x8000)
		playerLookAngle -= (1.0f)*deltaTime;

	if (GetAsyncKeyState((VK_RIGHT)) & 0x8000)
		playerLookAngle += (1.0f)*deltaTime;

	if (GetAsyncKeyState((unsigned short)'W') & 0x8000)
	{
		playerX += sinf(playerLookAngle)*(walkSpeed)*deltaTime;
		playerY += cosf(playerLookAngle)*(walkSpeed)*deltaTime;

		if (maze[(int)playerY*mapWidth + (int)playerX] == 0) {
			playerX -= sinf(playerLookAngle)*(walkSpeed)*deltaTime;
			playerY -= cosf(playerLookAngle)*(walkSpeed)*deltaTime;
		}
	}
	if (GetAsyncKeyState((unsigned short)'S') & 0x8000)
	{
		playerX -= sinf(playerLookAngle)*(walkSpeed)*deltaTime;
		playerY -= cosf(playerLookAngle)*(walkSpeed)*deltaTime;

		if (maze[(int)playerY*mapWidth + (int)playerX] == 0) {
			playerX += sinf(playerLookAngle)*(walkSpeed)*deltaTime;
			playerY += cosf(playerLookAngle)*(walkSpeed)*deltaTime;
		}
	}

	if (GetAsyncKeyState((unsigned short)'D') & 0x8000)
	{
		playerX += cosf(playerLookAngle)*(walkSpeed)*deltaTime;
		playerY -= sinf(playerLookAngle)*(walkSpeed)*deltaTime;

		if (maze[(int)playerY*mapWidth + (int)playerX] == 0) {
			playerX -= cosf(playerLookAngle)*(walkSpeed)*deltaTime;
			playerY += sinf(playerLookAngle)*(walkSpeed)*deltaTime;
		}
	}
	if (GetAsyncKeyState((unsigned short)'A') & 0x8000)
	{
		playerX -= cosf(playerLookAngle)*(walkSpeed)*deltaTime;
		playerY += sinf(playerLookAngle)*(walkSpeed)*deltaTime;

		if (maze[(int)playerY*mapWidth + (int)playerX] == 0) {
			playerX += cosf(playerLookAngle)*(walkSpeed)*deltaTime;
			playerY -= sinf(playerLookAngle)*(walkSpeed)*deltaTime;
		}
	}

	if (GetAsyncKeyState((unsigned short)'M') & 0x8000)
	{

		showMinimap = true;
	}
	else
	{
		showMinimap = false;
	}


	// Handle Mouse Input - Check for window events
	INPUT_RECORD inBuf[32];
	DWORD events = 0;
	GetNumberOfConsoleInputEvents(m_hConsoleIn, &events);
	if (events > 0)
		ReadConsoleInput(m_hConsoleIn, inBuf, events, &events);
	for (DWORD i = 0; i < events; i++)
	{
		switch (inBuf[i].EventType)
		{
		case FOCUS_EVENT:
		{
			m_bConsoleInFocus = inBuf[i].Event.FocusEvent.bSetFocus;
		}
		break;

		case MOUSE_EVENT:
		{
			switch (inBuf[i].Event.MouseEvent.dwEventFlags)
			{
			case MOUSE_MOVED:
			{
				m_mousePosX = inBuf[i].Event.MouseEvent.dwMousePosition.X;
				m_mousePosY = inBuf[i].Event.MouseEvent.dwMousePosition.Y;

			}
			break;

			case 0:
			{
				for (int m = 0; m < 5; m++)
					m_mouseNewState[m] = (inBuf[i].Event.MouseEvent.dwButtonState & (1 << m)) > 0;

			}
			break;

			default:
				break;
			}
		}
		break;

		default:
			break;

		}
	}

	for (int m = 0; m < 5; m++)
	{
		m_mouse[m].bPressed = false;
		m_mouse[m].bReleased = false;

		if (m_mouseNewState[m] != m_mouseOldState[m])
		{
			if (m_mouseNewState[m])
			{
				m_mouse[m].bPressed = true;
				m_mouse[m].bHeld = true;
			}
			else
			{
				m_mouse[m].bReleased = true;
				m_mouse[m].bHeld = false;
			}
		}

		m_mouseOldState[m] = m_mouseNewState[m];
	}

	if (screenWidth - m_mousePosX <= 80) {
		playerLookAngle += 0.010;
	}
	else if (m_mousePosX < 80) {
		playerLookAngle -= 0.010;
	}

}

void Render(int* maze, wchar_t *screen, float deltaTime) {
	for (int x = 0; x < screenWidth; x++) {
		float rayAngle = (playerLookAngle - fieldOfView / 2.0f) + ((float)x / (float)screenWidth)*fieldOfView;

		distanceToWall = 0;
		bool hitWall = false;
		bool hitCornerWall = false;

		float eyeX = sinf(rayAngle);
		float eyeY = cosf(rayAngle);

		while (!hitWall && distanceToWall < depthOfView) {
			distanceToWall += 0.1f;


			int testX = (int)(playerX + eyeX * distanceToWall);
			int testY = (int)(playerY + eyeY * distanceToWall);

			if (testX < 0 || testX >= mapWidth || testY < 0 || testY >= mapHeight) {
				hitWall = true;
				distanceToWall = depthOfView;
			}
			else {
				if (maze[testY*mapWidth + testX] == 0)
				{
					hitWall = true;
					std::vector < std::pair<float, float >> p;//distance, dot
					for (int tx = 0; tx < 2; tx++)
						for (int ty = 0; ty < 2; ty++)
						{
							float vy = (float)testY + ty - playerY;
							float vx = (float)testX + tx - playerX;
							float d = sqrt(vx*vx + vy * vy);
							float dot = (eyeX*vx / d) + (eyeY*vy / d);
							p.push_back(std::make_pair(d, dot));
						}

					sort(p.begin(), p.end(), [](const std::pair<float, float> &left, const std::pair<float, float>&right) {
						return left.first < right.first;
					});

					float bound = 0.002f;
					if (acos(p.at(0).second) < bound) hitCornerWall = true;
					if (acos(p.at(1).second) < bound) hitCornerWall = true;

				}

			}
		}

		int ceiling = (float)(screenHeight / 2.0) - screenHeight / ((float)distanceToWall);
		int floor = screenHeight - ceiling;

		short shade = ' ';

		if (distanceToWall <= depthOfView / 4.0f)		shade = 0x2588;
		else if (distanceToWall < depthOfView / 3.0f) {
			shade = 0x2593;
		}

		else if (distanceToWall < depthOfView / 2.0f) shade = 0x2219;
		else if (distanceToWall < depthOfView / 1.0f) shade = 0x2219;
		else if (distanceToWall < depthOfView )		shade = 0x2219;
		else									shade = ' ';

		if (hitCornerWall) shade = 0x2593;

		for (int y = 0; y < screenHeight; y++) {
			if (y < ceiling)
				screen[y*screenWidth + x] = ' ';
			else if (y == ceiling || y == floor)
				screen[y*screenWidth + x] = 0x2593;
			else if (y > ceiling&&y <= floor && hitCornerWall)
				screen[y*screenWidth + x] = shade;




			else if (y > ceiling&&y < floor)
				screen[y*screenWidth + x] = '|';
			else
			{
				float b = 1.0f - (((float)y - screenHeight / 2.0f) / ((float)screenHeight / 2.0f));
				if (b < 0.15) shade = 'O';
				else if (b < 0.25) shade = '$';
				else if (b < 0.55) shade = '!';
				else if (b < 0.9) shade = 0x2219;
				else  shade = shade;
				screen[y*screenWidth + x] = shade;
			}

		}

	}


	//DRAW
	wchar_t s[256];
	swprintf_s(s, 256, L"PosX=%3.2f,PosY=%3.2f,Angle=%3.2f FPS=%3.2f ,RayLenght=%3.2f, MouseX=%3.2i,MouseY=%3.2i", playerX, playerY, playerLookAngle, 1.0f / deltaTime, distanceToWall, m_mousePosX, m_mousePosY);


	SetConsoleTitle(s);





	if (showMinimap) {



		//// Displays the finished maze to the screen.
		for (int y = 0; y < mapHeight * 4; ++y)
		{
			for (int x = 0; x < mapWidth * 4; ++x)
			{

				if (maze[XYToIndex(x / 4, y / 4)] != 0)
					screen[(y + (screenHeight / 2 - mapHeight * 4 / 2))*screenWidth + (x + screenWidth / 2 - mapWidth * 4 / 2)] = ' ';

				else
					screen[(y + (screenHeight / 2 - mapHeight * 4 / 2))*screenWidth + (x + screenWidth / 2 - mapWidth * 4 / 2)] = '@';
				
				
				
					

			}

		}

		screen[(((int)playerY) * 4 + 2 + (screenHeight / 2 - mapHeight * 4 / 2))*screenWidth + (((int)playerX) * 4 + 1 + screenWidth / 2 - mapWidth * 4 / 2)] = 'L';
		screen[(((int)playerY) * 4 + 1 + (screenHeight / 2 - mapHeight * 4 / 2))*screenWidth + (((int)playerX) * 4 + 2 + screenWidth / 2 - mapWidth * 4 / 2)] = 'L';
		screen[(((int)playerY) * 4 + 2 + (screenHeight / 2 - mapHeight * 4 / 2))*screenWidth + (((int)playerX) * 4 + 2 + screenWidth / 2 - mapWidth * 4 / 2)] = 'L';
		screen[(((int)playerY) * 4 + 1 + (screenHeight / 2 - mapHeight * 4 / 2))*screenWidth + (((int)playerX) * 4 + 1 + screenWidth / 2 - mapWidth * 4 / 2)] = 'L';


	}


	screen[screenWidth*screenHeight - 1] = '\0';
	WriteConsoleOutputCharacter(hConsole, screen, screenWidth*screenHeight, { 0,0 }, &dwBytesWritten);

}


//Setup Command console
void setupCommandConsole() {
	hConsole = GetStdHandle(STD_OUTPUT_HANDLE);
	m_hConsoleIn = GetStdHandle(STD_INPUT_HANDLE);

	screen = new wchar_t[screenWidth*screenHeight];
	SetConsoleActiveScreenBuffer(hConsole);
	dwBytesWritten = 0;
}

void configureCommandConsole() {

	SMALL_RECT rectWindow;
	rectWindow = { 0, 0, 1, 1 };
	SetConsoleWindowInfo(hConsole, TRUE, &rectWindow);




	COORD coord = { (short)screenWidth, (short)screenHeight };
	SetConsoleScreenBufferSize(hConsole, coord);

	SetConsoleActiveScreenBuffer(hConsole);

	CONSOLE_FONT_INFOEX cfi;
	cfi.cbSize = sizeof(cfi);
	cfi.nFont = 0;
	cfi.dwFontSize.X = fontSizeWidth;
	cfi.dwFontSize.Y = fontSizeHeight;
	cfi.FontFamily = FF_DONTCARE;
	cfi.FontWeight = FW_NORMAL; wcscpy_s(cfi.FaceName, L"Consolas");
	SetCurrentConsoleFontEx(hConsole, false, &cfi);

	SetConsoleActiveScreenBuffer(hConsole);
	rectWindow = { 0,0,(short)screenWidth - 1, (short)screenHeight - 1 };

	SetConsoleWindowInfo(hConsole, TRUE, &rectWindow);

	SetConsoleMode(m_hConsoleIn, ENABLE_EXTENDED_FLAGS | ENABLE_WINDOW_INPUT | ENABLE_MOUSE_INPUT);
}

int IsInBounds(int x, int y)
{
	// Returns "true" if x and y are both in-bounds.
	if (x < 1 || x >= mapWidth - 1)
		return false;
	if (y < 1 || y >= mapHeight - 1)
		return false;
	return true;
}

int XYToIndex(int x, int y) {

	return y * mapWidth + x;
}

int N = 2;
int S = 3;
int E = 0;
int W = 1;

//------------------| E| W| N| S|		 
int directionsX[] = { 1,-1, 0, 0 };
int directionsY[] = { 0, 0,-1, 1 };


//Recursive Back Tracking
void carvePassage_RecursiveBacktracking(int x, int y)
{
	std::vector<int> ALL_DIRECTIONS({ N, E, S, W });
	std::random_shuffle(ALL_DIRECTIONS.begin(), ALL_DIRECTIONS.end());
	for (int it : ALL_DIRECTIONS) {

		int nx = x + (directionsX[it] * 2);
		int ny = y + (directionsY[it] * 2);

		if (IsInBounds(nx, ny) && maze[XYToIndex(nx, ny)] == 0) {
			maze[XYToIndex(nx, ny)] = 1;
			maze[XYToIndex(x, y)] = 1;
			maze[XYToIndex(nx - directionsX[it], ny - directionsY[it])] = 1;

			carvePassage_RecursiveBacktracking(nx, ny);
		}

	}

}

struct Node {
	int x;
	int y;
};

Node* hunt() {
	std::vector<int> ALL_DIRECTIONS({ N, E, S, W });
	std::random_shuffle(ALL_DIRECTIONS.begin(), ALL_DIRECTIONS.end());
	Node* node = new Node;
	for (size_t x = 0; x < mapWidth; x++)
	{
		for (size_t y = 0; y < mapHeight; y++)
		{
			if (maze[XYToIndex(x, y)] == 0) {

				bool noNeighborsClose = true;
				for (int it : ALL_DIRECTIONS)
				{
					int nx = x + (directionsX[it]);
					int ny = y + (directionsY[it]);
					if (IsInBounds(nx, ny))
					{
						if (maze[XYToIndex(nx, ny)] == 1) {
							noNeighborsClose = false;
						}
					}


				}
				if (noNeighborsClose&&IsInBounds(x, y))
					for (int it : ALL_DIRECTIONS)
					{
						int nx = x + (directionsX[it] * 2);
						int ny = y + (directionsY[it] * 2);
						if (IsInBounds(nx, ny) &&
							maze[XYToIndex(nx, ny)] == 1
							&& maze[XYToIndex(nx - directionsX[it], ny - directionsY[it])] == 0)
						{
							node->x = x;
							node->y = y;
							maze[XYToIndex(x, y)] = 1;
							maze[XYToIndex(nx - directionsX[it], ny - directionsY[it])] = 1;
							return node;
						}
					}
			}
		}
	}
	return nullptr;
}

Node* walk(int x, int y) {
	std::vector<int> ALL_DIRECTIONS({ N, E, S, W });
	std::random_shuffle(ALL_DIRECTIONS.begin(), ALL_DIRECTIONS.end());
	if (IsInBounds(x, y))
		for (int it : ALL_DIRECTIONS) {

			int nx = x + (directionsX[it] * 2);
			int ny = y + (directionsY[it] * 2);

			if (IsInBounds(nx, ny) && maze[XYToIndex(nx, ny)] == 0) {
				maze[XYToIndex(nx, ny)] = 1;
				maze[XYToIndex(x, y)] = 1;
				maze[XYToIndex(nx - directionsX[it], ny - directionsY[it])] = 1;

				Node* node = new Node;
				node->x = nx;
				node->y = ny;
				return node;
			}


		}
	return nullptr;

}

void carvePassage_HuntAndKill()
{


	Node* node;
	node = new Node;
	node->x = 1;
	node->y = 1;


	do {


		do {

			node = walk(node->x, node->y);

		} while (node != nullptr);

		node = hunt();

	} while (node != nullptr);
}

void  resetGrid() {
	maze = new int[mapWidth*mapHeight];

	for (size_t x = 0; x < mapWidth; x++) {
		for (size_t y = 0; y < mapHeight; y++)
		{
			maze[XYToIndex(x, y)] = 0;
		}
	}

}

void generateMaze(int selection) {

	switch (selection)
	{
	case 0:
		carvePassage_RecursiveBacktracking(1, 1);
		break;

	case 1:
		carvePassage_HuntAndKill();
		break;


	}


}
