**题目描述**

给定一个矩阵matrix，按照“之”字形的方式打印这个矩阵，如样例所示。

**输入描述**:

```
输入包含多行，第一行包含两个整数n和m(1 \leq n,m \leq 200)(1≤n,m≤200)，代表矩阵的行数和列数，
接下来n行，每行m个整数，代表矩阵matrix(1\leq matrix[i][j] \leq 40000)(1≤matrix[i][j]≤40000)。
```

**输出描述**:

```
输出一行 ，代表“之”字形输出的矩阵。
```

**输入**

```
4 4
1 2 3 4
5 6 7 8
9 10 11 12
13 14 15 16
```

**输出**

```
1 2 5 9 6 3 4 7 10 13 14 11 8 12 15 16
```

```cpp

#include<iostream>
#include<vector>

using namespace std;

/***********读取二维方阵*****************/
vector<vector<int> > READ_ARR_2D() {
	int m, n;
	cin >> m >>n;
	vector<vector<int> > arr(m, vector<int>(n));
	for (int i = 0; i < m; i++) {
		for (int j = 0; j < n; j++) {
			int temp;
			cin >> temp;
			arr[i][j] = temp;
		}
	}
	return arr;
}
/*
//打印数组
void PrintArr(vector<vector<int> > matrix) {
	for (int i = 0; i<matrix.size(); i++) {
		for (int j = 0; j<matrix[0].size(); j++) {
			cout << matrix[i][j] << " ";//cout << matrix[i][j] + " ";
		}
		cout << endl;
	}
}*/

void Print_Line(vector<vector<int> > arr, int sx, int sy, int ex, int ey, bool flag ){
    if(flag){
        //e2s
        while(ex != sx-1){
            cout<<arr[ex--][ey++]<<" ";
        }
    }else{
        while(sx !=ex+1){
            cout<<arr[sx++][sy--]<<" ";
        }
    }
}

void Print_Z_Arr(vector<vector<int> > arr){
    bool flag =true;//斜向上
    int sx =0 ;int sy=0;
    int ex=0; int ey=0;
    int ENDX= arr.size()-1;
    int ENDY = arr[0].size()-1;
    while(sx!=ENDX+1){
        Print_Line(arr,sx,sy,ex,ey,flag);
        sx = sy==ENDY? sx+1:sx;
        sy = sy==ENDY? sy  :sy+1;
        //以下两行的顺序不能换，因为是按照ex来比较的，所以要保证ex最后变化！！！
        ey = ex==ENDX? ey+1:ey;
        ex = ex==ENDX? ex  :ex+1;
       
        flag=!flag;
    }
    cout<<endl;
}


int main(){
    vector<vector<int> > arr = READ_ARR_2D();
    Print_Z_Arr(arr);
}
```
