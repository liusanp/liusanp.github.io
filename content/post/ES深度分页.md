---
title: "ES深度分页"
date: 2022-06-26T11:59:58+08:00
tags: [ES, 分页]
categories: 资料
draft: false
---

ES分页查询有三种情况：

## from + size

从各分片查询（from + size）条，合并后取（from - size）条
注：（from+size）不能超过 max_result_window，默认10000条

## scroll

不适合实时查询，用于查询大批量数据遍历，游标过期会报错
所有结果缓存，类似查询时间点快照，用游标遍历
不能聚合，只有最初的查询有聚合结果
最佳排序是 _doc 入库时间
返回结果最大条数是size * 分片数

## search_after

正常查询后，将前一次查询的最后一条数据的sort放置在search_after字段查询
实时查询，排序可能会变更，并行滚动多个查询
只能一页页下翻


> 原链接：[ES深度分页](/post/ES深度分页)