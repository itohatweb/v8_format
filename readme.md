# Explaination

Idk why I am doing this. But this "guide" explains the v8 internal binary format
used by the (V8 Engine)[https://v8.dev]

# General format

The format always starts with 2 magic bytes `0xFF 0x0F` Then it will use a
indicator byte to tell the deserializer on how to deserialize the next section
of bytes.

| Datatype       | Indicator |
| -------------- | --------- |
| string         | " (0x21)  |
| Int Signed     | I (0x49)  |
| Float          | N (0x4E)  |
| Null           | 0 (0x30)  |
| Undefined      | _ (0x5F)  |
| Array          | A (0x41)  |
| Object Literal | { (0x7B)  |

Note to self. Add binary data like Uint8Array aswell

## String format

Each string starts off with a `"` (0x21) to indicate the string datatype. Then
uses a LEB128 encoded varint to indicate the length of the data of the string.
Then the string data comes in.

serializing a string like `HelloWorld` this will look like this.

```
0xFF  0x0F    Magic bytes
0x21  0x0A    String indicator byte and varint encoded length
0x48  0x65    He
0x6C  0x6C    ll
0x6F  0x57    oW
0x6F  0x72    or
0x6C  0x64    ld
```