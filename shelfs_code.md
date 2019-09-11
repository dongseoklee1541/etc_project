
``` C
#include <stdio.h>
#include <stdlib.h> // calloc, free 함수가 선언된 헤더 파일
#include <opencv2/opencv.hpp> // opencv2


using namespace cv;
typedef struct box
{
	int w;
	int h;
	bool r;
}Box;


typedef struct Canvas
{
	int x;
	int y;
	int w;
	int h;
	bool v;
}Canvas;

typedef struct color
{
	int R;
	int G;
	int B;
}Color;
void memoryalloc(Box**r, int size);
void inputvalue(Box*r, int*testcase, int size);
void Nextfit(Box *r, int size);
void Smaller_height_first(Box *r, int size);
void Lager_height_first(Box *r, int size);
void Lager_height_first2(Box *r, int size);


int main()
{
	int testCase[] = { 11, 21, 43, 39, 13, 52, 35, 25, 28, 20, 66, 55, 54, 62, 50, 61, 49, 16, 60, 51, 39, 27, 54, 62, 65, 13, 34, 50, 48, 63, 19, 41, 18, 61, 23, 51, 39, 58, 35, 14, 35, 31, 67, 58, 25, 24, 58, 63, 59, 57, 69, 57, 34, 28, 61, 30, 18, 57, 10, 26, 42, 66, 31, 50, 63, 13, 42, 10, 57, 63, 61, 54, 50, 53, 20, 54, 66, 31, 48, 35, 28, 25, 37, 54, 32, 50, 42, 49, 62, 37, 31, 24, 63, 44, 66, 29, 25, 51, 52, 17, 67, 55, 27, 48, 23, 39, 38, 41, 46, 15, 46, 68, 24, 41, 38, 20, 33, 42, 12, 12, 51, 31, 53, 41, 25, 28, 39, 69, 61, 12, 55, 59, 35, 60, 13, 60, 22, 41, 60, 68, 28, 33, 31, 60, 27, 48, 38, 60, 19, 63, 28, 50, 24, 31, 42, 38, 11, 17, 50, 36, 27, 59, 42, 25, 16, 61, 35, 19, 32, 67, 40, 53, 33, 42, 15, 40, 62, 23, 42, 19, 57, 42, 43, 59, 12, 10, 45, 68, 16, 15, 15, 34, 67, 33, 27, 54, 53, 64, 18, 22 };
	//int testCase[] = {20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,20, 40,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,40, 30,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,16, 16,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,36, 10,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,10, 36,6,15,6,15,6,15,6,15,6,15,6,15,6,15,6,15,6,15,15,6,15,6,15,6,15,6,15,6,15,6,15,6,15,6,15,6,15,6,15,6};
	//int testCase[] = { 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11, 10,20, 22,11 };
	//int testCase[] = { 160,160,80,80,80,80,60,60,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10 };
	//int testCase[] = {67,75,67,75,67,75,67,75,67,75,67,75,67,75,67,75,67,75,67,75,67,75,67,75,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11,50,11};
	// testCase 5번째는 4번, 1번째는 3번
	int numCase = (sizeof(testCase) / sizeof(int)) / 2; // 집어 넣을 박스의 숫자
	int w = 256; // canvas 의 width
	int h = 256; // canvas 의 height

	Box *r;
	Mat img(h, w, CV_8UC3, Scalar(0, 0, 0));
	memoryalloc(&r, numCase); // 메모리 동적 할당
	inputvalue(r, testCase, numCase); // 값 할당
	//Nextfit(r, numCase); // (1번)
	//Smaller_height_first(r, numCase); //높이가 작은 순으로 정렬 (2번)
	//Lager_height_first(r, numCase); // 높이가 큰 순으로 정렬
	Lager_height_first2(r, numCase);

	//Color col;
	//Canvas can = { 0, 0, w, h, true }; // 구조체로 canvas 선언 및 초기화
	//int u = 0; // used space
	//int m = 0; // max height
	//int i = numCase;
	//int j;
	//int temp;
	//while (i--) //find item for next column
	//{
	//   for (j = i; j >= 0; j--)
	//   {
	//      if (r[j].w <= can.w - can.x) // 박스의 너비가 캔버스의 남은 너비 보다 작거나 같으면 나가
	//      {
	//         break;
	//      }
	//   }
	//   //go to the next row
	//   if (j < 0)
	//   {
	//      j = i; // 다시 초기화
	//      can.x = 0; // x부터 시작
	//      can.y += m;
	//      m = 0;
	//   }
	//   //find item for bottom of space
	//   if (r[j].h > can.h - can.y)
	//   { // 상자의 높이가 캔버스의 남은 높이보다 길면서 너비가 남은 높이보다 길다면 처음으로
	//      if (r[j].w <= can.h - can.y)
	//      {// 상자의 너비가 캔버스의 남은 높이보다 작다면 실행
	//         temp = r[i].w;
	//         r[i].w = r[i].h;
	//         r[i].h = temp;
	//         r[i].r = false;
	//      }// 너비와 높이를 캔버스 크기에 맞게 회전
	//      else continue;
	//   }

	//   if (r[j].r) // 회전된 녀석과 아닌 녀석 색으로 구별
	//   {
	//      col = { 0,0,255 };
	//   }
	//   else
	//   {
	//      col = { 255,0,0 };
	//   }//draw
	//    //선굵기 3인 빨간색 사각형을 그립니다.  
	//   rectangle(img, Point(can.x, can.y), Point(can.x + r[j].w, can.y + r[j].h), Scalar(col.R, col.G, col.B), -1);
	//   rectangle(img, Point(can.x, can.y), Point(can.x + r[j].w, can.y + r[j].h), Scalar(255, 255, 255), 1);
	//   can.x += r[j].w;
	//   u += r[j].w*r[j].h;
	//   if (r[j].h > m)
	//   {
	//      m = r[j].h;	
	//   }
	//	memmove(&r[j], &r[j + 1], sizeof(Box)*(i - j + 1));
	//	//for (int k = j; k <= i; k++) // splice 와 같이 사용한 상자는 배열에서 삭제한다.
	//	// {
	//	//	 memmove(&r[k], &r[k + 1], sizeof(box));
	//	// }
	//   
	//}
	//imshow("result", img);
	//waitKey(0);
	free(r); // 메모리 동적할당 해제
	return 0;
}

void memoryalloc(Box**r, int size)
{
	*r = (Box *)calloc((size), sizeof(Box)); // 구조체 배열 선언
	if (*r == NULL) // 동적 할당에 실패하면 NULL 반환 및 프로그램 종료
	{
		printf("메모리가 부족합니다.\n");
		exit(1);
	}
}

void inputvalue(Box*r, int*testcase, int size)
{
	int temp = 0;
	for (int i = 0; i < size; i++) // w,h 값을 할당
	{
		r[i].w = testcase[i * 2]; // (*(r+i)).w  또는 (r+i) -> w 도 가능
		r[i].h = testcase[i * 2 + 1];
		printf("%d %d %d\n", i, r[i].w, r[i].h);
		r[i].r = false;
		//if (r[i].w > r[i].h) // rotation
		//{
		//   temp = r[i].w;
		//   r[i].w = r[i].h;
		//   r[i].h = temp;

		//}
	}
}

void Smaller_height_first(box *r, int numcase) // Bubble Sort
{
	Box temp;
	//sorting by height
	for (int i = 0; i < numcase - 1; i++)
	{
		for (int j = 0; j < numcase - 1 - i; j++)
		{
			if (r[j].h > r[j + 1].h)
			{
				temp = r[j];
				r[j] = r[j + 1];
				r[j + 1] = temp;
			}
		}
	}
	Nextfit(r, numcase);
}

void Lager_height_first(box *r, int numcase) // Bubble Sort
{
	Box temp;
	//sorting by height
	for (int i = 0; i < numcase - 1; i++)
	{
		for (int j = 0; j < numcase - 1 - i; j++)
		{
			if (r[j].h < r[j + 1].h)
			{
				temp = r[j];
				r[j] = r[j + 1];
				r[j + 1] = temp;
			}
		}
	}
	Nextfit(r, numcase);
}

void Lager_height_first2(Box *r, int size) // 3-2 번
{
	
	int i, j;
	int u = 0;//used space
	int maxHeight = 0;//maxHeight
	Box temp;
	Canvas can = { 0, 0, 256, 256, true };
	Mat img(256, 256, CV_8UC3, Scalar(0, 0, 0));
	rectangle(img, Point(0, 0), Point(256, 256), Scalar(0, 255, 0), 2);
	for (int i = 0; i < size - 1; i++) // 높이가 작은 순서대로 정렬
	{
		for (int j = 0; j < size - 1 - i; j++)
		{
			if (r[j].h > r[j + 1].h)
			{
				temp = r[j];
				r[j] = r[j + 1];
				r[j + 1] = temp;
			}
		}
	}
	i = size;
	 for(i=size;i > -1 ; i--)//i가 -1까지 가서 멈춤 while (i--)
	{
		for (j = i; j >= -1; j--) // j>=-1
		{
			if (can.x + r[j].w <= can.w) // 너비에 들어간다면 그걸 넣고
			{
				break;
			}
		}
		if (j < 0) // 마지막까지 탐색후 y값을 높이고 다시 돌아가기 
		{
			j = i;
			can.x = 0;
			can.y += maxHeight;
			maxHeight = 0;
		}
		if (can.y + r[j].h > can.h)//혹시.. 밑 칸에 들어갈 작은 상자 있니?
		{
			continue;
		}	
		//draw
		rectangle(img, Point(can.x, can.y), Point(can.x + r[j].w, can.y + r[i].h), Scalar(0, 0, 255), -1);
		rectangle(img, Point(can.x, can.y), Point(can.x + r[j].w, can.y + r[i].h), Scalar(255, 0, 0), 1);
		can.x += r[j].w;
		if (r[j].h > maxHeight)
		{
			maxHeight = r[j].h;
		}
		u += (r[j].w)*(r[j].h);
		memmove(&r[j], &r[j + 1], sizeof(Box)*(i - j + 1));
		//for (int k = j; k <= i; k++) // splice 와 같이 사용한 상자는 배열에서 삭제한다.
		//{
		//	memmove(&r[k], &r[k+1], sizeof(Box));
		//}
	}
	imshow("result", img);
	waitKey(0);
}


void Nextfit(Box *r, int size)
{
	int i;
	int u = 0;
	int maxHeight = 0;

	Canvas can = { 0, 0, 256, 256, true };
	Mat img(can.h, can.w, CV_8UC3, Scalar(0, 0, 0));
	rectangle(img, Point(0, 0), Point(256, 256), Scalar(0, 255, 0), 3);
	
	for (i = 0; i < size; i++)
	{
		if (can.x + r[i].w > can.w)
		{
			maxHeight = r[i].h;
			can.x = 0;
			can.y += maxHeight;
			maxHeight = 0; // reset
		}
		if (can.y + r[i].h > can.h) // 상자넣기 끝
		{
			break;
		}
		rectangle(img, Point(can.x, can.y), Point(can.x + r[i].w, can.y + r[i].h), Scalar(0, 0, 255), -1);
		rectangle(img, Point(can.x, can.y), Point(can.x + r[i].w, can.y + r[i].h), Scalar(255, 0, 0), 1);
		can.x += r[i].w;
		u += (r[i].w)*(r[i].h);
		//if (r[i].h > maxHeight)
		//{
		//   maxHeight = r[i].h;
		//}
	}
	int center_x = int(256 / 2.0);
	int center_y = int(256 / 2.0);
	int thickness = 2;
	//float res = (u / 256 * 256) * 100;
	//putText(img, "OpenCV", location, font, fontScale, (255, 255, 255), 3);
	imshow("result", img);
	waitKey(0);
}
```
