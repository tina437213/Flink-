# 测试页面

## Managed, Key-partitioned State <a href="#thich" id="thich"></a>

### 举例：去重 <a href="#xay1a" id="xay1a"></a>

对流去重，仅保留每个key的第一条

```
// Some code
```

```
private static class Event {
  public final String key;
  public final long timestamp;
  ...
}

public static void main(String[] args) throws Exception {
  StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
  
  env.addSource(new EventSource())
    .keyBy(e -> e.key)
    .flatMap(new Deduplicate())
    .print();
  
  env.execute();
}

public static class Deduplicate extends RichFlatMapFunction<Event, Event> {
  ValueState<Boolean> seen;

  @Override
  public void open(Configuration conf) {
    ValueStateDescriptor<Boolean> desc = new ValueStateDescriptor<>("seen", Types.BOOLEAN);
    seen = getRuntimeContext().getState(desc);
  }

  @Override
  public void flatMap(Event event, Collector<Event> out) throws Exception {
    if (seen.value() == null) {
      out.collect(event);
      seen.update(true);
    }
  }
}
```

为了设置Flink的内置状态，我们需要：

* 使用一种Rich function
* 创建StateDescriptor，描述我们想要存储的数据
* 绑定本地定义的state变量到由Flink提供和维护的state上

open()在初始化过程中被调用，在这个函数开始处理任何event之前。

{% file src=".gitbook/assets/Flink流式计算简介.md" %}
