# Python 与 C++ 对照代码库
> 配套《1-计算思维训练计划》第七部分、《2-学习材料》《3-教师手册》。
> 每个任务给出**同一算法的 Python 版和 C++ 版**，并附实际运行输出。
> **验证环境**：Python 3.13；C++ 用 Apple clang 21（`g++ -std=c++17 -Wall -Wextra`），零错误零警告。所有代码在上述环境编译并运行通过。
---

## 使用说明
**运行方式**

| 语言 | 运行命令 |
|---|---|
| Python | `python3 文件名.py`（Windows 用 `python 文件名.py`） |
| C++ | `g++ -std=c++17 -Wall -Wextra -o 程序名 文件名.cpp`，然后运行 `./程序名`（Windows 为 `程序名.exe`） |

**注意事项**
1. 学生用的集成环境（如 Dev-C++）默认的 C++ 标准可能较低，需在“编译选项”里加上 `-std=c++17`。本库使用了结构化绑定（C++17）。
2. Windows 控制台显示中文可能乱码，可先执行 `chcp 65001`，或让学生程序输出英文。
3. 带输入的程序（`g1_input`）在 IDE 中直接输入数据；命令行可用 `echo "55 60 72 48 90 -1" | ./程序名`。
4. 标有“数值因机器而异”的程序，教师上课前请自己运行一遍。

**目录**

| 年级 | 课时 | 任务 |
|---|---|---|
| 高一 | L7 / L16 | 成绩统计：计数、最值、平均 |
| 高一 | L7 | 从键盘读入一串成绩 |
| 高一 | L10 | 十进制与二进制互转 |
| 高一 | L13 | 队列与堆栈（认识用法） |
| 高一 | L14 / L15 | 顺序查找、二分查找、三数排序 |
| 高一 | L16 | 班级成绩小助手 + 测试用例表 |
| 高二 | L5 / L11 | 找重复 → 函数 → 通用函数；异常处理 |
| 高二 | L9 | 数据结构选择：排行榜与按学号查询 |
| 高二 | L7 | 正则表达式 |
| 高二 | L12 | 抛体仿真 |
| 高二 | L13 | 算法实测：求最大值 vs 先排序再取最后一个 |
| 高二 | L11 | 下标越界与除以零：两种语言的“出错方式” |
| 高二 | L14 / L15 | 测试用例 + 审查有缺陷的“AI 代码” |
| 高二 | L16 | 图书漂流：状态转移 + 边界测试 |
| 高三 | L10 | 食堂排队仿真 |
| 高三 | L13 | 随机算法：蒙特卡洛估计 π |
| 高三 | L11 | 用类表示图书（封装） |
| 高三 | L6 / L7 | 手写 K 近邻 + 留出法评估 |
| 高三 | L8 | 规则里的偏见：按邮编筛选 vs 按岗位条件筛选 |

---

# 高一

## 高一 L7 / L16　成绩统计：计数、最值、平均

对应问卷第 5 题。C++ 的 `int` 相除会取整，求平均要有一个操作数是 `double`。

**Python 版**（`g1_stats.py`）

```python
# 高一 L7 / L16：成绩统计（计数、最值、平均）
scores = [55, 60, 72, 48, 90]

count = 0
for s in scores:
    if s >= 60:
        count += 1
print("及格人数:", count)

if not scores:                      # 空数据要单独处理
    print("没有成绩")
else:
    mx = mn = scores[0]             # 初始值取第一个成绩，不要取 0
    for s in scores:
        if s > mx:
            mx = s
        if s < mn:
            mn = s
    print("最高分", mx, "最低分", mn, "平均", sum(scores) / len(scores))
```

**C++ 版**（`g1_stats.cpp`）

```cpp
// 高一 L7 / L16：成绩统计（计数、最值、平均）
#include <iostream>
#include <vector>
#include <numeric>
using namespace std;

int main() {
    vector<int> scores = {55, 60, 72, 48, 90};

    int count = 0;
    for (int s : scores) {
        if (s >= 60) count++;
    }
    cout << "及格人数: " << count << endl;

    if (scores.empty()) {                   // 空数据要单独处理
        cout << "没有成绩" << endl;
        return 0;
    }
    int mx = scores[0], mn = scores[0];     // 初始值取第一个成绩，不要取 0
    for (int s : scores) {
        if (s > mx) mx = s;
        if (s < mn) mn = s;
    }
    double avg = accumulate(scores.begin(), scores.end(), 0.0) / scores.size();
    cout << "最高分 " << mx << " 最低分 " << mn << " 平均 " << avg << endl;
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
及格人数: 3
最高分 90 最低分 48 平均 65.0
```

C++：

```text
及格人数: 3
最高分 90 最低分 48 平均 65
```

## 高一 L7　从键盘读入一串成绩

输入 `55 60 72 48 90 -1`（-1 表示结束）。Python 用 `input().split()`；C++ 用 `cin >> x`。

**Python 版**（`g1_input.py`）

```python
# stdin: 55 60 72 48 90 -1
# 高一 L7：从键盘读入成绩，遇到 -1 结束
scores = []
for token in input().split():
    x = int(token)
    if x == -1:
        break
    scores.append(x)

print("共输入", len(scores), "个成绩")
if scores:
    passed = sum(1 for s in scores if s >= 60)
    print("及格", passed, "人，平均", round(sum(scores) / len(scores), 1))
```

**C++ 版**（`g1_input.cpp`）

```cpp
// stdin: 55 60 72 48 90 -1
// 高一 L7：从键盘读入成绩，遇到 -1 结束
#include <iostream>
#include <vector>
#include <numeric>
using namespace std;

int main() {
    vector<int> scores;
    int x;
    while (cin >> x && x != -1) {           // 读取失败（如输入了字母）也会结束循环
        scores.push_back(x);
    }
    cout << "共输入 " << scores.size() << " 个成绩" << endl;
    if (!scores.empty()) {
        int passed = 0;
        for (int s : scores) if (s >= 60) passed++;
        double avg = accumulate(scores.begin(), scores.end(), 0.0) / scores.size();
        cout << "及格 " << passed << " 人，平均 " << avg << endl;
    }
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
共输入 5 个成绩
及格 3 人，平均 65.0
```

C++：

```text
共输入 5 个成绩
及格 3 人，平均 65
```

## 高一 L10　十进制与二进制互转

对应问卷第 8 题。Python 可用内置 `bin()` / `int(s, 2)` 核对结果。

**Python 版**（`g1_binary.py`）

