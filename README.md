# Ex26 Prim's Algorithm
## AIM:
To write a Java program to implement Prim's Algorithm for finding Total Cost of tree.

## Algorithm

1. Import Scanner class and define constants INFINITY and MAX.
2. Declare adjacency matrix G, spanning matrix, and number of vertices n.
3. Read the number of vertices and adjacency matrix using Scanner.
4. Convert adjacency matrix into cost matrix.
5. Initialize distance, visited, and from arrays.
6. Repeatedly select the minimum unvisited vertex.
7. Add the edge to the spanning tree and update distances.
8. Print the spanning tree matrix and total cost.

## Program:

```java
/*
Program to implement Prim's Algorithm

*/
import java.util.*;

public class PrimsAlgorithm {
    static final int INFINITY = 9999;
    static final int MAX = 20;

    static int[][] G = new int[MAX][MAX];
    static int[][] spanning = new int[MAX][MAX];
    static int n;

    static int prims() {
        int[][] cost = new int[MAX][MAX];
        int[] distance = new int[MAX];
        int[] from = new int[MAX];
        boolean[] visited = new boolean[MAX];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                cost[i][j] = (G[i][j] == 0) ? INFINITY : G[i][j];
                spanning[i][j] = 0;
            }
        }

        distance[0] = 0;
        visited[0] = true;

        for (int i = 1; i < n; i++) {
            distance[i] = cost[0][i];
            from[i] = 0;
        }

        int minCost = 0;
        int edges = n - 1;

        while (edges > 0) {
            int minDistance = INFINITY;
            int v = 0;

            for (int i = 1; i < n; i++) {
                if (!visited[i] && distance[i] < minDistance) {
                    minDistance = distance[i];
                    v = i;
                }
            }

            int u = from[v];
            spanning[u][v] = distance[v];
            spanning[v][u] = distance[v];

            visited[v] = true;
            edges--;

            for (int i = 1; i < n; i++) {
                if (!visited[i] && cost[i][v] < distance[i]) {
                    distance[i] = cost[i][v];
                    from[i] = v;
                }
            }

            minCost += cost[u][v];
        }

        return minCost;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        n = sc.nextInt();

        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                G[i][j] = sc.nextInt();

        int totalCost = prims();

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++)
                System.out.print(spanning[i][j] + " ");
            System.out.println();
        }

        System.out.println("Total cost of spanning tree = " + totalCost);
    }
}
```
## Output:

