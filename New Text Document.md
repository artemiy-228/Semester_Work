## Condition Задача 1

```
Реализуйте АТД Очередь на односвязном списке (forward_list).
```

### Implementation

```
template <typename T>
class Queue {
private:
    std::forward_list<T> q;
    typename std::forward_list<T>::iterator it;

public:
    Queue() : it(q.before_begin()) {}

    int size() const {
        return std::distance(q.begin(), q.end());
    }

    bool isEmpty() const {
        return size() == 0;
    }

    void push(const T& element) {
        q.insert_after(it, element);
        ++it;
    }

    T front() const {
        return q.front();
    }

    void pop() {
        q.pop_front();
    }
};
```

### Testing

```
int main() {
    Queue<int> q;
    for(int i = 0; i < 10; i++) {
        q.push(i + 1);
    }
    for(int i = 0; i < 5; i++) {
        cout << q.front() << ' ';
        q.pop();
    }
    for(int i = 0; i < 5; i++) {
        q.push(i + 1);
    }
    for(int i = 0; i < 10; i++) {
        cout << q.front() << ' ';
        q.pop();
    }
}
```

### Outputs
```
Output: 1 2 3 4 5 6 7 8 9 10 1 2 3 4 5
Expected output: 1 2 3 4 5 6 7 8 9 10 1 2 3 4 5
```

## Condition Задача 2

```
2. Используя класс stack из STL решите следующую задачу.
Напишите функцию с использованием стека для проверки корректности XML-строки. XML-строка называется корректной, если она может быть получена по следующим правилам:
Пустая строка является корректной XML-строкой.
Если A и B — корректные XML-строки, то строка AB, получающаяся приписыванием строки B в конец строки A, также является корректной XML-строкой.
Если A — корректная XML-строка, то строка <X>A</X>, получающаяся приписыванием в начало A открывающегося тега, а в конец — закрывающегося с таким же именем, также является корректной XML-строкой. Здесь X — любая непустая строка из строчных букв латинского алфавита.
Например, представленные ниже строки:
<a></a>
<a><ab></ab><c></c></a>
<a></a><a></a><a></a>
являются корректными XML-строками, а такие строки как:
<a></b>
<a><b>
<a><b></a></b>
не являются корректными XML-строками.
```

### Implementation

```
bool isValidXml(const string& xml) {
    stack<string> tags;

    size_t pos = 0;
    while (pos < xml.size()) {
        if (xml[pos] == '<') {
            size_t end_pos = xml.find('>', pos);
            if (end_pos == string::npos)
                return false;
            string tag = xml.substr(pos + 1, end_pos - pos - 1);
            if (tag[0] == '/') {
                if (tags.empty() || tags.top() != tag.substr(1))
                    return false;
                tags.pop();
            }
            else
                tags.push(tag);
            pos = end_pos + 1;
        }
        else
            pos++;
    }
    return tags.empty();
}
```

### Testing

```
int main() {
    vector<string> vec = { "<a></a>", "<a><ab></ab><c></c></a>", "<a></a><a></a><a></a>", "<a></b>", "<a><b>", "<a><b></a></b>"};
    for(auto &s : vec){
        cout << s << ' ' << isValidXml(s) << '\n';
    }
}
```

### Outputs
```
Output: <a></a> 1
<a><ab></ab><c></c></a> 1
<a></a><a></a><a></a> 1
<a></b> 0
<a><b> 0
<a><b></a></b> 0

Expected output: <a></a> 1
<a><ab></ab><c></c></a> 1
<a></a><a></a><a></a> 1
<a></b> 0
<a><b> 0
<a><b></a></b> 0
```

## Condition Задача 5

```
Напишите функцию для симметричного (in-order) обхода бинарного дерева, заданного следующей структурой:
struct node { int value; node *left, *right; };
К каждому значению, хранящемуся в дереве, функция применяет функцию, указанную в качестве аргумента:
void inorder(node *n, void (*f)(int));
```

### Implementation

```
//Пример обхода
void inorder(Node *n, void (*f)(int)) {
    if (n == nullptr) {
        return;
    }
    inorder(n->left, f);
    f(n->value);
    inorder(n->right, f);
}

```

## Condition Задача 7

