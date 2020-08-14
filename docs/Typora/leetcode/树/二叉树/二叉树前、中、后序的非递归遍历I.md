# 二叉树前、中、后序的非递归遍历

### 思路

此思路来自力扣PualKing的回答，这种思路的优点是可以通过一种统一的方式来实现三种遍历。

1. 压栈的内容是待执行的内容，并且后执行的语句需先入栈，所以进栈的顺序就决定了前中后序
1. 需要一个标志来区分每个递归调用栈（即节点是否已经访问），这里使用`null`来表示

不过在这种方法中每个节点都入栈和出栈2次，性能上是比较差的，等以后能力提高了再学些别的解法吧

### 代码实现

具体的步骤直接看后序遍历的实现，其他两种遍历方式也是相同的思路

**1.先序遍历**

```java
public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> ret=new ArrayList<>();
    if(root==null) return ret;
    Stack<TreeNode> stack=new Stack<>();
    stack.push(root);
    while(!stack.isEmpty()){
        TreeNode topNode=stack.pop();
        if(topNode!=null){
            if(topNode.right!=null){
                stack.push(topNode.right);
            }
            if(topNode.left!=null){
                stack.push(topNode.left);
            }
            stack.push(topNode);
            stack.push(null);
        }else{
            ret.add(stack.pop().val);
        }
    }
    return ret;
}
```

**2.后序遍历**

```java
public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> ret=new ArrayList<>();
    if(root==null) return ret;
    Stack<TreeNode> stack=new Stack<>();
    stack.push(root);
    while(!stack.isEmpty()){
        TreeNode topNode=stack.pop(); //弹出访问过的节点
        if(topNode!=null){
            stack.push(topNode);//在右节点之前重新插入该节点，以便在最后处理（访问值）
            stack.push(null);//null值随该节点插入，表示已经访问过，但还未被处理
            if(topNode.right!=null){
                stack.push(topNode.right);//右节点压栈
            }
            if(topNode.left!=null){
                stack.push(topNode.left);//左节点压栈
            }
        }else{
            ret.add(stack.pop().val);//第二次遇到该节点时，将该节点的值加入结果list中
        }
    }
    return ret;
}
```

**3.中序遍历**

```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> ret=new ArrayList<>();
    if(root==null) return ret;
    Stack<TreeNode> stack=new Stack<>();
    stack.push(root);
    while(!stack.isEmpty()){
        TreeNode topNode=stack.pop();
        if(topNode!=null){
            if(topNode.right!=null){
                stack.push(topNode.right);
            }
            stack.push(topNode);
            stack.push(null);
            if(topNode.left!=null){
                stack.push(topNode.left);
            }
        }else{
            ret.add(stack.pop().val);
        }
    }
    return ret;
}
```

