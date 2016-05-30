# View的触摸屏传递机制小结

## 1. onTouch和onClick


## 2. View触摸屏事件传递小结
1. 触摸控件（View）首先执行dispatchTouchEvent方法。
2. 在dispatchTouchEvent方法中先执行onTouch方法，后执行onClick方法
（onClick方法在onTouchEvent中执行，下面会分析）。
3. 如果控件（View）的onTouch返回false或者mOnTouchListener为null（控件没有设置setOnTouchListener方法）
或者控件不是enable的情况下会调运onTouchEvent，dispatchTouchEvent返回值与onTouchEvent返回一样。
4. 如果控件不是enable的设置了onTouch方法也不会执行，只能通过重写控件的onTouchEvent方法处理（上面已经处理分析了），
dispatchTouchEvent返回值与onTouchEvent返回一样。
5. 如果控件（View）是enable且onTouch返回true情况下，dispatchTouchEvent直接返回true，不会调用onTouchEvent方法。
6. onTouchEvent方法中会在ACTION_UP分支中出发onClick的监听
7. 当dispatchTouchEvent在进行事件分发的时候，只有前一个action返回true，才会触发下一个action

## 2.复写dispatchTouchEvent和onTouchEvent方法

编写一个TestButton类

复写两个方法：

```java
@Override
public boolean dispatchTouchEvent(MotionEvent event) {
    Log.i(null, "dispatchTouchEvent-- action=" + event.getAction());
    super.dispatchTouchEvent(event);
    return true;
}

@Override
public boolean onTouchEvent(MotionEvent event) {
    Log.i(null, "onTouchEvent-- action=" + event.getAction());
    super.onTouchEvent(event);
    return false;
}
```

### 1. dispatchTouchEvent:true onTouchEvent:false