```python
# 高一 L10：十进制与二进制互转
def to_binary(n):
    if n == 0:
        return "0"
    r = ""
    while n > 0:
        r = str(n % 2) + r          # 余数放在最左边
        n //= 2                     # 整除
    return r

def to_decimal(b):
    v = 0
    for c in b:
        v = v * 2 + int(c)
    return v

print(to_binary(13), to_decimal("10110"))     # 1101 22
print(bin(13), int("10110", 2))               # Python 内置函数，用来核对自己的结果
```

**C++ 版**（`g1_binary.cpp`）

```cpp
// 高一 L10：十进制与二进制互转
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

string toBinary(int n) {
    if (n == 0) return "0";
    string r;
    while (n > 0) {
        r += char('0' + n % 2);     // 余数转成字符 '0' 或 '1'
        n /= 2;                     // 两个 int 相除，结果取整
    }
    reverse(r.begin(), r.end());    // 余数是从低位到高位得到的，要翻转
    return r;
}

int toDecimal(const string& b) {
    int v = 0;
    for (char c : b) v = v * 2 + (c - '0');
    return v;
}

int main() {
    cout << toBinary(13) << " " << toDecimal("10110") << endl;   // 1101 22
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
1101 22
0b1101 22
```

C++：

```text
1101 22
```

## 高一 L13　队列与堆栈（认识用法）

只要求为场景选型并会用，不要求自己实现。注意 C++ 的 `pop()` 只删除、不返回元素。

**Python 版**（`g1_queue_stack.py`）

```python
# 高一 L13：队列与堆栈（认识用法即可）
from collections import deque

q = deque()                       # 队列：先进先出
q.append("甲"); q.append("乙"); q.append("丙")
print("队列先出:", q.popleft())   # 甲

st = []                           # 堆栈：用列表实现，后进先出
st.append("页面1"); st.append("页面2"); st.append("页面3")
print("堆栈先出:", st.pop())      # 页面3
```

**C++ 版**（`g1_queue_stack.cpp`）

```cpp
// 高一 L13：队列与堆栈（认识用法即可）
#include <iostream>
#include <queue>
#include <stack>
#include <string>
using namespace std;

int main() {
    queue<string> q;                        // 队列：先进先出
    q.push("甲"); q.push("乙"); q.push("丙");
    cout << "队列先出: " << q.front() << endl;   // 甲
    q.pop();                                // C++ 的 pop 只删除，不返回元素

    stack<string> st;                       // 堆栈：后进先出
    st.push("页面1"); st.push("页面2"); st.push("页面3");
    cout << "堆栈先出: " << st.top() << endl;    // 页面3
    st.pop();
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
队列先出: 甲
堆栈先出: 页面3
```

C++：

```text
队列先出: 甲
堆栈先出: 页面3
```

## 高一 L14 / L15　顺序查找、二分查找、三数排序

对应问卷第 11、12 题。C++ 用引用参数让函数改变外部变量；Python 用多值返回。

**Python 版**（`g1_search_sort.py`）

```python
# 高一 L14 / L15：顺序查找、二分查找、三数排序
def linear_search(a, target):
    steps = 0
    for i, x in enumerate(a):
        steps += 1
        if x == target:
            return i, steps
    return -1, steps

def binary_search(a, target):            # 前提：a 已经从小到大排好
    lo, hi, steps = 0, len(a) - 1, 0
    while lo <= hi:
        mid = (lo + hi) // 2
        steps += 1
        if a[mid] == target:
            return mid, steps
        if a[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1, steps

def sort3(a, b, c):                      # 从大到小
    if a < b: a, b = b, a                # Python 可以直接交换
    if a < c: a, c = c, a
    if b < c: b, c = c, b
    return a, b, c

d = [3, 8, 12, 17, 25, 31, 46, 52, 68]
print("顺序查找 46:", linear_search(d, 46))     # (位置, 比较次数)
print("二分查找 46:", binary_search(d, 46))
print(sort3(3, 9, 5))                           # (9, 5, 3)
```

**C++ 版**（`g1_search_sort.cpp`）

```cpp
// 高一 L14 / L15：顺序查找、二分查找、三数排序
#include <iostream>
#include <vector>
using namespace std;

int linearSearch(const vector<int>& a, int target, int& steps) {
    steps = 0;
    for (int i = 0; i < (int)a.size(); i++) {
        steps++;
        if (a[i] == target) return i;
    }
    return -1;
}

int binarySearch(const vector<int>& a, int target, int& steps) {   // 前提：a 已从小到大排好
    int lo = 0, hi = (int)a.size() - 1;
    steps = 0;
    while (lo <= hi) {
        int mid = (lo + hi) / 2;
        steps++;
        if (a[mid] == target) return mid;
        if (a[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}

void sort3(int& a, int& b, int& c) {      // 引用参数：函数里交换，外面的变量跟着变
    if (a < b) swap(a, b);
    if (a < c) swap(a, c);
    if (b < c) swap(b, c);
}

int main() {
    vector<int> d = {3, 8, 12, 17, 25, 31, 46, 52, 68};
    int s1, s2;
    int i1 = linearSearch(d, 46, s1);
    int i2 = binarySearch(d, 46, s2);
    cout << "顺序查找 46: 位置" << i1 << " 比较" << s1 << "次" << endl;
    cout << "二分查找 46: 位置" << i2 << " 比较" << s2 << "次" << endl;
    int a = 3, b = 9, c = 5;
    sort3(a, b, c);
    cout << a << " " << b << " " << c << endl;       // 9 5 3
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
顺序查找 46: (6, 7)
二分查找 46: (6, 2)
(9, 5, 3)
```

C++：

```text
顺序查找 46: 位置6 比较7次
二分查找 46: 位置6 比较2次
9 5 3
```

## 高一 L16　班级成绩小助手 + 测试用例表

6 个必测用例：正常、全及格、全不及格、只有一个、恰好 60 分、空数据。C++ 比较小数用 `fabs(a-b) < 1e-9`，不要用 `==`。

**Python 版**（`g1_summarize.py`）

```python
# 高一 L16：班级成绩小助手 + 测试用例表
def summarize(scores):
    if not scores:
        return None                       # 约定：没有成绩返回 None
    passed = sum(1 for s in scores if s >= 60)
    return passed, max(scores), sum(scores) / len(scores)

cases = [
    ("正常",     [55, 60, 72, 48, 90], (3, 90, 65.0)),
    ("全及格",   [60, 70],             (2, 70, 65.0)),
    ("全不及格", [10, 20],             (0, 20, 15.0)),
    ("只有一个", [80],                 (1, 80, 80.0)),
    ("恰好60分", [60],                 (1, 60, 60.0)),
    ("空数据",   [],                   None),
]
for name, data, expected in cases:
    got = summarize(data)
    print("通过" if got == expected else "失败", name, got)
```

