---
title: JNI智能指针
date: 2018-02-03 16:21:31
categories: [notes]
tags: [jni, c++, 指针]
---

封装JNIEnv智能指针。

<!-- more -->

# 封装智能指针类：

``` c++
class JNIEnvPtr {
public:
    JNIEnvPtr() : env_{nullptr}, need_detach_{false} {
        if (GetJVM()->GetEnv((void**) &env_, JNI_VERSION_1_6) ==
            JNI_EDETACHED) {
            GetJVM()->AttachCurrentThread(&env_, nullptr);
            need_detach_ = true;
        }
    }
    ~JNIEnvPtr() {
        if (need_detach_) {
            GetJVM()->DetachCurrentThread();
        }
    }
    JNIEnv* operator->() {
        return env_;
    }
private:
    JNIEnvPtr(const JNIEnvPtr&) = delete;
    JNIEnvPtr& operator=(const JNIEnvPtr&) = delete;
private:
    JNIEnv* env_;
    bool need_detach_;
};
```

# 智能指针类使用方式：
``` c++
// native 代码需要回调 Java 代码
NativeClass::NativeMethod() {
    JNIEnvPtr env;
    env->CallVoidMethod(instance, method, args...);
}
```

