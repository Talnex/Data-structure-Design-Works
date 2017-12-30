```c++

#include <iostream>
#include <string>
#include <math.h>
using namespace std;

#define NUM_VIEWS 13//图的节点的个数
#define NUM_POINTS 20//所有点的个数

class data{
public:
    string name[NUM_POINTS];//存储的节点的name信息，根据搜索结果的下标取出文件中的内容
    int Matrix[NUM_POINTS][NUM_POINTS];//图的所有节点的邻接矩阵，前NUM_VIEWS个是景点，后面的是中转点
    int Points[NUM_POINTS][2];//存放所有点的像素坐标
    double Distance[NUM_POINTS];//dijkstra算法专用存放最短路径长度
    int Path[NUM_POINTS];//dijkstra算法专用存放最短路径向量组
    int FootPrint[NUM_POINTS];//prim算法记录的路径点的下标数组
    int NUM_ALLPATHS;//由深度优先遍历赋值，为找到的路径的条数
    int** AllPaths;//所有路径的向量组，为二维数据
    
    int** creat_array(int NUM_ALLPATHS){
//        根据搜索出所有路径的数量NUM_ALLPATHS创建合适长度的二维数组
        return NULL;
    }
    int search_name(string name){
//        根据传入的name与name[]依次比较返回对应下标
        return 0;
    }
};

class file:public data
{
//    创建文件指针指向introduction.csv
public:
    void data_init(){
//        将文件Point.csv读入到Points[]中
//        将文件Matrix.csv读入到Matrix[][]中
    }
    void searchdata(string name){
//        根据Methods类中SearchData方法传入的string类型的name调用search_name然后根据返回的下标移动文件指针到指定位置然后输出
    }
};


class tools{
    double to_distance(int a,int b){
//        通过传入的两个点的编号根据勾股定理计算两点距离并返回
        return 0;
    }
    
};

class Methods:public tools,public file{//最短路径算法工具包
public:
    void Dijkstra(string name1,string name2){//一个点到其他点的最短路径算法
//        调用search_name()得到起点的下标
//        所有的距离需要调用to_distance()函数
//        结果写入Distance[]和Path[]
//        调用OpenGL(int *path,int b)动画演示最短路径
//        输出路径长度
    }
    void OpenGL(int *path,int b){
//        dijkstra算法调用此方法，动画演示最短路径
    }
    void OpenGL(int **allpaths,int NUM_ALLPATHS){
//        深度优先遍历算法调用此方法，静态显示所有路径
    }
    void OpenGL(int point){
//        查询类调用此方法，在地图上显示查询的点的位置
    }
    void OpenGl(int *footprint){
//        prim类调用此方法，输出最优路径
    }
    void OpenGL(){
//        主函数调用此方法，加载地图并显示
    }
    void DFS(string name1,string name2){
//        调用search_name()将名称转化为下标
//        创建计数器并将符合条件的路径的数量赋值给NUM_ALLPATHS
//        调用creat_array()函数创建路径的数组并赋值给All_Paths[][]
//        采用深度优先遍历方法将所有合适的路径记录到All_Paths[][]中
//        调用OpenGL(int **allpaths,int NUM_ALLPATHS)输出所有路径
    }
    void SearchData(){
//        设计交互，可以多次输入
//        调用search_data()
    }
    void Prim(){
//        输入界面，根据输入的节点使用prim算法
//        将路径输入到FootPrint[]数组里
//        调用OpenGL(int *footprint)显示路径
//        输出路径长度
//        充分考虑实际情况
    }
};

class graph:public Methods{
public:
//    在此调用多个方法
    void lab_init(){
        
    }
    void lab_1(){
        
    }
    void lab_2(){
        
    }
    void lab_3(){
        
    }
    void lab_4(){
        
    }
};
int main(){
//    在此主要设计用户交互和界面逻辑等
    return 0;
}


```
