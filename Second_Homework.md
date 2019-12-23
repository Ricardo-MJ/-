# Cpp2
## 9.11
> 对6种创建和初始化vector对象的方法，每一种都给出一个实例。解释每个vector包含什么值。
```CPP
vector<int> vec;  // 0
vector<int> vec(10);  // 0
vector<int> vec(10,1); // 1
vector<int> vec{1,2,3,4,5}; //1, 2, 3, 4, 5
vector<int> vec(other_vec); // 与 other_vec 一样
vector<int> vec(other_vec.begin(), other_vec.end());  // 与 other_vec 一样
```
## 9.20
> 编写程序，从一个list<int>拷贝元素到两个deque中。值为偶数的所有元素都拷贝到一个deque中，而奇数值元素都拷贝到另一个deque中。
```CPP
/*Copyright [2019] <Copyright MrM>
*/
#include <iostream>
#include <deque>
#include <list>
int main()
{
std::list<int> list1{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
std::deque<int> deque_jishu, deque_oushu;
for(auto i : list1)
  if(i % 2 == 0)
      deque_oushu.push_back(i);
  else
      deque_jishu.push_back(i);
for(auto i : deque_oushu)
  std::cout << i << ' ';
std::cout << '\n';
for(auto i : deque_jishu)
  std::cout << i << ' ';
return 0;
}
```
	
## 9.29
> 假定vec包含25个元素，那么vec.resize(100)会做什么？如果接下来调用vec.resize(10)会做什么？
* 将75个值为0的元素添加到vec的末尾
* 从vec的末尾删除90个元素

## 9.43
> 编写一个函数，接受三个string参数是s、oldVal 和newVal。使用迭代器及insert和erase函数将s中所有oldVal替换为newVal。测试你的程序，用它替换通用的简写形式，如，将"tho"替换为"though",将"thru"替换为"through"。
```CPP
/*Copyright [2019] <Copyright MrM>
*/
#include <iostream>
#include <string>
void f(std::string& s, std::string oldVal, std::string newVal)
{
    auto x = s.begin();
    while(x != s.end() - oldVal.size())
    {
        if(std::string(x, x + oldVal.size()) == oldVal)
        {
            s.erase(x, x + oldVal.size());
            s.insert(x, newVal.begin(), newVal.end());
            x += newVal.size();
        }
        else
            x++;
    }
}

int main()
{
    std::string s("tho thru");
    f(s, "tho", "though");
    std::cout << s;
    std::cout << '\n';
    f(s, "thru", "through");
    std::cout << s;
    return 0;
}
```
## 9.52
> 使用stack处理括号化的表达式。当你看到一个左括号，将其记录下来。当你在一个左括号之后看到一个右括号，从stack中pop对象，直至遇到左括号，将左括号也一起弹出栈。然后将一个值（括号内的运算结果）push到栈中，表示一个括号化的（子）表达式已经处理完毕，被其运算结果所替代。
```CPP
/*Copyright [2019] <Copyright MrM>
*/
#include<iostream>
#include<stack>
#include<string>

std::string ji_suan(std::string l, std::string op, std::string r)
{
	std::string s;
	if (op == "-")
		s = std::to_string(stoi(l) - stoi(r));
    else if (op == "+")
		s = std::to_string(stoi(l) + stoi(r));
    else if (op == "*")
		s = std::to_string(stoi(l) * stoi(r));
    else if (op == "/")
		s = std::to_string(stoi(l) / stoi(r));
	return s;
}

int main()
{
    std::string s("1+2+3*(3*3)");
    std::stack<std::string> stack1;
    for(auto i = s.begin(); i != s.end(); )
    {
        if(*i == '(')
        {
            stack1.push(std::string(1, *i));
            i++;
            while(*i != ')')
            {
                stack1.push(std::string(1, *i));
                i++;
            }
        }
        else if(*i == ')')
        {
            auto r = stack1.top();
            stack1.pop();
            auto op = stack1.top();
            stack1.pop();
            auto l = stack1.top();
            stack1.pop();
            stack1.push(ji_suan(l, op, r));
            i++;
        }
        else
            i++;
    }
    std::cout << stack1.top();
    stack1.pop();
    return 0;
}
```
## 10.3
> 用accumulate求一个 vector 中元素之和。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <iostream>
#include <vector>
#include <numeric>
int main() {
    std::vector<int> v = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    std::cout << std::accumulate(v.begin(), v.end(), 0) << std::endl;
    return 0;
}
```
## 10.15
> 编写一个 lambda ，捕获它所在函数的 int，并接受一个 int参数。lambda 应该返回捕获的 int 和 int 参数的和。
```cpp
int a = 1;
auto f = [a](int b) {return a + b;};
```

## 10.34
> 使用 reverse_iterator 逆序打印一个vector。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <iostream>
#include <vector>
int main() {
    std::vector<int> v = {1, 2, 3, 4, 5};
    for(auto p = v.crbegin(); p != v.crend(); p++)
        std::cout << *p << ' ';
    return 0;
}
```

