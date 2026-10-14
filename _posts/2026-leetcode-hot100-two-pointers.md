---
layout: post
title: "leetcode-hot100-双指针-C语言"
date: 2026-10-01 18:00:00 +0800
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

解题思路：指针cur用来遍历数组，指针target用来标识非零元素即将出现的位置，cur遇到非零元素就将该元素交换至target所指位置，然后cur和target同时右移，cur遇到零则只有cur右移。

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