```
Используя дерево отрезков решите следующую задачу.
(110) Есть массив, содержащий 100000 элементов, первоначально все элементы равны 0. В массиве производятся изменения элементов и требуется находить суммы части массива с i
-го по j
-ый элементы.
В первой строке содержится число K
 (1≤K≤50000
) -- количество запросов. Далее следует K
 строк, в каждой строке содержится либо команда "S i
 v
", где 1≤i≤100000
, −100≤v≤100
, заменяющая значение i
-го элемента массива на v
, либо команда "Q i
 j
", где 1≤i≤j≤100000
, требующая вывести сумму части массива с i
-го по j
-ый элементы.
Для каждой команды 'Q' вывести на отдельной строке результат запроса

```
P.S I hadn't created build function because we have start condition with full 0 tree. But build function is
similar with update function and it isn't hard to create
### Implementation

```
class SegmentTree {
private:
    vector<long long> segmentTree;
    int size;

    long long sum(int vertex, int i, int j, int l, int r) {
        if (i > r || j < l) {
            return 0;
        }
        if (i >= l && j <= r) {
            return segmentTree[vertex];
        }
        int mid = (j + i) / 2;
        return sum(2 * vertex + 1, i, mid, l, r) + sum(2 * vertex + 2, mid + 1, j, l, r);
    }

    void update(int vertex, int l, int r, int position, int value) {
        if (l == r){
            segmentTree[vertex] = value;
        }
        else{
            int mid = (r + l) / 2;
            if(position <= mid){
                update(2 * vertex + 1, l, mid, position, value);
            }
            else{
                update(2 * vertex + 2, mid + 1, r, position, value);
            }
            segmentTree[vertex] = segmentTree[2 * vertex + 1] + segmentTree[2 * vertex + 2];
        }
    }

public:
    SegmentTree(int n) {
        size = n;
        segmentTree.resize(4 * n, 0);
    }

    void update(int position, int value) {
        update(0, 1, size, position, value);
    }

    long long sum(int l, int r) {
        return sum(0, 1, size, l, r);
    }
};
```

### Testing

```
int main() {
    int K;
    cin >> K;

    SegmentTree segmentTree(100000);

    for (int k = 0; k < K; k++) {
        char command;
        cin >> command;

        if (command == 'S') {
            int i, v;
            cin >> i >> v;
            segmentTree.update(i, v);
        } else if (command == 'Q') {
            int i, j;
            cin >> i >> j;
            cout << segmentTree.sum(i, j) << '\n';
        }
    }
}
```

### Outputs
```
Input: 
5
S 1 100
S 5 10
Q 5 10
S 5 -1
Q 1 100000

Output: 
10
99

Expected output:
10
99
```

## Condition Задача 8

```
Используя map из STL напишите решение следующей задачи с эффективностью O(NlogN)
.
Имеется список, первоначально пустой. Команда add v
 добавляет v
 в конец списка. Команда remove v
 удаляет первое вхождение v
 из списка. После каждой команды вывести "1", если в списке нет одинаковых элементов, или "2", если есть одинаковые.
```

### Implementation

```
#include <iostream>
#include <map>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int n;
    cin >> n;

    map<int, int> freq;
    vector<int> arr;

    for (int i = 0; i < n; i++) {
        string cmd;
        int v;
        cin >> cmd >> v;

        if (cmd == "add") {
            arr.push_back(v);
            freq[v]++;
        }
        else if (cmd == "remove") {
            auto it = find(arr.begin(), arr.end(), v);
            if (it != arr.end() && freq[v] > 0) {
                arr.erase(it);
                freq[v]--;
            }
        }

        bool hasDuplicates = false;
        for (const auto& pair : freq) {
            if (pair.second > 1) {
                hasDuplicates = true;
                break;
            }
        }

        cout << (hasDuplicates ? 2 : 1) << '\n';
    }
}

```

### Outputs
```
5
add 1
1
add 1
2
remove 1
1
add 1
2
add 2
2
```

## Condition Задача 14

```
14. Используя поиск в ширину, решите задачу.
(1343) На доске 8x8 некоторые клетки произвольным образом покрашены в черный цвет (кроме верхнего левого и правого нижнего угла доски). Требуется определить имеется ли путь для шахматного коня из верхнего левого в правый нижний угол доски, не проходящий по черным клеткам, и минимальное количество ходов, требующееся для этого.
```

### Implementation

