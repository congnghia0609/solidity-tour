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
// address payable : Giống với address, nhưng có thêm 2 thuộc tính là: 
// transfer và send. ==> với address payable có ý nghĩa là địa chỉ mà 
// bạn có thể gửi Ether đến, còn address thì không.

// Có thề ép kiểu ngầm định từ address payable thành address, nhưng 
// chiều ngược lại thì phải ép kiểu tường minh qua: payable(<address>)
address payable a = address(0x123);
address b = a;
address payable c = payable(b);

// Cũng có thể ép kiểu tường minh từ address thành uint160, kiểu 
// số nguyên integer, bytes20 và contract.

// Members of Addresses
//// balance và transfer
address payable x = address(0x123);
address myAddress = address(this);
if (x.balance < 10 && myAddress.balance >= 10) x.transfer(10);
//==> Hàm transfer sẽ fail nếu balance của contract không đủ lớn hoặc 
// nếu Ether chuyển đi bị tài khoản nhận từ chối thì việc hoàn tiền 
// của hàm transfer sẽ bị lỗi --> contract sẽ bị dừng với 1 exeption.

//// send : là 1 hàm cấp thấp của transfer. Nếu thực thi fail, thì 
// contarct hiện tại sẽ không bị dừng với 1 exeption. Hàm send sẽ trả 
// về false. --> dùng không cẩn thận sẽ loop stack dẫn đến bị tràn stack.

//// call, delegatecall và staticcall : các hàm cấp thấp này được 
// cung cấp để làm việc trực tiếp với contract khi không theo chuẩn 
// ABI. --> không nên dùng.


// 3.4. Contract : có biễu diễn dữ liệu giống kiểu address
//// contract có thể được ép kiểu ngầm định thành contract mà nó kế thừa.
//// contract có thể được ép kiểu tường minh thành address, và ngượi lại.
//// contract chỉ có thể ép kiểu tường minh thành address payable và 
// ngược lại, nếu contract đó có hàm receive hoặc payable fallback. 
// Nếu contract không có hàm receive hoặc payable fallback thì để ép kiểu 
// thành address payable ta có thể dùng: payable(address(x)).


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
//// Reference Types là các kiểu tham chiếu mà giá trị sẽ được 
// thay đổi thông qua nhiều cái tên khác nhau. 
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
        uint[] storage y = x; // works, assigns a pointer, data location of 
        // y is storage
        y[7]; // fine, returns the 8th element
        y.pop(); // fine, modifies x through y
        delete x; // fine, clears the array, also modifies y
        // The following does not work; it would need to create a new temporary
        // unnamed array in storage, but storage is "statically" allocated:
        // y = memoryArray;
        // This does not work either, since it would "reset" the pointer, 
        // but there is no sensible location it could point to.
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
// Thay vì dùng x**3 thì dùng x*x*x chi phí rẻ hơn.

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
        delete dataArray; // this sets dataArray.length to zero, but as 
        // uint[] is a complex object, also y is affected which is an 
        // alias to the storage object. On the other hand: "delete y" 
        // is not valid, as assignments to local variables referencing 
        // storage objects can only be made from existing storage objects.
        assert(y.length == 0);
    }
}
```

### 5. Data Structure
```go
// 5.1. Enum
//// Enum không thể có nhiều hơn 256 thành phần. Thứ tự các options  
// của Enum là số nguyên không dấu bắt đầu từ 0.
enum State { Created, Locked, Inactive } // Enum
State constant defaultState = State.Inactive;


// 5.2. Array
//// Array có thể có kích thước cố định trong thời gian biên dịch 
// hoặc chúng có thể có kích thước động.
//// Array có kích thước cố định k khi khai báo: a[k]
//// Array có kích thước động khi khai báo: a[]
uint[][] memory X = new uint[][5] // Khai báo 1 mảng chứa 5 mảng động. 
// Cách khai báo ngược so với 1 số ngôn ngữ khác.
//// Chỉ mục mảng bắt đầu từ 0 và được truy suất ngược với khai báo.
X[2][6] // Truy suất đến phần tử thứ 7 trong mảng động thứ 3.
X[2] // Truy suất đến toàn bộ mảng động thứ 3.
//// Các phần tử của mảng có thể là 1 kiểu bất kỳ, bao gồm: map và struct.
//// Các kiểu bytes và string là các kiểu đặc biệt của array.

// a) Allocating Memory Arrays
//// Cấp phát mảng động bằng toán tử new
function f(uint len) public pure {
    uint[] memory a = new uint[](7);
    bytes memory b = new bytes(len);
    assert(a.length == 7);
    assert(b.length == len);
    a[6] = 8;
}
//// Không thể gán các mảng bộ nhớ có kích thước cố định (memory arrays) 
// cho các mảng bộ nhớ có kích thước động (dynamically-sized memory arrays).
// ==> Error: uint[3] memory cannot be converted to uint[] memory.
uint[] memory x = [uint(1), 3, 4]; 

