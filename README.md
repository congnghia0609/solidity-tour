# solidity-tour
solidity-tour is a cheat sheet, quick reference to learn solidity programming  

## Quick start
### Init project
```bash
mkdir solidity-tour
cd solidity-tour
truffle -h
truffle init
npm init

or

# unbox: https://www.trufflesuite.com/boxes
truffle unbox pet-shop
## Truffle and Express.js dApp: https://www.trufflesuite.com/boxes/express-box
truffle unbox arvindkalra/express-box
## Truffle and Vue.js dApp: https://www.trufflesuite.com/boxes/eth-vue
truffle unbox DOkwufulueze/eth-vue
```

## Cheat sheet

### 1. Importing other Source Files
```go
import "filename";
import * as symbolName from "filename";
import "filename" as symbolName;
import {symbol1 as alias, symbol2} from "filename";
```

### 2. Comments
```go
// This is a single-line comment.

/*
This is a
multi-line comment.
*/
```

### 3. Data Types
```go
// 3.1. Booleans
bool b = true; // false


// 3.2. Integers
// Signed integers with step size 8 bits, range[-2**(k-1), 2**(k-1) - 1]
int i = 1; // int is an alias of int256.
int8 i8 = 8; // 8 bits
int16 i16 = 16; // 16 bits
int24 i24 = 24; // 24 bits
int32 i32 = 32; // 32 bits
...
int64 i64 = 64; // 64 bits
...
int128 i128 = 128; // 128 bits
...
int160 i160 = 160; // 160 bits
...
int224 i224 = 224; // 224 bits
...
int256 i256 = 256; // 256 bits

// Unsigned integers with step size 8 bits, range[0, 2**k - 1]
uint ui = 1; // uint is an alias of uint256.
uint8 ui8 = 8; // 8 bits
uint16 ui16 = 16; // 16 bits
uint24 ui24 = 24; // 24 bits
uint32 ui32 = 32; // 32 bits
...
uint64 ui64 = 64; // 64 bits
...
uint128 ui128 = 128; // 128 bits
...
uint160 ui160 = 160; // 160 bits
...
uint224 ui224 = 224; // 224 bits
...
uint256 ui256 = 256; // 256 bits

// minimum and maximum integer x
int x = 0;
int min = type(x).min;
int max = type(x).max;


// 3.3. Address - có 2 loại.
// address : Giữ một giá trị 20 byte = kích thước của Ethereum address.
// address payable : Giống với address, nhưng có thêm 2 thuộc tính là: transfer và send. ==> với address payable có ý nghĩa là địa chỉ mà bạn có thể gửi Ether đến, còn address thì không.

// Có thề ép kiểu ngầm định từ address payable thành address, nhưng chiều ngược lại thì phải ép kiểu tường minh qua: payable(<address>)
address payable a = address(0x123);
address b = a;
address payable c = payable(b);

// Cũng có thể ép kiểu tường minh từ address thành uint160, kiểu số nguyên integer, bytes20 và contract.

// Members of Addresses
//// balance và transfer
address payable x = address(0x123);
address myAddress = address(this);
if (x.balance < 10 && myAddress.balance >= 10) x.transfer(10);
//==> Hàm transfer sẽ fail nếu balance của contract không đủ lớn hoặc nếu Ether chuyển đi bị tài khoản nhận từ chối thì việc hoàn tiền của hàm transfer sẽ bị lỗi --> contract sẽ bị dừng với 1 exeption.

//// send : là 1 hàm cấp thấp của transfer. Nếu thực thi fail, thì contarct hiện tại sẽ không bị dừng với 1 exeption. Hàm send sẽ trả về false. --> dùng không cẩn thận sẽ loop stack dẫn đến bị tràn stack.

//// call, delegatecall và staticcall : các hàm cấp thấp này được cung cấp để làm việc trực tiếp với contract khi không theo chuẩn ABI. --> không nên dùng.


// 3.4. Contract : có biễu diễn dữ liệu giống kiểu address
//// contract có thể được ép kiểu ngầm định thành contract mà nó kế thừa.
//// contract có thể được ép kiểu tường minh thành address, và ngượi lại.
//// contract chỉ có thể ép kiểu tường minh thành address payable và ngược lại, nếu contract đó có hàm receive hoặc payable fallback. Nếu contract không có hàm receive hoặc payable fallback thì để ép kiểu thành address payable ta có thể dùng: payable(address(x)).


// 3.5. Fixed-size byte arrays
bytes1, bytes2, bytes3, ..., bytes32


// 3.6. Dynamically-sized byte array
bytes b; // Dynamically-sized byte array
string s; // Dynamically-sized UTF-8-encoded string


// 3.7. String Literals and Types
string s1 = "foo";
string s2 = 'bar';
string s3 = "foo" "bar"; // == "foobar"

//// escape characters:
/**
\<newline> (escapes an actual newline)
\\ (backslash)
\' (single quote)
\" (double quote)
\n (newline)
\r (carriage return)
\t (tab)
\xNN (hex escape, see below)
\uNNNN (unicode escape, see below)
*/


// 3.8. Unicode Literals
string memory a = unicode"Hello 😃";


// 3.9. Hexadecimal Literals
string h1 = hex"001122FF";
string h2 = hex'0011_22_FF';
string s3 = hex"00112233" hex"44556677"; // == hex"0011223344556677"


// 3.10. Reference Types
//// Reference Types là các kiểu tham chiếu mà giá trị sẽ được thay đổi thông qua nhiều cái tên khác nhau. 
//// Các kiểu tham chiếu bao gồm: struct, array, map.

// 3.10.1. Data location
//// Có 3 loại vị trí dữ liệu: memory, storage và calldata.
/**
memory : tồn tại ở tầm vực hàm function.
storage : tồn tại ở tầm vực contract.
calldata : tồn tại ở tham số của hàm function arguments.
*/

// Data location and assignment behaviour
/**
- Phép gán giữa storage và memory (hoặc calldata) luôn tạo 1 bản sao độc lập.
- Phép gán từ memory đến memory tạo ra 1 tham chiếu.
- Phép gán từ storage đến local storage variable tạo ra 1 tham chiếu.
- Phép gán từ những cái khác đến storage luôn tạo 1 bản sao độc lập.
*/

// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.5.0 <0.9.0;

contract C {
    // The data location of x is storage.
    // This is the only place where the
    // data location can be omitted.
    uint[] x;

    // The data location of memoryArray is memory.
    function f(uint[] memory memoryArray) public {
        x = memoryArray; // works, copies the whole array to storage
        uint[] storage y = x; // works, assigns a pointer, data location of y is storage
        y[7]; // fine, returns the 8th element
        y.pop(); // fine, modifies x through y
        delete x; // fine, clears the array, also modifies y
        // The following does not work; it would need to create a new temporary /
        // unnamed array in storage, but storage is "statically" allocated:
        // y = memoryArray;
        // This does not work either, since it would "reset" the pointer, but there
        // is no sensible location it could point to.
        // delete y;
        g(x); // calls g, handing over a reference to x
        h(x); // calls h and creates an independent, temporary copy in memory
    }

    function g(uint[] storage) internal pure {}
    function h(uint[] memory) public pure {}
}
```

