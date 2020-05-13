# Infinite Scroll

🌏 [한국어](README.md)

## Overview

Scroll Rect (Scroll View) creates items to fit in content and allows them to be reusable.

* InfiniteScroll creates elements of content with user-inserted InfiniteScrollData (or inherited) class.
    * InfiniteScroll.InsertData()
* In the Prefab which is to be used as element of content, the InfiniteScrollItem (or inherited) class must be attached before use.
    * Implement Prefab on InfiniteScrollItem.UpdateData()

## 🔨 API

Regarding how to use API, see Assets/TOAST/Kit/UI/InfiniteScroll/Sample/Scripts/InfiniteScrollSample.cs.

### InsertData

Add data as the element of content.

```cs
public void InsertData(InfiniteScrollData data)
```

### UpdateData

Update inserted data.

```cs
public void UpdateData(InfiniteScrollData data)
```

### UpdateAllData

Update all data.

```cs
public void UpdateAllData()
```

### RemoveData

Delete inserted data.

```cs
public void RemoveData(InfiniteScrollData data)
```
```cs
public void RemoveData(int dataIndex)
```

### Clear

Delete all data.

```cs
public void Clear()
```

### MoveToFirstData

Move content to the first data.

```cs
public void MoveToFirstData()
```

### MoveToLastData

Move content to the last data.

```cs
public void MoveToLastData()
```

### IsMoveToLastData

Check if content has been moved to the last data.

```cs
public bool IsMoveToLastData()
```

### MoveTo

Move content to the specified data.

```cs
public void MoveTo(InfiniteScrollData data, MoveToType moveToType)
```

```cs
public void MoveTo(int dataIndex, MoveToType moveToType)
```

### ResizeScrollView

The API is required to have Infinite Scroll deal with size changes when the size of ScrollView has been changed.

```cs
public void ResizeScrollView()
```

## 🐾 Example

Assets/TOAST/Kit/UI/InfiniteScroll/Sample

![infinitescroll_sample](images/infinitescroll_sample.gif)

