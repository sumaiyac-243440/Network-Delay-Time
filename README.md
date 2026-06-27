class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {

        vector<pair<int,int>> adj[n + 1];

        for (auto &e : times) {
            adj[e[0]].push_back({e[1], e[2]});
        }

        vector<int> dist(n + 1, INT_MAX);

        priority_queue<pair<int,int>,
                       vector<pair<int,int>>,
                       greater<pair<int,int>>> pq;

        dist[k] = 0;
        pq.push({0, k});

        while (!pq.empty()) {

            int d = pq.top().first;
            int u = pq.top().second;
            pq.pop();

            if (d > dist[u]) continue;

            for (auto &it : adj[u]) {

                int v = it.first;
                int w = it.second;

                if (dist[v] > dist[u] + w) {

                    dist[v] = dist[u] + w;
                    pq.push({dist[v], v});
                }
            }
        }

        int ans = 0;

        for (int i = 1; i <= n; i++) {

            if (dist[i] == INT_MAX)
                return -1;

            ans = max(ans, dist[i]);
        }

        return ans;
    }
};
