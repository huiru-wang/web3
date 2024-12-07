
## SPDX 许可标识符

## 函数

```sol
function <function name>(<parameter types>) {internal|external|public|private} [pure|view|payable] [returns (<return types>)]
```
- public：次函数在合约内部和合约外部都可见；
- private：只能从合约内部访问，继承的合约也不能使用。
- external：只能从`合约外部`访问（内部可以通过`this.f()`来调用）。
- internal: 只能从`合约内部`访问，继承的合约可以用。
- payable：此函数被允许在运行的时候可以向合约转入ETH
- pure：纯函数，不能读取合约状态、也不能写合约状态；
- view：仅读合约状态，不能写合约状态；

### public
当需要从合约外部（包括其他合约和用户账户）访问函数，并且可能在继承合约中也被访问时，使用public关键字；
```sol
contract Token {
    mapping(address => uint) public balances;
    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }
    function getBalance(address _address) public view returns (uint) {
        return balances[_address];
    }
}
```

### private
用于限制函数只能在当前合约内部访问，不能被外部合约或者用户账户调用，也不能在继承合约中被访问。当函数包含只与合约内部逻辑相关的操作，不希望外部干扰时使用。

假设我们有一个合约，内部有一个计算利息的函数，这个函数的计算逻辑不应该被外部访问
```sol
contract BankAccount {
    uint private interestRate;
    function setInterestRate(uint _rate) private {
        interestRate = _rate;
    }
    function calculateInterest() public view returns (uint) {
        // 这里假设还有其他存储变量代表账户余额等
        uint balance = 100; // 假设余额为100，实际情况可能从存储中获取
        return balance * interestRate;
    }
}
```

### external
用于定义只能从合约外部调用的函数，在合约内部不能直接调用（除非通过this关键字）。这种函数通常用于接收外部输入并更新合约状态，或者提供外部可调用的接口

假设我们有一个投票合约，有一个外部函数用于用户投票
```sol
contract Voting {
    mapping(uint => uint) public votes;
    function vote(uint _candidateId) external {
        votes[_candidateId]++;
    }
}
```

### internal
当函数需要在当前合约内部以及继承该合约的子合约中使用，但不允许外部合约和用户账户直接访问时，使用internal关键字

```sol
contract Asset {
    function _calculateBaseValue() internal pure returns (uint) {
        return 100; // 假设基础价值为100，实际可能是复杂计算
    }
}
contract ExtendedAsset is Asset {
    function calculateTotalValue() public pure returns (uint) {
        // 假设还有其他因素影响总价值，这里先只考虑基础价值
        return _calculateBaseValue();
    }
}
```

## 数据类型

值类型
- bool
- 无符号整数：uint8、uint16、uint32、uint64、uint128、uint256、uint;
- 有符号整数：int8、int16、int32、int64、int128、int256、int;
- 固定长度字节数组：bytes1表示长度为1的字节数组，如`bytes1`、`bytes2`、`bytes3`等；
- 枚举
- 地址类型：address；存储一个`20字节`(`160位`)的值
引用类型：
- string：动态字节数组
- 定长数组：`uint[3] fixedArray;`
- 动态数组：`uint[] dynamicArray;`
- 映射mapping
- 结构体


### 以太单位


### 时间单位

solidity中的最小时间单位是秒；`seconds`、`minutes`、`hours`、`days`、`weeks`都是时间单位，可以进行运算，如：`1 minutes + 1 seconds`
- seconds：`1`
- minutes：`60 seconds = 1 minute`
- hours：`60 minutes = 3600 seconds`
- days：`24 hours = 86400 seconds`
- weeks：`7 days = 604800 seconds`

```sol
function minutesUnit() external pure returns(uint) {
    assert(1 minutes == 60);
    assert(1 minutes == 60 seconds);
    return 1 minutes + 1 seconds;
}
```

### 变量的存储位置

- storage：存储在合约的持久化存储中，可以跨函数调用，可以修改；（消耗gas最多）
- memory：存储在内存中，不上链中，可修改；返回数据类型是变长的情况下，必须添加`memory`，如`string`、`bytes`、`array`等；
- calldata：存储在内存中，不上链，不能修改；(immutable)


### 变量的作用域

- 状态变量(state variable)：存储在链上的变量，所有合约内函数都可以访问，在合约内、函数外声明；（消耗Gas高）

- 局部变量(local variable)：仅在函数执行过程中有效的变量，不上链（Gas第）

- 全局变量(global variable)：Solidity内置的全局变量，不需要定义；