## 10.42
> 使用 list 代替 vector 重新实现10.2.3节中的去除重复单词的程序。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <iostream>
#include <string>
#include <list>
void elimDups(std::list<std::string> &words)
{
    words.sort();
    words.unique();
}
int main() {
    std::list<std::string> words = {"the", "quick", "red", "fox", "jumps", "over", "the", "slow", "red", "turtle"};
    elimDups(words);
    for(auto i : words)
        std::cout << i << ' ';
    return 0;
}
```

## 11.12
> 编写程序，读入string和int的序列，将每个string和int存入一个pair 中，pair保存在一个vector中。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <vector>
#include <utility>
#include <string>
#include <iostream>
int main()
{
	std::vector<std::pair<std::string, int>> v;
	std::string s;
	int x;
	while(std::cin >> s >> x)
        v.push_back(std::pair<std::string, int>(s, x));
    for(auto i : v)
        std::cout << i.first << ":" << i.second << std::endl;
}
```
## 11.17
> 假定 c 是一个string的multiset，v 是一个string 的vector，解释下面的调用。指出每个调用是否合法：
```cpp
copy(v.begin(), v.end(), inserter(c, c.end()));
copy(v.begin(), v.end(), back_inserter(c));
copy(c.begin(), c.end(), inserter(v, v.end()));
copy(c.begin(), c.end(), back_inserter(v));
```
copy(v.begin(), v.end(), back_inserter(c));这个调用不合法,因为multiset 没有 push_back 方法, 所以也不能使用back_inserter。
其他三个都合法。

## 11.38
> 用 unordered_map 重写单词计数程序和单词转换程序。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include<iostream>
#include<string>
#include <fstream>
#include<sstream>
#include<unordered_map>

void Count(std::unordered_map<std::string, size_t> word_count)
{
    std::string word;
    while(std::cin >> word)
        word_count[word]++;
    for(auto i : word_count)
        std::cout << i.first << "出现了" << i.second << "次" << std::endl;
}

std::unordered_map<std::string, std::string> buildMap(std::ifstream &map_file)
{
    std::unordered_map<std::string, std::string> trans_map;
    std::string key;
    std::string value;
    while(map_file >> key && getline(map_file, value))//map_file输入以" "为结束标志，会储存到后续输入的value中，所以后续要用value.substr(1)
        if(value.size() > 1)
            trans_map[key] = value.substr(1);
        else
            throw std::runtime_error("no rule for" + key);
    return trans_map;
}

const std::string & transform(const std::string &s, const std::unordered_map<std::string, std::string> &m)
{
    auto map_it = m.find(s);
    if(map_it != m.cend())
        return map_it -> second;
    else
        return s;
}

void trans(std::ifstream &map_file, std::ifstream &input)
{
    auto trans_map = buildMap(map_file);
    std::string text;
    while (getline(input, text))
    {
        std::istringstream stream(text);
        std::string word;
        bool firstword = true;
        while(stream >> word)
        {
            if (firstword)
                firstword = false;
            else
                std::cout << " ";
            std::cout << transform(word, trans_map);
        }
        std::cout << std::endl;
    }
}

