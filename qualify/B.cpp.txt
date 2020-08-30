#include <bits/stdc++.h>


using namespace std;

int T;
int N;

string C;
int a, b;

int main()
{
	ios::sync_with_stdio(false);
	cin>>T;
	for(int t = 1; t <= T; ++t)
	{
		cin>>N;
		cin>>C;
		a = b = 0;
		for(int i = 0; i < N; ++i)
		{
			if(C[i] == 'A')
				++a;
			else
				++b;
		}
		cout<<"Case #"<<t<<": ";
		if(abs(a-b) == 1)
			cout<<"Y"<<endl;
		else
			cout<<"N"<<endl;
		
	}
	
	
}
