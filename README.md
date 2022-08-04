# dinosaur-jumping-game
# c program
# dinosaur-jumping-game
#include<stdio.h>
#include<windows.h>
#include<conio.h>
#include<time.h>
#include <stdbool.h>
#define DINO_BOTTOM_Y 12
#define TREE_BOTTOM_Y 20
#define TREE_BOTTOM_X 45
void SetConsoleView(){
	system("mode con:cols=100 lines=25");
	system("title Google?Dinosaurs. By BlockDMask.");
}
void GotoXY(int x,int y){
	COORD Pos;
	Pos.X=2*x;
	Pos.Y=y;
	SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE),Pos);
}
int GetKeyDown(){
	if(_kbhit()!=0){
		return _getch();
	}
	return 0;
}
void DrawDino(int dinoY){
	GotoXY(0,dinoY);
	static bool legFlag=true;
	printf("        $$$$$$$\n");
	printf("       $$ $$$$$$\n");
	printf("       $$$$$$$$$\n");
	printf("$      $$$      \n");
	printf("$$     $$$$$$$  \n");
	printf("$$$   $$$$$     \n");
	printf(" $$   $$$$$$$$$$ \n");
	printf(" $$$$$$$$$$$     \n");
	printf("  $$$$$$$$$$  \n");
	printf("    $$$$$$$$   \n");
	printf("     $$  $$    \n");
	if(legFlag){
		printf("   $   $$$   \n");
		printf("   $$          ");
	}
	else{
		printf("   $$$  $     \n");
		printf("        $$      ");
		legFlag=true;
	}
}
void DrawTree(int treeX){
	GotoXY(treeX, TREE_BOTTOM_Y);
	printf("$$$$");
	GotoXY(treeX,TREE_BOTTOM_Y+1);
	printf(" $$ ");
	GotoXY(treeX,TREE_BOTTOM_Y+2);
	printf(" $$ ");
	GotoXY(treeX,TREE_BOTTOM_Y+3);
	printf(" $$ ");
	GotoXY(treeX,TREE_BOTTOM_Y+4);
	printf(" $$ ");
}
void DrawGameOver(const int score){
	system("cls");
	int x=18;
	int y=8;
	GotoXY(x,y);
	printf("===========================");
	GotoXY(x,y+1);
	printf("======G A M E O V E R======");
	GotoXY(x,y+2);
	printf("===========================");
	GotoXY(x,y+5);
	printf("SCORE : %d",score);
	printf("\n\n\n\n\n\n\n\n\n");
	system("pause");
}
bool isCollision(const int treeX, const int dinoY){
	GotoXY(0,0);
	printf("treeX : %d, dinoY : %d",treeX,dinoY);
	if(treeX<=8&&treeX>=4&&dinoY>8){
		return true; //충돌하면 true 아니면 false  
	}
	return false;
}
int main(void){
	SetConsoleView();
	while(true){
	bool isJumping=false;
	bool isBottom=true;
	const int gravity=3;
	int dinoY=DINO_BOTTOM_Y;
    int treeX=TREE_BOTTOM_X;
    int score=0;
	clock_t start,curr;
	start=clock();
    while(true){
    	if(GetKeyDown()=='z'&&isBottom){
    		isJumping=true;
    		isBottom=false;
		}
		if(isJumping){
			dinoY-=gravity;
		}
		else{
			dinoY+=gravity;
		}
		if(dinoY>=DINO_BOTTOM_Y){
			dinoY=DINO_BOTTOM_Y;
			isBottom=true;
		}
		if(dinoY<=3){
			isJumping=false;
		}
		treeX-=2; //나무가 왼쪽으로 가게하고 왼쪽 끝으로 가면 닷 초기 위치로 소환  
		if(treeX<=0){
			treeX=TREE_BOTTOM_X;
		}
		DrawDino(dinoY);
		DrawTree(treeX);
		curr=clock(); //현재시간 받기  
		if(((curr-start)/CLOCKS_PER_SEC)>=1){ //1초를 넘었을때  
			score++;
			start=clock();
		}
		sleep(60);
		system("cls");
		GotoXY(22,0);
		printf("Score : %d ",score);
	}
		DrawGameOver(score);
	}
	return 0;
}
