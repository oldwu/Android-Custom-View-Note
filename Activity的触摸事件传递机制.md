# Activity的触摸事件传递机制

1. 首先触发Activity的dispatchTouchEvent方法
2. 在dispatchTouchEvent方法中如果是ACTION_DOWM，则调用onUserInteraction方法
3. 接着在dispatchTouchEvent方法中会通过Activity的root View（id为content的FrameLayout），实质是ViewGroup，通过super.dispatchTouchEvent把touchevent派发给各个activity的子view，也就是我们再Activity.onCreat方法中setContentView时设置的view。
4. 若Activity下面的子view拦截了touchevent事件(返回true)则Activity.onTouchEvent方法就不会执行。

## dispatchTouchEvent方法
* 当ACTION_DOWN时，会执行onUserInteraction方法
* 接着调用getWindow().superDispatchTouchEvent()
    * 该方法实际调用的是root View(FrameLayout)的dispatchTouchEvent()，之后按照viewGroup的模式继续将事件派发


## onUserInteraction方法
