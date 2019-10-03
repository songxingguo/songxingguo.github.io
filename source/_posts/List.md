title: 线性表
author: songxingguo
tags: [数据结构]
categories:

  - 编程基础
  - ''
date: 2018-09-24 21:41:00
---
## 线性表

<!-- more -->

### 笔记

![栈和队列的存储](https://graphbed.qiniu.songxingguo.com/mylist/%E6%A0%88%E5%92%8C%E9%98%9F%E5%88%97%E7%9A%84%E5%AD%98%E5%82%A8.png)

### 代码

- #### list 接口

  ```
  package mylist;

  public interface MyList<E> {

      public void add(E e);

      public void add(int index, E e);

      public void clear();

      public boolean contains(E e);

      public E get(int index);

      public int indexOf(E e);

      public int lastIndexOf(E e);

      public boolean isEmpty();

      public boolean remove(E e);

      public E remove(int index);

      public Object set(int index, E e);

      public int size();
  }
  ```
  ```
  package mylist;

  public class MyQueue<E> {
      private MyLinkedList<E> list = new MyLinkedList<>();

      public void enqueue(E e) {
          list.addLast(e);
      }

      public E dequeue() {
          return list.removeFirst();
      }

      public int getSize() {
          return list.size;
      }

      public String toString() {
          return "Queue: " + list.toString();
      }

  }
  ```
- #### 抽象线性表

  ```
  package mylist;

  public  abstract class MyAbstractList<E> implements MyList<E> {
      protected int size = 0;	

      protected MyAbstractList() {
      }

      protected MyAbstractList(E[] objects) {
          for (int i = 0; i < objects.length; i++) {
              add(objects[i]);
          }
      }

      public void add(E e) {
          add(size, e);
      }

      public boolean remove(E e) {
          if (indexOf(e) >= 0) {
              remove(indexOf(e));
              return true;
          }
          else
              return false;
      }

      public boolean isEmpty() {
          return size == 0;
      }

      public int size() {
          return size;
      }
  }
  ```
- #### 数组线性表

  ```
  package mylist;

  public class MyArrayList<E> extends MyAbstractList<E> {

      public static final int INITIAL_CAPACITY = 16;	

      private E[] data = (E[])new Object[INITIAL_CAPACITY];

      public MyArrayList() {
      }

      public MyArrayList(E[] objects) {
          for (int i = 0; i < objects.length; i++) {
              add(objects[i]); 
          }
      }

      public void add(int index, E e) {
          ensureCapacity(); 

          for (int i = size - 1; i >= index; i--)
              data[i + 1] = data[i];

          data[index] = e;

          size++;
      }

      private void ensureCapacity() {
          if (size >= data.length) {
              E[] newData = (E[])(new Object[size * 2 + 1]);
              System.arraycopy(data, 0, newData, 0, size);
              data = newData;
          }
      }

      public void clear() {
          data = (E[])new Object[INITIAL_CAPACITY];
          size = 0;
      }

      public boolean contains(E e) {
          for (int i = 0; i < size; i ++) {
              if (e.equals(data[i])) 
                  return true;
          }
          return false;
      }

      public E get(int index) {
          return data[index];
      }

      public int indexOf(E e) {
          for (int i = 0; i < size; i++) 
              if (e.equals(data[i])) return i;
          return -1;
      }

      public int lastIndexOf(E e) {
          for (int i = size - 1; i >= 0; i--) 
              if (e.equals(data[i])) return i;

          return -1;
      }

      public E remove(int index) {
          E e = data[index];

          for (int j = index; j < size - 1; j++) 
              data[j] = data[j + 1];

          data[size - 1] = null;

          size--; 

          return e;
      }

      public E set(int index, E e) {
          E old  = data[index];
          data[index]  = e;
          return old;
      }

      public String toString() { 
          StringBuilder  result = new StringBuilder("["); 

           for (int i = 0; i < size; i++) {
               result.append(data[i]);
               if (i < size - 1) result.append(", "); 
           }

           return result.toString() + "]";
      }

      public void trimToSize() {
          if (size != data.length) {
              E[] newData = (E[])(new Object[size]);
              System.arraycopy(data, 0, newData, 0, size);
              data = newData; 
          }
      }

      public int getCapacity() {
          return data.length;
      }
  }
  ```
- #### 链表

  ```
  package mylist;

  public class MyLinkedList<E> extends MyAbstractList<E> {
      private Node<E> head, tail;

      public MyLinkedList() {
      }

      public MyLinkedList(E[] objects) {
          super(objects);
      }

      public E getFirst() {
          if (size == 0) {
              return null;
          }
          else {
              return head.element;
          }
      }
      public E getLast() {
          if (size == 0) {
              return null;
          }
          else {
              return tail.element;
          }
      }
      @Override
      public E get(int index) {
          Node<E> current = toIndexNode(index);

          return current.element;
      }

      public void addFirst(E e) {
          Node<E> newNode = new Node<E>(e); 
          newNode.next = head; 
          head = newNode;  
          size++;

          if (tail == null) {
              tail = head;
          }
      }
      public void addLast(E e) {
          Node<E> newNode = new Node<E>(e);

          if (tail == null) {
              head = tail = newNode;
          }
          else {
              tail.next = newNode; 
              tail = tail.next; 
          }

          size++; 
      }
      @Override
      public void add(int index, E e) {
          if (index == 0) addFirst(e); 
          else if (index >= size) addLast(e);
          else {
              Node<E> current = toIndexNode(index);

              Node<E> temp = current.next; 
              current.next = new Node<>(e); 
              (current.next).next = temp; 
              size++;
          }

      }

      public E removeFirst() {
          if (size == 0) return null; 

          else if (size == 1) {
              Node<E> temp = head;
              head = tail = null;
              size = 0;
              return temp.element;
          }
          else {
              Node<E> temp = head;
              head = head.next;
              size--;
              return temp.element;
          }
      }
      public E removeLast() {
          if (size == 0) return null; 

          else if (size == 1) {
              Node<E> temp = head;
              head = tail = null; 
              size = 0;
              return temp.element;
          }
          else {
              Node<E> crrent = toIndexNode(size - 2); 

              Node<E> temp = tail; 
              tail = crrent;  
              tail.next = null;
              size--; 
              return temp.element;
          }
      }
      @Override
      public E remove(int index) {
          if (index == 0) return removeFirst();
          else if (index >= size) return removeLast();
          else {
              Node<E> previous = toIndexNode(index - 1);

              Node<E> current = previous.next; 
              previous.next = current.next;
              size--;
              return current.element;
          }
      }

      @Override
      public E set(int index, E e) {
          Node<E> current = toIndexNode(index);

          Node<E> temp = current;
          current.element = e; 
          return temp.element;
      }

      @Override
      public int indexOf(E e) {
          Node<E> current = head;

          for (int i = 0; i < size; i++) {
              if (current.element.equals(e)) {
                  return i;
              }
              current = current.next;
          }

          return -1;
      }
      @Override
      public int lastIndexOf(E e) {
          Node<E> current = head;
          int tempIndex = -1;

          for (int i = 0; i < size - 1; i++) {
              if (current.element.equals(e)) {
                  tempIndex = i;
              }
              current = current.next;
          }

          return tempIndex;
      }
      @Override
      public boolean contains(E e) {
          Node<E> current = head;
          for (int i = 0; i < size; i ++) {
              if (current.element.equals(e)) {
                  return true;
              }
              current = current.next; 
          }
          return false;
      }

      private Node<E> toIndexNode(int index) {
          Node<E> current = head;

          for (int i = 0; i < index; i++) {
              current = current.next;
          }

          return current;
      }

      @Override
      public void clear() {
          head  = tail = null;
      }

      //	toString
      @Override
      public String toString() {
          StringBuilder result = new StringBuilder("[");

          Node<E> current = head;
          for (int i = 0; i < size; i++) {
              result.append(current.element);
              current = current.next;
              if (current != null) {
                  result.append(", ");
              }
              else {
                  result.append("]");
              }
          }

          return result.toString();
      }

      private static class Node<E> {
          E element; 
          Node<E> next; 

          public Node(E element) {
              this.element = element;
          }
      }
  }
  ```
- #### 链表

  ```package mylist;

  public class MyLinkedListNoTail<E> extends MyAbstractList<E> {
      private Node<E> head;

      public MyLinkedListNoTail() {
      }

      public MyLinkedListNoTail(E[] objects) {
          super(objects);
      }

      public E getFirst() {
          if (size == 0) {
              return null;
          }
          else {
              return head.element;
          }
      }
      public E getLast() {
          if (size == 0) {
              return null;
          }
          else {
              return toIndexNode(size - 1).element;
          }
      }

      @Override
      public E get(int index) {
          Node<E> current = toIndexNode(index);
          return current.element;
      }

      public void addFirst(E e) {
          Node<E> newNode = new Node<E>(e); 
          newNode.next = head;
          head = newNode;  
          size++; 
      }
      public void addLast(E e) {
          Node<E> newNode = new Node<E>(e);

          if (head == null) {
              head = newNode; 
          }
          else {
              Node<E> current = toIndexNode(size - 1);
              current.next = newNode;
          }

          size++; 
      }
      @Override
      public void add(int index, E e) {
          if (index == 0) addFirst(e); 
          else if (index >= size) addLast(e); 
          else {
              Node<E> current = toIndexNode(index);

              Node<E> temp = current.next; 
              current.next = new Node<>(e); 
              (current.next).next = temp; 
              size++;
          }

      }

      public E removeFirst() {
          if (size == 0) return null; 
          else if (size == 1) {
              Node<E> temp = head;
              head = null;
              size = 0;
              return temp.element;
          }
          else {
              Node<E> temp = head;
              head = head.next;
              size--;
              return temp.element;
          }
      }
      public E removeLast() {
          if (size == 0) return null; 
          else if (size == 1) {
              Node<E> temp = head;
              head = null; //±äÎª¿Õ
              size = 0;
              return temp.element;
          }
          else {
              Node<E> current = toIndexNode(size - 2); 
              Node<E> temp = current.next;
              current.next = null;
              size--;
              return temp.element;
          }
      }

      @Override
      public E remove(int index) {
          if (index == 0) return removeFirst();
          else if (index >= size) return removeLast();
          else {
              Node<E> previous = toIndexNode(index - 1);

              Node<E> current = previous.next; 
              previous.next = current.next;
              size--;
              return current.element;
          }
      }

      @Override
      public E set(int index, E e) {
          Node<E> current = toIndexNode(index);

          Node<E> temp = current;
          current.element = e; 
          return temp.element;
      }

      @Override
      public int indexOf(E e) {
          Node<E> current = head;

          for (int i = 0; i < size; i++) {
              if (current.element.equals(e)) {
                  return i;
              }
              current = current.next;
          }

          return -1;
      }

      @Override
      public int lastIndexOf(E e) {
          Node<E> current = head;
          int tempIndex = -1;

          for (int i = 0; i < size  - 1; i++) {
              if (current.element.equals(e)) {
                  tempIndex = i;
              }
              current = current.next;
          }

          return tempIndex;
      }
      @Override
      public boolean contains(E e) {
          Node<E> current = head;

          for (int i = 0; i < size; i ++) {
              if (current.element.equals(e)) {
                  return true;
              }
              current = current.next;
          }
          return false;
      }

      private Node<E> toIndexNode(int index) {
          Node<E> current = head;

          for (int i = 0; i < index; i++) {
              current = current.next;
          }

          return current;
      }

      //toString()
      @Override
      public String toString() {
          StringBuilder result = new StringBuilder("[");

          Node<E> current = head;
          for (int i = 0; i < size; i++) {
              result.append(current.element);
              current = current.next;

              if (current != null) {
                  result.append(", ");
              }
              else {
                  result.append("]");
              }

          }

          return result.toString();
      }

      //Çå¿Õ
      @Override
      public void clear() {
          head  = null;
      }

      private static class Node<E> {
          E element; 
          Node<E> next; 

          public Node(E e) {
              this.element = e;
          }
      }
  }
  ```
- #### 队列

  ```
  package mylist;

  public class MyQueueByInherit<E> extends MyLinkedList<E> {
      public MyQueueByInherit() {
          super();
      }

      public void enqueue(E e) {
          this.addLast(e);
      }

      public E dequeue() {
          return this.removeFirst();
      }

      public int getSize() {
          return this.size;
      }

      public String toString() {
          return "Queue: " + this.toString();
      }	
  }
  ```
  ```
  package mylist;

  public class MyStack<E> {
      private MyLinkedList<E> list = new MyLinkedList<E>();

      public void push(E e) {
          list.addLast(e);
      }

      public E pop() {
          return list.removeLast();
      }

      public E peek() {
          return list.get(list.size() - 1);
      }

      public int getSize() {
          return list.size;
      }

      public String toString() {
          return "Stack: " + list.toString();
      }

  }
  ```
- #### 测试

  ```
  package mylist;
  
  import org.omg.CosNaming.NamingContextExtPackage.AddressHelper;
  
  public class TestMyList {
      public static void main(String[] args) {
          testMyLinkedList();
      }
  
      public static void testReMyLinkedList() {
          ReMyLinkedList<Integer> list = new ReMyLinkedList<>();
  
          list.add(0, 1);
          list.add(0, 2);
          list.add(0, 3);
          list.add(0, 4);
          list.add(0, 5);
          list.add(0, 3);
          list.addLast(90);
  
          System.out.println(list.toString());
  
          list.removeFirst();
          System.out.println(list.toString());
  
          list.removeLast();
          System.out.println(list.toString());
  
          System.out.println(list.lastIndexOf(3));
  
          System.out.println("element " + list.get(3));
      }
  
      public static void testiMyLinkedList() {
          MyLinkedList<Integer> list = new MyLinkedList<>();
  
          list.addFirst(30);
          list.addLast(90);
          System.out.println(list.toString());
  
          list.add(1, 40);	
          list.add(2, 50);
          System.out.println(list.toString());
      }
  ```


      public static void testMyLinkedList() {
  //		MyLinkedListNoTail<Integer> list = new MyLinkedListNoTail<>();
          MyLinkedList<Integer> list = new MyLinkedList<>();

          System.out.println();
          System.out.println();
          System.out.println("Ìí¼ÓÔªËØ");
          System.out.println(list.toString());
          list.add(0, 1);
          System.out.println(list.toString());
          System.out.println("100Ìí¼ÓÔÚµÚÒ»¸ö£º");
          list.addFirst(100);
          System.out.println(list.toString());
          list.add(0, 2);
          list.add(0, 99);
          list.add(0, 4);
          list.add(0, 5);
          list.add(0, 6);
          System.out.println(list.toString());
          System.out.println("100Ìí¼Ó×îºó");
          list.addLast(100);
          System.out.println(list.toString());
    
          System.out.println();
          System.out.println("»ñÈ¡ÔªËØ");
          System.out.println(list.toString());
          System.out.println("µÚÒ»¸öÔªËØ" +  list.getFirst());
          System.out.println("×îºóÒ»¸öÔªËØ" + list.getLast());
          System.out.println("ÏÂ±êÎª3µÄÔªËØ" + list.get(3));
    
          System.out.println();
          System.out.println();
          System.out.println("ÒÆ³ýÔªËØ");
          System.out.println(list.toString());
          System.out.println("ÒÆ³ýµÚÒ»¸öÔªËØ");
          list.removeFirst();
          System.out.println(list.toString());
          System.out.println();
          System.out.println(list.toString());
          System.out.println("ÒÆ³ý×îºóÒ»¸öÔªËØ");
          list.removeLast();
          System.out.println(list.toString());
          System.out.println();
          System.out.println(list.toString());
          System.out.println("ÒÆ³ýÏÂ±êÎª1µÄÔªËØ");
          list.remove(1);
          System.out.println(list.toString());
    
          System.out.println();
          System.out.println();
          System.out.println("ÐÞ¸ÄÔªËØ");
          System.out.println(list.toString());
          System.out.println("½«ÏÂ±êÎª1µÄÔªËØ¸ÄÎª100");
          list.set(1, 100);
          System.out.println(list.toString());
    
          System.out.println();
          System.out.println();
          System.out.println("²éÑ¯ÔªËØ");
          System.out.println(list.toString());
          System.out.println("µÚÒ»Æ¥ÅäµÄ100:" + list.indexOf(100));
          System.out.println();
          System.out.println("×îºóÆ¥ÅäµÄ100:" + list.lastIndexOf(100));
          System.out.println("ÊÇ·ñ°üº¬100:" + list.contains(100));
    
          System.out.println("Çå¿Õ");
          list.clear();
  //		System.out.println(list.toString());

      }
  }
  ```
## 中缀表达式转后缀表达式

### 中缀转后缀

- 遇到操作数直接输出。
- 栈为空，遇到操作符入栈。
- 如果栈不为空，遇到操作符与栈内元素比较，弹出优先级高的操作符直	到发现优先级较低的操作符，再将操作符入栈。（操作符中，+-优先级最低，（）优先级最高。）
- 遇到左括号将其入栈。
- 遇到右括号，执行出栈操作直到遇到左括号（左括号弹出但不输出）。
- 如果读到输入的末尾，将栈元素弹出直到该栈变成空栈，将符号写到输出中。

判断转化是否正确：运算数顺序不变，先来先算。

### 后缀的计算

- 遇到操作数压栈。
- 遇到操作符，弹出栈顶两个元素，压入结果。
- 如果读到输入的末尾，弹出最后结果。

注意：同时弹出栈的两个数，第一个为除数（减数），第二个为被除数（被减数）；

### 代码

  ```
