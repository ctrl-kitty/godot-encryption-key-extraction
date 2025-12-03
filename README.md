# godot-encryption-key-extraction
guide how to extract encryption key for godot engine games (usefull for .pck unencryption)

## Godot engine
source extraction tool https://github.com/GDRETools/gdsdecomp/releases
tool for getting encryption key  - ida
#### How to get enc key?
1. Open {game}.exe with IDA. Wait before functions loads.
2. Search for text `open_and_parse`
![1.png](1.png)
function signature (on screen) + key size=32 (https://github.com/godotengine/godot/blob/master/core/io/file_access_encrypted.cpp#L44)
![[2.png]](2.png)
renamed function to original name
![[3.png]](3.png)
can see jz and jnz to our function right up here
![[4.png]](4.png)
interested in second parameter (which stored in rdx register). So, add breakpoint to first jz
screen with rdx address (encryption key starting range)
![[5.png]](5.png)
so, just jump to rdx address and copy our 32bytes key
![[6.png]](6.png)

or copy address(my is 000001D0C2394E60) and run script 
```python
ea = 0x000001D0C2394E60
size = 32
data = idaapi.dbg_read_memory(ea, size)
print(''.join(f'{b:02X}' for b in data))
```
