### C++ STL

1. std::accumulate 函数,  求和/求积等运算

   ```c++
   // 头文件
   #include <vector>
#include <numeric>
   #include <functional>

   
   
   // 代码片段
   vector<int> v(20);
   for(int i=1; i<21; i++) {
       v.push_back(i);
   }
   // 求和
   int total;
   total = accumulate(v.begin(), v.end(), 0);
   
   // 求积
   int ptotal;
   ptotal = accumulate(v.begin(), v.end(), 1, multiplies<int>() );
   ```
   
2. std::advance 函数, 移动迭代器

   ```c++
   template <class InputIterator, class Distance>
   void advance( InputIterator& it, Distance n);
   ```

   ```c++
   // 头文件
   #include <iterator>
   #include <list>
   
   
   
   // 代码片段
   list<int> ll;
   for(int i=0; i<10; i++) { 
       ll.push_back(i); 
   }
   auto it = ll.begin();
   // 向后移动5
   advance(it, 5);
   cout << *it << endl;
   // 向前移动1
   advance(it,-1);
   
   ```

3. std::for_each 函数, 对区间内元素进行修改

   ```c++
   class Add{
       int val;
       int num=100;
   public:
       // 拷贝构造
       Add( const int v) : val(v) {}
       // 函数对象
       void operator() (int &el ) const {
           el += val;
       }
       // 重载, 以便于把class转变为double类型
       operator double() {
           return static_cast<double>(val) / static_cast<double>(num) ;
       }
       
   }
   
   
   // 代码片段
   std::vector<int> v;
   for(int i=1; i<21; i++) {
       v.push_back(i);
   }
   // lambda 函数
   for_each( v.begin(), v.end(), [](int &elem){
      // do something 
   });
   // 普通函数
   for_each( v.begin(), v.end(), print);
   // 函数对象, 并强制转化为double类型
   double result = for_each( v.begin(), v.end(), Add(10) );
   ```

4. std::mem_fn 函数, 把成员函数转为函数对象, 使用对象指针进行绑定

   ```c++
   // 头文件
   #include <functional>
   
   
   // 代码片段
   struct holder {
       int val;
       int calc() { return val*3; }
   }
   
   int main() {
       // 初始化对象, 成员赋值为5
       holder h {5};
       // 获取到holder的成员函数calc
       auto fn = std::mem_fn( &holder::calc );
       // 调用时第一个参数必须是对象
       std::cout << fn(h) ;
       
       // 其他例子: 针对容器使用 ( 此处也可以使用 lambda )
       for_each( v.begin(), v.end(), mem_fn(&Foo::print));
   }
   ```

5.  std::bind1st, std:bind2nd 函数, std::copy_if函数:

   ```c++
   // copy_if 原型
   template < typename InputIterator, typename OutputIterator, typename UnaryPredicate> 
   OutputIterator copy_if ( InputIterator first, InputIterator last, 
                            Outputiterator result, UnaryPredicate pred)
   ```

   

   ```c++
   // 有些函数需要两个参数, 比如std::less<T>() 比较两个数的大小, std::minus, std::plus, std:equal_to
   // 但有些算法的参数有要去必须是一元函数, 比如 std:copy_if(start, end, op, condition ) 的第四个参数就是一元函数
   // std:bind1st, std:bind2nd 可以把二元函数变为一元函数
   // C++11中使用bind()来代替bind1st()和bind2nd()
   
   #include <iostream>
   #include <vector>
   #include <string>
   #include <iterator>
   #include <algorithm>
   #include <functional>
   using namespace std;
   
   int main(){
       int arr[] = {1,2,3,4,56,7,8};
       vector<int> vec;
       // 将6绑定为less<int>()函数的第一个参数, 即做比较 6 < value
       copy_if(bein(arr), end(arr), back_inserter(vec), bind1st( less<int>(),6) );  // C++11使用bind: bind(less<int>(), 6, placeholders::_2)
       // 打印的匿名函数
       auto print = []( int val ) {
           cout<< val << endl;
       }
       for_each( vec.begin(), vec.end(), print );  // 7 8 9 
       
       // 将6绑定为less<int>()函数的第二个参数, 即做比较 value < 6
       copy_if(begin(arr), end(arr), back_inserter(vec), bind1st( less<int>(),6) );     // C++11使用bind: bind(less<int>(), 6, placeholders::_1)
       for_each( vec.begin(), vec.end(), print );  // 1 2 3 4 5   
   }
   ```

   

6. 

7. 

8. 