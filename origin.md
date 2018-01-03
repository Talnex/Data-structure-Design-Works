# 第二次作业：设计模式--工厂模式
ps：因为当时有数据结构实验于是就与设计模式的作业一起用c++写了 </br>
工厂模式部分在最后面
##### 程序功能
- 建立一个图，输出一个城市到其他城市的最短路径或任意两个城市（Floyd算法有点问题，欢迎指正）的最短路径
- 城市的数量可修改，使用邻接表时可以自己决定是否停止输入
- 使用邻接矩阵、邻接表（暂时不能求最短路径和画图）、文件读写（暂未实现）创建图
- 利用opencv将图及最短路径图输出
- 图的大小和城市在图中的位置会根据城市的数量自己调整
- 利用工厂模式实现模式的选择
- 面向对象编程，图的工具箱等功能可扩展
- 时间关系不是所有的地方都考虑了异常情况，一些输入地方如果输错可以重新输入

#### 头文件及宏定义

```c++
#include <iostream>
#include <string>
#include <opencv2/opencv.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv/cvaux.hpp>
#include <math.h>
using namespace cv;
using namespace std;

int A;//建立的图的长宽
#define num 6//图的节点的个数
#define wuqiong 100//定义无穷大为100
```
#### 基本数据结构的创建
```c++
class node{//图的邻接表的节点类
public:
    int distance;
    int no;
    node* next;
    node(){
        distance=0;
        no=-1;
        next=NULL;
    }
};


class head{//图的表头类
public:
    string name;
    int no;
    node* first;
    friend class graph;
    head(){
        for(int i=0;i<num;i++){
            first=NULL;
            name="空";
            no=i;
        }
    }
};

class graph//基础图类
{
public:
    head head[num];
    int matrix[num][num];
    int search_no(string name){
        for(int i=0;i<num;i++){
            if(name==head[i].name) return i;
        }
        return -1;
    }
    graph(){
        cout<<"请输入图的节点的名字"<<endl;
        for(int i=0;i<num;i++){
            cin>>head[i].name;
        }
    }
    virtual void in(){//使用虚函数实现多态
    }
    virtual void out(){
    }
};
```
#### 三种继承类
```c++
class graph_matrix:public graph{//输入邻接矩阵创建图方法类
public:
    void in(){
        cout<<"请输入邻接矩阵："<<endl;
        for(int i=0;i<num;i++){
            for(int j=0;j<num;j++){
                cin>>matrix[i][j];
            }
        }
    }
    
    void out(){
        cout<<"邻接矩阵为："<<endl;
        for(int i=0;i<num;i++){
            for(int j=0;j<num;j++){
                cout<<matrix[i][j]<<" ";
            }
            cout<<endl;
        }
    }
};

class graph_node:public graph{//使用邻接表存储图
    void node_show(class head* a){
        if(a->first!=NULL){
            node* p=a->first;
            while (a->name!="空") {
                cout<<a->name<<"的关系:  ";
                while(p!=NULL){
                    cout<<"与"<<a[p->no].name<<"的距离:"<<p->distance<<"  ";
                    p=p->next;
                }
                a++;
                p=a->first;
                cout<<endl;
            }
        }
        else{
            cout<<a->name<<"的关系:  "<<endl;
            a++;
            node_show(a);
        }
    }
    void in(){
        string name;
        int i;
        while(1){
            cout<<"请输入要改变的城市的name:输入exit结束"<<endl;
            cin>>name;
            if(name=="exit") break;
            i=search_no(name);
            if(i==-1) cout<<"没有找到，请重新输入"<<endl;
            else insert(i);
        }
    }
    void out(){
        node_show(head);
    }
    void to_matrix(){//邻接表转化成邻接矩阵
    }
    void insert(int i){
        string name;
        int distance;
        int no;
        while (1) {
 ins:   cout<<"请输入要连接的城市的name"<<endl;
        cin>>name;
        if(name=="exit") break;
        no=search_no(name);
            if(no==-1){
                cout<<"没找到连接的城市"<<endl;
                continue;
            }
        if(head[i].first==NULL){
            cout<<"请输入两城市之间的距离d"<<endl;
            cin>>distance;
            node*p=new node;
            head[i].first=p;
            p->no=no;
            p->distance=distance;
            cout<<"成功创建第一个边"<<endl;
        }
        else{
            node* q=head[i].first;
            while(q->next!=NULL){
                if(head[q->no].name==name){
                    cout<<"已经有此条数据"<<endl;
                    goto ins;
                }
                q=q->next;
            }
            cout<<"请输入两城市之间的距离"<<endl;
            cin>>distance;
            node* t=new node;
            q->next=t;
            t->no=no;
            t->distance=distance;
            cout<<"成功添加边"<<endl;
        }
        }
    }
};

class graph_file:public graph{//使用文件io
};
```
#### 图的工具基础类
包括可视化工具和求最短路径算法，我觉得唯一的亮点就是计算点的像素坐标那里了
```c++
class opencv{//使用opencv对图进行可视化操作
public:
    void cv_show(graph * head){
        A=num*100;
        Mat picture(A,A,CV_8UC3,Scalar(255,255,255));
        Point point[num];
        string store;
        double x,y;
        for (int i=0; i<num; i++) {
            x=A/8+i*(A/4*3/num);
            y=A/2+pow(-1, i)*(sqrt(pow(A/4*3/2,2)-pow((x-A/2),2)));
            point[i]=Point(x,y);
        }
        for (int i=0; i<num; i++) {
            circle(picture, point[i], A/100, Scalar(0,255,0),-1);
            putText(picture, head->head[i].name, point[i], CV_FONT_HERSHEY_COMPLEX, 0.5, Scalar(255,0,0));
        }
        for(int i=0;i<num;i++){
            for (int j=i; j<num; j++) {
                if(head->matrix[i][j]==0||head->matrix[i][j]>=wuqiong) continue;
                line(picture, point[i], point[j], Scalar(0,0,0));
                store=to_string(head->matrix[i][j])+"km";
                putText(picture, store, Point((point[i].x+point[j].x)/2,(point[i].y+point[j].y)/2), CV_FONT_HERSHEY_COMPLEX, 0.3, Scalar(0,0,255));
            }
        }
        imshow("图的结构图", picture);
        cout<<"图的结构图已创建"<<endl;
        waitKey(0);
    }
    
};

class one_short{//最短路径算法工具包
public:
    int dist[num];//djtesila算法最短路径大小
    int path[num];//djtesila算法方向向量
    int d[num][num];//florid算法最短路径大小
    int p[num][num];//florid算法方向向量
    int v0;//djtesila要找的最短路径的点的下标
    void djtesila(graph * head,string name){//一个点到其他点的最短路径
        v0=head->search_no(name);
        bool fin[num];
        int i,k,v,min;
        for(v=0; v<num; v++)
        {
            dist[v] = head->matrix[v0][v];
            fin[v] = false;
            if(dist[v] <wuqiong&&dist[v]!=0)
                path[v] =v0;
            else
                path[v] = -1;
        }
        fin[v0]=true;
        dist[v0]=0;
        for(i=1; i<num; i++)
        {
            min= wuqiong;
            for(k=0; k<num; k++)
                if(!fin[k] && dist[k]<min)
                {
                    v=k;
                    min = dist[k];
                }
            if(min==wuqiong) return;
            fin[v]=true;
            for(k=0; k<num; k++)
                if(!fin[k] && (min+head->matrix[v][k])<dist[k])
                {
                    dist[k]=min+head->matrix[v][k];
                    path[k]=v;
                }
        }
        
        
    }
    void flroid(graph * head){//所有点到其他点的最短路径
        int i,j,k;
        for(i=0;i<num;i++)
        {
            for(j=0;j<num;j++)
            {
                d[i][j]=head->matrix[i][j];
                if ( i!=j && d[i][j] < wuqiong ) {
                    p[i][j]=i;
                }
                else p[i][j]=-1;
            }
        }
        for(k=0;k<num;k++)
            for(i=0;i<num;i++)
                for(j=0;j<num;j++)
                {
                    if(d[i][j]>(d[i][k]+d[k][j]))
                    {
                        d[i][j]=d[i][k]+d[k][j];
                        p[i][j]=k;
                    }
                }
                    
                    
                    
    }
};
```
#### 图的工具包继承类，可扩展
```c++
class graph_tools:public opencv,public one_short{//图操作的工具包合集
public:
    void florid_out(){
        cout<<"florid的最短路径数组"<<endl;
        for (int i=0; i<num; i++) {
            for (int j=0; j<num; j++) {
                cout<<d[i][j]<<"  ";
            }
            cout<<endl;
        }
        cout<<"florid的最短路径向量"<<endl;
        for (int i=0; i<num; i++) {
            for (int j=0; j<num; j++) {
                cout<<p[i][j]<<"  ";
            }
            cout<<endl;
        }
    }
    void djtesila_out(graph * head){
        cout<<"djtesila的最短路径数组"<<endl;
        for (int i=0; i<num; i++) {
            cout<<dist[i]<<"  ";
        }
        cout<<endl;
        cout<<"djtesila的最短路径向量"<<endl;
        for (int i=0; i<num; i++) {
            cout<<path[i]<<"  ";
        }
        cout<<endl;
        int t;
        for (int i=0; i<num; i++) {
            t=i;
            cout<<"到"<<head->head[i].name<<"的最短路径：  ";
            while (t!=-1) {
                cout<<head->head[t].name<<"<-";
                t=path[t];
            }
            cout<<endl;
        }
    }
    void cv_show_florid(graph * head,int no1,int no2){
        int t;
        A=num*100;
        Mat picture(A,A,CV_8UC3,Scalar(255,255,255));
        Point point[num];
        double x,y;
        for (int i=0; i<num; i++) {
            x=A/8+i*(A/4*3/num);
            y=A/2+pow(-1, i)*(sqrt(pow(A/4*3/2,2)-pow((x-A/2),2)));
            point[i]=Point(x,y);
        }
        for (int i=0; i<num; i++) {
            circle(picture, point[i], A/100, Scalar(0,255,0),-1);
            putText(picture, head->head[i].name, point[i], CV_FONT_HERSHEY_COMPLEX, 0.5, Scalar(255,0,0));
        }
        t=no2;
        cout<<head->head[no1].name<<"到"<<head->head[no2].name<<"的最短路径：  ";
        while (t!=-1) {
            if(p[no1][t]!=-1){
                line(picture, point[t],point[p[no1][t]],Scalar(0,255,255),1.2);
            }
            cout<<head->head[t].name<<"<-";
            t=p[no1][t];
            
        }
        cout<<endl;
        imshow(head->head[no1].name+"到"+head->head[no2].name+"的最短路径图", picture);
        cout<<head->head[no1].name+"到"+head->head[no2].name+"的最短路径图已创建"<<endl;
        waitKey(0);
    }
};
```
#### 作业部分：简单工厂模式
```c++

class factory{
public:
    graph * opr=NULL;//利用基类指针实现工厂模式
    void set_mode(int mode){
        switch (mode) {
            case 1:
                opr=new graph_matrix;
                cout<<"Mode: matrix"<<endl;
                break;
            case 2:
                opr=new graph_node;
                cout<<"Mode: node"<<endl;
                break;
            case 3:
                opr=new graph_file;
                cout<<"Mode: file"<<endl;
                break;
        }
    }
};
```
#### main函数部分
```c++
int main(){
    graph_tools tools;
    int mode;
    string name1,name2;
    cout<<"请输入模式：1.邻接矩阵 2.邻接表 3.文件读写"<<endl;
IN:    try {
        cin>>mode;
    } catch (...) {
        cout<<"请重新输入"<<endl;
        goto IN;
    }
    factory factory;
    factory.set_mode(mode);
    factory.opr->in();
    factory.opr->out();
    cout<<"请输入djtesila算法求最短路径的城市的名字："<<endl;
    cin>>name1;
    tools.djtesila(factory.opr,name1);
    tools.djtesila_out(factory.opr);
    tools.flroid(factory.opr);
    tools.florid_out();
    tools.cv_show(factory.opr);
    cout<<"请输入florid算法的起点城市名字："<<endl;
    cin>>name1;
    cout<<"请输入florid算法的终点城市名字："<<endl;
    cin>>name2;
    tools.cv_show_florid(factory.opr,factory.opr->search_no(name1),factory.opr->search_no(name2));
    return 0;
}

//样例邻接矩阵
/*
0 999 999 1 999 1
999 0 999 999 1 1
999 999 0 999 4 1
1 999 999 0 5 999
999 1 4 5 0 999
1 1 1 999 999 0
*/

```
#### 样例输入和样例输出
<img width="600" alt="69138468-691b-403a-84c6-b2068dbbc8a5" src="https://user-images.githubusercontent.com/33219211/33715553-f09fc05c-db8d-11e7-865a-fcf3562dd77f.png">