int main()
{
    std::unordered_map<std::string, size_t> word_count;
    std::ifstream map_file("E:\\cpp\\1.txt");
    std::ifstream input("E:\\cpp\\2.txt");
    //Count(word_count);
    trans(map_file, input);
    return 0;
}
```

## 13.12
> 在下面的代码片段中会发生几次析构函数的调用
```cpp
bool fcn(const Sales_data *trans, Sales_data accum)
{
	Sales_data item1(*trans), item2(accum);
	return item1.isbn() != item2.isbn();
}
```
三次, accum、item1和item2各一次。

## 13.18
> 定义一个 Employee 类，它包含雇员的姓名和唯一的雇员证号。为这个类定义默认构造函数，以及接受一个表示雇员姓名的 string 的构造函数。每个构造函数应该通过递增一个 static 数据成员来生成一个唯一的证号。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include<iostream>
#include<string>

class Employee
{
public:
    Employee();
    Employee(std::string name);
    static int n;
private:
    std::string m_name;
    int m_num;
};

Employee::Employee()
{
    m_name = "Alice";
    m_num = n++;
    std::cout << m_name << ' ' << m_num << std::endl;
}

Employee::Employee(std::string name)
{
    m_name = name;
    m_num = n++;
    std::cout << m_name << ' ' << m_num << std::endl;
}

int Employee::n = 1;

int main()
{
    Employee mem1;
    Employee mem2("Bob");
    Employee mem3("Eric");
    return 0;
}
```

## 13.46
> 什么类型的引用可以绑定到下面的初始化器上？
```cpp
int f();
vector<int> vi(100);
int? r1 = f();
int? r2 = vi[0];
int? r3 = r1;
int? r4 = vi[0] * f();
```
```cpp
int f();
vector<int> vi(100);
int&& r1 = f();
int& r2 = vi[0];
int& r3 = r1;
int&& r4 = vi[0] * f();
```

## 13.49
> 为你的 StrVec、String 和 Message 类添加一个移动构造函数和一个移动赋值运算符。
* StrVec类
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include<iostream>
#include<string>
#include<memory>
#include<utility>
class StrVec
{
public:
    StrVec():elements(nullptr), first_free(nullptr), cap(nullptr) {}
    StrVec(const StrVec&);
    StrVec(StrVec &&s) noexcept; //移动构造函数
    StrVec &operator=(const StrVec&);
    StrVec &operator=(StrVec &&rhs) noexcept //移动赋值运算
{
    if(this != &rhs)
    {
        free();
        elements = rhs.elements;
        first_free = rhs.first_free;
        cap = rhs.cap;
        rhs.elements = rhs.first_free = rhs.cap = nullptr;
    }
    return *this;
}
    ~StrVec();
    void push_back(const std::string&);
    size_t size() const {return first_free - elements;}
    size_t capacity() const {return cap - elements;}
    std::string *begin() const {return elements;}
    std::string *end() const {return first_free;}
private:
    static std::allocator<std::string> alloc;
    void chk_n_alloc()
        {if (size() == capacity()) reallocate();}
    std::pair<std::string*, std::string*> alloc_n_copy
        (const std::string*, const std::string*);
    void free();
    void reallocate();
    std::string *elements;
    std::string *first_free;
    std::string *cap;
};

void StrVec::push_back(const std::string& s)
{
    chk_n_alloc();
    alloc.construct(first_free++, s);
}

std::pair<std::string*, std::string*> StrVec::alloc_n_copy(const std::string *b, const std::string *e)
{
    auto data = alloc.allocate(e - b);
    return {data, std::uninitialized_copy(b, e, data)};
}

void StrVec::free()
{
    if(elements)
        for(auto p = first_free; p != elements; )
            alloc.destroy(--p);
        alloc.deallocate(elements, cap - elements);
}

StrVec::StrVec(const StrVec &s)
{
    auto newdata = alloc_n_copy(s.begin(), s.end());
    elements = newdata.first;
    first_free = cap = newdata.second;
}

StrVec::~StrVec()
{
    free();
}

StrVec& StrVec::operator=(const StrVec &rhs)
{
    auto data = alloc_n_copy(rhs.begin(), rhs.end());
    free();
    elements = data.first;
    first_free = cap = data.second;
    return *this;
}

void StrVec::reallocate()
{
    auto newcapacity = size() ? 2*size() : 1;
    auto newdata = alloc.allocate(newcapacity);
    auto dest = newdata;
    auto elem = elements;
    for (size_t i = 0; i != size(); ++i)
        alloc.construct(dest++, std::move(*elem++));
    free();
    elements = newdata;
    first_free = dest;
    cap = elements + newcapacity;
}

StrVec::StrVec(StrVec &&s) noexcept: elements(s.elements), first_free(s.first_free), cap(s.cap)
    {s.elements = s.first_free = s.cap = nullptr;}
