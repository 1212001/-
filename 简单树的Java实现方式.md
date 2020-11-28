## 树的Java实现方式

### 递归实现

```java
package com.ghostcloud.until;

public class Tree {
    Tree left;
    Tree right;
    Object data;
    public Tree(){};
    public Tree(Object o){
        this.data=o;
    }
    public Tree(Object o,Tree left,Tree right){

    }
    public static class TreeNo{
        public Tree createTree(Object[] arr,Integer index){
            if (index > arr.length){
                return null;
            }
            Object o=arr[index-1];
            if(o == null){
                return null;
            }
            Tree tree = new Tree(o);
            tree.left=createTree(arr, index * 2);
            tree.right=createTree(arr, index * 2 +1);
            return tree;
        }
        /**
         * 先序遍历
         *
         * 这三种不同的遍历结构都是一样的，只是先后顺序不一样而已
         *
         * @param node
         *            遍历的节点
         */
        public  void preOrderTraverse(Tree node) {
            if (node == null)
                return;
            System.out.print(node.data + " ");
            preOrderTraverse(node.left);
            preOrderTraverse(node.right);
        }
        /**
         * 中序遍历
         */
        public  void inOrderTraverse(Tree node) {
            if (node == null)
                return;
            inOrderTraverse(node.left);
            System.out.print(node.data + " ");
            inOrderTraverse(node.right);
        }
        /**
         * 后序遍历
         */
        public  void postOrderTraverse(Tree node) {
            if (node == null)
                return;
            postOrderTraverse(node.left);
            postOrderTraverse(node.right);
            System.out.print(node.data + " ");
        }
    }

    public static void main(String[] args) {
        Integer[] arr={1,23,4,53,2,5};
        Tree.TreeNo treeNo = new Tree.TreeNo();
        Tree tree = treeNo.createTree(arr, 1);

        //先序遍历
        System.out.println("先序遍历");
        treeNo.preOrderTraverse(tree);
        System.out.println("\n"+"中序遍历");
        treeNo.inOrderTraverse(tree);
        System.out.println("\n"+"后续遍历");
        treeNo.postOrderTraverse(tree);
    }
}

```

### 循环实现

```java
package com.ghostcloud.until;

import java.util.ArrayList;
import java.util.List;

/**
 * 树
 * @param <T>
 */
public class TreeNode<T> {
    private static List<TreeNode> nodeList = new ArrayList<>();
    public  Object[] objects;
    T node;     //原始节点
    TreeNode<T> left=null;
    TreeNode<T> right=null;
    public TreeNode(Object[] objects,T TreeNode){
        this.objects=objects;
        this.node=TreeNode;
    }
    /**
     * 构造树
     */
    public void createTree(){
        //将每个值转化为Node节点值
        for (int indexNode = 0; indexNode < objects.length; indexNode++) {
            nodeList.add(new TreeNode(null,objects[indexNode]));
        }
        // 对前lastParentIndex-1个父节点按照父节点与孩子节点的数字关系建立二叉树
        for (int parentNode = 0; parentNode < objects.length / 2 - 1; parentNode++) {
            //左
            nodeList.get(parentNode).left = nodeList.get(parentNode *2 +1);
            //右
            nodeList.get(parentNode).right = nodeList.get(parentNode * 2 +2);
        }
        //最后一个节点没有孩子
        int lastParentIndex= objects.length/2 -1 ;
        //左孩子
        nodeList.get(lastParentIndex).left=nodeList.get(lastParentIndex*2+1);
        //右孩子，奇数才会有
        if (objects.length % 2==1){
            nodeList.get(lastParentIndex).right = nodeList.get(lastParentIndex * 2 +2);
        }
    }
    /**
     * 先序遍历
     *
     * 这三种不同的遍历结构都是一样的，只是先后顺序不一样而已
     *
     * @param node
     *            遍历的节点
     */
    public  void preOrderTraverse(TreeNode node) {
        if (node == null)
            return;
        System.out.print(node.node + " ");
        preOrderTraverse(node.left);
        preOrderTraverse(node.right);
    }
    /**
     * 中序遍历
     */
    public  void inOrderTraverse(TreeNode node) {
        if (node == null)
            return;
        inOrderTraverse(node.left);
        System.out.print(node.node + " ");
        inOrderTraverse(node.right);
    }
    /**
     * 后序遍历
     */
    public  void postOrderTraverse(TreeNode node) {
        if (node == null)
            return;
        postOrderTraverse(node.left);
        postOrderTraverse(node.right);
        System.out.print(node.node + " ");
    }
    public static void main(String[] args) {
        Integer[] arr={20,23,4,53,2,5};
        TreeNode<Class<Integer>> classTreeNode = new TreeNode<>(arr, Integer.class);
        classTreeNode.createTree();
        TreeNode treeNode = nodeList.get(0);
        System.out.println("先序遍历");
        classTreeNode.preOrderTraverse(treeNode);
        System.out.println("\n"+"中序遍历");
        classTreeNode.inOrderTraverse(treeNode);
        System.out.println("\n"+"后序遍历");
        classTreeNode.postOrderTraverse(treeNode);
    }
}
```