```
int dx[8] = {-1, 1, 2, 2, 1, -1, -2, -2};
int dy[8] = {-2, -2, -1, 1, 2, 2, 1, -1};

struct Cell{
    int x, y;
    Cell(int x, int y) : x(x), y(y) {}
};

vector<string> chessDesk(8, "n");
vector<vector<int>> path(8, vector<int>(8, INT_MAX));
vector<vector<bool>> visited(8, vector<bool>(8, false));


void bfs(Cell startPoint){
    queue<Cell> q;
    q.push(startPoint);
    visited[0][0] = true;
    path[0][0] = 0;

    while (!q.empty()) {
        int x = q.front().x;
        int y = q.front().y;
        q.pop();

        for (int i = 0; i < 8; ++i) {
            int nx = x + dx[i];
            int ny = y + dy[i];

            if (nx >= 0 && nx < 8 && ny >= 0 && ny < 8 && chessDesk[nx][ny] == '.' && !visited[nx][ny]) {
                visited[nx][ny] = true;
                path[nx][ny] = path[x][y] + 1;
                q.emplace(nx, ny);
            }
        }
    }
}
```

### Testing

```
int main() {
    for(int i = 0; i < 8; i++)
        cin >> chessDesk[i];
    Cell startPoint(0, 0);
    bfs(startPoint);
    if(path[7][7] == INT_MAX){
        cout << "NO";
    }
    else
        cout << "YES " << path[7][7];
}
```

### Outputs
```
Input: 
......##
.....##.
....##..
...##...
..##....
.##.....
##......
#.......

Output: 
YES 6

Expected output:
YES 6

```

## Condition Задача 20

```
Сравните время сортировки с помощью sort, stable_sort, make_heap/sort_heap для вектора из 10^6
 случайных чисел. Результаты оформить в виде таблицы.
```

### Implementation

```
using namespace std;
using namespace std::chrono;

int main() {
    vector<int> vec(1000000);
    srand(time(0));
    for (int i = 0; i < 1000000; ++i) {
        vec[i] = rand();
    }

    vector<int> vec_copy = vec;

    // Измерение времени для sort
    auto start = high_resolution_clock::now();
    sort(vec.begin(), vec.end());
    auto end = high_resolution_clock::now();
    long long  duration_1 = duration_cast<milliseconds>(end - start).count();

    // Измерение времени для stable_sort
    vec = vec_copy;
    start = high_resolution_clock::now();
    stable_sort(vec.begin(), vec.end());
    end = high_resolution_clock::now();
    long long  duration_2 = duration_cast<milliseconds>(end - start).count();

    // Измерение времени для make_heap/sort_heap
    vec = vec_copy;
    start = high_resolution_clock::now();
    make_heap(vec.begin(), vec.end());
    sort_heap(vec.begin(), vec.end());
    end = high_resolution_clock::now();
    long long duration_3 = duration_cast<milliseconds>(end - start).count();

    // Вывод результатов
    cout << duration_1 << " ms" << '\n';
    cout << duration_2<< " ms" << '\n';
    cout << duration_3 << " ms" << '\n';
}
```

### Outputs Таблица результата.
```
|        |  sort  | stable_sort  | make_heap/sort_heap |
| test_1 | 360 ms |    445 ms    |       871 ms        |      
| test_2 | 355 ms |    451 ms    |       833 ms        |     
| test_3 | 359 ms |    428 ms    |       836 ms        |     
| test_4 | 358 ms |    429 ms    |       793 ms        |     
| test_5 | 357 ms |    438 ms    |       780 ms        |     

```

## Condition Задача 25

```
25. Напишите функцию разложения числа на простые множители.
```

### Implementation

```
vector<int> splitNumberByPrimeDivisors(int number) {
    vector<int> primeDivisors;
    int tempN = number;
    for(int p = 2; p * p <= tempN; p++){
        if(tempN % p == 0){
            while(tempN % p == 0){
                primeDivisors.push_back(p);
                tempN /= p;
            }
        }
    }
    if(tempN > 1)
        primeDivisors.push_back(tempN);
    return primeDivisors;
}
```

### Testing

```
int main() {
    vector<int> primeDiv = splitNumberByPrimeDivisors(560);
    for(auto &n : primeDiv){
        cout << n << ' ';
    }
}
```

### Outputs
```
Output: 2 2 2 2 5 7

Expected output: 2 2 2 2 5 7
```