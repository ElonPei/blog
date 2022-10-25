---
title: 微信支付生成10位一天内不能重复的数字工具类
date: 2019-04-01
tags:
  - Algorithm
---

在开发微信支付现金红包功能时遇到生成商户号的需要生成10位一天内不能重复的数字，如果用单纯的随机数，有可能随机数碰撞，产生相同商户号的问题，所以自己写了个工具类。


<!-- more -->


## 代码实现


```Java
    private static List<String> list = new ArrayList<>();
    private static String todayIndex = DateUtil.getSDFFDate();
    
    /**
     * 商户订单号（每个订单号必须唯一）
     * <p>
     * 组成：mch_id+yyyymmdd+10位一天内不能重复的数字
     *
     * @return
     */
    public synchronized static String generateRedPackMchBillno() {
        if (!todayIndex.equals(DateUtil.getSDFFDate())) {
            list.clear();
        }
        String mchBillno = MAC_ID + DateUtil.getSDFFDate() + getRandomNumber();
        if (list.contains(mchBillno)) {
            return generateRedPackMchBillno();
        } else {
            list.add(mchBillno);
            todayIndex = DateUtil.getSDFFDate();
        }
        return mchBillno;
    }
```


其中由于代码存在多个竟态条件，如果不采取同步，在多线程条件下会存在线程安全问题，所以方法要同步。


DateUtil部分代码如下：


```Java

    static SimpleDateFormat sdff = new SimpleDateFormat("yyyyMMdd");
    
    public static String getSDFFDate(){
        return sdff.format(new Date());
    }
    
```


## 缺陷思考


缓存的已经生成过的商户号在 JVM 重启的时候会重置，会导致有几率重复生成，所以需要把已经生成过的商户进行持久化缓存，可以借助开启持久化的 Redis 来进行缓存。



The end！