### 数组按大小插入

```java
package com.ghostcloud.until;

public class TreeList {
    static Trees root;  //根节点
    public static class Trees{
        Object data;    //数据
        int dataNum;
        Trees left;     //左指针
        Trees right;    //右指针
        public Trees(Object data){
            this.data=data;
            this.left=null;
            this.right=null;
        }
        public Trees(int dataNum){
            this.dataNum=dataNum;
            this.left=null;
            this.right=null;
        }
        public Trees(){
            this.left=null;
            this.right=null;
        }
        //顺序插入
        public void insertNumberTree(int data){
            Trees treeList = new Trees(data);
            if (root ==null){
                root =treeList;
            }else {
                Trees parent=root;
                while (true){
                    //比头节点小放左边
                    if (data < parent.dataNum){
                        if (parent.left==null){ //左节点为空就放入此处
                            parent.left=treeList;
                            break;
                        }
                        //进入左节点
                        parent=parent.left;
                    }else { //比头节点大放右边
                        if (parent.right==null){ //右点为空就放入此处
                            parent.right=treeList;
                            break;
                        }
                        //进入右节点
                        parent=parent.right;
                    }
                }
            }
        }
        //不分类型插入
        public void insertObject(Object data){
            Trees trees = new Trees(data);
            if (root == null){
                root=trees;
            }else {
                Trees parent =root;
                    while (true){
                    //先左后右
                    if (parent.left==null){
                        parent.left=trees;
                        break;
                    }else if (parent.right==null){
                        parent.right=trees;
                        break;
                    }else {
                        parent=parent.left;
                    }
                }
            }
        }

        //批量插入
        public void numberArray(int[] arr){
            for (int i : arr) {
                insertNumberTree(i);
            }
        }
        /**
         * 前序遍历
         */
        public void prePrint(){
            System.out.print("前序遍历\t");
            if (root != null){
                pre(root);
            }
            System.out.println();
        }

        private void pre(Trees node){
            if (node != null) {
                System.out.print(node.dataNum + "\t");
                pre(node.left);
                pre(node.right);
            }
        }

        /**
         * 中序遍历
         */
        public void prePrint2(){
            System.out.print("中序遍历\t");
            if (root != null){
                pre2(root);
            }
            System.out.println();
        }

        private void pre2(Trees node){
            if (node != null) {
                pre(node.left);
                System.out.print(node.dataNum + "\t");
                pre(node.right);
            }
        }
        /**
         * 后序遍历
         */
        public void prePrint3(){
            System.out.print("后序遍历\t");
            if (root != null){
                pre3(root);
            }
            System.out.println();
        }

        private void pre3(Trees node){
            if (node != null) {
                pre(node.left);
                pre(node.right);
                System.out.print(node.dataNum + "\t");
            }
        }
    }

    public static void main(String[] args) {
        Trees trees = new Trees();
        int[] arr={20,23,4,53,2,5};
        trees.numberArray(arr);
        trees.prePrint();
        trees.prePrint2();
        trees.prePrint3();
    }
}

```