//// Nếu bạn muốn khởi tạo mảng có kích thước động, bạn phải gán các 
// phần tử riêng lẻ:
uint[] memory x = new uint[](3); // Cấp phát mảng động có kích thước là 3.
x[0] = 1;
x[1] = 3;
x[2] = 4;


// b) Array Members : Các phương thức áp dụng cho Dynamic storage arrays 
// và bytes.
/**
- length : chiều dài mảng.
- push() : Thêm 1 phần tử có giá trị khởi tạo của kiểu và cuối mảng, 
trả về tham chiếu đến phần tử đó. Nên có thể được dùng như sau: 
x.push().t = 2 hoặc x.push() = b
- push(x) : Thêm phần tử x vào cuối mảng, không trả về gì cả.
- pop : Dùng để xóa phần tử ở cuối mảng.
*/


// c) Array Slices
//// x[start:end] khi phần tử đầu tiên là x[start], còn phần tử cuối 
// là x[end - 1]. start mặc định là 0, end mặc định là chiều dài mảng.
x[4:] // mảng từ phần tử thứ 4 đến cuối mảng.
x[:4] // mảng từ đầu mảng đến phần tử thứ 4.



// 5.3. Mapping Types
//// syntax: mapping(_KeyType => _ValueType)
//// _KeyType là các kiểu nguyên thủy, bytes, string, contract hoặc enum.
//// _ValueType là một kiểu bất kỳ, bao gồm: map, array, struct. 

//// Map chỉ được lưu trữ ở dạng storage, được dùng để lưu trữ trạng thái 
// (state), kiểu tham chiếu storage trong hàm hoặc tham số của các thư viện hàm.
//// Map không được sử dụng làm tham số hoặc giá trị trả về của các hàm 
// trong contract được công khai (public). Điều này cũng áp dụng cho array 
// và struct có chứa map.

// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.22 <0.9.0;

contract MappingExample {

    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function allowance(address owner, address spender) public view 
        returns (uint256) {
        return _allowances[owner][spender];
    }

    function transferFrom(address sender, address recipient, uint256 amount) public 
        returns (bool) {
        _transfer(sender, recipient, amount);
        approve(sender, msg.sender, amount);
        return true;
    }

    function approve(address owner, address spender, uint256 amount) public 
        returns (bool) {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }
}

// a) Iterable Mappings
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.6.8 <0.9.0;

struct IndexValue { uint keyIndex; uint value; }
struct KeyFlag { uint key; bool deleted; }

struct itmap {
    mapping(uint => IndexValue) data;
    KeyFlag[] keys;
    uint size;
}

library IterableMapping {
    function insert(itmap storage self, uint key, uint value) internal 
        returns (bool replaced) {
        uint keyIndex = self.data[key].keyIndex;
        self.data[key].value = value;
        if (keyIndex > 0)
            return true;
        else {
            keyIndex = self.keys.length;
            self.keys.push();
            self.data[key].keyIndex = keyIndex + 1;
            self.keys[keyIndex].key = key;
            self.size++;
            return false;
        }
    }

    function remove(itmap storage self, uint key) internal 
        returns (bool success) {
        uint keyIndex = self.data[key].keyIndex;
        if (keyIndex == 0)
            return false;
        delete self.data[key];
        self.keys[keyIndex - 1].deleted = true;
        self.size --;
    }

    function contains(itmap storage self, uint key) internal view 
        returns (bool) {
        return self.data[key].keyIndex > 0;
    }

    function iterate_start(itmap storage self) internal view 
        returns (uint keyIndex) {
        return iterate_next(self, type(uint).max);
    }

    function iterate_valid(itmap storage self, uint keyIndex) internal view 
        returns (bool) {
        return keyIndex < self.keys.length;
    }

    function iterate_next(itmap storage self, uint keyIndex) internal view 
        returns (uint r_keyIndex) {
        keyIndex++;
        while (keyIndex < self.keys.length && self.keys[keyIndex].deleted)
            keyIndex++;
        return keyIndex;
    }

    function iterate_get(itmap storage self, uint keyIndex) internal view 
        returns (uint key, uint value) {
        key = self.keys[keyIndex].key;
        value = self.data[key].value;
    }
}

