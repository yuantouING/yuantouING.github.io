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
