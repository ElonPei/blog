---
categories:
  - Android
created_time: February 19, 2022 9:31 AM
date: 2015/04/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: Android代码笔记
---


Android代码笔记

## 返回数据给上一个Activity

> FirstActivity：
> 

```
Intent Intent = new Intent(FirstAcitivty.this, SecondActivity.class);
startActivityForResult(intent, 1);
```

> SecondActivity:
> 

```
Intent intent = new Intent();
intent.putExtra(“date”,”hello world!”);
setResult(RESULT_OK, intent);
```

> FirstAcitvity:
> 

```
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
switch (requestCode) {
case 1:
if (resultCode == RESULT_OK) {
String returnedData = data.getStringExtra("data_return");
Log.d("FirstActivity", returnedData);
}
break;
default:
}
}
```

## 调用系统拨号

```
Intent intent = new Intent(Intent.ACTION_DIAL);
intent.setData(Uri.parse("tel:10086"));
startActivity(intent);
```

## Activity启动模式

> AndroidManifest.xml
> 

```
<activity
android:launchMode = “standard”   //默认启动模式
//singleTop    //处于栈顶的Activity启动模式是singleTop，不会再次创建实例
//singleTask   //查看栈中有是否有Activity实例，如果有不会再次创建实例
//singleInstance  //被设定为这种模式的Activity将存在于新的返回栈中
>
</activity>
```

## 统一退出所有Activity的类

```
public class ActivityCollector {
public static List<Activity> activities = new ArrayList<Activity>();
public static void addActivity(Activity activity) {
activities.add(activity);
}
public static void removeActivity(Activity activity) {
activities.remove(activity);
}
public static void finishAll() {
for (Activity activity : activities) {
if (!activity.isFinishing()) {
activity.finish();
}
}
}
}
```

## 比较好的传递数据的代码习惯

> 接收数据的Activity：
> 

```
public class SecondActivity extends BaseActivity {
public static void actionStart(Context context, String data1, String data2) {
Intent intent = new Intent(context, SecondActivity.class);
intent.putExtra("param1", data1);
intent.putExtra("param2", data2);
context.startActivity(intent);
}
}
```

> 发送数据的activity类
> 

```
button1.setOnClickListener(new OnClickListener() {
@Override
public void onClick(View v) {
SecondActivity.actionStart(FirstActivity.this, "data1", "data2");
}
});
```

## 自定义标题栏控件

```
public class TitleLayout extends LinearLayout {
public TitleLayout(Context context, AttributeSet attrs) {
super(context, attrs);
LayoutInflater.from(context).inflate(R.layout.title, this);
//找控件  设置监听
}
}
```