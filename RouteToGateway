import java.util.*;

public class RouteToGateway {

    static final int INF = Integer.MAX_VALUE / 4;

    static class DijkstraResult {
        int[] dist;
        List<Integer>[] parents;

        DijkstraResult(int n) {
            dist = new int[n];
            parents = new ArrayList[n];
            for (int i = 0; i < n; i++) {
                parents[i] = new ArrayList<>();
            }
        }
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        int n = Integer.parseInt(sc.nextLine().trim());

        int[][] graph = new int[n][n];
        int[][] transpose = new int[n][n];

        for (int i = 0; i < n; i++) {
            String[] parts = sc.nextLine().trim().split("\\s+");
            for (int j = 0; j < n; j++) {
                int w = Integer.parseInt(parts[j]);
                if (w == -1) {
                    graph[i][j] = INF;
                } else {
                    graph[i][j] = w;
                }
                transpose[j][i] = graph[i][j];
            }
        }

        String[] gatewayParts = sc.nextLine().trim().split("\\s+");
        Set<Integer> gateways = new LinkedHashSet<>();
        for (String g : gatewayParts) {
            gateways.add(Integer.parseInt(g) - 1);
        }

        int sa = Integer.parseInt(sc.nextLine().trim()) - 1;

        DijkstraResult fromSA = dijkstra(graph, sa);

        DijkstraResult toSA = dijkstra(transpose, sa);

        for (int src = 0; src < n; src++) {

            if (gateways.contains(src)) continue;

            System.out.println("Forwarding Table for " + (src + 1));
            System.out.println("To Cost Next Hop");

            for (int g : gateways) {

                if (src == sa) {
                    if (fromSA.dist[g] >= INF) {
                        System.out.println((g + 1) + " -1 -1");
                    } else {
                        List<Integer> hops = firstHops(fromSA, sa, g);
                        printEntry(g, fromSA.dist[g], hops);
                    }
                    continue;
                }

                if (toSA.dist[src] >= INF || fromSA.dist[g] >= INF) {
                    System.out.println((g + 1) + " -1 -1");
                    continue;
                }

                int totalCost = toSA.dist[src] + fromSA.dist[g];

                List<Integer> hops = firstHopsToSA(toSA, src, sa);

                if (hops.isEmpty()) {
                    System.out.println((g + 1) + " -1 -1");
                } else {
                    printEntry(g, totalCost, hops);
                }
            }

            System.out.println();
        }

        sc.close();
    }

    static DijkstraResult dijkstra(int[][] graph, int src) {

        int n = graph.length;
        DijkstraResult res = new DijkstraResult(n);

        Arrays.fill(res.dist, INF);
        res.dist[src] = 0;

        PriorityQueue<int[]> pq =
                new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));

        pq.add(new int[]{src, 0});

        while (!pq.isEmpty()) {

            int[] cur = pq.poll();
            int u = cur[0];
            int d = cur[1];

            if (d > res.dist[u]) continue;

            for (int v = 0; v < n; v++) {

                if (graph[u][v] >= INF) continue;

                int newDist = res.dist[u] + graph[u][v];

                if (newDist < res.dist[v]) {
                    res.dist[v] = newDist;
                    res.parents[v].clear();
                    res.parents[v].add(u);
                    pq.add(new int[]{v, newDist});
                } else if (newDist == res.dist[v]) {
                    res.parents[v].add(u);
                }
            }
        }

        return res;
    }

    static List<Integer> firstHops(DijkstraResult res, int src, int dest) {
        List<Integer> result = new ArrayList<>();
        findFirstHops(res, src, dest, result, new HashSet<>());
        Collections.sort(result);
        return result;
    }

    static List<Integer> firstHopsToSA(DijkstraResult res, int src, int dest) {
        List<Integer> result = new ArrayList<>();
        for (int parent : res.parents[src]) {
            result.add(parent);
        }
        Collections.sort(result);
        return result;
    }

    static void findFirstHops(DijkstraResult res, int src, int current,
                              List<Integer> result, Set<Integer> visited) {

        if (visited.contains(current)) return;
        visited.add(current);

        for (int parent : res.parents[current]) {
            if (parent == src) {
                result.add(current);
            } else {
                findFirstHops(res, src, parent, result, visited);
            }
        }
    }

    static void printEntry(int gateway, int cost, List<Integer> hops) {

        if (hops.isEmpty()) {
            System.out.println((gateway + 1) + " -1 -1");
            return;
        }

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < hops.size(); i++) {
            if (i > 0) sb.append(",");
            sb.append(hops.get(i) + 1);
        }

        System.out.println((gateway + 1) + " " + cost + " " + sb);
    }
}
