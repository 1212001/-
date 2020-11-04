## 单链表Java实现

```java
package com.ghostcloud.until;

import lombok.Data;

/**
 * 链表
 */
@Data
public class LinkedNode {
    int data;   //存放数据
    public LinkedNode next;  //指向下一个节点
    public LinkedNode(int data){
        this.data=data;
    }
    public LinkedNode(){

    }

    public static class Link{
        public LinkedNode head;  //头节点
        public int size=0;
         public Link(){
            this.head=new LinkedNode();
        }
        //判断是否为空
        public boolean isEmp(){
            return head.next==null;
        }
        public void add(int data){
            LinkedNode temp=head;
            while (temp.next!=null){
                temp=temp.next;
            }
            LinkedNode node = new LinkedNode(data);
            temp.next=node;
            node.next=null;
            size++;
        }
        //入栈式插入链表
        public void addNode(int data){
            LinkedNode node = new LinkedNode(data);
            node.next=head;
            head=node;
        }
        //删除最后元素
        public int deleteEnd(){
            LinkedNode temp=head.next;
            LinkedNode end = null;
            while (temp.next != null){
                end=temp;
                temp=temp.next;
            }
            int data=end.next.data;
            end.next=null;
            return data;
        }
        //删除指定元素
        public void deleteIndex(int data){
            LinkedNode temp=head;

             while (data != temp.next.data ){
                 if (temp.next.next==null){
                     System.out.println("没有此元素:"+data);
                     return ;
                 }
                 temp=temp.next;
             }
            temp.next=temp.next.next;
        }
    }
}

```

### 测试方法

```java
   public void Tree(){
        LinkedNode.Link link = new LinkedNode.Link();

        link.add(1);
        link.add(2);
        link.add(3);
        link.add(4);

//        link.addNode(1);
//        link.addNode(2);
//        link.addNode(3);



        System.out.println(link.deleteEnd());

        link.deleteIndex(5);
        LinkedNode node=link.head.next;
        while (node!=null){
            System.out.println(node.getData());
            node=node.next;
        }
    }
```

