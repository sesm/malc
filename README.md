# malc

mal (Make A Lisp) compiler

## Object structure

`mal_val_t` is a 32-bit value which can be one of the following:

* Integer
* Constant (nil, false, true)
* Pointer to object

### Integers

(`the_int_value << 1`) & 0x1

### Constants

nil = 0x00000002
false = 0x00000004
true = 0x00000006

### Objects

12 bytes header:

```
struct mal_obj_t {
  uint32_t flags;
  uint32_t len;
  byte* data;
}
```

flags:

17 - 0x11 - symbol
18 - 0x12 - string
19 - 0x13 - keyword

 len - N number of chars in data
 data - points to char array of length N

33 - 0x21 - list
34 - 0x22 - vector
35 - 0x23 - hash-map

  len = N number of elements
  data - points to array of N `mal_val_t` entries

49 - 0x31 - atom - actually a vector of size 1.
