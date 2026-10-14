---
layout: post
title: "leetcode-hot100-双指针-C语言"
categories: 题解更新
tags: [leetcode, hot100]
---

## 题目
- 移动零
- 盛水最多的容器
- 三数之和
- 接雨水

## 注意事项
- 处理输入输出

移动零
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
void swap(int* a, int* b) {
	int x = *a;
	*a = *b;
	*b = x;
}
void remove_zero(int* nums, int nums_size) {
	int cur = 0;
	int target = 0;
	while (cur < nums_size) {
		if (nums[cur] != 0) {
			swap(&nums[cur], &nums[target]);
			cur++;
			target++;
		}
		else {
			cur++;
		}
	}
}
int main() {
	int* nums = calloc(1000, sizeof(int));
	int x;
	int nums_size = 0;
	while (scanf("%d", &x) == 1) {
		nums[nums_size++] = x;
	}
	remove_zero(nums, nums_size);
	for (int i = 0; i < nums_size; i++) {
		printf("%d ", nums[i]);
	}
	free(nums);
	return 0;
}

解题思路：指针cur用来遍历数组，指针target用来标识非零元素即将出现的位置，cur遇到非零元素就将该元素交换至target所指位置，
然后cur和target同时右移，cur遇到零则只有cur右移。


盛水最多的容器
---c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
int Having_water(int* nums, int nums_size) {
	int left = 0;
	int right = nums_size - 1;
	int cur_water = 0;
	int maxwater = 0;
	while (left < right) {
		if (nums[left] < nums[right]) {
			cur_water = (right - left) * nums[left];
			left++;
		}
		else {
			cur_water = (right - left) * nums[right];
			right--;
		}
		maxwater = cur_water > maxwater ? cur_water : maxwater;
	}
	return maxwater;
}
int main() {
	int nums_size = 0;
	int* nums = calloc(1000, sizeof(int));
	int x;
	while (scanf("%d", &x) == 1) {
		nums[nums_size++] = x;
	}
	int ans = Having_water(nums, nums_size);
	printf("%d", ans);
	free(nums);
	return 0;
}

解题思路：由于需要盛水最多，因此优先考虑宽度最大，指针left和right初始分别指向左右边界，同时由于要盛水，只有较低一侧才可以保证盛水，
此时宽度为right-left，高度为nums[left]，cur_water实时计算盛水量，maxwater更新全局最大盛水量。


三数之和
---c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
int cmp(const void* a, const void* b) {
	return *(int*)a - *(int*)b;
}
int** Three_sum(int* nums, int nums_size, int target, int* return_size, int** return_col_size) {
	if (nums_size < 3) {
		*return_size = 0;
		return NULL;
	}
	int** ret = calloc(nums_size * nums_size, sizeof(int*));
	*return_col_size = calloc(nums_size * nums_size, sizeof(int));
	qsort(nums, nums_size, sizeof(int), cmp);
	for (int left = 0; left < nums_size - 2; left++) {
		int mid = left + 1;
		if (left > 0 && nums[left] == nums[left - 1]) {
			continue;
		}
		int right = nums_size - 1;
		while (mid < right) {
			int sum = nums[left] + nums[mid] + nums[right];
			if (sum == target) {
				ret[*return_size] = calloc(3, sizeof(int));
				ret[*return_size][0] = nums[left];
				ret[*return_size][1] = nums[mid];
				ret[*return_size][2] = nums[right];
				(*return_col_size)[*return_size] = 3;
				(*return_size)++;
				while (mid < right && nums[mid] == nums[mid + 1]) {
					mid++;
				}
				while (mid < right && nums[right] == nums[right - 1]) {
					right--;
				}
				mid++;
				right--;
			}
			else if (sum < target) {
				mid++;
			}
			else {
				right--;
			}
		}
	}
	return ret;
}
int main() {
	int* nums = calloc(1000, sizeof(int));
	int nums_size = 0;
	int x;
	while (scanf("%d", &x) == 1) {
		nums[nums_size++] = x;
	}
	int return_size = 0;
	int* return_col_size;
	int** ans = Three_sum(nums, nums_size, 0, &return_size, &return_col_size);
	for (int i = 0; i < return_size; i++) {
		for (int j = 0; j < return_col_size[i]; j++) {
			if (j > 0) {
				printf(" ");	
			}
			printf("%d", ans[i][j]);
		}
		printf("\n");
	}
	for (int i = 0; i < return_size; i++) {
		free(ans[i]);
	}
	free(ans);
	free(return_col_size);
	free(nums);
	return 0;
}

解题思路：首先需要对原数组排序，方便后续进行有序的遍历；指针left标识三元组的第一个元素，可能的下标范围为0~nums_size-3；
指针mid标识三元组的第二个元素，遍历left过程中mid初始位置为left+1，指针right标识三元组中的第三个元素，初始位置为nums_size-1；
遍历left时，mid右移、right左移转化为双指针；每次遇到符合题意的三元组就记录，并更新结果数组的大小；
left向右遍历、mid向右遍历以及right向左遍历时时如果存在重复元素，由于题目要求不重复，因此跳过。