**C++ 版**（`g1_summarize.cpp`）

```cpp
// 高一 L16：班级成绩小助手 + 测试用例表
#include <iostream>
#include <vector>
#include <string>
#include <optional>
#include <numeric>
#include <algorithm>
#include <cmath>
using namespace std;

struct Summary { int passed; int maxScore; double avg; };

optional<Summary> summarize(const vector<int>& scores) {
    if (scores.empty()) return nullopt;                 // 约定：没有成绩 -> 空
    int passed = 0;
    for (int s : scores) if (s >= 60) passed++;
    int mx = *max_element(scores.begin(), scores.end());
    double avg = accumulate(scores.begin(), scores.end(), 0.0) / scores.size();
    return Summary{passed, mx, avg};
}

struct Case { string name; vector<int> data; bool expectEmpty; Summary expect; };

int main() {
    vector<Case> cases = {
        {"正常",     {55, 60, 72, 48, 90}, false, {3, 90, 65.0}},
        {"全及格",   {60, 70},             false, {2, 70, 65.0}},
        {"全不及格", {10, 20},             false, {0, 20, 15.0}},
        {"只有一个", {80},                 false, {1, 80, 80.0}},
        {"恰好60分", {60},                 false, {1, 60, 60.0}},
        {"空数据",   {},                   true,  {0, 0, 0.0}},
    };
    for (const auto& c : cases) {
        auto got = summarize(c.data);
        bool ok = c.expectEmpty ? !got.has_value()
                                : got && got->passed == c.expect.passed &&
                                  got->maxScore == c.expect.maxScore &&
                                  fabs(got->avg - c.expect.avg) < 1e-9;   // 小数不能直接用 ==
        cout << (ok ? "通过 " : "失败 ") << c.name << endl;
    }
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
通过 正常 (3, 90, 65.0)
通过 全及格 (2, 70, 65.0)
通过 全不及格 (0, 20, 15.0)
通过 只有一个 (1, 80, 80.0)
通过 恰好60分 (1, 60, 60.0)
通过 空数据 None
```

C++：

```text
通过 正常
通过 全及格
通过 全不及格
通过 只有一个
通过 恰好60分
通过 空数据
```

# 高二

## 高二 L5 / L11　找重复 → 函数 → 通用函数；异常处理

重构的核心：不同的部分变成参数，相同的部分写进函数体。

**Python 版**（`g2_refactor.py`）

```python
# 高二 L5 / L11：找重复 -> 函数 -> 通用函数
def average(scores):
    if not scores:
        raise ValueError("空列表无法求平均")
    return sum(scores) / len(scores)

def grade_point(s):
    if s >= 90: return 4.0
    if s >= 80: return 3.0
    if s >= 70: return 2.0
    if s >= 60: return 1.0
    return 0.0

def calculate_gpa(scores):
    return sum(grade_point(s) for s in scores) / len(scores)

subjects = {"数学": [88, 92, 75, 60], "英语": [70, 85, 90, 66], "物理": [55, 78, 82, 91]}
for name, scores in subjects.items():
    print(name + "平均分", average(scores))
print("GPA", calculate_gpa([95, 82, 71]))
try:
    average([])
except ValueError as e:
    print("异常:", e)
```

**C++ 版**（`g2_refactor.cpp`）

```cpp
// 高二 L5 / L11：找重复 -> 函数 -> 通用函数
#include <iostream>
#include <vector>
#include <map>
#include <string>
#include <numeric>
#include <stdexcept>
using namespace std;

double average(const vector<double>& v) {            // 参数 v（只读引用），返回 double
    if (v.empty()) throw invalid_argument("空列表无法求平均");
    return accumulate(v.begin(), v.end(), 0.0) / v.size();
}

double gradePoint(double s) {
    if (s >= 90) return 4.0;
    if (s >= 80) return 3.0;
    if (s >= 70) return 2.0;
    if (s >= 60) return 1.0;
    return 0.0;
}

double calculateGPA(const vector<double>& scores) {
    double sum = 0;
    for (double s : scores) sum += gradePoint(s);
    return sum / scores.size();
}

int main() {
    map<string, vector<double>> subjects = {
        {"数学", {88, 92, 75, 60}}, {"英语", {70, 85, 90, 66}}, {"物理", {55, 78, 82, 91}}};
    for (const auto& [name, v] : subjects)            // C++17 结构化绑定
        cout << name << "平均分 " << average(v) << endl;
    cout << "GPA " << calculateGPA({95, 82, 71}) << endl;
    try { average({}); }
    catch (const exception& e) { cout << "异常: " << e.what() << endl; }
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
数学平均分 78.75
英语平均分 77.75
物理平均分 76.5
GPA 3.0
异常: 空列表无法求平均
```

C++：

```text
数学平均分 78.75
物理平均分 76.5
英语平均分 77.75
GPA 3
异常: 空列表无法求平均
```

## 高二 L9　数据结构选择：排行榜与按学号查询

Python：列表套字典 + 字典；C++：`vector<struct>` + `map`。

**Python 版**（`g2_ranking.py`）

```python
# 高二 L9：数据结构选择 —— 列表套字典做排行榜，字典做按学号查询
cls = [
    {"id": "01", "name": "小明", "score": 72},
    {"id": "02", "name": "小红", "score": 95},
    {"id": "03", "name": "小刚", "score": 88},
    {"id": "04", "name": "小丽", "score": 60},
]

def top_n(students, n):
    return sorted(students, key=lambda s: s["score"], reverse=True)[:n]   # 返回新列表，原列表不变

for s in top_n(cls, 3):
    print(s["name"], s["score"])

by_id = {s["id"]: s for s in cls}          # 按学号快速查
print(by_id["03"]["name"])
```

**C++ 版**（`g2_ranking.cpp`）

