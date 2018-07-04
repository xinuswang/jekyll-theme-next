---
title: UITableView中多出来的header
date: 2016-11-17 10:04:06
tags:
- tableview
categories:
- ios
---

### UITableView异常的35高度header
如果UITableView的样式是UITableViewStyleGrouped，那么在设置它的tableViewHeader属性时，可能会遇到一些问题。

1.如果不设置tableViewHeader，那么顶部不会出现header的区域，但一旦调用
```objc
self.tableView.tableViewHeader = nil;
```
头部就会出现高度为35的一个区域。

2.即便不设置为nil,创建一个View, 但没有高度，仍旧会出现这样的情况:
```objc
self.tableView.tableViewHeader = [[UIView alloc] initWithFrame:CGRectNull];
```
<!-- more -->
### 解决方案
解决这个问题，只能设置一个高度极小的view:
```objc
self.tableView.tableViewHeader = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 0, CGFLOAT_MIN)];
```