#### 这个地方有个错误欢迎指正
Floyd算法计算出的路径向量数组与上面的dj算法计算的数组每一行几乎都会错一个，导致输出路径出现问题，但距离数组没有问题
<img width="570" alt="e8261612449efeb3f81a34b45df4b12c" src="https://user-images.githubusercontent.com/33219211/33715571-fecd3d76-db8d-11e7-9db6-a8a2f8f4c0b5.png">

<img width="600" alt="c34df19d-5f47-4333-b8e1-3e59655349c9" src="https://user-images.githubusercontent.com/33219211/33715581-08793690-db8e-11e7-940c-94c1bed5eb56.png">

#### 这是极端情况……
<img width="600" alt="c9dc6a66f400fc136234c7a965fda931" src="https://user-images.githubusercontent.com/33219211/33715730-85b27aae-db8e-11e7-90f7-150d40f92df4.png">

## java中为什么有了接口还要有抽象类？
- 可以认为接口是抽象的方法而抽象类是可以包含具体对象的抽象对象，也就是说对于部分自带具体对象的情形接口无法做到。
- 抽象类中可以自带部分具体的方法，对于一些大型程序来说每次实现接口都要重写大量的代码，而如果选择继承抽象类可以省去大量的代码
- 抽象类对于部分重要算法和数据的保密性、封装性更好，接口实现需要知道的内部的东西更多，而抽象类只要给他一张说明表就行了，不需要知道核心。 
- 为了与java以前的程序进行兼容保留了抽象类

  