```cpp
// 高二 L9：数据结构选择 —— vector<struct> 做排行榜，map 做按学号查询
#include <iostream>
#include <vector>
#include <map>
#include <string>
#include <algorithm>
using namespace std;

struct Student { string id; string name; int score; };

vector<Student> topN(vector<Student> v, size_t n) {          // 按值传入，不改原数据
    sort(v.begin(), v.end(),
         [](const Student& a, const Student& b) { return a.score > b.score; });
    if (v.size() > n) v.resize(n);
    return v;
}

int main() {
    vector<Student> cls = {{"01", "小明", 72}, {"02", "小红", 95}, {"03", "小刚", 88}, {"04", "小丽", 60}};
    for (const auto& s : topN(cls, 3)) cout << s.name << " " << s.score << endl;

    map<string, Student> byId;                                // 按学号快速查
    for (const auto& s : cls) byId[s.id] = s;
    cout << byId["03"].name << endl;
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
小红 95
小刚 88
小明 72
小刚
```

C++：

```text
小红 95
小刚 88
小明 72
小刚
```

## 高二 L7　正则表达式

C++11 起标准库自带 `<regex>`；用原始字符串 `R"(...)"` 可避免写双反斜杠。

**Python 版**（`g2_regex.py`）

```python
# 高二 L7：正则表达式
import re
text = "电话 13812345678，邮箱 zhang3@school.cn，日期 2026-11-15"
print("手机号:", re.findall(r"1[3-9]\d{9}", text))
print("邮箱:  ", re.findall(r"[A-Za-z0-9_.]+@[A-Za-z0-9.]+\.[A-Za-z]+", text))
print("日期:  ", re.findall(r"\d{4}-\d{2}-\d{2}", text))
print(bool(re.fullmatch(r"25\d{6}", "25123456")), bool(re.fullmatch(r"25\d{6}", "26123456")))   # True False
```

**C++ 版**（`g2_regex.cpp`）

```cpp
// 高二 L7：正则表达式（C++11 起标准库自带 <regex>）
#include <iostream>
#include <regex>
#include <string>
using namespace std;

void findAll(const string& text, const string& pattern) {
    regex re(pattern);
    for (sregex_iterator it(text.begin(), text.end(), re), end; it != end; ++it)
        cout << "  " << it->str() << endl;
}

int main() {
    string text = "电话 13812345678，邮箱 zhang3@school.cn，日期 2026-11-15";
    cout << "手机号:" << endl; findAll(text, R"(1[3-9]\d{9})");        // R"(...)" 原始字符串：不用写双反斜杠
    cout << "邮箱:" << endl;   findAll(text, R"([A-Za-z0-9_.]+@[A-Za-z0-9.]+\.[A-Za-z]+)");
    cout << "日期:" << endl;   findAll(text, R"(\d{4}-\d{2}-\d{2})");
    regex xh(R"(25\d{6})");
    cout << regex_match("25123456", xh) << regex_match("26123456", xh) << endl;   // 10
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
手机号: ['13812345678']
邮箱:   ['zhang3@school.cn']
日期:   ['2026-11-15']
True False
```

C++：

```text
手机号:
  13812345678
邮箱:
  zhang3@school.cn
日期:
  2026-11-15
10
```

## 高二 L12　抛体仿真

`dt` 越小越接近解析解 `v²·sin2θ/g`（10 m/s、45° 约 10.2 m）。

**Python 版**（`g2_projectile.py`）

```python
# 高二 L12：抛体仿真（保留：初速度、角度、出手高度、重力；忽略：空气阻力）
import math

def trajectory(v0, angle_deg, h0=0.0, g=9.8, dt=0.01):
    a = math.radians(angle_deg)
    x, y = 0.0, h0
    vx, vy = v0 * math.cos(a), v0 * math.sin(a)
    pts = []
    while y >= 0:
        pts.append((x, y))
        x += vx * dt
        vy -= g * dt
        y += vy * dt
    return pts

for ang in range(15, 76, 15):
    print(ang, "度 射程约", round(trajectory(10, ang)[-1][0], 2), "米")
print("出手高2米 45度:", round(trajectory(10, 45, 2)[-1][0], 2), "30度:", round(trajectory(10, 30, 2)[-1][0], 2))
```

**C++ 版**（`g2_projectile.cpp`）

```cpp
// 高二 L12：抛体仿真（保留：初速度、角度、出手高度、重力；忽略：空气阻力）
#include <iostream>
#include <cmath>
#include <utility>
#include <vector>
using namespace std;

vector<pair<double, double>> trajectory(double v0, double angleDeg, double h0 = 0, double g = 9.8, double dt = 0.01) {
    const double PI = acos(-1.0);
    double a = angleDeg * PI / 180, x = 0, y = h0, vx = v0 * cos(a), vy = v0 * sin(a);
    vector<pair<double, double>> pts;
    while (y >= 0) {
        pts.push_back({x, y});
        x += vx * dt;
        vy -= g * dt;
        y += vy * dt;
    }
    return pts;
}

int main() {
    for (int ang = 15; ang <= 75; ang += 15)
        cout << ang << " 度 射程约 " << trajectory(10, ang).back().first << " 米" << endl;
    cout << "出手高2米 45度: " << trajectory(10, 45, 2).back().first
         << " 30度: " << trajectory(10, 30, 2).back().first << endl;
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
15 度 射程约 4.93 米
30 度 射程约 8.75 米
45 度 射程约 10.11 米
60 度 射程约 8.75 米
75 度 射程约 5.07 米
出手高2米 45度: 11.81 30度: 11.34
```

C++：

```text
15 度 射程约 4.92622 米
30 度 射程约 8.74686 米
45 度 射程约 10.1116 米
60 度 射程约 8.75 米
75 度 射程约 5.07285 米
出手高2米 45度: 11.8087 30度: 11.3449
```

## 高二 L13　算法实测：求最大值 vs 先排序再取最后一个

**数值因机器而异，下面不给固定输出。** C++ 必须在报告里写明编译选项，见本文件末尾的实测对照。

**Python 版**（`g2_timing.py`）

```python
# 高二 L13：算法实测（不要求 Big-O，用测量数据下结论）
import random, time

def max_loop(a):
    m = a[0]
    for x in a:
        if x > m:
            m = x
    return m

def max_sort(a):
    return sorted(a)[-1]

def run(f, a, repeat=3):
    best = float("inf")
    for _ in range(repeat):                  # 重复测量取最小值，减少偶然干扰
        t = time.perf_counter(); f(a)
        best = min(best, time.perf_counter() - t)
    return best

for n in (100, 10_000, 1_000_000):
    data = [random.randint(0, 10**9) for _ in range(n)]
    print(n, "循环", round(run(max_loop, data), 6), "s  排序", round(run(max_sort, data), 6), "s")
```

**C++ 版**（`g2_timing.cpp`）

