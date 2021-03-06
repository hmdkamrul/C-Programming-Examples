# 11 Operation on bits

The C language was developed with systems programming applications in mind. System programmers frequently must get in and twiddle with the bits of particular computer words. 

In this chapter you will learn how to manipulate bits with C-programming operators, including:

* The bitwise **AND operator**.
* The bitwise inclusive **OR operator**.
* The bitwise exclusive **OR operator**.
* The **ones complement operator**.
* The **left shift operator**.
* The **right shift operator**.
* **Bit fields**.

## 11.1 The basics of bits

On most computer systems, a byte consists of eight smaller units called **bits**, which can assume a value of zero or one. A byte can be conceptualized as a string of eight **bits**:

```c
01100100
```

The rightmost bit of a byte is known as the least significant or **low-order bit**, whereas the leftmost bit is known as the most significant or **high-order bit**. To represent negative numbers, most computers used the **twos complement notation**. In this notation, the high-order bit represents the sign bit, if this bit is one, the number is negative, if this bit is zero, the number is positive. The remaining bits represent the value of the number.

The **twos complement notation** limits the range of a variable because of the sign bit, you can use the unsigned modifier to state that a variable will only contain positive numbers, this can effectively increase the range of a variable.

A convenient way to convert a negative number from decimal to binary is to first add 1 to the value, express the absolute value of the result in binary, and then complement all the bits, this means changing all the ones to zeros and all the zeros to ones.

A convenient way to convert a negative number from binary back to decimal is to first complement all of the bits (changing ones to zeros and zeros to ones), convert the result to decimal, change the sign of the result and then substract 1.

## 11.2 Bit operators

The list of C operators you can use to manipulate bits can be found in the following table:

![IMG1.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG1.PNG)

All of the operators listed in the table are **binary operators**, which means they take two operands, except for the ones complement operator, which is an unary operator. Bit operations can be performed on any type of integer value in C (`int`, `short`, `long`, `long long`, `signed`, `unsigned`) and on characters, but can't be performed on floating point values.

### 11.2.1 The bitwise AND operator

When two valuse are ANDed in C, the binary representations of the values are compared bit by bit. The truth table for this operations is as follows:

![IMG2.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG2.PNG)

So, for example, if `w1` and `w2` are defined as `short` ints, and `w1` is set equal to `25` and `w2` is set equal to `77`, then the C statement:

```c
w3 = w1 & w2;
```

can be conceptualized as follows:

![IMG3.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG3.PNG)

Do not confuse the logical AND operator (&&), which is only true if both operands are true, with the **bitwise AND operator**. The latter is frequently used for masking operations. That is, this operator can be used to easily set specific bits of a data item to zero. Binary bit operators can also be used as assignment operators like this:

```c
word &= 15;
```

Which performs the same function as:

```c
word = word & 15;
```

When working with constants, is usually more convenient to express them in hexadecimal or octal notation, depending on the size of the data. Doing this will aid readability.

### 11.2.2 The bitwise Inclusive OR operator

When two values are bitwise inclusive-ORed in C, the binary representation of the two values are once again compared bit by bit. The truth table for this operator is:

![IMG4.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG4.PNG)

So, if `w1` is an `unsigned int` equal to octal `0431` and `w2` is an `unsigned int` equal to octal `0152`, then a **bitwise inclusive-OR** of `w1` and `w2` can be written as:

```c
w3 = w1 | w2;
```

And conceptualized as:

![IMG5.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG5.PNG)

Do not confuse the bitwise ORing (|) with the logical ORing (||), the latter is used to determine if either of two logical values is true.

Bitwise Inclusive-ORing is frequently used to set some specified bits of a word to one.

### 11.2.3 The bitwise exclusive OR operator

The **bitwise exclusive OR operator**, which is often called the XOR operator, is another binary operator. The truth table is as follows:

![IMG6.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG6.PNG)

If `w1` and `w2` were set to octal `0536` and octal `0266` respectively, then the XOR prodecure can be written as:

```c
w3 = w1 ^ w2;
```

And illustrated as:

![IMG7.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG7.PNG)

One interesting property is that a value XORed with itself produces a zero value. This was used in assembly to set a variable to zero or to compare if two values were equal, however this doesn't save time in C and makes the program more obscure, so it's not recommended.

Another interesting application of the XOR is to swap the value of two variables without the need for an extra variable:

```c
i1 ^= i2;
i2 ^= i1;
i1 ^= i2;
```

### 11.2.4 The ones complement operator

This is an unary operator, and its effect is to flip the bits of its operand. The truth table for this operator is quite simple:

![IMG8.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG8.PNG)

If `w1` is a `short int`, 16 bits wide, and is set to equal `0122457`, then the **ones complement operator** can be written as follows:

```c
w1 = ~w1;
```

And conceptualized as:

![IMG10.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG9.PNG)

The **ones complement operator** should not be confused with the arithmetic minus operator (-) or with the logical negation operator (!).

The **ones complement operator**is useful when you don't now the precise bit size of the quantity that you are dealing within an operation. Its use can help make a program more portable. For example, to set the low-order bit of an int called w1 to zero, you can AND w1 with an int consisting of all 1s except for a single zero in the rightmost bit. So a statement in C such as:

```c
w1 &= 0xFFFFFFFE;
```

works fine on machines in which an integer is represented by 32 bits, if you replace the preceding statement with:

```c
w1 &= ~1;
```

