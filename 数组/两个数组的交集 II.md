## 两个数组的交集 II

最优时间  50ms


给定两个数组，写一个方法来计算它们的交集。

例如:
给定 nums1 = [1, 2, 2, 1], nums2 = [2, 2], 返回 [2, 2].

注意：

  *  输出结果中每个元素出现的次数，应与元素在两个数组中出现的次数一致。
   
  * 我们可以不考虑输出结果的顺序。


跟进:

* 如果给定的数组已经排好序呢？你将如何优化你的算法？

* 如果 nums1 的大小比 nums2 小很多，哪种方法更优？

* 如果nums2的元素存储在磁盘上，内存是有限的，你不能一次加载所有的元素到内存中，你该怎么办？

### 我的第一次解法 140ms
 > 思路：我是想着，用哈希表将两个数组的所有属性和出现次数记录一次，时间复杂度为m * n，然后循环某个哈希表，这样能减少循环。不过看起来，效果不佳
 
 ```
 function initObj(nums1, obj1) {
  for (let index = 0; index < nums1.length; index++) {
    const element = nums1[index];
    if (obj1[element]) {
      obj1[element].count++;
    } else {
      obj1[element] = {
        count: 1,
      };
    }
  }
}

var intersect = function(nums1, nums2) {
  let obj1 = {},
    obj2 = {},
    result = [];
  initObj(nums1, obj1);
  initObj(nums2, obj2);
  console.log(obj1);
  let obj2Array = Object.entries(obj2);
  for (let index = 0; index < obj2Array.length; index++) {
    const element = obj2Array[index];
    let testObj = obj1[element[0]];
    if (testObj !== undefined) {
      let obj1Count = testObj.count;
      let obj2Count = element[1].count;
      let actualCount = obj1Count > obj2Count ? obj2Count : obj1Count;
      for (let index = 0; index < actualCount; index++) {
        result.push(Number(element[0]));
      }
    }
  }
  return result;
};
 ```

###改进的解法：


> 思路：后来想想，不必要两个数组都弄成哈希表，只要一个弄成哈希表，另外一个遍历就好了。如果找到符合的，将count-- 就好了，少了一次循环

```
function initObj(nums1, obj1) {
  for (let index = 0; index < nums1.length; index++) {
    const element = nums1[index];
    if (obj1[element]) {
      obj1[element].count++;
    } else {
      obj1[element] = {
        count: 1,
      };
    }
  }
}

var intersect = function(nums1, nums2) {
  let obj1 = {},
    obj2 = {},
    result = [];
  initObj(nums1, obj1);
  for (let index = 0; index < nums2.length; index++) {
    const element = nums2[index];
    let testObj = obj1[element];
    if (obj1[element] && obj1[element].count > 0) {
      testObj.count--;
      result.push(element);
    }
  }
  return result;
};

```


### 如果是排序的情况 用时300ms

> 如果已经排好了序，可以根据两个不同指针互相比较，小的就前进一位，如果相等就用一个数组存储起来


```
function compareUpNumbers(a, b) {
  return a - b;
}
var intersect = function(nums1, nums2) {
  let i = 0,
    j = 0;
  let result = [];
  nums1.sort(compareUpNumbers);
  nums2.sort(compareUpNumbers);
  let nums1Length = nums1.length,
    nums2Length = nums2.length;
  if (nums1Length === 0 || nums2Length === 0) {
    return result;
  }
  let bigOne = nums1Length + nums2Length;
  for (let index = 0; index < bigOne; index++) {
    console.log(index);
    if (nums1[i] < nums2[j]) {
      i++;
    } else if (nums1[i] > nums2[j]) {
      j++;
    } else if (nums1[i] === nums2[j]) {
      result.push(nums1[i]);
      i++;
      j++;
    } else {
      console.error('出现未知判断');
    }
    if (i === nums1.length || j === nums2.length) {
      return result;
    }
  }
  return result;
};
```

> 遗留的两个问题
> 
> * 如果 nums1 的大小比 nums2 小很多，哪种方法更优？
* 如果nums2的元素存储在磁盘上，内存是有限的，你不能一次加载所有的元素到内存中，你该怎么办？