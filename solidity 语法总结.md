# solidity 语法总结  

## 访问权限  
1. 权限从小到大依次是:**private**,**internal**,**public**. **private** 和 **internal** 只能被合约内部访问, **public** 能被外部访问
2. 属性的默认访问权限为 **internal**.  
3. 函数默认的访问权限是 **public**.  
4. 指针能访问 **public** 权限的属性和函数,但不能访问 **private** 和 **internal** 权限的属性和函数,即使是在合约内部通过也不能通过 **this** 访问.  

## 数据类型  
##### 值类型  
1. bool,
2. int8-int256(int),
3. uint8-uint256(uint),
4. address(uint160),  
    * address 类型的成员变量  
        * balance  

        
```
contract addressBalance {
    function getBalance(address addr) public constant returns(uint){
        return addr.balance;
    }
}
```  


##### 引用类型  

##### 语法  
1. 形参中 memery 表示值传递; storage 表示引用传递.
2. 形参是引用类型时,函数的权限必须是 **internal** 或者 **private**  

## 继承  
##### 语法  

```
contract A is B,C {}
```  

##### 规则  
1. 支持多继承.  
2. 只有 **internal** 和 **public** 权限的属性能够被继承.  
3. 只有 **public** 权限的函数能被继承.  


## 函数重写  

```
pragma solidity ^0.4.4;

contract Person{
    address _owner;
    string internal _name;
    uint private _age;
    uint public _weight;

    constructor() {
        _name = "xiaoming";
        _age = 10;
        _owner = msg.sender;
    }

    function name() constant returns (string){
        return _name;
    }

    function age() constant returns (uint){
        return _age;
    }

    function owner() constant returns (address){
        return _owner;
    }

    function setName(string name){
        _name = name;
    }

    function setAge(uint age){
        _age = age;
    }

    function setWeight(uint weight){
        _weight = weight;
    }

    function weight() constant returns (uint) {
        return _weight;
    }

    function test1() private returns (uint){
        return this.weight();
    }

    function test2() internal returns (uint){
        return test1();
    }
    
    function test3() public returns (string){
        return this._name;//报错.this 不能引用非 public 权限的属性
    }


    function kill() {
        if(_owner == msg.sender){
            //析构函数
            selfdestruct(_owner);
        }
    }
}

contract Animal {
    string _name;
    function animalName() constant returns (string){
        return  _name;
    }
}

//多继承
contract Dog is Person,Animal{
    function test2() internal returns (uint){
        return 1000;
    }
    
    function test1() private returns (uint){
        return this.weight();
    }
}

``` 

