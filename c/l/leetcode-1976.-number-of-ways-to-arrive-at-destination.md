# Leetcode 1976. Number of Ways to Arrive at Destination

[Explanation Video Link on Youtube](https://youtu.be/PAbOk-p1aJ4)

```cpp
class Solution {
public:
    int countPaths(int n, vector<vector<int>>& roads) {
        vector<vector<pair<int,long>>> graph(n);
        
        for(auto r : roads){
            graph[r[0]].push_back({r[1],r[2]});
            graph[r[1]].push_back({r[0],r[2]});
        }
        
        vector<long> ways(n,0);
        vector<long> dist(n,LONG_MAX);
        priority_queue<pair<long,int>, vector<pair<long, int>>, greater<pair<long, int>>> pq;
        dist[0] = 0; ways[0] = 1;
        pq.emplace(0,0);
        long MOD = 1e9+7;

        while(!pq.empty()){
            auto [cur_cost, cur] = pq.top(); pq.pop();
            
            for(auto [nbr, cost] : graph[cur]){
                const auto new_cost = cur_cost + cost;
                if(new_cost < dist[nbr]){
                    ways[nbr] = ways[cur];
                    dist[nbr] = new_cost;
                    pq.emplace(dist[nbr], nbr);
                }else if(new_cost == dist[nbr]){
                    ways[nbr] = (ways[nbr] + ways[cur]) % MOD;
                }
            }
        }
        return ways[n-1];
    }
};
```
