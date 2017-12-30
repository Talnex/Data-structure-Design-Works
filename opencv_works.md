```c++

#include <cv.h>
#include <cxcore.h>
#include <cvaux.h>
#include <opencv2/opencv.hpp>
#include <opencv2/imgproc.hpp>
#include <highgui.h>
#include <iostream>

using namespace cv;
using namespace std;

#define NUM 3
#define WIDE 400
static Mat map1=imread("/Users/talnex/Downloads/map.jpg");
static Mat mapshow;
static Mat map1ROI;
double scale=0.20;

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
    }
    void Dynamic_Show(){
        
    }
    void Show_3D(string file){
        
    }
    
    
private:
    int Path[NUM];//动态演示路径专用
    int Points[NUM+1];
    int AllPaths[NUM][NUM+1];
    
};

int main(){
    OPENCV opencv;
    opencv.MapShow();
    return 0;
}

//    IplImage* map=cvLoadImage("/Users/talnex/Downloads/map.jpg");
//    IplImage* map2;
//    CvSize size;
//    double scale=0.22;
//    size.width=map->width*scale;
//    size.height=map->height*scale;
//    map2=cvCreateImage(size, map->depth,map->nChannels);
//    cvResize(map, map2,CV_INTER_AREA);
//    cvNamedWindow("map");
//    cvShowImage("map", map2);
//    waitKey(0);
//    cvDestroyWindow("map");


//使用第二种方法缩放图像 CV_INTER_AREA采样方式适合去除波纹
//http://blog.csdn.net/zxj820/article/details/50594955
//   动态演示路径核心算法
//    Point points[2]={Point(100,100),Point(5000,5000)};
//    Mat map=imread("/Users/talnex/Downloads/map.jpg");
//    Mat mapshow;
//    double scale=0.22;
//    Size dsize=Size(map.cols*scale,map.rows*scale);
//    int num=50;
//    Point a=points[0],b=points[1];
//    for (int i=0; i<num; i++) {
//        resize(map, mapshow, dsize,0,0,CV_INTER_AREA);
//        imshow("map", mapshow);
//        waitKey(1);
//        line(map,Point(a.x,a.y),Point(a.x+(b.x-a.x)/num,a.y+(b.y-a.y)/num), Scalar(0,0,255),10);
//        a.x=a.x+(b.x-a.x)/num;
//        a.y=a.y+(b.y-a.y)/num;
//    }
//    waitKey(0);

```