```

* String类
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <memory>
class String {
public:
    String() : String("") {}
    String(const char*);
    String(const String&);
    String& operator=(const String&);
    String(String&&) noexcept;
    String& operator=(String&&) noexcept;
    ~String();

    const char* c_str() const { return elements; }
    size_t size() const { return end - elements; }
    size_t length() const { return end - elements - 1; }

private:
    std::pair<char*, char*> alloc_n_copy(const char*, const char*);
    void range_initializer(const char*, const char*);
    void free();

private:
    char* elements;
    char* end;
    std::allocator<char> alloc;
};

#include <algorithm>

std::pair<char*, char*> String::alloc_n_copy(const char* b, const char* e)
{
    auto str = alloc.allocate(e - b);
    return {str, std::uninitialized_copy(b, e, str)};
}

void String::range_initializer(const char* first, const char* last)
{
    auto newstr = alloc_n_copy(first, last);
    elements = newstr.first;
    end = newstr.second;
}

String::String(const char* s)
{
    char* sl = const_cast<char*>(s);
    while (*sl) ++sl;
    range_initializer(s, ++sl);
}

String::String(const String& rhs)
{
    range_initializer(rhs.elements, rhs.end);
}

void String::free()
{
    if (elements) {
        std::for_each(elements, end, [this](char& c) { alloc.destroy(&c); });
        alloc.deallocate(elements, end - elements);
    }
}

String::~String()
{
    free();
}

String& String::operator=(const String& rhs)
{
    auto newstr = alloc_n_copy(rhs.elements, rhs.end);
    free();
    elements = newstr.first;
    end = newstr.second;
    return *this;
}

String::String(String&& s) noexcept : elements(s.elements), end(s.end)
{
    s.elements = s.end = nullptr;
}

String& String::operator=(String&& rhs) noexcept
{
    if (this != &rhs) {
        free();
        elements = rhs.elements;
        end = rhs.end;
        rhs.elements = rhs.end = nullptr;
    }
    return *this;
}
```

* Message类
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <set>
#include <string>
#include <iostream>
class Folder;

class Message {
    friend void swap(Message&, Message&);
    friend void swap(Folder&, Folder&);
    friend class Folder;

public:
    explicit Message(const std::string& str = "") : contents(str) {}
    Message(const Message&);
    Message& operator=(const Message&);
    Message(Message&&);
    Message& operator=(Message&&);
    ~Message();
    void save(Folder&);
    void remove(Folder&);

    void print_debug();

private:
    std::string contents;
    std::set<Folder*> folders;

    void add_to_Folders(const Message&);
    void remove_from_Folders();
    void move_Folders(Message*);

    void addFldr(Folder* f) { folders.insert(f); }
    void remFldr(Folder* f) { folders.erase(f); }
};

void swap(Message&, Message&);

class Folder {
    friend void swap(Message&, Message&);
    friend void swap(Folder&, Folder&);
    friend class Message;

public:
    Folder() = default;
    Folder(const Folder&);
    Folder& operator=(const Folder&);
    Folder(Folder&&);
    Folder& operator=(Folder&&);
    ~Folder();

    void print_debug();

private:
    std::set<Message*> msgs;

    void add_to_Messages(const Folder&);
    void remove_from_Messages();
    void move_Messages(Folder*);

    void addMsg(Message* m) { msgs.insert(m); }
    void remMsg(Message* m) { msgs.erase(m); }
};

void swap(Folder&, Folder&);

void swap(Message& lhs, Message& rhs)
{
    using std::swap;
    lhs.remove_from_Folders();
    rhs.remove_from_Folders();

    swap(lhs.folders, rhs.folders);
    swap(lhs.contents, rhs.contents);

    lhs.add_to_Folders(lhs);
    rhs.add_to_Folders(rhs);
}


Message::Message(const Message& m) : contents(m.contents), folders(m.folders)
{
    add_to_Folders(m);
}

Message& Message::operator=(const Message& rhs)
{
    remove_from_Folders();
    contents = rhs.contents;
    folders = rhs.folders;
    add_to_Folders(rhs);
    return *this;
}

Message::Message(Message&& m) : contents(std::move(m.contents))
{
    move_Folders(&m);
}

Message& Message::operator=(Message&& rhs)
{
    if (this != &rhs) {
        remove_from_Folders();
        contents = std::move(rhs.contents);
        move_Folders(&rhs);
    }
    return *this;
}

