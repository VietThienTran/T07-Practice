
# Olympic Tin học sinh viên - T07 Practice 04
***Nguồn: OLP Tin học sinh viên Việt Nam năm 2022 - Bảng không chuyên***
> Hướng dẫn này có thể khác so với hướng dẫn do BTC đưa ra.

**A. Nghệ thuật trừu tượng**

***_Giải thuật:_*** Duyệt tuần tự trên mảng 2 chiều.

Để đơn giản, ta sẽ tưởng tượng cách ta di chuyển và nhìn các ô cột. Mỗi ô cột có thể nhìn từ 4 phía/cạnh, tuy nhiên khi di chuyển ta chỉ có thể nhìn theo 2 hướng. Do đó, ta sẽ duyệt tuần tự theo chiều từ trên xuống dưới và từ trái qua phải, sau đó đi ngược lại từ dưới lên trên và từ phải qua trái.

Điều kiện một ô có thể được nhìn thấy là chiều cao của nó không vượt quá chiều cao của các ô trong hàng/cột đã đi qua trước đó. Tại mỗi ô, ta cập nhật lại ô cao nhất mỗi cột/hàng sau khi duyệt ô đó. Nếu nhìn được ô đó thì đánh dấu lại.

Sau khi đi 2 vòng, ta tiến hành ghi nhận lại các ô đã đánh dấu.
```
#include <iostream>
using namespace std;
int n, m, res, a[1002][1002], ha[1002], co[1002], cl[1002][1002];
int main()
{
    cin>>n>>m;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)   cin>>a[i][j];
    for(int i=1;i<=n;i++)	// di tu tren xuong duoi
        for(int j=1;j<=m;j++)	// di tu trai qua phai
        {
            if (a[i][j]>ha[i] || a[i][j]>co[j])    cl[i][j]=1;	// danh dau cac o nhin duoc
            ha[i]=max(ha[i],a[i][j]);	
            co[j]=max(co[j],a[i][j]);
        }
    for(int i=1;i<=n;i++) ha[i]=0; // di xong nho khoi tao lai de di lai tu dau
    for(int j=1;j<=m;j++) co[j]=0;
    for(int i=n;i>0;i--)
        for(int j=m;j>0;j--)
        {
            if (a[i][j]>ha[i] || a[i][j]>co[j])    cl[i][j]=1;	// danh dau cac o nhin duoc
            ha[i]=max(ha[i],a[i][j]);
            co[j]=max(co[j],a[i][j]);
        }
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)	res=res+cl[i][j];
    cout << res;
    return 0;
}
```
---

**B. Cắt dán**

***_Giải thuật:_*** tạm gọi là tham lam, vì cũng không biết gọi nó là gì.

Ta nhận thấy khi 2 tứ giác lồi bằng nhau, dù có xoay ngang/dọc ra sao đi nữa thì khi sắp xếp lại độ dài các cạnh của 2 tứ giác theo chiều tăng/giảm dần, chúng sẽ tương ứng bằng nhau.

Do đó, ta chỉ việc tính độ dài các cạnh của tứ giác ban đầu và sắp xếp tăng dần theo độ dài của chúng. Với mỗi tứ giác của học sinh, ta cũng thực hiện tính độ dài các cạnh của tứ giác ban đầu và sắp xếp tăng dần theo độ dài của chúng. Nếu tất cả các cạnh đều bằng nhau thì chứng tỏ 2 tứ giác bằng nhau ---> Ghi nhận kết quả
```
#include <iostream>
#include <algorithm>
using namespace std;
int n, ans, x[5], y[5], st[5], f[5];
int dist( int u, int v)     //tinh khoang cach
{
	if (v>4)    v=1;
	return (x[u]-x[v])*(x[u]-x[v])+(y[u]-y[v])*(y[u]-y[v]);
}
int main()
{
	for(int i=1;i<=4;i++)   cin>>x[i]>>y[i];
	for(int i=1;i<=4;i++)   st[i]=dist(i,i+1);
	sort(st+1,st+1+4);
	cin>>n;
	while (n--){
		int d=0;
		for(int i=1;i<=4;i++)   cin>>x[i]>>y[i];
		for(int i=1;i<=4;i++)   f[i]=dist(i,i+1);
		sort(f+1,f+1+4);
		for(int i=1;i<=4;i++)
            if (f[i]==st[i])    d++;
		if (d==4) ans++;
	}
	cout<<ans;
    return 0;
}
```
---
**C. Quà Noel**

***_Giải thuật:_*** Duyệt đồ thị, tìm kiếm theo chiều rộng/sâu
```
#include <iostream>
#include <vector>
using namespace std;
int n, m, ans, res, visited[100002];
vector <int> a[100002];
void DFS(int u)
{
    visited[u]=0;
    res++;
    ans=ans+a[u].size();
    for(int v=0;v<a[u].size();v++)
        if (visited[a[u][v]]==1)    DFS(a[u][v]);
}
int main()
{
    cin >> n >> m;
    while (m--){
        int u, v;
        cin>>u>>v;
        a[u].push_back(v);
        a[v].push_back(u);
        visited[u]=visited[v]=1;
    }
    int sum=0;
    for(int i=1;i<=n;i++)
        if (visited[i]==1)
        {
            ans=res=0;
            DFS(i);
            sum=sum+min(res,ans/2);
        }
    cout<<sum;
    return 0;
}
```
---
**D. Cửa hàng năng lượng thông minh**

***_Giải thuật:_*** Tìm kiếm nhị phân

Ở đây anh dùng một mẹo để tìm ra giới hạn giá trị lũy thừa lớn nhất có thể của 2, 3 và 5 *(xem code để hiểu rõ)*
Đầu tiên duyệt các viên pin, lọc lấy ***_các viên pin có mức năng lượng là số lý tưởng và phù hợp giới hạn._*** sau đó sắp xếp lại chúng.
Ở mỗi truy vấn, ta chỉ việc tìm kiếm viên pin có mức năng lượng trong khoảng từ ```low``` đến ```high``` bằng giải thuật tìm kiếm nhị phân.
Có thể sử dụng các hàm ```lower_bound()``` và ```upper_bound``` trong thư viện ```<algorithm>```
```
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
int n, m, p2=536870912, p3=387420489, p5=244140625;   // cac gia tri lon nhat co the là 2^29, 3^18, 5^12
vector<int> v;
bool check(int x) 
{
    if (x!=0)
        if (p2 % x == 0 || p3 % x == 0 || p5 % x == 0)  return true;
    return false;
}
int main()
{
    cin >> n >> m;
    for(int i=1;i<=n;i++) 
    {
        int x;
        cin >> x;
        if (check(x))   v.push_back(x);
    }
    sort(v.begin(), v.end());
    for(int i=1;i<=m;i++) 
    {
        int l, r;
        cin >> l >> r;
        int x = upper_bound(v.begin(), v.end(), r) - v.begin();
        int y = lower_bound(v.begin(), v.end(), l) - v.begin();
        cout << x - y << '\n';
    }
    return 0;
}
```
---