![image](https://github.com/user-attachments/assets/29cb3c21-0913-4917-9156-bb943c7ccb31)

## Result:
Thus, the Java program to implement Prim's Algorithm for finding Total Cost of tree is implemented successfully.

# Ex27 Kruskal's Algorithm
## AIM:
To write a Java program to implement Kruskal's Algorithm for finding minimum cost.

## Algorithm

1. Read graph data using Scanner.
2. Replace 0 values with 999.
3. Find minimum edge repeatedly.
4. Use Disjoint Set operations find() and union().
5. Add valid edges into MST.
6. Display minimum cost.

## Program:

```java
/*
Program to implement Kruskal's Algorithm

*/
import java.util.*;

public class KruskalAlgorithm {

    static int[] parent = new int[10];

    static int find(int i) {
        while (parent[i] != 0)
            i = parent[i];
        return i;
    }

    static boolean union(int i, int j) {
        if (i != j) {
            parent[j] = i;
            return true;
        }
        return false;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int[][] cost = new int[10][10];

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                cost[i][j] = sc.nextInt();
                if (cost[i][j] == 0)
                    cost[i][j] = 999;
            }
        }

        int ne = 1, mincost = 0;

        while (ne < n) {
            int min = 999;
            int a = 0, b = 0, u = 0, v = 0;

            for (int i = 1; i <= n; i++)
                for (int j = 1; j <= n; j++)
                    if (cost[i][j] < min) {
                        min = cost[i][j];
                        a = u = i;
                        b = v = j;
                    }

            u = find(u);
            v = find(v);

            if (union(u, v)) {
                System.out.println(ne++ + " edge (" + a + "," + b + ") = " + min);
                mincost += min;
            }

            cost[a][b] = cost[b][a] = 999;
        }

        System.out.println("Minimum cost = " + mincost);
    }
}
```
## Output:

![image](https://github.com/user-attachments/assets/12b2ed8b-bc94-495d-b5d3-22eabb5c0e1f)

## Result:
Thus, the Java program to implement Kruskal's Algorithm for finding minimum cost is implemented successfully.

# Ex28 Dijkstra’s Algorithm

## AIM:
To write a Java Program to implement Dijkstra's Algorithm to find the shortest path.

## Algorithm

1. Define constants `INFINITY = 9999` and `MAX = 10`.
2. Read the number of nodes and adjacency matrix using `Scanner`.
3. Replace all `0` values with `INFINITY` except diagonal elements.
4. Read the starting node.
5. Initialize `distance[]`, `pred[]`, and `visited[]` arrays.
6. Mark the start node as visited and set its distance to 0.
7. Repeatedly select the nearest unvisited node.
8. Update distances of adjacent vertices if a shorter path is found.
9. Display the shortest distance and path from the source node to every other node.

## Program:

```java
/*
Program to implement Dijkstra's Algorithm

*/

import java.util.Scanner;

public class DijkstraAlgorithm {

    static final int INFINITY = 9999;
    static final int MAX = 10;

    static void dijkstra(int[][] G, int n, int startNode) {

        int[][] cost = new int[MAX][MAX];
        int[] distance = new int[MAX];
        int[] pred = new int[MAX];
        boolean[] visited = new boolean[MAX];

        int count, minDistance, nextNode = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (G[i][j] == 0)
                    cost[i][j] = INFINITY;
                else
                    cost[i][j] = G[i][j];
            }
        }

        for (int i = 0; i < n; i++) {
            distance[i] = cost[startNode][i];
            pred[i] = startNode;
            visited[i] = false;
        }

        distance[startNode] = 0;
        visited[startNode] = true;
        count = 1;

        while (count < n - 1) {

            minDistance = INFINITY;

            for (int i = 0; i < n; i++) {
                if (!visited[i] && distance[i] < minDistance) {
                    minDistance = distance[i];
                    nextNode = i;
                }
            }

            visited[nextNode] = true;

            for (int i = 0; i < n; i++) {
                if (!visited[i]) {
                    if (minDistance + cost[nextNode][i] < distance[i]) {
                        distance[i] = minDistance + cost[nextNode][i];
                        pred[i] = nextNode;
                    }
                }
            }

            count++;
        }

        for (int i = 0; i < n; i++) {
            if (i != startNode) {

                System.out.println("Distance of node " + i + " = " + distance[i]);
                System.out.print("Path = " + i);

                int j = i;

                do {
                    j = pred[j];
                    System.out.print(" <- " + j);
                } while (j != startNode);

                System.out.println();
            }
        }
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        int[][] G = new int[MAX][MAX];

        int n = sc.nextInt();

        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                G[i][j] = sc.nextInt();

        int startNode = sc.nextInt();

        dijkstra(G, n, startNode);

        sc.close();
    }
}
```
## Output:

***![image](https://github.com/user-attachments/assets/121d86c2-a203-4174-aa9d-5389b62714ef)

## Result:
Thus, the Java Program to implement Dijkstra's Algorithm to find the shortest path is implemented successfully.

---

# Ex29 Travelling Salesman Problem

## AIM:
To write a Java Program to implement Travelling Salesman Problem for finding shortest path.

## Algorithm

1. Read the number of cities and cost matrix.
2. Initialize all cities as unvisited.
3. Start from city 0 and mark it visited.
4. Find the nearest unvisited city.
5. Move to the nearest city and add the cost.
6. Repeat until all cities are visited.
7. Return to the starting city.
8. Display the minimum cost.

## Program:

```java
/*
Program to implement Travelling Salesman Problem

*/

import java.util.Scanner;

public class TravellingSalesman {

    static int[][] a = new int[10][10];
    static int[] visited = new int[10];
    static int n;
    static int cost = 0;

    static void mincost(int city) {

        visited[city] = 1;
        System.out.print((city + 1) + " --> ");

        int ncity = least(city);

        if (ncity == 999) {
            ncity = 0;
            System.out.print(ncity + 1);
            cost += a[city][ncity];
            return;
        }

        mincost(ncity);
    }

    static int least(int c) {

        int nc = 999;
        int min = 999;
        int kmin = 0;

        for (int i = 0; i < n; i++) {

            if (a[c][i] != 0 && visited[i] == 0) {

                if (a[c][i] < min) {

                    min = a[i][0] + a[c][i];
                    kmin = a[c][i];
                    nc = i;
                }
            }
        }

        if (min != 999)
            cost += kmin;

        return nc;
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        n = sc.nextInt();

        for (int i = 0; i < n; i++) {

            for (int j = 0; j < n; j++) {
                a[i][j] = sc.nextInt();
            }

            visited[i] = 0;
        }

        mincost(0);

        System.out.println("\n\nMinimum cost : " + cost);

        sc.close();
    }
}
```
## Output:

![image](https://github.com/user-attachments/assets/cba89abe-c91d-4dd9-9101-0b6508c93555)

## Result:
Thus, the Java Program to implement Travelling Salesman Problem for finding shortest path is implemented successfully.

---

# Ex30 Finding Total Cost of Spanning Tree

## DATE:

## AIM:
To write a Java Program to implement Prim's Algorithm for finding Total Cost of spanning tree.

## Algorithm

1. Read the number of vertices and adjacency matrix.
2. Replace all 0 values with infinity.
3. Initialize visited, distance, and from arrays.
4. Select the minimum cost edge repeatedly.
5. Add selected edge to the spanning tree.
6. Update minimum distances.
7. Repeat until all vertices are included.
8. Display the spanning tree matrix.
9. Display the total cost of spanning tree.

## Program:

```java
/*
Program to implement Prim's Algorithm for finding Total Cost of spanning tree

*/

import java.util.Scanner;

public class TotalCostSpanningTree {

    static final int INFINITY = 9999;
    static final int MAX = 20;

    static int[][] G = new int[MAX][MAX];
    static int[][] spanning = new int[MAX][MAX];
    static int n;

    static int prims() {

        int[][] cost = new int[MAX][MAX];
        int[] distance = new int[MAX];
        int[] from = new int[MAX];
        boolean[] visited = new boolean[MAX];

        for (int i = 0; i < n; i++) {

            for (int j = 0; j < n; j++) {

                if (G[i][j] == 0)
                    cost[i][j] = INFINITY;
                else
                    cost[i][j] = G[i][j];

                spanning[i][j] = 0;
            }
        }

        distance[0] = 0;
        visited[0] = true;

        for (int i = 1; i < n; i++) {

            distance[i] = cost[0][i];
            from[i] = 0;
            visited[i] = false;
        }

        int minCost = 0;
        int edges = n - 1;

        while (edges > 0) {

            int minDistance = INFINITY;
            int v = 0;

            for (int i = 1; i < n; i++) {

                if (!visited[i] && distance[i] < minDistance) {

                    minDistance = distance[i];
                    v = i;
                }
            }

            int u = from[v];

            spanning[u][v] = distance[v];
            spanning[v][u] = distance[v];

            visited[v] = true;
            edges--;

            for (int i = 1; i < n; i++) {

                if (!visited[i] && cost[i][v] < distance[i]) {

                    distance[i] = cost[i][v];
                    from[i] = v;
                }
            }

            minCost += cost[u][v];
        }

        return minCost;
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        n = sc.nextInt();

        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                G[i][j] = sc.nextInt();

        int totalCost = prims();

        for (int i = 0; i < n; i++) {

            for (int j = 0; j < n; j++)
                System.out.print(spanning[i][j] + " ");

            System.out.println();
        }

        System.out.println("Total cost of spanning tree = " + totalCost);

        sc.close();
    }
}
```
## Output:

![image](https://github.com/user-attachments/assets/551b82e4-0ef7-413c-91cc-c3d55c6e454d)

## Result:
Thus, the Java Program to implement Prim's Algorithm for finding Total Cost of spanning tree is implemented successfully.