// How to use it
contract User {
    // Just a struct holding our data.
    itmap data;
    // Apply library functions to the data type.
    using IterableMapping for itmap;

    // Insert something
    function insert(uint k, uint v) public returns (uint size) {
        // This calls IterableMapping.insert(data, k, v)
        data.insert(k, v);
        // We can still access members of the struct,
        // but we should take care not to mess with them.
        return data.size;
    }

    // Computes the sum of all stored data.
    function sum() public view returns (uint s) {
        for (
            uint i = data.iterate_start();
            data.iterate_valid(i);
            i = data.iterate_next(i)
        ) {
            (, uint value) = data.iterate_get(i);
            s += value;
        }
    }
}



// 5.4. Structs

// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.6.0 <0.9.0;

// Defines a new type with two fields.
// Declaring a struct outside of a contract allows
// it to be shared by multiple contracts.
// Here, this is not really needed.
struct Funder {
    address addr;
    uint amount;
}

contract CrowdFunding {
    // Structs can also be defined inside contracts, which makes them
    // visible only there and in derived contracts.
    struct Campaign {
        address payable beneficiary;
        uint fundingGoal;
        uint numFunders;
        uint amount;
        mapping (uint => Funder) funders;
    }

    uint numCampaigns;
    mapping (uint => Campaign) campaigns;

    function newCampaign(address payable beneficiary, uint goal) public 
        returns (uint campaignID) {
        campaignID = numCampaigns++; // campaignID is return variable
        // We cannot use "campaigns[campaignID] = Campaign(beneficiary, goal, 0, 0)"
        // because the right hand side creates a memory-struct 
        // "Campaign" that contains a mapping.
        Campaign storage c = campaigns[campaignID];
        c.beneficiary = beneficiary;
        c.fundingGoal = goal;
    }

    function contribute(uint campaignID) public payable {
        Campaign storage c = campaigns[campaignID];
        // Creates a new temporary memory struct, initialised with 
        // the given values and copies it over to storage.
        // Note that you can also use Funder(msg.sender, msg.value) to initialise.
        c.funders[c.numFunders++] = Funder({addr: msg.sender, amount: msg.value});
        c.amount += msg.value;
    }

    function checkGoalReached(uint campaignID) public 
        returns (bool reached) {
        Campaign storage c = campaigns[campaignID];
        if (c.amount < c.fundingGoal)
            return false;
        uint amount = c.amount;
        c.amount = 0;
        c.beneficiary.transfer(amount);
        return true;
    }
}

```

### 6. Loop
```go
// Các cấu trúc điều khiển như: if, else, while, do, for, break, 
// continue, return đều tương tự như ngôn ngữ C và JavaScript.
```

### 7. Function
```go
// Function có 2 loại: internal và external functions.
//// internal function : Các hàm nội bộ chỉ có thể được gọi bên trong 
// hợp đồng (contract) hiện tại (cụ thể hơn, bên trong đơn vị mã hiện 
// tại, cũng bao gồm các hàm thư viện nội bộ và các hàm kế thừa).
//// external function : Các hàm bên ngoài bao gồm một địa chỉ và một 
// chữ ký hàm và chúng có thể được chuyển qua và trả về từ các lệnh 
// gọi hàm bên ngoài.
function (<parameter types>) {internal|external} [pure|view|payable] 
    [returns (<return types>)]{
    // do something...
}
// Nếu hàm không trả về gì thì toàn bộ returns (<return types>) sẽ được bỏ đi.
// Mặc định hàm là internal, nên từ khóa internal có thể bỏ qua.

// Hàm A được chuyển đổi ngầm định qua hàm B khi và chỉ khi chúng có: 
// tham số đầu vào giống nhau, kiểu trả về giống nhau, thuộc tính 
// internal/external giống nhau, khả năng đột biến trạng thái 
// (the state mutability) của A hạn chế hơn khả năng đột biến trạng thái 
// của B. Cụ thể :
//// pure có thể chuyển đổi thành view và non-payable.
//// view có thể chuyển đổi thành non-payable.
//// payable có thể chuyển đổi thành non-payable.

//// Nếu 1 hàm là payable, có nghĩa là nó cũng chấp nhận thanh toán bằng 
// 0 Ether, vì vậy nó cũng không phải là khoản thanh toán.
//// Ngược lại, 1 hàm là non-payable, nó sẽ từ chối Ether được gửi đến nó, 
// vì vậy hàm non-payable không thể chuyển đổi thành hàm payable.

// Hàm public f có thể được dùng trong cả internal và external function. 
// Nếu f được dùng trong hàm internal thì sử dụng f, còn dùng trong hàm 
// external thì dùng this.f.

// Nếu 1 hàm là External hoặc public thì có các thuộc tính sau:
//// .address : trả về địa chỉ của hợp đồng (contract) của hàm.
//// .selector : trả về ABI function selector
```
