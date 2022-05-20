# C#Leetcode解题

## 1.Two Sum

>给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。
>
>你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
>
>你可以按任意顺序返回答案。
>
>示例 1：
>
>输入：nums = [2,7,11,15], target = 9
>输出：[0,1]
>解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
>示例 2：
>
>输入：nums = [3,2,4], target = 6
>输出：[1,2]
>示例 3：
>
>输入：nums = [3,3], target = 6
>输出：[0,1]

### code：

#### - 暴力双循环

```c++
int[] TwoSum(int[] nums, int target)
{
    var targetA = 0;
    var targetB = 0;
    for (int i = 0; i < nums.Length; i++)
    {
        var value = nums[i];
        for (int j = i+1; j < nums.Length; j++)
        {
            if (value+nums[j] == target)
            {
                targetA = i;
                targetB = j;
            }
        }
    }
   return new int[] { targetA, targetB};
}
```

简单粗暴 循环第一个数后 再循环后面的数 如果加起来等于目标值的话

#### - 用字典来进行追踪

```c#
int[] TwoSum2(int[] nums, int target)
{
    var dic = new Dictionary<int, int>();
    for (int i = 0; i < nums.Length; i++)
    {
        var value = target - nums[i];
        if (dic.ContainsKey(value))
        {
         return new int[] { dic[value], i};
        }
        dic[nums[i]] = i;

    }
    return new int[] {-1, -1};
}
```

我们创建一个字典，用减法来查找对应的值

这里为什么用减法是因为我们不一个一个循环数组做相加

targee - value = searchValue.简单的说 我们需要在字典里面 查找对应的searchValue

用字典的好处在于降低时间复杂度

双重for循环时间复杂度为 O(N^2)

用字典为O(1)