```cpp
// 高二 L13：算法实测（不要求 Big-O，用测量数据下结论）
// 注意：C++ 编译优化会大幅改变结果，必须在报告中写明编译命令（如 -O0 或 -O2）
#include <iostream>
#include <vector>
#include <random>
#include <chrono>
#include <algorithm>
using namespace std;
using namespace std::chrono;

int maxLoop(const vector<int>& a) { int m = a[0]; for (int x : a) if (x > m) m = x; return m; }
int maxSort(vector<int> a)        { sort(a.begin(), a.end()); return a.back(); }   // 按值传入，复制了一份

template <class F> double timeIt(F f, int repeat = 3) {
    double best = 1e9;
    for (int i = 0; i < repeat; i++) {
        auto t0 = steady_clock::now();
        volatile int r = f(); (void)r;                 // volatile：防止编译器把整段计算优化掉
        best = min(best, duration<double>(steady_clock::now() - t0).count());
    }
    return best;
}

int main() {
    mt19937 rng(1);
    for (int n : {100, 10000, 1000000}) {
        vector<int> d(n);
        for (int& x : d) x = rng() % 1000000000;
        cout << n << " 循环 " << timeIt([&] { return maxLoop(d); })
             << " s  排序 " << timeIt([&] { return maxSort(d); }) << " s" << endl;
    }
    return 0;
}
```

## 高二 L14 / L15　测试用例 + 审查有缺陷的“AI 代码”

`find_max_bad` 把初值设为 0，全负数输入会出错。先写测试，再判断对错。

**Python 版**（`g2_tests.py`）

```python
# 高二 L14 / L15：测试用例 + 审查"AI 生成的代码"
def find_max_bad(nums):                # 待审查：AI 生成
    best = 0
    for n in nums:
        if n > best:
            best = n
    return best

def find_max_good(nums):               # 修正版
    if not nums:
        raise ValueError("空列表")
    best = nums[0]
    for n in nums:
        if n > best:
            best = n
    return best

cases = [("正常", [3, 9, 2], 9), ("单个", [5], 5), ("全负数", [-3, -1, -7], -1),
         ("重复最大", [4, 9, 9], 9), ("正负混合", [-2, 0, 2], 2)]

def check(title, f):
    print("==", title)
    ok = 0
    for name, arg, expected in cases:
        got = f(arg)
        ok += got == expected
        print("通过" if got == expected else "失败", name, "期望", expected, "实际", got)
    print(f"{ok}/{len(cases)} 通过")

check("待审查版本", find_max_bad)
check("修正版本", find_max_good)
```

**C++ 版**（`g2_tests.cpp`）

```cpp
// 高二 L14 / L15：测试用例 + 审查"AI 生成的代码"
#include <iostream>
#include <vector>
#include <functional>
#include <string>
#include <stdexcept>
using namespace std;

int findMaxBad(const vector<int>& a) {            // 待审查：AI 生成
    int best = 0;
    for (int x : a) if (x > best) best = x;
    return best;
}
int findMaxGood(const vector<int>& a) {           // 修正版
    if (a.empty()) throw invalid_argument("空列表");
    int best = a[0];
    for (int x : a) if (x > best) best = x;
    return best;
}

struct Case { string name; vector<int> in; int expected; };

void check(const string& title, function<int(const vector<int>&)> f, const vector<Case>& cases) {
    cout << "== " << title << endl;
    int ok = 0;
    for (const auto& c : cases) {
        int got = f(c.in);
        ok += (got == c.expected);
        cout << (got == c.expected ? "通过 " : "失败 ") << c.name
             << " 期望" << c.expected << " 实际" << got << endl;
    }
    cout << ok << "/" << cases.size() << " 通过" << endl;
}

int main() {
    vector<Case> cases = {{"正常", {3, 9, 2}, 9}, {"单个", {5}, 5}, {"全负数", {-3, -1, -7}, -1},
                          {"重复最大", {4, 9, 9}, 9}, {"正负混合", {-2, 0, 2}, 2}};
    check("待审查版本", findMaxBad, cases);
    check("修正版本", findMaxGood, cases);
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
== 待审查版本
通过 正常 期望 9 实际 9
通过 单个 期望 5 实际 5
失败 全负数 期望 -1 实际 0
通过 重复最大 期望 9 实际 9
通过 正负混合 期望 2 实际 2
4/5 通过
== 修正版本
通过 正常 期望 9 实际 9
通过 单个 期望 5 实际 5
通过 全负数 期望 -1 实际 -1
通过 重复最大 期望 9 实际 9
通过 正负混合 期望 2 实际 2
5/5 通过
```

C++：

```text
== 待审查版本
通过 正常 期望9 实际9
通过 单个 期望5 实际5
失败 全负数 期望-1 实际0
通过 重复最大 期望9 实际9
通过 正负混合 期望2 实际2
4/5 通过
== 修正版本
通过 正常 期望9 实际9
通过 单个 期望5 实际5
通过 全负数 期望-1 实际-1
通过 重复最大 期望9 实际9
通过 正负混合 期望2 实际2
5/5 通过
```

## 高二 L16　图书漂流：状态转移 + 边界测试

两个版本的日期表示不同：Python 用 `datetime`；C++ 用“第几天”的整数，避免引入日期库。

**Python 版**（`g2_library.py`）

```python
# 高二 L16：图书漂流（字典 + 状态转移）
from datetime import date, timedelta

books = {
    "B001": {"title": "三体",     "status": "在架", "borrower": None, "due": None},
    "B002": {"title": "时间简史", "status": "在架", "borrower": None, "due": None},
}

def update_status(books, book_id, action, who=None, today=None):
    today = today or date.today()
    if book_id not in books:
        return "错误：没有这本书"
    book = books[book_id]
    if action == "borrow":
        if book["status"] != "在架":
            return "错误：这本书已借出"
        book.update(status="已借出", borrower=who, due=today + timedelta(days=30))
        return f"借阅成功，应还日期 {book['due']}"
    if action == "return":
        if book["status"] != "已借出":
            return "错误：这本书没有被借出"
        book.update(status="在架", borrower=None, due=None)
        return "归还成功"
    return "错误：未知操作"

def overdue(books, today):
    return [bid for bid, b in books.items() if b["due"] and b["due"] < today]

d0 = date(2026, 11, 15)
print(update_status(books, "B001", "borrow", "小明", d0))
print(update_status(books, "B001", "borrow", "小红", d0))
print(update_status(books, "B999", "borrow", "小红", d0))
print(update_status(books, "B002", "return"))
print("逾期:", overdue(books, d0 + timedelta(days=31)))
print(update_status(books, "B001", "return"))
```