w1 gets ANDed with the correct value on any machine because the ones complement of 1 is calculated and consists of as many leftmost one bits as are necessary to fill the size of an `int`.

### 11.2.5 The left shift operator:

When a **left shift operation** is performed on a value, the bits contained in the value are literally shifted to the left. Associated with this operation is the number of bits that the value is to be shifted. Bits that are shifted out through the high-order bit of the data item are lost, and zeros are always shifted in through the low-order bit of the value. For example:

```c
w1 = w1 << 2; // Can also be written as w1 <<= 2.
```

Can be conceptualized in the following images:

![IMG10.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG10.PNG)

And the second shift:

![IMG11.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG11.PNG)

Left shifting actually has the effect of multiplying the value that is shifted by two. In fact, some C compilers automatically perform multiplication by a power of two by left shifting the value the appropiate number of places because shifting is a much faster operation than multiplication on most computers.

It should be noted that C language does not produce a defined result if an attempt is made to shift a value to the left by an amount that is greater than or equal to the number of bits in the size of the data item. Shifting left a value by a negative amount will also cause an undefined result.

### 11.2.6 The right shift function

The **right shift operator** >> shifts the bits of a value to the right. Bits shifted out of the low-order bit of the value are lost. Right shifting an unsigned value always results in zeros being shifted in on the left, through the high-order bits. 

What is shifted in on the left for signed values depends on the sign of the value that is being shifted and also on how this operation is implemented on your computer system. If the sign bit is zero (meaning the value is positive), zeros are shifted in regardless of which machine you are running. If the sign bit is 1, on some machines 1s are shifted in, and on others zeros are shifted in. This former type of operation is known as an **arithmetic right shift**, whereas the latter is known as a **logical right shift**.

Never make any assumptions about whether a system implements an arithmetic or a logical right shift. A program that shifts signed values right might work correctly on one system but fail on another due to this type of assumption. 

If `w1` is set to equal `0xF777EE22`, then shifting one place to the right can be written with the statement:

```c
w1 >>= 1;
```

This can be illustrated with the following image:

![IMG12.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG12.PNG)

It should be noted that C language does not produce a defined result if an attempt is made to shift a value to the right by an amount that is greater than or equal to the number of bits in the size of the data item. Shifting right a value by a negative amount will also cause an undefined result.

## 11.3 Bit Fields

Bit operations are frequently performed on data items that contain packed information. For example, an 32 bit wide integer can contain up to 32 flags that can be used for true/false conditions. Representing each flag using a `_Bool` variable would take a byte each flag, and a lot of memory would be wasted.

Two methods are available in C that can be used to pack information together to make better use of memory. One way is to simply represent the data inside a normal int and then access the desired bits of the int using the bit operators described in the previous sections.

To describe this first method, assume you have an integer where you want to store data, particularly three flags (`f1`, `f2`, `f3`) a fourth value which ranges from `1` to `255` (`type`) and the final value is an integer which ranges from `0` to `100000` (`index`). This can be conceptualized in the following image:

![IMG13.PNG](https://raw.githubusercontent.com/dmartinezgarcia/C-Programming-Examples/master/Lesson%2011%20-%20Operation%20on%20bits/IMG13.PNG)

For example, you can set the field type to the value contained in the eight low order bits of `n` using the following statement:

```c
packed_data = (packed_data & ~(0xFF << 18)) | ((n & 0xFF) << 18);
```

And to extract the field type to an external variable:

```c
n = (packed_data >> 18) & 0xFF;
```

The second method is to use a more convenient way, using a special syntax in the structure definition that allows you to define a field of bits and assign a name to that field. Whenever the term bit fields is applied to C, it is this approach that is referenced.

You can define the same packed data item as before using this structure:

```c
struct packed_struct
{
	unsigned int	:3;
	unsigned int	f1:1;
	unsigned int	f2:1;
	unsigned int	f3:1;
	unsigned int	type:8;
	unsigned int	index:18;	
};
```

The structure `packed_struct` is defined to contain six members. The first member is not named. The `:3` specifies three unnamed bits. `f1`, `f2` and `f3` are also `unsigned int` and are one bit long each. Similarly for `type` and `index`.

The C compiler automatically packs the preceding **bit field** definitions together, now the fields of a variable defined to be of type `packed_struct` can now be referenced in the same convenient way normal structure members are referenced. For example:

```c
packed_data.type = 7;
n = packed_data.type;
i = packed_data.index / 5 + 1;
if (packed_data.f2)
	...
```

There is no guarantee whether the fields are assigned from left to right or from right to left. This should only be a problem when you are dealing with data that was created by a different program or a different machine. You can also include normal data types within a structure that contains **bit fields**:

```c
struct table_entry
{
	int				count;
	char			c;
	unsigned int	f1:1;
	unsigned int	f2:1;
};
```

Some considerations regarding bit fields:

1. They can only be declared of `int` or `_Bool` type.
2. If just `int` is used in the declaration, it's implementation dependent whether this is interpreted as a signed or unsigned value.
3. You cannot have arrays of bit fields.
4. You cannot take the address of a bit field, there is no such thing as a type "pointer to bit field".
5. Bit fields are packed into units as they appear in the structure definition, where the size of a unit is defined by the implementation and is most likely a word.
6. The C compiler does not rearrange the bit field definitions to try to optimize storage space.
7. An unnamed field of length zero can be used to force align the next field in the structure at the start of a unit boundary.
