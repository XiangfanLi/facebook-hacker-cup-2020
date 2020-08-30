#include <bits/stdc++.h>


using namespace std;

#define MAXN 1000010
#define ll long long
int T;
int N, K;
char S[MAXN];
int E[MAXN];
vector<int> sons[MAXN];
vector<int> neigh[MAXN];
ll A, B, C;
int cnt[MAXN];
int color[MAXN];
int nums[MAXN];
int big1, big2;
int uninfec, infec;
bool aleady;
ll ans;


int dfs1(int u, int p, int c)
{
	int ret = 1;
	color[u] = c;
	for(int v: neigh[u])
	{
		if(v == p)
			continue;
		if(S[v] == '*')
			ret += dfs1(v, u, c);
	}
	return ret;
}


// already the maximum
pair<int, int> dfs2(int u)
{
	pair<int, int> ret {1, 0};
	if(S[u] == '*')
		ret.second = 1;
	for(int v: sons[u])
	{
		pair<int, int> tmp = dfs2(v);
		ret.first += tmp.first;
		ret.second += tmp.second;
		if(S[u] == '#' || S[v] == '#')
		{
			ans += (ll)(tmp.first) * (ll)(N - tmp.first);
		}
		else
		{
			
			ans += (ll)(tmp.second) * (ll)(uninfec - tmp.second);
		}
	}
	return ret;
}

pair<int, int> dfs3(int u)
{
	pair<int, int> ret{0, 0};
	if(u != 1 && S[E[u]] == '#' && S[u] == '*' && nums[color[u]] == big1)
	{
		ret.first = 1;
	}
	else if(u != 1 && S[E[u]] == '#' && S[u] == '*' && nums[color[u]] == big2)
	{
		ret.second = 1;
	}
	for(int v: sons[u])
	{
		pair<int, int> tmp = dfs3(v);
		ret.first += tmp.first;
		ret.second += tmp.second;
		if(S[u] == '*' && S[v] == '*')
			continue;
		if(big2 == -1 || cnt[big1] > 1)
		{
			if(tmp.first > 0 && tmp.first < cnt[big1])
			{
				ans += (ll)(tmp.first) * (ll)big1 * (ll)(cnt[big1]-tmp.first) * (ll)big1;
			}
		}
		else
		{
			if(tmp.first > 0 && tmp.second < cnt[big2])
			{
				ans += (ll)(tmp.first) * (ll)big1 * (ll)(cnt[big2]-tmp.second) * (ll)big2;
			}
			if(tmp.second > 0 && tmp.first < cnt[big1])
			{
				
				ans += (ll)(tmp.second) * (ll)big2 * (ll)(cnt[big1]-tmp.first) * (ll)big1;
			}
		}
	}
	//cout<<"df3 "<<u<<" :"<<ret.first<<" "<<ret.second<<endl;
	return ret;
}


int main()
{
	cin>>T;
	for(int t = 1; t <= T; ++t)
	{
		cin>>N>>K;
		scanf("%s", S+1);
		infec = uninfec = 0;
		for(int i = 1; i <= N; ++i)
		{
			if(S[i] == '#')
				++infec;
			else
				++uninfec;
		}
		for(int i = 2; i <= K + 1; ++i)
		{
			cin>>E[i];
		}
		cin>>A>>B>>C;
		for(int i = K + 2; i <= N; ++i)
		{
			E[i] = ((A * E[i-2] + B * E[i-1] + C) % (i-1)) + 1;
		}
		for(int i = 1; i <= N; ++i)
		{
			sons[i].clear();
			neigh[i].clear();
		}
		for(int i = 2; i <= N; ++i)
		{
			sons[E[i]].push_back(i);
			neigh[i].push_back(E[i]);
			neigh[E[i]].push_back(i);
		}
		for(int i = 1; i <= N; ++i)
		{
			color[i] = i;
			cnt[i] = 0;
		}
		for(int i = 1; i <= N; ++i)
		{
			if(S[i] == '*' && color[i] == i)
			{
				int ret = dfs1(i, 0, i);
				cnt[ret]++;
				nums[i] = ret;
			}
		}
		big1 = -1;
		big2 = -1;
		for(int i = N; i >= 1; --i)
		{
			if(cnt[i] > 0)
			{
				if(big1 == -1)
					big1 = i;
				else if(big2 == -1)
				{
					big2 = i;
					break;
				}
			}
		}
		ans = 0;
		if((big1 != -1 && big2 == -1 && cnt[big1] == 1) || big1 == -1)
		{
			//cout<<"dfs2"<<endl;
			dfs2(1);
		}
		else
		{
			//cout<<"dfs3"<<endl;
			dfs3(1);
		}
		ll ans1 = 0;
		for(int i = 1; i <= N; ++i)
		{
			if(cnt[i] > 0)
				ans1 += (ll)(cnt[i]) * (ll)(i) * (ll)(i-1) / 2;
		}
		if((big1 != -1 && big2 == -1 && cnt[big1] == 1) || big1 == -1)
		{
			
		}
		else
		{
			if(cnt[big1] > 1)
				ans1 += (ll)big1 * (ll)big1;
			else
				ans1 += (ll)big1 * (ll)big2;
		}
		cout<<"Case #"<<t<<": "<<ans1<<" "<<ans<<endl;
	}
	
	
}
