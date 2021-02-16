# [1049 Counting Ones (30 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805430595731456)
题目意思很简单，就是要你计算从1到N中有多少个1。


当然不可能枚举计算，不然就不可能30分了。


计算方法参考该CSDN[博客](https://blog.csdn.net/yi_Afly/article/details/52012593)


我们可以逐一统计每个数位上出现1的次数，我们设当前数位为weight，从左往右第一个到当前数位的前一个为round，从当前数位后一个到末尾为former。


<div align=center><img src="https://img-blog.csdn.net/20160726125148698" /></div>

###### <div align=center> （当我们在3这个数位上，round就是5， former就是4）</div>
就拿本例534说：
* 当我们在数位在4上面的时候，round是53：
	* 若weight为0：weight出现1的次数是round * 1（即数据为530，round从0到53，每次round变化weight都有1出现，所以一共出现了round*10次）
	* 若weight为1： weight出现1的次数是round * 1 + 1（即数据为531，在上个条件下，还多了个当round为53的情况下，weight为1的情况
 	* 若weight大于1： weight出现1的次数仍然是round * 1 + 1（即数据为534，同上）
	
本例中weight为4，故出现round * 1 + 1 = 54次

* 当我们在数位在3上面的时候，round是5，former是4：
	* 若weight为0：weight出现1的次数是round * 10（即数据为504，当round变化一次且weight从0变为1的时候，former变化10次）
	* 若weight为1： weight出现1的次数是round * 10 + former + 1（即数据为514，除此之外，当round变成5的时候，weight为1的时候，还可以从510变化到514，一共former + 1个）
 	* 若weight大于1： weight出现1的次数是round * 10 + 10（即数据为534，在第一次的情况以外，还有510~519的10个）
	
本例中weight为3，故出现round * 10 + 10 = 60次

* 当我们在数位在5上面的时候，round是0，former是34：
	* 若weight为0：weight出现1的次数是round * 100（即034，无）
	* 若weight为1： weight出现1的次数是round * 100 + former + 1（即134，当weight从0变为1的时候，former变化100次，但此时weight都是0，只有当weight变成1的时候，才算，所以是100~134的范围）
 	* 若weight大于1： weight出现1的次数是round * 100 + 100（即534，在第一次的情况以外，还有100~199的100个）
本例中weight为5，故出现round * 100 + 100 = 100次
故一起214次

归纳得：
* 对于每个数位：
	* 若weight为0：weight出现1的次数是round * base
	* 若weight为1： weight出现1的次数是round * base + former + 1
 	* 若weight大于1： weight出现1的次数仍然是round * base + base
	
其中，base一开始为1，每左移一位后乘以10

#### 代码：
~~~C++
#include <bits/stdc++.h>
using namespace std;
int ans;
int main()
{
	int n;
	cin >> n;
	int base = 1, former = 1;//former本来为0，懒得算former+1就直接赋值1
	while (n)
	{
		ans += n / 10 * base + (((n % 10) > 1) ? base: (((n % 10) == 1) ? former: 0));
		former += (n % 10) * base;
		n /= 10;
		base *= 10;
	}
	cout << ans << endl;
	return 0;
}
~~~

