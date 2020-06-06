# nw
#include <iostream> 
#include <string> 
using namespace std;
struct seq{
int w;//w=0；上，w=1:左上，w=2：左
int v;//v是该位置的累计积分
};
int trans (char a);//输入的AGCT转换为数字 
int max(int a,int b,int c);
int main(){
	string str1;
	cout << "please enter sequence1:"<<endl;
	cin >> str1;
	string str2;
	cout << "please enter sequence2:"<<endl;
	cin >> str2;
	int m = str1.size();
	int n = str2.size();
	int m1,m2,m3,mmax;
	int gap = -5;//空位罚分 
	int s[5][5];//打分矩阵
	s[0][0]=5;s[1][0]=0;s[1][1]=5;
	s[2][0]=0;s[2][1]=-4;s[2][2]=5; 
	s[3][0]=-4;s[3][1]=0;s[3][2]=0;s[3][3]=5;
	struct seq score[10][10];
	score[0][0].v=0;//总共积分 
	score[0][0].w=0;//方向0：上，1:左上，2：左 
	for(int i=0;i<=m;i++){
		for(int j=0;j<=n;j++){
			//第一行 
			if(i==0&&j!=0){
				score[i][j].v=score[i][j-1].v+ gap;
				score[i][j].w=2;
			}
			//第一列 
			if(j==0&&i!=0){
				score[i][j].v=score[i-1][j].v+gap;
				score[i][j].w=0;
			}
		}
	}
	//动态规划计算分值 
	for(int i=1;i<=m;i++){
		for(int j=1;j<=n;j++){
			int x=trans(str1[i-1]);
			int y=trans(str2[j-1]);
			m1 = score[i-1][j].v + gap;
			score[i][j].w=0;
			m2 = score[i-1][j-1].v +s[x][y];
			score[i][j].w=1;
			m3 = score[i][j-1].v + gap;
			score[i][j-1].w=2;
			mmax = max(m1,m2,m3);
			score[i][j].v= mmax;		
		}
	}
	//打印得分矩阵 
	cout <<"score matrix:"<<endl;
	for(int i=0;i<=m;i++){
		for(int j=0;j<=n;j++){
			cout << score[i][j].v <<" ";
			if(j==n){
				cout<<endl;
			}
		}
    }
    cout<<"max score:"<<score[m][n].v<<endl;
    string str3,str4;
    int a=m;
    int b=n;
    int c=0;
    //向上回溯一格 
    if(score[a][b].w==0) {
    	str3[c]=str1[a-1];
    	str4[c]=' ';
    	
	}
	//向左上回溯一格
	if(score[a][b].w==1) {
		str3[c]=str1[a-1];
		str4[c]=str2[b-1];
	}
	//向左回溯一格 
	if(score[a][b].w==2) {
		str3[c]=' ';
		str4[c]=str2[b-1];
	}
cout << "marked trace：" << endl;
for(int i=0;i<=m;i++){
	for(int j=0;j<=n;j++){
		cout << score[i][j].w<< " ";
		if(j==n){
			cout << endl;
		    }
        }
    }
    return 0; 
}

int trans(char a){
	int b;
	if (a =='A')
    b = 0;
    if(a == 'G')
    b = 1;
    if (a =='C')
    b = 2;
    if (a == 'T')
    b = 3;
    return b;
}

int max(int a,int b,int c){
	int f = a > b ? a : b;
	return f > c ? f : c;
}
