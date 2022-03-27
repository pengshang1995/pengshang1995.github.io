---
title: 优先队列(PriorityQueue)
date: 2018-05-08 11:53:16
categories:
  - 算法
tags:
  - 队列
---
#### 概述
优先队列（priority queue）是一种用来维护由一组元素构成的集合S的数据结构，其中每一个元素都有一个相关的值，称为关键字（key）。
**队列的定义**
队列属于先进先出型，Frist in Frist out（FIFO）
优先队列基于堆排序的方法进行实现的，堆排序每次都要进行建立最大堆，第一个元素为整个队列中的最大值，优先队列也是利用了堆排序这个性质达到优先队列中权值最大的先出的效果。
![](http://ps-blog.oss-cn-beijing.aliyuncs.com/18-5-4/77248587.jpg)
#### 优先队列的方法
1. HeapMaximum方法实现了返回最大值
2. HeapExtractMax方法实现删除队列中的最大值并返回最大值
3. HeapIncreaseKey方法实现更改某个值。
4. MaxHeapInsert方法实现将元素插入到队列队尾

#### PHP实现优先队列

```
	public function HeapMaximum($arr) {
		return $arr[0];
	}

	public function HeapExtractMax(&$arr,$length) {
		if($length < 1) {
			return false;
		}

		$max = $arr[0];
		$arr[0] = $arr[$length-1];
		$length = $length - 1;
		$this->MaxHeapify($arr,1,$length);
		return $max;
	}

	public function HeapIncreaseKey(&$arr,$i,$key) {
	    if ($key < $arr[$i]) {
	        return false;
        }
        $arr[$i] = $key;
	    $flag = $this->parent($i);
	    while ($i > 1 && $arr[$flag] < $arr[$i]) {
	        $this->swap($arr,$flag,$i);
	        $i = $this->parent($i);
        }
    }

    public function MaxHeapInsert(&$arr,$key) {
	    $length = count($arr) + 1;
	    $arr[$length] = 0;
	    $this->HeapIncreaseKey($arr,$length,$key);
    }
```
#### 基于堆排序的PHP的部分代码

```
class PriorityQueue
{
	public function __construct(&$arr) {
			$arr_length = count($arr)-1;
			$this->BuildMaxHeap($arr,$arr_length);
	}


	public function BuildMaxHeap(&$arr,$arr_length) {
			$count = count($arr)-1;
			for ($i = floor($count/2); $i >=0; $i--) {
				$this->MaxHeapify($arr,$i,$arr_length);
			}
    }
	public function MaxHeapify(&$arr,$i,$arr_length) {
        $left = $this->left($i);
        $right = $this->right($i);
        if($left <= $arr_length && $arr[$left] >= $arr[$i]) {
            $this->swap($arr,$i,$left);
            $largest = $left;
        } else {
            $largest = $i;
        }
        if ($right <= $arr_length && $arr[$right] >= $arr[$largest]) {
            $this->swap($arr,$largest,$right);
            $largest = $right;
        }
        if ($largest != $i) {
            $this->MaxHeapify($arr,$largest);
        }
	}

	public function swap(&$arr,$exist,$largest) {
		$temp = $arr[$exist];
		$arr[$exist] = $arr[$largest];
		$arr[$largest] = $temp;
	}
	private function left($i) {
		return 2*$i+1;
	}

	private function right($i) {
		return 2*$i+2;
	}
	private  function parent($i) {
	    return floor($i/2);
    }

	//以下是队列操作
}
```



