---
title: Android中EditText常用属性与软键盘控制
date: 2015-02-25
---


Android中EditText常用属性与软键盘控制


<!--more-->

#### 为EditText对应的软键盘添加关闭按钮

调用EditText的setImeOptions()方法

editText.setImeOptions(EditorInfo.IME_ACTION_DONE);


---

#### 默认启动数字键盘，根据属性作出内容限制

在EditText中设置inputType属性

android:inputType="numberDecimal"


---

#### 使EditText失去焦点

editText.setFocusableInTouchMode(false)；


---

#### 强制关闭软键盘

InputMethodManager imm = (InputMethodManager)getActivity().getSystemService(Context.INPUT_METHOD_SERVICE);

imm.hideSoftInputFromWindow(editText.getWindowToken(), 0);