### 4. Operators
```go
/**
Logical: !, &&, ||, ==, !=
Comparisons: <=, <, ==, !=, >=, >
Bit operators: &, |, ^ (xor), ~ (Đảo bít)
Shift operators: << (left shift), >> (right shift)
Arithmetic operators: +, -, *, /, % (modulo), ** (exponentiation - lũy thừa)

Operators Involving LValues: +=, -=, *=, /=, %=, |=, &= and ^=
a += e <==> a = a + e
a++ <==> a += 1
a-- <==> a-= 1
++a // tăng giá trị của a thêm 1 trước khi thực hiện hành động khác
--a // giảm giá trị của a xuống 1 trước khi thực hiện hành động khác

delete a // gán 1 giá trị khởi tạo của kiểu dữ liệu của biến cho a.
*/

// Division
int256(-5) / int256(2) == int256(-2)

// Modulo
int256(5) % int256(2) == int256(1)
int256(5) % int256(-2) == int256(1)
int256(-5) % int256(2) == int256(-1)
int256(-5) % int256(-2) == int256(-1)

// Exponentiation
0**0 == 1
Thay vì dùng x**3 thì dùng x*x*x chi phí rẻ hơn.

// delete
//// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.0 <0.9.0;

contract DeleteExample {
    uint data;
    uint[] dataArray;

    function f() public {
        uint x = data;
        delete x; // sets x to 0, does not affect data
        delete data; // sets data to 0, does not affect x
        uint[] storage y = dataArray;
        delete dataArray; // this sets dataArray.length to zero, but as uint[] is a complex object, also
        // y is affected which is an alias to the storage object
        // On the other hand: "delete y" is not valid, as assignments to local variables
        // referencing storage objects can only be made from existing storage objects.
        assert(y.length == 0);
    }
}
```

### 5. Data Structure
```go

```

### 6. Loop
```go

```

### 7. Function
```go

```