Message::~Message()
{
    remove_from_Folders();
}

void Message::save(Folder& f)
{
    addFldr(&f);
    f.addMsg(this);
}

void Message::remove(Folder& f)
{
    remFldr(&f);
    f.remMsg(this);
}

void Message::print_debug()
{
    std::cout << contents << std::endl;
}

void Message::add_to_Folders(const Message& m)
{
    for (auto f : m.folders) f->addMsg(this);
}

void Message::remove_from_Folders()
{
    for (auto f : folders) f->remMsg(this);
}

void Message::move_Folders(Message* m)
{
    folders = std::move(m->folders);
    for (auto f : folders) {
        f->remMsg(m);
        f->addMsg(this);
    }
    m->folders.clear();
}

void swap(Folder& lhs, Folder& rhs)
{
    using std::swap;
    lhs.remove_from_Messages();
    rhs.remove_from_Messages();

    swap(lhs.msgs, rhs.msgs);

    lhs.add_to_Messages(lhs);
    rhs.add_to_Messages(rhs);
}

Folder::Folder(const Folder& f) : msgs(f.msgs)
{
    add_to_Messages(f);
}

Folder& Folder::operator=(const Folder& rhs)
{
    remove_from_Messages();
    msgs = rhs.msgs;
    add_to_Messages(rhs);
    return *this;
}

Folder::Folder(Folder&& f)
{
    move_Messages(&f);
}

Folder& Folder::operator=(Folder&& f)
{
    if (this != &f) {
        remove_from_Messages();
        move_Messages(&f);
    }
    return *this;
}

Folder::~Folder()
{
    remove_from_Messages();
}

void Folder::print_debug()
{
    for (auto m : msgs) std::cout << m->contents << " ";
    std::cout << std::endl;
}

void Folder::add_to_Messages(const Folder& f)
{
    for (auto m : f.msgs) m->addFldr(this);
}

void Folder::remove_from_Messages()
{
    for (auto m : msgs) m->remFldr(this);
}

void Folder::move_Messages(Folder* f)
{
    msgs = std::move(f->msgs);
    for (auto m : msgs) {
        m->remFldr(f);
        m->addFldr(this);
    }
    f->msgs.clear();
}
```

## 13.58
> 编写新版本的 Foo 类，其 sorted 函数中有打印语句，测试这个类，来验证你对前两题的答案是否正确。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <vector>
#include <iostream>
#include <algorithm>

using std::vector; using std::sort;

class Foo
{
public:
	Foo sorted() && ;
	Foo sorted() const &;
private:
	vector<int> data;
};

Foo Foo::sorted() && {
	sort(data.begin(), data.end());
	std::cout << "&&" << std::endl; // debug
	return *this;
}

Foo Foo::sorted() const &
{
	//    Foo ret(*this);
	//    sort(ret.data.begin(), ret.data.end());
	//    return ret;

	std::cout << "const &" << std::endl; // debug

	//    Foo ret(*this);
	//    ret.sorted();     // Exercise 13.56
	//    return ret;

	return Foo(*this).sorted(); // Exercise 13.57
}

int main()
{
	Foo().sorted(); // call "&&"
	Foo f;
	f.sorted(); // call "const &"
}
```

## 14.3
> string 和 vector 都定义了重载的==以比较各自的对象，假设 svec1 和 svec2 是存放 string 的 vector，确定在下面的表达式中分别使用了哪个版本的==？
```cpp
(a) "cobble" == "stone"
(b) svec1[0] == svec2[0]
(c) svec1 == svec2
(d) "svec1[0] == "stone"
```
* (a)都不是
* (b)string
* (c)vector
* (d)string

## 14.20
> 为你的 Sales_data 类定义加法和复合赋值运算符。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <string>
#include <iostream>

class Sales_data
{
	friend std::istream& operator>>(std::istream&, Sales_data&);
	friend std::ostream& operator<<(std::ostream&, const Sales_data&);
	friend Sales_data operator+(const Sales_data&, const Sales_data&);

public:
	Sales_data(const std::string &s, unsigned n, double p) :bookNo(s), units_sold(n), revenue(n*p) {}
	Sales_data() : Sales_data("", 0, 0.0f) {}
	Sales_data(const std::string &s) : Sales_data(s, 0, 0.0f) {}
	Sales_data(std::istream &is);

