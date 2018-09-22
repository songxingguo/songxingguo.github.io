title: 排序算法
author: songxingguo
tags: []
categories:
  - 数据结构
date: 2018-08-02 05:08:00
---

## 排序

- 是否稳定：数据之间发生跳跃式交换，则不稳定；反之，则稳定。（通常用两个相同的数据来判定对否稳定）

- 基本有序时的效率：

  冒泡排序 > 插入排序 >堆排序（好坏无影响）>快速排序	（平均性能最好）
  
<!-- more -->

### 插入排序

#### 笔记

![插入排序](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F.jpg)

#### 代码

```java
package mysort;

public class InsertSort {
	public static<E extends Comparable<E>> void insertSort(E[] datas) {
		System.out.println("ÅÅÐò¹ý³Ì£º");
		
		for (int i = 0; i < datas.length; i++) {
			printArray(datas);
			
			for (int j = i; j > 0; j--) {
				if(datas[j].compareTo(datas[j - 1]) < 0) {
					E temp = datas[j];
					datas[j] = datas[j - 1];
					datas[j - 1] = temp;
				}
			}
		}
	}
	
	public static <E> void printArray(E[] datas) {
		for (int i = 0; i < 10; i++) {
			System.out.print(datas[i] + " ");
		}
		System.out.println();
	}
	
	//测试InsertSort
	public static void main(String[] args) {
		Integer[] numbers = new Integer[10];
		
		for (int i = 0; i < 10; i++) {
			numbers[i] = (int)(Math.random() * 100 + 1);
		}
		
		System.out.println("ÅÅÐòÇ°£º");
		printArray(numbers);
		
		insertSort(numbers);
		
		System.out.println("ÅÅÐòºó£º");
		printArray(numbers);
	}
}
```

### 希尔排序

希尔排序（又称缩小增量排序）:
       
  - 是插入排序的一种。
  - 分组进行直接插入排序。

### 冒泡排序

#### 笔记

![冒泡排序](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F.jpg)

#### 代码

```
package mysort;

public class BubbleSort {
	
	public static<E extends Comparable<E>> void bubbleSort(E[] datas) {
		System.out.println("ÅÅÐò¹ý³Ì£º");
		
		for (int i = 0; i < datas.length; i++) {
			printArray(datas);
			
			for (int j = 0; j < datas.length - i - 1; j++) {
				if (datas[j + 1].compareTo(datas[j])  < 0) {
					E temp = datas[j];
					datas[j] = datas[j + 1];
					datas[j + 1] = temp;
				}
			}
		}
	}
	
	public static <E> void printArray(E[] datas) {
		for (int i = 0; i < 10; i++) {
			System.out.print(datas[i] + " ");
		}
		System.out.println();
	}
	
	//²âÊÔSelectSort
	public static void main(String[] args) {
		Integer[] numbers = new Integer[10];
		
		for (int i = 0; i < 10; i++) {
			numbers[i] = (int)(Math.random() * 100 + 1);
		}
		
		System.out.println("ÅÅÐòÇ°£º");
		printArray(numbers);
		
		bubbleSort(numbers);
		
		System.out.println("ÅÅÐòºó£º");
		printArray(numbers);
	}
}

```

### 选择排序

#### 笔记

![选择排序](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F.jpg)

#### 代码

```java
package mysort;

public class SelectSort {
	
	public static <E extends Comparable<E>> void selectSort(E[] datas) {
		System.out.println("ÅÅÐò¹ý³Ì£º");
		
		for (int i = 0; i <= datas.length; i++) {
			int minI = minIndex(datas, i);
			printArray(datas);
			
			if (minI != i) {
				E temp = datas[i];
				datas[i] = datas[minI];
				datas[minI] = temp;
			}
		}
		
	}
	
	//×îÐ¡ÔªËØÏÂ±ê
	private static <E extends Comparable<E>> int minIndex(E[] datas, int index) {
		int minI = index;
		
		for (int i = index + 1; i < datas.length; i++) {
			if (datas[minI].compareTo(datas[i]) > 0) {
				minI = i;
			}
		}
		
		return minI;
	}
	
	
	public static <E> void printArray(E[] datas) {
		for (int i = 0; i < 10; i++) {
			System.out.print(datas[i] + " ");
		}
		System.out.println();
	}
	
	//²âÊÔSelectSort
	public static void main(String[] args) {
		Integer[] numbers = new Integer[10];
		
		for (int i = 0; i < 10; i++) {
			numbers[i] = (int)(Math.random() * 100 + 1);
		}
		
		System.out.println("ÅÅÐòÇ°£º");
		printArray(numbers);
		
		selectSort(numbers);
		
		System.out.println("ÅÅÐòºó£º");
		printArray(numbers);
	}
}
```

### 堆排序

