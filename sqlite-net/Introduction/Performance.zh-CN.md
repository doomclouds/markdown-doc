# 性能评估

## 使用iPhone 4S在iOS 5上序列化5000个对象

测试使用以下代码执行： https://gist.github.com/3137502

### 序列化

| 序列化器                | 时间(s)       |
| ----------------------- | ------------- |
| BinaryFormatter         | 0.6671089     |
| BinaryWriter Explicit   | 0.1163388     |
| BinaryWriter Reflection | 0.4785611     |
| **sqlite-net**          | **0.7273159** |

### 反序列化

| 反列化器                | 时间 (s)      |
| ----------------------- | ------------- |
| BinaryFormatter         | 1.3048520     |
| BinaryWriter Explicit   | 0.1911826     |
| BinaryWriter Reflection | 0.6832386     |
| **sqlite-net**          | **0.7749375** |

在这些测试中使用了以下对象：

~~~c#
```
[Serializable]
class TestObject {		
	public int Id { get; set; }
	public string Name { get; set; }
	public DateTime ModifiedTime { get; set; }
	public double Value { get; set; }
	public string Text { get; set; }
	public int Last { get; set; }
}
```
~~~
