# [学习日记] 牛客华为笔试题练习


Nowcoder 华为笔试

易忘点总结
<!--more-->


#  牛客网华为题库：

1、如果使用string字符串，要输入带空格的字符串得使用

```c++
getline(cin,str);
```

2、字符串的大小写转换

```c++
#include<string>
#include<algorithm>
//string类型
string str;
//转大写
transform(str.begin(), str.end(), str.begin(), ::toupper);//1
//第三个参数表示写入的位置
for(int i=0;i<str.length();i++)//2
        str[i]=toupper(str[i]);
//转小写
transform(str.begin(), str.end(), str.begin(), ::tolower);//1
for(int i=0;i<str.length();i++)//2
        str[i]=tolower(str[i]);
//char类型
char ch;
ch=toupper(ch);
ch=tolower(ch);

```

3、字符串前后翻转

```c++
#include<algorithm>
string str="hello world";
reverse(str.begin(),str.end());
```

4、字符与整型转换

```c++
string str;
int a;
str=to_string(a);//整数转为string
//string字符串转整数---带进制
string str="0xAA";
a=stoi(str,nullptr,16);//a=170
string str="11";
a=stoi(str);//a=11
a=stoi(str,nullptr,8);//a=9
a=stoi(str,nullptr,2);//a=3
a=stoi(str,nullptr,16);//a=17
//第三个参数指字符串表示是几进制的
//第二个参数是一个pos指针，一般是nullptr
```

5、浮点型数转整型数

```c++
double a=4.6;
int b=int(a);//b=4强制类型转换只保留整数部分，不进行四舍五入
int c=int(a+0.5);//c=5四舍五入的保留整数
```

6、STL容器map的使用，适合用于处理二维数组型的，坐标那种

```c++
#include<map>
//定义，第一个元素是map的键，第二个元素是值，键不可重复
map<int,string>m;//map<string,string>m;
//插入元素，两种插入方法皆可
m.insert(pair<int,string>(5,'c'));
m[123]="cc";
//访问元素
//第一种是通过下标访问
m[5]='c';
//第二种是通过迭代器进行访问
map<int,string>::iterator it;
//利用迭代器遍历
for(it=m.begin();it!=m.end();it++)
{
    cout<<it->first<<" "<<it->second<<endl;
    //迭代器的first属性对应键值，second属性
}
```

7、string操作

```c++
#include<string>
#include<algorithm>
string str="123451";
//翻转string
reverse(str.begin(),str.end());
//寻找string中的元素
int pos1=str.find('1');//pos1=0
int pos4=str.rfind('1');//pos4=5,rfind函数是从右向左找
int pos2=str.find("123");//pos2=0，返回第一个元素的下标
string a='c';
int pos3=str.find(a);//pos3=npos，代表没有找到该字符
//输出
cout<<str<<endl;//可以直接cout，char*数组的话得遍历输出
```

8、vector操作

```c++
vector<string>v{"I","am","a","boy"};
```


