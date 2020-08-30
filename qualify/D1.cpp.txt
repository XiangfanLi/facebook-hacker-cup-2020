#include <bits/stdc++.h>


using namespace std;
#define MAXN 1000010
#define ll long long
int M, N, T;
ll C[MAXN];
priority_queue<pair<ll, int> > q;
ll dp[MAXN];
int main()
{
	ios::sync_with_stdio(false);
	cin>>T;
	for(int t = 1; t <= T; ++t)
	{
		cin>>N>>M;
		for(int i = 1; i <= N; ++i)
		{
			cin>>C[i];
			dp[i] = -1;
		}
		q = priority_queue<pair<ll, int> > ();
		q.push(make_pair(-0, 1));
		dp[1] = -0;
		bool ok = true;
		for(int i = 2; i <= N; ++i)
		{
			while(!q.empty() && q.top().second < i - M)
			{
				q.pop();
			}
			if(q.empty())
			{
				ok = false;
				break;
			}
			if(C[i] == 0)
				continue;
			dp[i] = -q.top().first+C[i];
			q.push(make_pair(-dp[i], i));
		}
		cout<<"Case #"<<t<<": ";
		if(!ok)
		{
			cout<<-1<<endl;
			continue;
		}
		ll ans = -1;
		for(int i = N; i >= N - M; --i)
		{
			if(dp[i] == -1)
				continue;
			if(ans == -1)
				ans = dp[i];
			else
				ans = min(ans, dp[i]);
		}
		cout<<ans<<endl;
	}
}
