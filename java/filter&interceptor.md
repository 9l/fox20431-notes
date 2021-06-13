# 拦截器

## filter



## interceptor

AOP风格的拦截器

拦截器中的方法将按`preHandle+Controller→postHandle－afterCompletion`的顺序执行。注意，只有preHandle方法返回true时后面的方法才会执行。当拦截器链内存在多个拦截器时，postHandler在拦截器链内的所有拦截器返回成功时才会调用，而afterCompletion只有preHandle返回true才调用，但若拦截器链内的第一个拦截器的preHandle方法返回false，则后面的方法都不会执行。