#### 笔记

 - #### 堆

  ![堆-书-1](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E5%A0%86-%E4%B9%A6.jpg)

  ![堆-书-1](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E5%A0%86-%E4%B9%A6-2.jpg)

- #### 创建堆

  ![创建堆](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E5%88%9B%E5%BB%BA%E5%A0%86.jpg)

- #### 堆排序

  ![创建推和堆排序](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E6%8E%A8%E7%9A%84%E5%88%9B%E5%BB%BA%E5%92%8C%E5%A0%86%E6%8E%92%E5%BA%8F.jpg)

  ![堆排序](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E5%A0%86%E6%8E%92%E5%BA%8F.jpg)

#### 代码

- ##### 堆

  ###### 最大堆
  
  ```java
  package mysort;

  public class Heap<E extends Comparable<E>> {
        private java.util.ArrayList<E> list = new java.util.ArrayList<E>();

        /** Create a default heap */
        public Heap() {
        }

        /** Create a heap from an array of objects */
        public Heap(E[] objects) {
          for (int i = 0; i < objects.length; i++)
            add(objects[i]);
        }

        /** Add a new object into the heap */
        public void add(E newObject) {
          list.add(newObject); // Append to the heap
          int currentIndex = list.size() - 1; // The index of the last node

          while (currentIndex > 0) {
            int parentIndex = (currentIndex - 1) / 2;
            // Swap if the current object is greater than its parent
            if (list.get(currentIndex).compareTo(
                list.get(parentIndex)) > 0) {
              E temp = list.get(currentIndex);
              list.set(currentIndex, list.get(parentIndex));
              list.set(parentIndex, temp);
            }
            else
              break; // the tree is a heap now

            currentIndex = parentIndex;
          }
        }

        /** Remove the root from the heap */
        public E remove() {
          if (list.size() == 0) return null;

          E removedObject = list.get(0);
          list.set(0, list.get(list.size() - 1));
          list.remove(list.size() - 1);

          int currentIndex = 0;
          while (currentIndex < list.size()) {
            int leftChildIndex = 2 * currentIndex + 1;
            int rightChildIndex = 2 * currentIndex + 2;

            // Find the minimum between two children
            if (leftChildIndex >= list.size()) break; // The tree is a heap
            int maxIndex = leftChildIndex;
            if (rightChildIndex < list.size()) {
              if (list.get(maxIndex).compareTo(
                  list.get(rightChildIndex)) < 0) {
                maxIndex = rightChildIndex;
              }
            }      

            // Swap if the current node is less than the minimum 
            if (list.get(currentIndex).compareTo(
                list.get(maxIndex)) < 0) {
              E temp = list.get(maxIndex);
              list.set(maxIndex, list.get(currentIndex));
              list.set(currentIndex, temp);
              currentIndex = maxIndex;
            }   
            else 
              break; // The tree is a heap
          }

          return removedObject;
        }

        /** Get the number of nodes in the tree */
        public int getSize() {
          return list.size();
        }
  }
  ```

  ###### 最小堆
  
  ```java
  package mysort;

  public class MinHeap<E extends Comparable<E>> {
        private java.util.ArrayList<E> list = new java.util.ArrayList<E>();

        /** Create a default heap */
        public MinHeap() {
        }

        /** Create a heap from an array of objects */
        public MinHeap(E[] objects) {
          for (int i = 0; i < objects.length; i++)
            add(objects[i]);
        }

        /** Add a new object into the heap */
        public void add(E newObject) {
          list.add(newObject); // Append to the heap
          int currentIndex = list.size() - 1; // The index of the last node

          while (currentIndex > 0) {
            int parentIndex = (currentIndex - 1) / 2;
            // Swap if the current object is greater than its parent
            if (list.get(currentIndex).compareTo(
                list.get(parentIndex)) < 0) {
              E temp = list.get(currentIndex);
              list.set(currentIndex, list.get(parentIndex));
              list.set(parentIndex, temp);
            }
            else
              break; // the tree is a heap now

            currentIndex = parentIndex;
          }
        }

        /** Remove the root from the heap */
        public E remove() {
          if (list.size() == 0) return null;

          E removedObject = list.get(0);
          list.set(0, list.get(list.size() - 1));
          list.remove(list.size() - 1);

          int currentIndex = 0;
          while (currentIndex < list.size()) {
            int leftChildIndex = 2 * currentIndex + 1;
            int rightChildIndex = 2 * currentIndex + 2;

            // Find the minimum between two children
            if (leftChildIndex >= list.size()) break; // The tree is a heap
            int maxIndex = leftChildIndex;
            if (rightChildIndex < list.size()) {
              if (list.get(maxIndex).compareTo(
                  list.get(rightChildIndex)) > 0) {
                maxIndex = rightChildIndex;
              }
            }      

            // Swap if the current node is less than the minimum 
            if (list.get(currentIndex).compareTo(
                list.get(maxIndex)) > 0) {
              E temp = list.get(maxIndex);
              list.set(maxIndex, list.get(currentIndex));
              list.set(currentIndex, temp);
              currentIndex = maxIndex;
            }   
            else 
              break; // The tree is a heap
          }

          return removedObject;
        }

        /** Get the number of nodes in the tree */
        public int getSize() {
          return list.size();
        }
  }
  ```

