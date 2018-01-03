```c++

void data_init()
	{
		ifstream infile;
		infile.open("E:\\桌面上的一些东西\\Points.txt", ios::in);// 将文件Point.csv读入到Points[]中
		if (!infile)
		{
			printf("Points open error!\n");
			system("pause");
			exit(1);
		}
		for (int i = 0; i < NUM_POINTS; i++)
		{
			for (int j = 0; j < 2; j++)
			{
				infile >> Points[i][j];
				cout << Points[i][j] << endl;
			}
		}
		infile.close();

		infile.open("E:\\桌面上的一些东西\\aMatrix.txt");   //将文件Matrix.csv读入到Matrix[][]中
		if (!infile)
		{
			printf("Matrix open error!\n");
			system("pause");
			exit(1);
		}
		for (int i = 0; i < NUM_POINTS; i++)
		{
			for (int j = 0; j < NUM_POINTS; j++)
			{
				infile >> Matrix[i][j];
			}
		}
		for (int i = 0; i < NUM_POINTS; i++)
		{
			for (int j = 0; j < NUM_POINTS; j++)
			{
				cout << Matrix[i][j] << " ";
			}
			cout << endl;

		}
		infile.close();


		infile.open("E:\\桌面上的一些东西\\Name.csv");//根据Methods类中SearchData方法传入的string类型的name调用search_name然后根据返回的下标移动文件指针到指定位置然后输出
		if (!infile)
		{
			printf("Name open error!\n");
			system("pause");
			exit(1);
		}
		for (int i = 0; i < NUM_VIEWS; i++)
		{
			infile >> name[i];
		}
		for (int i = 0; i < NUM_VIEWS; i++)
		{
			cout << name[i] << endl;
		}
		infile.close();
	}
  
  ```