	Sales_data& operator=(const std::string&);

	Sales_data& operator+=(const Sales_data&);
	std::string isbn() const { return bookNo; }

private:
	inline double avg_price() const;

	std::string bookNo; //书本编号
	unsigned units_sold = 0; //某本书总共销售了多少本
	double revenue = 0.0; //某本书的总销售额
};

std::istream& operator>>(std::istream&, Sales_data&);
std::ostream& operator<<(std::ostream&, const Sales_data&);
Sales_data operator+(const Sales_data&, const Sales_data&);

inline double Sales_data::avg_price() const
{
	return units_sold ? revenue / units_sold : 0;
}


Sales_data::Sales_data(std::istream &is) : Sales_data()
{
	is >> *this;
}

Sales_data operator+(const Sales_data &lhs, const Sales_data &rhs) //加法运算符
{
	Sales_data sum = lhs;
	sum += rhs;
	return sum;
}

Sales_data& Sales_data::operator+=(const Sales_data &rhs) //复合赋值运算符
{
	units_sold += rhs.units_sold;
	revenue += rhs.revenue;
	return *this;
}


std::istream& operator>>(std::istream &is, Sales_data &item)
{
	double price = 0.0;
	is >> item.bookNo >> item.units_sold >> price;
	if (is)
		item.revenue = price * item.units_sold;
	else
		item = Sales_data();
	return is;
}

std::ostream& operator<<(std::ostream &os, const Sales_data &item)
{
	os << item.isbn() << " " << item.units_sold << " " << item.revenue << " " << item.avg_price();
	return os;
}



Sales_data& Sales_data::operator=(const std::string &isbn)
{
	*this = Sales_data(isbn);
	return *this;
}

int main()
{
    //sij表示i号书第j次销售的情况；
	Sales_data s11("1", 3, 20); //1号书卖了3本，每本单价20元
	Sales_data s12("1", 5, 20);
	Sales_data s1;
	s1 = s12 + s11; //加号运算符
	std::cout << "s1 = " << s1 << std::endl;

    std::cout << "s11 = " << s11 << std::endl;
    s11 += s12;
    std::cout << "新s11 = " <<  s11 << std::endl;
	return 0;
}
```

## 14.38
> 编写一个类令其检查某个给定的 string 对象的长度是否与一个阈值相等。使用该对象编写程序，统计并报告在输入的文件中长度为1的单词有多少个，长度为2的单词有多少个、......、长度为10的单词有多少个。
```cpp
/*Copyright [2019] <Copyright MrM>
*/
#include <iostream>
#include <string>
#include <fstream>

class BoundTest {
public:
    BoundTest(std::size_t l) : lenth(l){}
    bool operator()(const std::string& s)
    {
        return s.length() == lenth;
    }

//private:
    std::size_t lenth;
};

