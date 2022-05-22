# Performance

## Serializing 5000 objects on iOS 5 with an iPhone 4S

The test was performed using the following code: https://gist.github.com/3137502

### Serialization

| Serializer              | Time (s)      |
| ----------------------- | ------------- |
| BinaryFormatter         | 0.6671089     |
| BinaryWriter Explicit   | 0.1163388     |
| BinaryWriter Reflection | 0.4785611     |
| **sqlite-net**          | **0.7273159** |

### Deserialization

| Serializer              | Time (s)      |
| ----------------------- | ------------- |
| BinaryFormatter         | 1.3048520     |
| BinaryWriter Explicit   | 0.1911826     |
| BinaryWriter Reflection | 0.6832386     |
| **sqlite-net**          | **0.7749375** |

The following object was used in these tests:

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