package mylist;

import java.util.Scanner;
/*
 * 中缀表达式转后缀表达式
    */
    public class InfixToPostfix {

    //输入表达式
    public static MyQueue<String> inputExpression() {
    	MyQueue<String> queue = new MyQueue<String>();
    	Scanner input = null;
    	boolean isContinue = true;
    	try {
    		input = new Scanner(System.in);
    		System.out.println("输入表达式：");
    		String string = input.nextLine();
    		
    		while(isContinue) {
    			String[] strings = string.split(" ");


    			
    			for (String e: strings) {
    				if (isOperationChar(e)) {
    					queue.enqueue(e);
    				}
    				else {
    					queue.clear();
    					System.out.println("输入有误！请重新输入：");
    					string = input.nextLine();
    					continue;
    				}
    			}
    			
    			isContinue = false;
    		}
    	}
    	finally {
    		input.close();
    	}
    	
    	return queue;
    }
    
    //中缀转后缀
    public static MyQueue<String> infixToPostfix(MyQueue<String> queue) {
    	String e = null;
    	MyStack<String> stack = new MyStack<String>();
    	System.out.println(queue.getSize());
    	while (queue.getSize() != 0) {
    		e = queue.dequeue();
    		System.out.print(e + "$");
    		
    		if (isOperand(e)) {
    			queue.enqueue(e);
    		}
    		else if (isOperator(e)){
    			if (stack.getSize() == 0) {
    				stack.push(e);
    			}
    			else {
    				if(isHighPriority(e, stack.peek())) {
    					stack.pop();
    					stack.push(e);
    				}
    				else {
    					stack.push(e);
    				}
    			}
    		}
    		else if (isLeftBracket(e)) {
    			stack.push(e);
    		}else if (isRightBracket(e)) {
    			while (isLeftBracket(stack.pop())) {
    				queue.enqueue(stack.pop());
    			}
    		}
    	}
    	
    	return queue;
    }
    
    //打印表达式
    public static void printExpression(MyQueue<String> queue) {
    	StringBuilder string = new StringBuilder();
    	
    	while (queue.getSize() != 0) {
    		string.append(queue.dequeue() + " ");
    	}
    	
    	System.out.println(string.toString());
    }
    
    //是否是操作符
    private static boolean isOperator(String e) {
    	if (isMulAndDiv(e) || isAddAndSub(e)) {
    		return true;
    	}
    	
    	return false;
    }


    //是否是操作数
    private static boolean isOperand(String e) {
    	if (e.matches("[0-9]{1}")) {
    		return true;
    	}
    	
    	return false;
    }
    
    //是否是左括号
    private static boolean isLeftBracket(String e) {
    	if (e.equals("(")) {
    		return true;
    	}
    	return false;
    }
    
    //是否是右括号
    private static boolean isRightBracket(String e) {
    	if (e.equals(")")) {
    		return true;
    	}
    	return false;
    }
    
    private static boolean isOperationChar(String e) {
    	if(isOperator(e) || isOperand(e) 
    			|| isLeftBracket(e) || isRightBracket(e)) {
    		return true;
    	}
    	
    	return false;
    }
    
    //是否是最高优先级
    private static boolean isHighPriority(String char1, String char2) {
    	if (isMulAndDiv(char1)) {
    		return true;
    	}
    	else if (isAddAndSub(char1) && isAddAndSub(char2)) {
    		return true;
    	}
    	
    	return false;
    }


    private static boolean isAddAndSub(String e) {
    	if (e.equals("+") || e.equals("-")) {
    		return true;
    	}
    	return false;
    }
    
    private static boolean isMulAndDiv(String e) {
    	if (e.equals("*") || e.equals("/")) {
    		return true;
    	}
    	return false;
    }
    }
```

```