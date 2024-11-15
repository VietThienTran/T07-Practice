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

**B. KHO BÁU**
Mã nguồn tham khảo
```
#include <iostream>
#include <algorithm>
#include <set>
using namespace std;
int n, S, dem;
int main() 
{
	cin>>n>>S;
	int s=S*2;
	set<pair<int,int> > checktree;
	pair<int,int> tree[n];
	for(int i=0;i<n;i++) {
		cin>>tree[i].first>>tree[i].second;
		checktree.insert(tree[i]);
	}
	sort(tree,tree+n);
	for(int i=0;i<n;i++) 
		for(int j=i+1;j<n;j++) 
			if (tree[i].first==tree[j].first) {
				int s1=abs(tree[i].second-tree[j].second);
				if (s%s1==0) 
                {
					int s2=s/s1;
					pair<int,int> o;
					o.first=tree[i].first+s2;
					o.second=tree[i].second;
					if(checktree.find(o)!=checktree.end())  dem++;	
					o.second=tree[j].second;
					if(checktree.find(o)!=checktree.end())  dem++;
					o.first=tree[i].first-s2;
					if(checktree.find(o)!=checktree.end())  dem++;
					o.second=tree[i].second;
					if(checktree.find(o)!=checktree.end())  dem++;
				}
			}
			else break;
	cout<<dem;
	return 0;
}
```

**C. BỂ XĂNG**
```
#include <iostream>
#include <algorithm>
#include <stack>
using namespace std;
const int maxn = 1e5 + 5;
int n, m, q, l, r, v[maxn], h[maxn], tmp;
long long K, Vl[maxn], Vr[maxn], T[maxn];
stack<int> st;
int main () 
{
    ios_base::sync_with_stdio(false), cin.tie(nullptr), cout.tie(nullptr);
    cin >> n >> m;
    v[n+1] = m,    h[0] = h[n+1] = 1e9;
    for(int i=1;i<=n;i++)   cin>>v[i];
    for(int i=1;i<=n;i++)   cin>>h[i];  
    for(int i=1;i<=n;i++)   T[i]=T[i-1]+h[i];
    st.push(0);
    for(int i=1;i<=n;i++)  {
        while (h[st.top()] < h[i]) st.pop();
        tmp = st.top(), st.push(i);
        Vl[i] = Vl[tmp] + (long long)(v[i] - v[tmp]) * h[i] - T[i] + T[tmp];
    }
    st = *new stack<int>;
    st.push(n+1);
    for(int i=n;i>0;i--) {
        while (h[st.top()] < h[i]) st.pop();
        tmp = st.top(), st.push(i);
        Vr[n + 1 - i] = Vr[n + 1 - tmp] + (long long)(v[tmp] - v[i] - 1) * h[i] - T[tmp - 1] + T[i];
    }
    cin>>q;
    while (q--) {
        cin >> K;
        l = lower_bound(Vl + 1, Vl + n + 1, K) - Vl - 1;
        r = lower_bound(Vr + 1, Vr + n + 1, K) - Vr - 1;
        cout << min(n, l + r) << '\n';
    }
    return 0;
}
```
**D. XÂU ĐẸP**
```
#include <bits/stdc++.h>
using namespace std;
int n, q, l, r; 
string s;
bool c_c[1000002][30];
int main() {
    cin >> n >> q >> s;
    for(int i=1;i<=n;i++)
    {
        for(int j=0;j<26;j++)   c_c[i][j] = c_c[i-1][j];
        c_c[i][s[i-1]-'a'] = !c_c[i][s[i-1]-'a'];
    }
    while (q--) {
        cin >> l >> r;
        int count = 0;
        for(int j=0;j<26;j++)
            if(c_c[l][j] != c_c[r+1][j]) count++;
        cout << count/2 << '\n';
    }
    return 0;
}
```
