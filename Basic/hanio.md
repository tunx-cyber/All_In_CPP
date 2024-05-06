将n-1个圆盘从A柱移动到B柱；将剩下的一个圆盘（最大的）从A柱移动到C柱；将n-1个圆盘从B柱移动到C柱。

```cpp
#include <iostream>
using namespace std;
void move(char A, char B)
{
  cout<< A << "->"<<B<<endl;
}
void hanoi(int n, char A, char B, char C)//把
{
  if(n==1)
  {
    move(A,C);
  }
  else
  {
    hanoi(n-1,A,C,B);
    move(A,C);
    hanoi(n-1,B,A,C);
  }
}
int main(int argc, char const *argv[]) {
  cout << "请输入盘子数量：";
  int disks;
  cin >> disks;
  hanoi(disks, 'A', 'B', 'C');
  
  return 0;
}
```

