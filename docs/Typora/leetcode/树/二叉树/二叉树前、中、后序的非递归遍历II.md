# 二叉树前、中、后序的非递归遍历II

可以参考这篇博客，思路讲的很详细

[二叉树前序、中序、后序遍历非递归写法的透彻解析](https://blog.csdn.net/zhangxiangDavaid/article/details/37115355)

## 中序遍历

```java
public List < Integer > inorderTraversal(TreeNode root) {
    List < Integer > res = new ArrayList < > ();
    Stack < TreeNode > stack = new Stack < > ();
    TreeNode curr = root;
    while (curr != null || !stack.isEmpty()) {
        while (curr != null) {
            stack.push(curr);
            curr = curr.left;
        }
        curr = stack.pop();
        res.add(curr.val);
        curr = curr.right;
    }
    return res;
}
```