**C++ 版**（`g2_library.cpp`）

```cpp
// 高二 L16：图书漂流（struct + map + 状态转移）。日期用"第几天"的整数代替，避免引入日期库。
#include <iostream>
#include <map>
#include <string>
#include <vector>
using namespace std;

struct Book {
    string title;
    bool borrowed = false;
    string borrower;
    int due = 0;
};
using Library = map<string, Book>;

string updateStatus(Library& lib, const string& id, const string& action,
                    const string& who = "", int today = 0) {
    auto it = lib.find(id);
    if (it == lib.end()) return "错误：没有这本书";
    Book& b = it->second;
    if (action == "borrow") {
        if (b.borrowed) return "错误：这本书已借出";
        b.borrowed = true; b.borrower = who; b.due = today + 30;
        return "借阅成功，应还日：第" + to_string(b.due) + "天";
    }
    if (action == "return") {
        if (!b.borrowed) return "错误：这本书没有被借出";
        b.borrowed = false; b.borrower.clear(); b.due = 0;
        return "归还成功";
    }
    return "错误：未知操作";
}

vector<string> overdue(const Library& lib, int today) {
    vector<string> r;
    for (const auto& [id, b] : lib) if (b.borrowed && b.due < today) r.push_back(id);
    return r;
}

int main() {
    Library lib;
    lib["B001"].title = "三体";
    lib["B002"].title = "时间简史";
    cout << updateStatus(lib, "B001", "borrow", "小明", 1) << endl;
    cout << updateStatus(lib, "B001", "borrow", "小红", 1) << endl;    // 重复借阅
    cout << updateStatus(lib, "B999", "borrow", "小红", 1) << endl;    // 不存在
    cout << updateStatus(lib, "B002", "return") << endl;               // 归还未借出
    cout << "逾期: ";
    for (const auto& id : overdue(lib, 32)) cout << id << " ";
    cout << endl;
    cout << updateStatus(lib, "B001", "return") << endl;
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
借阅成功，应还日期 2026-12-15
错误：这本书已借出
错误：没有这本书
错误：这本书没有被借出
逾期: ['B001']
归还成功
```

C++：

```text
借阅成功，应还日：第31天
错误：这本书已借出
错误：没有这本书
错误：这本书没有被借出
逾期: B001 
归还成功
```

# 高三

## 高三 L10　食堂排队仿真

**两种语言的随机数发生器不同，数值不同，规律一致**（窗口越多等待越短，且收益递减）。

**Python 版**（`g3_sim.py`）

```python
# 高三 L10：食堂排队仿真
import random

def simulate(windows, students=200, seed=1):
    random.seed(seed)
    free_at = [0] * windows            # 每个窗口什么时候空闲（秒）
    t = 0
    total_wait = 0
    for _ in range(students):
        t += random.randint(0, 6)                    # 下一个学生到达
        k = free_at.index(min(free_at))              # 去最早空闲的窗口
        start = max(t, free_at[k])
        total_wait += start - t
        free_at[k] = start + random.randint(10, 30)  # 服务时间
    return total_wait / students

for w in range(4, 11):
    print(w, "个窗口，平均等待", round(simulate(w), 1), "秒")
```

**C++ 版**（`g3_sim.cpp`）

```cpp
// 高三 L10：食堂排队仿真
#include <iostream>
#include <vector>
#include <random>
#include <algorithm>
using namespace std;

double simulate(int windows, int students = 200, unsigned seed = 1) {
    mt19937 rng(seed);
    uniform_int_distribution<int> gap(0, 6), service(10, 30);
    vector<int> freeAt(windows, 0);
    int t = 0;
    long long totalWait = 0;
    for (int i = 0; i < students; i++) {
        t += gap(rng);
        int k = min_element(freeAt.begin(), freeAt.end()) - freeAt.begin();   // 最早空闲的窗口
        int start = max(t, freeAt[k]);
        totalWait += start - t;
        freeAt[k] = start + service(rng);
    }
    return double(totalWait) / students;
}

int main() {
    for (int w = 4; w <= 10; w++)
        cout << w << " 个窗口，平均等待 " << simulate(w) << " 秒" << endl;
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
4 个窗口，平均等待 193.0 秒
5 个窗口，平均等待 93.2 秒
6 个窗口，平均等待 27.7 秒
7 个窗口，平均等待 2.3 秒
8 个窗口，平均等待 0.4 秒
9 个窗口，平均等待 0.1 秒
10 个窗口，平均等待 0.0 秒
```

C++：

```text
4 个窗口，平均等待 222.48 秒
5 个窗口，平均等待 119.255 秒
6 个窗口，平均等待 50.475 秒
7 个窗口，平均等待 6.025 秒
8 个窗口，平均等待 1.22 秒
9 个窗口，平均等待 0.52 秒
10 个窗口，平均等待 0.2 秒
```

## 高三 L13　随机算法：蒙特卡洛估计 π

每次运行（或换种子）结果略有不同，n 越大波动越小。

**Python 版**（`g3_pi.py`）

```python
# 高三 L13：随机算法 —— 蒙特卡洛估计 π
import random

def estimate_pi(n):
    inside = 0
    for _ in range(n):
        x, y = random.random(), random.random()
        if x * x + y * y <= 1:
            inside += 1
    return 4 * inside / n

for n in (100, 10_000, 1_000_000):
    print(n, [round(estimate_pi(n), 3) for _ in range(5)])
```

**C++ 版**（`g3_pi.cpp`）

```cpp
// 高三 L13：随机算法 —— 蒙特卡洛估计 π
#include <iostream>
#include <random>
using namespace std;

double estimatePi(int n, unsigned seed) {
    mt19937 rng(seed);
    uniform_real_distribution<double> u(0.0, 1.0);
    int inside = 0;
    for (int i = 0; i < n; i++) {
        double x = u(rng), y = u(rng);
        if (x * x + y * y <= 1) inside++;
    }
    return 4.0 * inside / n;
}

int main() {
    for (int n : {100, 10000, 1000000}) {
        cout << n << ":";
        for (unsigned s = 1; s <= 5; s++) cout << " " << estimatePi(n, s);
        cout << endl;
    }
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
100 [3.32, 2.88, 3.32, 3.32, 3.12]
10000 [3.174, 3.128, 3.149, 3.141, 3.125]
1000000 [3.14, 3.142, 3.142, 3.141, 3.139]
```

C++：

