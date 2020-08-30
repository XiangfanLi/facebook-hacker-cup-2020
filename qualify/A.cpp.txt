#include <bits/stdc++.h>


using namespace std;
#define MAXN 60
int T;
int N;

char I[MAXN];
char O[MAXN];

char P[MAXN][MAXN];

int main()
{
	cin>>T;
	for(int t = 1; t <= T; ++t)
	{
		cin>>N;
		scanf("%s", I+1);
		scanf("%s", O+1);
		for(int i = 1; i <= N; ++i)
		{
			P[i][N+1] = '\0';
		for(int j = 1; j <= N; ++j)
		{
			P[i][j] = 'N';
		}
		}
		for(int i = 1; i <= N; ++i)
		{
			P[i][i] = 'Y';
			for(int j = i + 1; j <= N; ++j)
			{
				if(O[j-1] == 'N')
					break;
				if(I[j] == 'N')
					break;
				P[i][j] = 'Y';
			}
			for(int j = i - 1; j >= 1; --j)
			{
				if(O[j+1] == 'N')
					break;
				if(I[j] == 'N')
					break;
				P[i][j] = 'Y';
			}
		}
		cout<<"Case #"<<t<<":"<<endl;
		for(int i = 1; i <= N; ++i)
		{
			printf("%s\n", P[i]+1);
		}
		
	}
	
	
}
