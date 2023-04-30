# Overview

Protocol Buffers are a language-neutral, platform-neutral extensible mechanism for **serializing structured** data

It’s like JSON, except it’s **smaller and faster**, and it **generates native language bindings**

The format is suitable for both ephemeral network traffic and long-term data storage.



How Protobuf works:

![img](https://protobuf.dev/images/protocol-buffers-concepts.png)

Required is Forever: Protobuf forbids the use of required field starting proto 3. **Semantics for required fields should be implemented at the application layer instead**.

After setting optionality and field type, you assign a field number. **Field numbers cannot be repurposed or reused**. If you delete a field, you should reserve its field number to prevent someone from accidentally reusing the number