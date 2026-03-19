#### a.
- 思路
采用分治法，每次将区间分成两个子区间寻找。基本情况是子区间只有一个元素，这时这个元素就是主元素，返回给上一层。每一层收到左右子区间的主元素后，判断是否相等，如果相等就返回该主元素，如果不等，就在自己的区间统计这两个元素有没有过半。如果都没过半（相等），就返回null
- 伪代码
```
int findME(vector<int> a,int i,int j){
	if i == j return a[i];
	mid = (i + j) / 2;
	leftME = findME(a,i,mid);
	rightME = findME(a,mid + 1,j);
	
	if leftME == rightME return leftME;
	
	leftcount = count(a,i,j,leftME);
	rightcount = count(a,i,j,rightME);
	
	n = j - i + 1;
	if leftcount > n / 2 return leftME;
	if rightcount > n / 2 return rightME;
	return null;
}
```
- 正确性
如果x是数组A的主元素，那么x也一定是A的右半部分或者左半部分的主元素。
反证法：
假设x不是A的右半部分或者左半部分的主元素。
- 左半部分长度为 $n/2$，则 $x$ 在左边出现的次数 $\leq (n/2) / 2 = n/4$。
- 右半部分长度为 $n/2$，则 $x$ 在右边出现的次数 $\leq (n/2) / 2 = n/4$。
- 那么 $x$ 在总数组中出现的次数 $\leq n/4 + n/4 = n/2$。
- 这与“$x$ 是主元素（次数 $> n/2$）”的定义矛盾。
则递归一定会找到主元素。
- 时间分析
每次将问题分为2个规模为 n/2 的子问题，每个子问题在统计上需消耗cn / 2 ，加起来就是cn。
则$T(n) = 2T(n/2) + O(n)$，由主定理，总复杂度为 **$O(n \log n)$。
#### b.
- 思路
设置一个count = 0，以及一个candidate。遍历数组中的元素：
当count == 0时，将candidate设为当前元素。count++；
count不为0时，判断当前元素与candidate是否相等，相等的话count++。不相等count--。
遍历完后得到的candidate需要再遍历一遍数组统计是否大于 n / 2，大于就return true；否则false。
- 伪代码
```
bool findME(A){
	int candidate;
	int count = 0;
	for(int i : A){
		if(count == 0) candidate = i;
		else{
			if(candidate == i) count++;
			else count--;
		}
	}
	bool res = count_candidate(A,candidate);
	return res;
}
```
- 正确性
每一次count-1就是把两个元素配对然后删除。
这样最后剩下的就是数量最多的元素。再遍历数组进行统计就可得知该元素是不是主元素。
- 复杂度
仅对数组进行了两次遍历，且每次遍历的消耗为常数时间，则时间复杂度为$O(n)$。