```text
100: 3.32 3.2 3.16 2.88 2.84
10000: 3.1568 3.174 3.1516 3.1396 3.1468
1000000: 3.13992 3.14311 3.14148 3.14029 3.13938
```

## 高三 L11　用类表示图书（封装）

C++ 的 `private` 由编译器强制；Python 的 `_` 前缀只是约定。

**Python 版**（`g3_oop.py`）

```python
# 高三 L11：用类表示图书（封装）
class Book:
    def __init__(self, book_id, title):
        self.book_id = book_id
        self.title = title
        self._borrower = None            # 下划线开头：约定"内部使用"，Python 不强制隐藏
        self._due = 0

    def is_available(self):
        return self._borrower is None

    def borrow(self, who, today):
        if not self.is_available():
            raise ValueError("这本书已借出")
        self._borrower, self._due = who, today + 30

    def give_back(self):
        self._borrower, self._due = None, 0

    @property
    def due(self):
        return self._due

b = Book("B001", "三体")
b.borrow("小明", 1)
print(b.title, "可借:", b.is_available(), "应还日:", b.due)
try:
    b.borrow("小红", 2)
except ValueError as e:
    print("异常:", e)
```

**C++ 版**（`g3_oop.cpp`）

```cpp
// 高三 L11：用类表示图书（封装）
#include <iostream>
#include <string>
#include <stdexcept>
using namespace std;

class Book {
public:                                              // 对外的"接口"
    Book(string id, string title) : id_(std::move(id)), title_(std::move(title)) {}
    bool isAvailable() const { return borrower_.empty(); }
    void borrow(const string& who, int today) {
        if (!isAvailable()) throw runtime_error("这本书已借出");
        borrower_ = who;
        due_ = today + 30;
    }
    void giveBack() { borrower_.clear(); due_ = 0; }
    int due() const { return due_; }
    const string& title() const { return title_; }
private:                                             // 隐藏的"实现细节"：外部无法直接修改
    string id_, title_, borrower_;
    int due_ = 0;
};

int main() {
    Book b("B001", "三体");
    b.borrow("小明", 1);
    cout << b.title() << " 可借:" << b.isAvailable() << " 应还日:" << b.due() << endl;
    try { b.borrow("小红", 2); }
    catch (const exception& e) { cout << "异常: " << e.what() << endl; }
    // b.borrower_ = "";   // 取消注释会编译失败 —— 这就是 private 的作用
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
三体 可借: False 应还日: 31
异常: 这本书已借出
```

C++：

```text
三体 可借:0 应还日:31
异常: 这本书已借出
```

## 高三 L6 / L7　手写 K 近邻 + 留出法评估

不依赖机器学习库，任何环境都能运行；有 scikit-learn 时再做对比（见学习材料）。

**Python 版**（`g3_knn.py`）

```python
# 高三 L6 / L7：自己实现 K 近邻（不依赖机器学习库），并用留出法评估
import math
from collections import Counter

def knn(train, q, k=3):
    dist = sorted((math.dist(x, q), label) for x, label in train)
    votes = Counter(label for _, label in dist[:k])
    return votes.most_common(1)[0][0]

# 特征：(每日学习小时, 睡眠小时)；标签 1=成绩高 0=成绩低
data = [((1, 8), 0), ((2, 8), 0), ((2.5, 7), 0), ((3, 7), 0),
        ((4, 6.5), 1), ((4.5, 6), 1), ((5, 6), 1), ((6, 5.5), 1)]

print("预测 (4.2,6.5):", knn(data, (4.2, 6.5)))     # 1
print("预测 (1.5,8):  ", knn(data, (1.5, 8)))       # 0

# 留出法：每个类别各留一个做测试，其余训练（数据很少，仅作演示）
test_idx = {3, 7}
train = [d for i, d in enumerate(data) if i not in test_idx]
test  = [d for i, d in enumerate(data) if i in test_idx]
acc = sum(knn(train, x) == y for x, y in test) / len(test)
print("测试集准确率:", acc)
```

**C++ 版**（`g3_knn.cpp`）

```cpp
// 高三 L6 / L7：自己实现 K 近邻（不依赖机器学习库），并用留出法评估
#include <iostream>
#include <vector>
#include <cmath>
#include <algorithm>
#include <map>
using namespace std;

struct Sample { vector<double> x; int label; };

int knn(const vector<Sample>& train, const vector<double>& q, int k = 3) {
    vector<pair<double, int>> dist;
    for (const auto& s : train) {
        double sum = 0;
        for (size_t i = 0; i < q.size(); i++) sum += (s.x[i] - q[i]) * (s.x[i] - q[i]);
        dist.push_back({sqrt(sum), s.label});
    }
    sort(dist.begin(), dist.end());
    map<int, int> votes;
    for (int i = 0; i < k; i++) votes[dist[i].second]++;
    return max_element(votes.begin(), votes.end(),
                       [](const auto& a, const auto& b) { return a.second < b.second; })->first;
}

int main() {
    // 特征：(每日学习小时, 睡眠小时)；标签 1=成绩高 0=成绩低
    vector<Sample> data = {{{1, 8}, 0}, {{2, 8}, 0}, {{2.5, 7}, 0}, {{3, 7}, 0},
                           {{4, 6.5}, 1}, {{4.5, 6}, 1}, {{5, 6}, 1}, {{6, 5.5}, 1}};
    cout << "预测 (4.2,6.5): " << knn(data, {4.2, 6.5}) << endl;   // 1
    cout << "预测 (1.5,8):   " << knn(data, {1.5, 8}) << endl;     // 0

    vector<Sample> train, test;                                      // 留出法
    for (size_t i = 0; i < data.size(); i++) (i == 3 || i == 7 ? test : train).push_back(data[i]);
    int right = 0;
    for (const auto& s : test) right += (knn(train, s.x) == s.label);
    cout << "测试集准确率: " << double(right) / test.size() << endl;
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
预测 (4.2,6.5): 1
预测 (1.5,8):   0
测试集准确率: 1.0
```

C++：

```text
预测 (4.2,6.5): 1
预测 (1.5,8):   0
测试集准确率: 1
```

## 高三 L8　规则里的偏见：按邮编筛选 vs 按岗位条件筛选

同一规则，换一种写法，结果不同。

**Python 版**（`g3_bias.py`）

