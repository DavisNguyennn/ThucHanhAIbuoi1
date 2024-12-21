#include <iostream>
#include <queue>
#include <vector>
#include <cmath>
#include <functional>

using namespace std;

// Cau truc cua 1 nut
struct Node {
    int id;
    int g; // Chi phi tu nut ban dau
    int h; // Uoc tinh chi phi den dich
    int f; // f(n) = g(n) + h(n)

    Node(int id, int g, int h) : id(id), g(g), h(h), f(g + h) {}

    // Dinh nghia ham tinh toan cho priority_queue
    bool operator>(const Node& other) const {
        return f > other.f;
    }
};

// Ham nhap gia tri g
int inputG() {
    int g;
    cout << "Nhap gia tri g: ";
    cin >> g;
    return g;
}

// Ham nhap gia tri h
int inputH() {
    int h;
    cout << "Nhap gia tri h: ";
    cin >> h;
    return h;
}

// Ham tinh toan heuristic
int calculateH(int current, int goal) {
    return abs(current - goal);
}

// Thuat toan tim kiem theo chieu rong (BFS)
void BFS(const vector<vector<bool>>& graph, int start, int goal) {
    queue<int> q;
    vector<bool> visited(graph.size(), false);

    q.push(start);
    visited[start] = true;

    while (!q.empty()) {
        int current = q.front();
        q.pop();
        cout << current << " ";

        if (current == goal) {
            cout << "\nDuong dan duoc tim thay!" << endl;
            return;
        }

        for (int i = 0; i < graph.size(); ++i) {
            if (graph[current][i] && !visited[i]) {
                q.push(i);
                visited[i] = true;
            }
        }
    }

    cout << "\nKhong tim thay duong dan!" << endl;
}

// Thuat toan A*
void AStar(const vector<vector<bool>>& graph, int start, int goal) {
    priority_queue<Node, vector<Node>, greater<Node>> pq;
    vector<bool> visited(graph.size(), false);

    int initialG = inputG();
    int initialH = inputH();

    pq.emplace(start, initialG, initialH);
    visited[start] = true;

    while (!pq.empty()) {
        Node current = pq.top();
        pq.pop();

        if (current.id == goal) {
            cout << "Duong dan tim thay voi chi phi: " << current.g << endl;
            return;
        }

        for (int i = 0; i < graph.size(); ++i) {
            if (graph[current.id][i] && !visited[i]) {
                int g = current.g + 1;
                int h = calculateH(i, goal);
                pq.emplace(i, g, h);
                visited[i] = true;
            }
        }
    }

    cout << "Khong tim thay duong dan!" << endl;
}

int main() {
    // Vi du do thi voi 5 nut
    vector<vector<bool>> graph = {
        {0, 1, 1, 0, 0},
        {1, 0, 1, 0, 1},
        {1, 1, 0, 1, 0},
        {0, 0, 1, 0, 1},
        {0, 1, 0, 1, 0}
    };

    int start = 0;
    int goal = 4;

    cout << "BFS: ";
    BFS(graph, start, goal);

    cout << "A*: "<<endl;
    AStar(graph, start, goal);

    return 0;
}
