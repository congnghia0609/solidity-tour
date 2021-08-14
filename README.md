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


```

### 4. Operators
```go

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
