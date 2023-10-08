## 简介

- 包含基于JVMTI开发的一些样板代码。
- 基于信号处理器使用`AsyncGetCallTrace` API完成一个简单的stack trace。

## 如何编译
```bash
cd SimpleProfiler/
mkdir build
cd build
cmake ..
make
```

## 如何运行

```bash
java -agentpath:/path/SimpleProfiler/build/libSimpleProfiler.so=interval=0.001s BasicSample
```

对应的Java代码：
```java
//BasicSample.java
public class BasicSample {

    public void waitForever() throws InterruptedException {
        System.out.print("Waiting forever...");
        for (int i = 0; i < 100; i++) {
            Thread.sleep(10);
            System.out.print(".");
        }
        System.out.println("done");
    }

    public static void main(String[] args) throws InterruptedException {
        new BasicSample().waitForever();
    }
}
```
## 结果
程序会将采样到的stack traces打印出来。同时，统计采样成功与失败的次数，计算ratio。

```bash
$ java -agentpath:/home/rain/Code/SimpleProfiler/build/libSimpleProfiler.so=interval=0.001s BasicSample

Waiting forever....................
---------------------
trace.num_frames: 13
frame 0 -> [lineno:-3, method_id:writeBytes]
frame 1 -> [lineno:16, method_id:write]
frame 2 -> [lineno:20, method_id:flushBuffer]
frame 3 -> [lineno:1, method_id:flush]
frame 4 -> [lineno:30, method_id:write]
frame 5 -> [lineno:127, method_id:writeBytes]
frame 6 -> [lineno:11, method_id:implFlushBuffer]
frame 7 -> [lineno:15, method_id:flushBuffer]
frame 8 -> [lineno:4, method_id:flushBuffer]
frame 9 -> [lineno:27, method_id:write]
frame 10 -> [lineno:5, method_id:print]
frame 11 -> [lineno:27, method_id:waitForever]
frame 12 -> [lineno:7, method_id:main]
............................................---------------------
trace.num_frames: 11
frame 0 -> [lineno:0, method_id:flushBuffer]
frame 1 -> [lineno:1, method_id:flush]
frame 2 -> [lineno:30, method_id:write]
frame 3 -> [lineno:127, method_id:writeBytes]
frame 4 -> [lineno:11, method_id:implFlushBuffer]
frame 5 -> [lineno:15, method_id:flushBuffer]
frame 6 -> [lineno:4, method_id:flushBuffer]
frame 7 -> [lineno:27, method_id:write]
frame 8 -> [lineno:5, method_id:print]
frame 9 -> [lineno:27, method_id:waitForever]
frame 10 -> [lineno:7, method_id:main]
.......................................done
Failed traces:          0
Total traces:           2
Failed ratio:        0.00%
```

## 参考
https://github.com/parttimenerd/writing-a-profiler