---
categories:
  - Android
created_time: February 19, 2022 9:31 AM
date: 2015/04/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: Android中EditText常用属性与软键盘控制
---


Android中EditText常用属性与软键盘控制

### 为EditText对应的软键盘添加关闭按钮

```
调用EditText的setImeOptions()方法
editText.setImeOptions(EditorInfo.IME_ACTION_DONE);
```

### 使EditText失去焦点

```
editText.setFocusableInTouchMode(false)；
```

---

### 强制关闭软键盘

```
InputMethodManager imm = (InputMethodManager)getActivity().getSystemService(Context.INPUT_METHOD_SERVICE);
    imm.hideSoftInputFromWindow(editText.getWindowToken(), 0);
```