- ##### 堆排序

  ```java
  package mysort;

  public class HeapSort {
      public static <E extends Comparable<E>> void heapSort(E[] list) {
          MinHeap<E>  heap = new MinHeap<>();

          for (int i = 0; i < list.length; i++) {
              heap.add(list[i]);
          }

          for (int i = list.length - 1; i >= 0; i--) {
              list[i] = heap.remove();
          }
      }

      public static void main(String[] args) {
          Integer[] list = {1, 2, 5, 6, 2, 8, 1, 10};

          heapSort(list);

          for (Integer e : list) 
              System.out.print(e + " ");
      }
  }
  ```

### 归并排序

#### 笔记

![归并排序](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E5%BD%92%E5%B9%B6%E6%8E%92%E5%BA%8F.jpg)

#### 代码

```java
package mysort;

public class MergeSort {
   public static void mergeSort(int[] list) {
	   if (list.length > 1) {
		   //²ð·Ö
		   int[] firstHalf = new int[list.length / 2];
		   System.arraycopy(list, 0, firstHalf, 0, list.length / 2);
		   mergeSort(firstHalf);
		   
		   int secondHalfLength = list.length - list.length / 2;
		   int[] secondHalf = new int[secondHalfLength];
		   System.arraycopy(list, list.length / 2, secondHalf, 0, secondHalfLength);
		   mergeSort(secondHalf);
		   
		   //¹é²¢
		   int[] temp = merge(firstHalf, secondHalf);
		   System.arraycopy(temp, 0, list, 0, temp.length);
	   }
   }

   private static int[] merge(int[] list1, int[] list2) {
	   int[] temp = new int[list1.length + list2.length];
	   
	   int current1 = 0;
	   int current2 = 0;
	   int current3 = 0;
	   
	   while (current1 < list1.length && current2 < list2.length) {
		   if (list1[current1] < list2[current2]) {
			   temp[current3++] = list1[current1++];
		   }
		   else {
			   temp[current3++] = list2[current2++]; 
		   }
	   }
	   
	   while (current1 < list1.length) {
		   temp[current3++] = list1[current1++];
	   }
	   
	   while (current2 < list2.length) {
		   temp[current3++] = list2[current2++];
	   }
	   
	   return temp;
   }
}
```

### 桶排序和基数排序

#### 笔记

- ##### 基数排序

  ![基数排序](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E5%9F%BA%E6%95%B0%E6%8E%92%E5%BA%8F.jpg)

- ##### 桶排序和基数排序

  ![桶排序和基数排序](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E6%A1%B6%E6%8E%92%E5%BA%8F%E4%B8%8E%E5%9F%BA%E6%95%B0%E6%8E%92%E5%BA%8F.jpg)

  ![桶排序和基数排序-2](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E6%A1%B6%E6%8E%92%E5%BA%8F%E4%B8%8E%E5%9F%BA%E6%95%B0%E6%8E%92%E5%BA%8F.jpg)

### 快速排序

#### 笔记

![快速排序](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F.jpg)

#### 代码

```java
package mysort;

public class QuickSort {
	public static void quickSort(int[] list) {
		quickSort(list, 0, list.length - 1);
	}
	
	private static void quickSort(int[] list, int first, int last) {
		if (last > first) {
			int pivotIndex = partition(list, first, last);
			quickSort(list, first, pivotIndex - 1);
			quickSort(list, pivotIndex + 1, last);
		}
	}

	private static int partition(int[] list, int first, int last) {
		int pivot = list[first];
		int low = first + 1;
		int high = last;
		
		while (high > low) {
			while (low <= high && list[high] <= pivot) {
				low++;
			}
			
			while (low <= high && list[high] >pivot) {
				high--;
			}
			
			if (high > low) {
				int temp = list[high];
				list[high] = list[low];
				list[low] = temp;
			}
		}
		
		while (high > first && list[high] >= pivot) {
			high--;
		}
		
		if (pivot > list[high]) {
			list[first] = list[high];
			list[high] = pivot;
			return high;
		}
		else {
			return first;
		}
	}
	
	public static void main(String[] args) {
		int[] list = {2, 3, 2, 5, 6, 1, -2, 3, 14, 12};
		System.out.println("dsjfk");
		quickSort(list);
		for (int i : list) {
			System.out.print(i + " ");
		}
	}
}
```

## 优秀博客

[十大经典排序算法（动图演示）](https://www.cnblogs.com/onepixel/articles/7674659.html)