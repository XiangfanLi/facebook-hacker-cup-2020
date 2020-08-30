#include <bits/stdc++.h>


using namespace std;
#define ll long long
#define MAXN 1000010
int T;
int N, M, K, S;
ll P[MAXN];
ll Q[MAXN];
ll AP, BP, CP, DP, AQ, BQ, CQ, DQ;


ll dist(ll pos, ll l, ll r)
{
	if(r <= pos)
	{
		return pos - l;
	}
	if(l >= pos)
	{
		return r - pos;
	}
	return min(r-pos, pos-l) + (r - l);
	
} 

bool can(ll t)
{
	int i1 = 1;
	int i2 = 1;
	for(; i1 <= N; ++i1)
	{
		int i3 = i2;
		for(; i3 <= M; ++i3)
		{
			if(dist(P[i1], Q[i2], Q[i3]) > t)
			{
				break;
			}
		}
		
		if(i3 > M)
			return true;
		/*
		if(i3 == i2)
			return false;
		*/
		i2 = i3;
	}
	return false;
}



int main()
{
	cin>>T;
	for(int t = 1; t <= T; ++t)
	{
		cin>>N>>M>>K>>S;
		for(int i = 1; i <= K; ++i)
			cin>>P[i];
		cin>>AP>>BP>>CP>>DP;
		for(int i = 1; i <= K; ++i)
			cin>>Q[i];
		cin>>AQ>>BQ>>CQ>>DQ;
		for(int i = K + 1; i <= N; ++i)
		{
			P[i] = ((AP * P[i-2] + BP * P[i-1] + CP) % DP) + 1;
		}
		for(int i = K + 1; i <= M; ++i)
		{
			Q[i] = ((AQ * Q[i-2] + BQ * Q[i-1] + CQ) % DQ) + 1;
		}
		sort(P+1, P+N+1);
		sort(Q+1, Q+M+1);
		/*
		for(int i = 1; i <= N; ++i)
		{
			cout<<"P["<<i<<"] = "<<P[i]<<endl;
		}
		* */
		ll low = 0;
		ll high = 1000000000;
		while(low < high)
		{
			ll mid = (low + high) >> 1;
			if(can(mid))
			{
				high = mid;
				//cout<<"can: "<<mid<<endl;
			}
			else
			{
				low = mid + 1;
				//cout<<"!can: "<<mid<<endl;
			}
		}
		cout<<"Case #"<<t<<": "<<low<<endl;
	}
	
}
