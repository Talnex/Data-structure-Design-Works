```c++

#include <cv.h>
#include <cxcore.h>
#include <cvaux.h>
#include <opencv2/opencv.hpp>
#include <opencv2/imgproc.hpp>
#include <highgui.h>
#include <iostream>
#include <math.h>

using namespace cv;
using namespace std;

#define NUM 5
#define WIDE 300
static Mat map1=imread("/Users/talnex/Downloads/map.jpg");
static Mat mapshow;
static Mat map1ROI;
double scale=0.20;
#define dynamic_num 10

void MOUSE(int event,int x,int y,int flags,void* param){
    if ( event == EVENT_MOUSEMOVE )
    {
        if(x>=WIDE*scale/2){
            if (x<=mapshow.cols-WIDE*scale/2) {
                if (y>=WIDE*scale/2) {
                    if (y<=mapshow.rows-WIDE*scale/2) {
                        map1ROI=map1(Rect(x/scale-WIDE/2,y/scale-WIDE/2,WIDE,WIDE));//x正常 y正常
                    }
                    else{
                        map1ROI=map1(Rect(x/scale-WIDE/2,(mapshow.rows-WIDE*scale/2)/scale-WIDE/2,WIDE,WIDE));//x正常 y在最下面
                    }
                }
                else{
                    map1ROI=map1(Rect(x/scale-WIDE/2,0,WIDE,WIDE));//x正常 y在最上面
                }
            }
            else{
                if (y>=WIDE*scale/2) {
                    if (y<=mapshow.rows-WIDE*scale/2) {
                        map1ROI=map1(Rect((mapshow.cols-WIDE*scale/2)/scale-WIDE/2,y/scale-WIDE/2,WIDE,WIDE));//x在最右边 y正常
                    }
                    else{
                        map1ROI=map1(Rect((mapshow.cols-WIDE*scale/2)/scale-WIDE/2,(mapshow.rows-WIDE*scale/2)/scale-WIDE/2,WIDE,WIDE));//x在最右边 y在最下面
                    }
                }
                else{
                    map1ROI=map1(Rect((mapshow.cols-WIDE*scale/2)/scale-WIDE/2,0,WIDE,WIDE));//x在最右边 y在最上面
                }
            }
        }
        else{
            if (y>=WIDE*scale/2) {
                if (y<=mapshow.rows-WIDE*scale/2) {
                    map1ROI=map1(Rect(0,y/scale-WIDE/2,WIDE,WIDE));//x在最左边 y正常
                }
                else{
                    map1ROI=map1(Rect(0,(mapshow.rows-WIDE*scale/2)/scale-WIDE/2,WIDE,WIDE));//x在最左边 y在最下面
                }
            }
            else{
                map1ROI=map1(Rect(0,0,WIDE,WIDE));//x在最左边 y在最上面
            }
        }
        imshow("map1ROI",map1ROI);
    }
}
class OPENCV{
public:
    void MapShow(){
        Size dsize=Size(map1.cols*scale,map1.rows*scale);
        resize(map1, mapshow, dsize,0,0,CV_INTER_AREA);
        imshow("mapshow", mapshow);
        setMouseCallback("mapshow",MOUSE,NULL);
        waitKey(0);
        destroyWindow("mapshow");
        destroyWindow("map1ROI");
    }
    void Dynamic_Show(){
        init();
        int a,b;
        Size dsize=Size(map1.cols*scale,map1.rows*scale);
        resize(map1, mapshow, dsize,0,0,CV_INTER_AREA);
        for (int i=0; Path[i+1]!=-1; i++) {
            a=(Points[Path[i+1]].x-Points[Path[i]].x)/dynamic_num;
            b=(Points[Path[i+1]].y-Points[Path[i]].y)/dynamic_num;
            for (int j=0; j<dynamic_num; j++) {
                line(map1,Point(Points[Path[i]].x+j*a,Points[Path[i]].y+j*b), Point(Points[Path[i]].x+(j+1)*a,Points[Path[i]].y+(j+1)*b), Scalar(0,0,255),10);
                line(mapshow,Point((Points[Path[i]].x+j*a)*scale,(Points[Path[i]].y+j*b)*scale), Point((Points[Path[i]].x+(j+1)*a)*scale,(Points[Path[i]].y+(j+1)*b)*scale), Scalar(0,0,255),10*scale);
                imshow("mapshow", mapshow);
                waitKey(10);
            }
        }
        setMouseCallback("mapshow",MOUSE,NULL);
        waitKey(0);
        destroyWindow("mapshow");
        destroyWindow("map1ROI");
        map1.release();
        map1=imread("/Users/talnex/Downloads/map.jpg");
    }
    void init(){
        Path[0]=1;
        Path[1]=2;
        Path[2]=0;
        Path[3]=-1;
        Points[0]=Point(100,100);
        Points[1]=Point(1000,1000);
        Points[2]=Point(1000,100);
    }
    
private:
    int Path[NUM];//动态演示路径专用
    Point Points[NUM+1];
    int AllPaths[NUM][NUM+1];
    
};

int main(){
    OPENCV opencv;
    opencv.MapShow();
    opencv.Dynamic_Show();
    return 0;
}


```