int main()
{
    std::ifstream fin("E:\\111.txt");
    size_t l1, l2, l3, l4, l5, l6, l7, l8, l9, l10;
    l1 = l2 = l3 = l4 = l5 = l6 = l7 = l8 = l9 = l10 = 0;
    BoundTest test1(1);
    BoundTest test2(2);
    BoundTest test3(3);
    BoundTest test4(4);
    BoundTest test5(5);
    BoundTest test6(6);
    BoundTest test7(7);
    BoundTest test8(8);
    BoundTest test9(9);
    BoundTest test10(10);

    for (std::string word; fin >> word; ) {
        if (test1(word))
            l1++;
        else if (test2(word))
            l2++;
        else if (test3(word))
            l3++;
        else if (test4(word))
            l4++;
        else if (test5(word))
            l5++;
        else if (test6(word))
            l6++;
        else if (test7(word))
            l7++;
        else if (test8(word))
            l8++;
        else if (test9(word))
            l9++;
        else if (test10(word))
            l10++;
    }
    std::cout << "There is/are " << l1 << "words of length equal to 1" << std::endl;
    std::cout << "There is/are " << l2 << "words of length equal to 2" << std::endl;
    std::cout << "There is/are " << l3 << "words of length equal to 3" << std::endl;
    std::cout << "There is/are " << l4 << "words of length equal to 4" << std::endl;
    std::cout << "There is/are " << l5 << "words of length equal to 5" << std::endl;
    std::cout << "There is/are " << l6 << "words of length equal to 6" << std::endl;
    std::cout << "There is/are " << l7 << "words of length equal to 7" << std::endl;
    std::cout << "There is/are " << l8 << "words of length equal to 8" << std::endl;
    std::cout << "There is/are " << l9 << "words of length equal to 9" << std::endl;
    std::cout << "There is/are " << l10 << "words of length equal to 10" << std::endl;
    return 0;
}
```

## 14.52
> 在下面的加法表达式中分别选用了哪个operator+？列出候选函数、可行函数及为每个可行函数的实参执行的类型转换：
```cpp
struct Longdouble {
	//用于演示的成员operator+;在通常情况下是个非成员
	LongDouble operator+(const SmallInt&);
	//其他成员与14.9.2节一致
};
LongDouble operator+(LongDouble&, double);
SmallInt si;
LongDouble ld;
ld = si + ld;
ld = ld + si;
```
* ld = si + ld; 调用不合法
* ld = ld + si; 两个都可以调用

## 15.12
> 有必要将一个成员函数同时声明成 override 和 final 吗？为什么？
* 有必要。override 的含义是重写基类中相同名称的虚函数，final 是阻止它的派生类重写当前虚函数。

## 15.16
> 改写你在15.2.2节练习中编写的数量受限的折扣策略，令其继承 Disc_quote。
```cpp
class Disc_quote : public Quote
{
public:
	Disc_quote();
	Disc_quote(const std::string& b, double p, std::size_t q, double d) :
		Quote(b, p), quantity(q), discount(d)
	{}

	virtual double net_price(std::size_t n) const override = 0;

protected:
	std::size_t quantity;
	double      discount;
};

class Limit_quote : public Disc_quote
{
public:
	Limit_quote() = default;
	Limit_quote(const std::string& b, double p, std::size_t max, double disc) :
		Disc_quote(b, p, max, disc)
	{}

	double net_price(std::size_t n) const override
	{
		return n * price * (n < quantity ? 1 - discount : 1);
	}

};
```

## 15.30
> 编写你自己的 Basket 类，用它计算上一个练习中交易记录的总价格。
```cpp
#include "quote.h"
#include <set>
#include <memory>

class Basket
{
public:
	// copy verison
	void add_item(const Quote& sale)
	{
		items.insert(std::shared_ptr<Quote>(sale.clone()));
	}
	// move version
	void add_item(Quote&& sale)
	{
		items.insert(std::shared_ptr<Quote>(std::move(sale).clone()));
	}

	double total_receipt(std::ostream& os) const;

private:

	// function to compare needed by the multiset member
	static bool compare(const std::shared_ptr<Quote>& lhs,
		const std::shared_ptr<Quote>& rhs)
	{
		return lhs->isbn() < rhs->isbn();
	}

	// hold multiple quotes, ordered by the compare member
	std::multiset<std::shared_ptr<Quote>, decltype(compare)*>
		items{ compare };
};

double Basket::total_receipt(std::ostream &os) const
{
	double sum = 0.0;

	for (auto iter = items.cbegin(); iter != items.cend();
		iter = items.upper_bound(*iter))
		//  ^^^^^^^^^^^^^^^^^^^^^^^^^^^
		// @note   this increment moves iter to the first element with key
		//         greater than  *iter.

	{
		sum += print_total(os, **iter, items.count(*iter));
	}                                   // ^^^^^^^^^^^^^ using count to fetch
	// the number of the same book.
	os << "Total Sale: " << sum << std::endl;
	return sum;
}

#include <iostream>
#include <string>
#include <vector>
#include <memory>
#include <fstream>

#include "quote.h"
#include "bulk_quote.h"
#include "limit_quote.h"
#include "disc_quote.h"
#include "basket.h"


int main()
{
	Basket basket;

	for (unsigned i = 0; i != 10; ++i)
		basket.add_item(Bulk_quote("Bible", 20.6, 20, 0.3));

	for (unsigned i = 0; i != 10; ++i)
		basket.add_item(Bulk_quote("C++Primer", 30.9, 5, 0.4));

	for (unsigned i = 0; i != 10; ++i)
		basket.add_item(Quote("CLRS", 40.1));

	std::ofstream log("log.txt", std::ios_base::app | std::ios_base::out);

	basket.total_receipt(log);
	return 0;
}
```
