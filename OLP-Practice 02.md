# Olympic Tin học sinh viên - T07 Practice 02

**A. RESORT**

***_Giải thuật: Tổng tiền tố._***

Ta cần lập bảng thống kê mỗi ngày có bao nhiêu người tới nghỉ. Hoạt động mang lại hiệu quả lớn nhất sẽ tổ chức vào ngày đông khách nhất, hiệu quả hoạt động cần bố trí giảm dần theo số lượng khách.

Vì chỉ cần tổng hiệu quả nên ta có thể sắp xếp 2 mảng _(khách và hiệu quả)_ theo cùng một tiêu chí và tính tích vô hướng của 2 mảng.

***_Tổ chức dữ liệu:_***
- Mảng int $a[100002]$ – lưu hiệu quả hoạt động,
- Mảng int $b[100002]$ – lưu số lượng khách từng ngày.

***_Xử lý:_*** Đánh dấu ngày khách đến và ngày rời đi, từ đó tính tổng số lượng khách trong từng ngày theo quy tắc tích lũy tổng tiền tố

***_Độ phức tạp của giải thuật:_*** $O(nlogn)$.
```
#include <iostream>
#include <algorithm>
using namespace std;
int n, q, l, r, a[100002], b[100002];
long long ans=0;
int main() {
	cin>>n>>q;
	for(int i=1;i<=n;i++) cin>>a[i];
	while (q--){
		cin>>l>>r;
		b[l]++;
		b[r+1]--;
	}
	for(int i=1;i<=n;i++)  b[i]=b[i]+b[i-1];
	sort(a,a+n+1);
	sort(b,b+n+1);
	for(int i=0;i<=n;i++)  ans=ans+(long long)a[i]*b[i];
	cout<<ans;
	return 0;
}
```
---
**B. POTTERY**

***_Giải thuật: Tìm kiếm nhị phân_***

Gọi $x$ là thời gian hoạt động của phân xưởng nặn, $t – x$ là thời gian hoạt động của phân xưởng vẽ.

Số sản phẩm phân xưởng nặn hoàn thành: 
$res1 = \sum\limits_{i=1}^n (\frac{x}{a_i})$

Số sản phẩm phân xưởng vẽ hoàn thành: 
$res2 = \sum\limits_{i=1}^m (\frac{t-x}{b_i})$

Kết quả của bài toán là $res = min(res1, res2)$

***_Độ phức tạp của giải thuật:_*** $O(nlogn)$
```
#include <iostream>
using namespace std;
int t, n, m, a[100002], b[100002];
long long res;
int main()
{
	cin>>t>>n;
	for(int i=1;i<=n;i++)	cin>>a[i];
	cin>>m;
	for(int i=1;i<=m;i++)	cin>>b[i];
	int l=0, r=t;
	while(l<=r)
	{
		int x=(l+r)/2;
		long long res1=0, res2=0;
		for(int i=1;i<=n;i++) res1=res1+x/a[i];
		for(int i=1;i<=m;i++) res2=res2+(t-x)/b[i];
		res=max(res,min(res1,res2));
		if (res1==res2) break;
		else if (res1<res2) l=x+1;
		else r=x-1;
	}
	cout<<res;
	return 0;
}
```
---
**C. BALLOON**
***_Giải thuật: Tổ hợp_***

Tính trước các giá trị tổ hợp $C_n^k$ cần thiết. Do $n$ không quá lớn nên ta sẽ sử dụng tam giác Pascal để tính trước các giá trị. Các kết quả lấy theo modulo $10^9+7$
Với mỗi $i$ lần di chuyển, ta sẽ có ${C_{n-k+1}^{i}} * {C_{k-1}^{i-1}}$ cách sắp xếp

***_Độ phức tạp_***: $O(n^2)$
```
#include <iostream> 
using namespace std; 
const int mod=1e9+7; 
long long C[2002][2002]; 
int n, k; 
int main() 
{ 
	cin>>n>>k; 
	C[0][0]=1, C[1][0]=1, C[1][1]=1; 
	for(int i=2;i<=2000;i++) 
	{ 
		C[i][0]=1, C[i][i]=1; 
		for(int j=1;j<i;j++) 
			C[i][j]=(C[i-1][j-1]+C[i-1][j])%mod; 
	} 
	for(int i=1;i<=k;i++) cout<<C[n-k+1][i]*C[k-1][i-1]%mod<<'\n'; 
	return 0; 
}
```