```python
# 高三 L8：规则里的偏见 —— 对比"按邮编筛选"与"按岗位相关条件筛选"
candidates = [
    {"name": "甲", "zipcode": "100001", "skill": 6, "years": 2},
    {"name": "乙", "zipcode": "200002", "skill": 9, "years": 5},
    {"name": "丙", "zipcode": "100002", "skill": 4, "years": 1},
]

def shortlist_biased(c):
    return c["zipcode"] in ("100001", "100002")           # 与岗位无关，却决定结果

def shortlist_fair(c):
    return c["skill"] >= 6 and c["years"] >= 2            # 只看岗位相关条件

print("有偏:", [c["name"] for c in candidates if shortlist_biased(c)])   # ['甲','丙'] —— 技能最强的乙被排除
print("相关:", [c["name"] for c in candidates if shortlist_fair(c)])     # ['甲','乙']
```

**C++ 版**（`g3_bias.cpp`）

```cpp
// 高三 L8：规则里的偏见 —— 对比"按邮编筛选"与"按岗位相关条件筛选"
#include <iostream>
#include <vector>
#include <string>
using namespace std;

struct Candidate { string name; string zipcode; int skill; int years; };

bool shortlistBiased(const Candidate& c) {
    return c.zipcode == "100001" || c.zipcode == "100002";    // 与岗位无关，却决定结果
}
bool shortlistFair(const Candidate& c) {
    return c.skill >= 6 && c.years >= 2;                      // 只看岗位相关条件
}

int main() {
    vector<Candidate> cs = {{"甲", "100001", 6, 2}, {"乙", "200002", 9, 5}, {"丙", "100002", 4, 1}};
    cout << "有偏:"; for (auto& c : cs) if (shortlistBiased(c)) cout << " " << c.name; cout << endl;  // 甲 丙
    cout << "相关:"; for (auto& c : cs) if (shortlistFair(c))   cout << " " << c.name; cout << endl;  // 甲 乙
    return 0;
}
```

**实际输出**（两种语言的数值一致，小数的显示格式可能不同，如 65.0 与 65）

Python：

```text
有偏: ['甲', '丙']
相关: ['甲', '乙']
```

C++：

```text
有偏: 甲 丙
相关: 甲 乙
```

---

## 高二 L11　下标越界与除以零：两种语言的"出错方式"

**要点**：Python 出错会立刻停下并告诉你；C++ 用 `[]` 越界可能悄悄继续运行，所以更需要边界测试。

Python：

```python
# 高二 L11：下标越界——Python 直接报错
v = [1, 2, 3]
total = 0
try:
    for i in range(4):
        total += v[i]
except IndexError as e:
    print("捕获到异常:", e)
```

输出：

```text
捕获到异常: list index out of range
```

C++：

```cpp
// 高二 L11：下标越界——[] 不检查，.at() 抛异常
#include <iostream>
#include <vector>
#include <stdexcept>
using namespace std;
int main() {
    vector<int> v = {1, 2, 3};
    int sum = 0;
    for (int i = 0; i <= 3; i++) sum += v[i];      // 越界一位：不报错，结果不可预测
    cout << "用 [] 求和: " << sum << "（程序没有报错，但结果不可信）" << endl;
    try {
        int s2 = 0;
        for (int i = 0; i <= 3; i++) s2 += v.at(i);  // .at() 会检查
        cout << s2 << endl;
    } catch (const out_of_range& e) {
        cout << "用 .at() 求和: 抛出 out_of_range 异常" << endl;
    }
    return 0;
}
```

输出（本机；越界读到的值**因平台而异**，不要依赖）：

```text
用 [] 求和: 6（程序没有报错，但结果不可信）
用 .at() 求和: 抛出 out_of_range 异常
```

C++ 空数据求平均（小数相除）：

```cpp
// 高一 L16：C++ 除以零——整数与小数的不同表现（依赖平台，不要依赖）
#include <iostream>
#include <vector>
using namespace std;
int main() {
    vector<int> scores;                       // 空数据
    double avg = 0.0 / scores.size();         // 0.0 / 0
    cout << "空数据直接求平均: " << avg << endl;
    double d = 5.0 / (double)scores.size();   // 5.0 / 0
    cout << "5.0 / 0 = " << d << endl;
    if (scores.empty()) cout << "正确做法：先判断 empty()，再求平均" << endl;
    return 0;
}
```

输出：

```text
空数据直接求平均: nan
5.0 / 0 = inf
正确做法：先判断 empty()，再求平均
```

> 教学提示：Python 中除以零会抛出 `ZeroDivisionError`。C++ 小数除以零得到 `inf` / `nan`，整数除以零是未定义行为（本机实测得到 0，其他平台可能崩溃），因此“空数据”必须写进测试用例表。

---

# 附：实测对照（教师参考）

## 编译优化对运行时间的影响（高二 L13）
同一份 `g2_timing.cpp`，求最大值（循环）在 n = 1,000,000 时的耗时（取 3 次最小值，**数值仅供参考，因机器而异**）：

| 语言 / 编译选项 | 耗时（秒） | 说明 |
|---|---|---|
| Python | 约 0.017 | 解释执行 |
| C++ `-O0`（不优化） | 约 0.0054 | 比 Python 快约 3 倍 |
| C++ `-O2`（优化） | 约 0.00007 | 编译器做了向量化等优化，比 `-O0` 快约 80 倍 |

**教学提示**：这个对照说明，比较算法速度时必须写明**语言、编译选项、数据规模、测量方法**；只写“更快”没有意义。高中阶段不要求解释为什么，只要求学生学会“记录条件，再下结论”。

## 两种语言的常见差异（教师参考，已实测）

| 现象 | Python | C++ | 教学含义 |
|---|---|---|---|
| `7 / 2` | 3.5 | **3**（整数相除取整） | 求平均要先转成小数 |
| 整除 | `7 // 2` = 3 | `7 / 2` = 3 | 二进制转换要用整除 |
| `0.1 + 0.2 == 0.3` | False | 0（False） | 两种语言都不能直接用 `==` 比较小数 |
| 整数范围 | 无限大 | `int` 约 ±21 亿，超出会溢出 | 数据大时要用 `long long` |
| 下标越界 | 报错 `IndexError` | 用 `[]` 不检查（结果不可预测），用 `.at()` 才抛异常 | C++ 初学者更需要边界测试 |
| 变量类型 | 自动判断 | 必须声明 | C++ 迫使学生想清楚“这个量是什么” |
| 交换两个变量 | `a, b = b, a` | `swap(a, b)` 或用临时变量 | 先理解临时变量，再用简写 |
| 隐藏数据 | `_` 前缀（约定） | `private`（编译器强制） | 体会“接口与实现”的区别 |
