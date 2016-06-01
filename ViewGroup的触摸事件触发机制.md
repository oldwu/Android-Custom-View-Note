# ViewGroup的触摸事件触发机制

## dispatchTouchEvent
当触摸控件时，会先调用viewGroup的该方法，并在方法中根据触摸的view的情况进行action的派发

1. 先对ACTION_DOWN进行处理，消除以往的Touch状态，并将mFirstTouchTarget设置为null，然后重置Touch的状态标示
2. 然后利用变量intercepted来记录ViewGroup是否拦截Touch事件的传递，当mFirstTouchTarget不为null的时候，intercepted设置为false\
    mFirstTouchTarget是否为null取决于Touch事件是否被消费，为null则说明touch事件找不到可以消费的子控件
    
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
    
该方法返回值特征：

| return        | description   | mFirstTouchTarget  |
| ------------- |-------------  | :-----:            |
| true          | 事件被消费     | !null              |
| false         | 事件未被消费   | null               |

##ViewGroup触摸事件传递总结
1. Android事件派发是先传递到最顶级的ViewGroup，再由ViewGroup递归传递到View的。
2. 在ViewGroup中可以通过onInterceptTouchEvent方法对事件传递进行拦截，onInterceptTouchEvent方法返回true代表不允许事件继续向子View传递，返回false代表不对事件进行拦截，默认返回false。
3. 子View中如果将传递的事件消费掉，ViewGroup中将无法接收到任何事件。