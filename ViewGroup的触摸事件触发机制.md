# ViewGroup的触摸事件触发机制

## dispatchTouchEvent
当触摸控件时，会先调用viewGroup的该方法，并在方法中根据触摸的view的情况进行action的派发

1. 先对ACTION_DOWN进行处理，消除以往的Touch状态，并将mFirstTouchTarget设置为null，然后重置Touch的状态标示
2. 然后利用变量intercepted来记录ViewGroup是否拦截Touch事件的传递，当mFirstTouchTarget不为null的时候，intercepted设置为false
3. 检查cancel，判断是否取消touch
4. 根据ViewGroup中的View进行事件分发，preorderedList用于存储View，倒序处理

## onInterceptTouchEvent
该方法ViewGroup独有，只返回了false，拦截事件

代码如下：
```java
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        return false;
    }
```

## dispatchTransformedTouchEvent
* 在dispatchTouchEvent()中调用dispatchTransformedTouchEvent()将事件分发给子View处理
* 该方法中第三个参数为的View child
    1. 当child == null时会将Touch事件传递给该ViewGroup自身的dispatchTouchEvent()处理，即super.dispatchTouchEvent(event)
    2. 当child != null时会将Touch事件传递给View，并调用child自身的dispatchTouchEvent()处理