# Olympic Tin học sinh viên - T07 Practice 03
***Nguồn: OLP Tin học sinh viên Việt Nam năm 2023 - Bảng không chuyên***
>Hướng dẫn này có thể khác so với hướng dẫn do BTC đưa ra

**A. DIỆN TÍCH TAM GIÁC**

***_Hình học cơ bản_***

Đây là một bài hình học cơ bản. Hầu hết các bạn đều chứng minh được công thức kết quả tổng quát: 

$S= \frac{u^2 + v^2}{4}$

Tuy nhiên, do giới hạn của $u, v$ khá lớn, khi dùng kiểu dữ liệu *long long* để lưu tổng bình phương sẽ có khả năng bị tràn số. Ngoài ra, khi dùng số thực *double* sẽ dễ dẫn đến sai số tích lũy.

Bài này có một điểm cần lưu ý về xuất kết quả dạng thập phân. 

Bài này không khó, nhưng nếu cài đặt không kỹ sẽ không thể ăn hết các test, dễ gây tâm lý ức chế, hoang mang cho thí sính. Đây cũng là điều các bạn cần chú ý khi thi để tránh ảnh hưởng đến kết quả các bài sau.

Mình có 2 cách trình bày cho bài này để các bạn tham khảo:

Cách 01: sử dụng kiểu dữ liệu *long double* và hàm ```fixed << setprecision()``` trong thư viện ```<iomanip> ```
```
#include <iostream>
#include <iomanip>
using namespace std;
long double u, v;
int main(){
    cin >> u >> v;
    cout << fixed << setprecision(2) << (u * u + v * v) / 4.0;
    return 0;
}
```
Cách 02: sử dụng kiểu dữ liệu *unsigned long long*
```
#include <iostream>
using namespace std;
unsigned long long u, v;
string mod[4]={".00",".25",".50",".75"};
int main()
{
    cin>>u>>v;
    cout<<(u*u+v*v)/4<<mod[(u*u+v*v)%4];
    return 0;
}
```

