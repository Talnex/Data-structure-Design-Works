```c++

int p[NUM_POINTS];
bool final[NUM_POINTS];//若final[M] = 1则说明 顶点vi已在集合S中
int n=NUM_POINTS;//顶点个数
int v0 = 0;//源点
class Methods :public data
{//最短路径算法工具包
public:
	void Dijkstra(int a, int b)
	{
		
		int min, v, w;
		for (v = 0; v < n; v++)//初始化
		{
			final[v] = 0;
			Distance[v] = Matrix[v0][v];
			if (Distance[v] < MAX && Distance[v] != 0)
				p[v] = v0;
			else
				p[v] = -1;
		}
		Distance[v0] = 0;
		final[v0] = 1;

		for (int i = 1; i < n; i++)
		{
			min = MAX;
			for (w = 0; w < n; w++)
			if (!final[w] && Distance[w] < min)
			{
				v = w;
				min = Distance[w];
			}//选出了最短的一条边
			if (min == MAX) return;
			final[v] = 1;//将最短边的点W放入到v数组里面
			for (w = 0; w < n; w++)
			if (!final[w] && (min + Matrix[v][w] < Distance[w]))//表示折线的距离小于直达的距离
			{
				Distance[w] = min + Matrix[v][w];
				p[w] = v;
			}
		}
		cout << "D数组为：" << endl;
		for (int i = 0; i < n; i++)
		{
			cout << Distance[i] << " ";
		}
		cout << endl;
		cout << "p数组为:" << endl;
		for (int i = 0; i < n; i++)
		{
			cout << p[i] << " ";
		}
		cout << endl;
		cout << "________a,b两点之间的最短距离为：____________" << endl;
		cout << Distance[b] << endl;
		cout << p[b] << endl;
		int j;
		for (int i = 0; p[b] != -1; i++)
		{
			Path[i] = p[b];
			p[b] = p[Path[i]];
			j = i;
		}
		cout << "从a到b的路径还原为Path数组：" << endl;
		cout << "-1" << " ";
		for (int i = j; i >= 0; i--)
		{
			cout << Path[i] << " ";
		}
		cout << endl;
	}
};

```
