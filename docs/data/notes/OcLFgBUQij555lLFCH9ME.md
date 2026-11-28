
*Bitwise operation* - an operation done on a series of bits. Therefore, it is supported directly by the CPU and thus more efficient
- the nature of bitwise is to treat a value as a series of bits, rather than as a numerical quantity
- used to manipulate values for comparisons and calculations
	- the values must be an array of chars (string), array of ints, or a binary numeral
- ex. NOT, AND, OR, XOR
- ex. Say we perform the bitwise operation `4&1`. What happens is that a check is performed on each bit:
```
  00000011
& 00000001
__________
  00000001
```

It is *not* their main use to substitute arithmetic operations. Cryptography, computer graphics, hash functions, compression algorithms, and network protocols are just some examples where bitwise operations are extremely useful.
- It is true that in most cases when you multiply an integer by a constant that happens to be a power of two, the compiler optimises it to use the bit-shift.

*Bitmask* (or simply mask) - in the example above (with `&`) we utilized the bitwise operator to make a mask. Basically, what comes after the `&` is the mask, and it specifies which bits from the leftside will "shine through".
- the mask is determined by the bitwise operator (i.e. AND, OR XOR) and the value following it.
	- `&` (AND) will extract a subset of bits from the value
	- `|` (OR) will set a subset of the bits in the value
	```
	  00110110
	| 11010011
	__________
	  11110111	
	```
	- `^` (XOR) will toggle a subset of the bits in the value
	```
	  00110110
	^ 11010011
	__________
	  1110001	
	```
- anal. imagine a mask in Inkscape who's job is to mask anything that isn't skin. The resulting mask would be black wherever there is not skin in the photo. When we apply this mask to the photo, the resulting image would be just skin. 

*Shifting* (`<<`/`>>`) - the action of shifting all bits to the left or right. Shifting to the left effectively doubles the value. Conversely, shifting to the right divides the value by 2.
- when shifting, if there is no bit to take a place, it defaults to 0
- Declaring the first argument to be unsigned ensures that when it is right-shifted, vacated bits will be filled with zeros, not sign bits

# UE Resources
[Example: implementing a calendar in JS](https://snook.ca/archives/javascript/creative-use-bitwise-operators)