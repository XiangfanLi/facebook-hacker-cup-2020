#include <bits/stdc++.h>


using namespace std;
int T;
int N;

int H, P;
map<int, int> Map;
int main()
{
	
	cin>>T;
	for(int t = 1; t <= T; ++t)
	{
		cin>>N;
		Map = map<int, int> ();
		vector<pair<int, int> > v;
		v.reserve(N);
		for(int i = 0; i < N; ++i)
		{
			cin>>P>>H;
			v.push_back(make_pair(P, H));
		}
		sort(v.begin(), v.end());
		int ans = 0;
		for(auto& p: v)
		{
			int pos = p.first;
			int hei = p.second;
			
			if(Map.count(pos) == 0)
			{
				Map[pos+hei] = (Map.count(pos+hei) == 0) ? hei : (max(Map[pos+hei], hei));
			}
			else
			{
				Map[pos+hei] = (Map.count(pos+hei) == 0) ? (hei + Map[pos]) : (max(Map[pos+hei], hei + Map[pos]));
			}
			ans = max(ans, Map[pos+hei]);
			if(Map.count(pos - hei) == 0)
			{
				Map[pos] = (Map.count(pos) == 0) ? hei : (max(Map[pos], hei));
			}
			else
			{
				Map[pos] = (Map.count(pos) == 0) ? (hei + Map[pos-hei]) : (max(Map[pos], hei + Map[pos-hei]));
			}
			ans = max(ans, Map[pos]);
		}
		cout<<"Case #"<<t<<": "<<ans<<endl;
	}
}
