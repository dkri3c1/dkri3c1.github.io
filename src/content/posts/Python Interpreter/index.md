---
title: Python Interpreter
published: 2025-05-21
tags: [Python]
category: Python
draft: false
image: 'image.png'
---


# Python Interpreter

## 專有名詞解釋

這邊先列一些專有名詞

### Code Object

- 意思是說經過 complied 的 python source code

### AST 抽象語法樹

-  表示程式語言語法結構的樹狀資料結構
-  只保留語意上的重要結構
-  每個節點代表一種元素(變數、條件、迴圈等)

以 python 來說

```py=
a= 1+2
```

對應的 AST 會長這個樣子

```py=
Assign
├── Name(id='a')
└── BinOp
    ├── Num(n=1)
    ├── Add()
    └── Num(n=2)
```



## Run python 

今天執行一個 python 的 code 的時候，他的流程大概會長這樣

- Interpreter 會把傳進來的 Python Source code `file.py` 轉進來變成 Byte Code 
- 然後經過 interpret 之後的 Byte Code 才會被 Virtual Machine Complie 成 Machine Code 跟 Execute code 等
- 最後 Virtual Machine 會 Call CPU ，經過一系列處理之後會變成我們程式碼想做的事

而我們通常用的都是 Cpython (以C為底所寫的python)，所以下面的 Virtual Machine 我們都已 Cpython VM 去解釋

![image](https://hackmd.io/_uploads/Sk3pkrvexl.png)



## Tiny Interpreter

我們今天有一個很小的直譯器，並且只能理解三個指令，然後要做一個求兩數和的東西，下面是指令

- LOAD_Value  告訴解釋器把一個數字壓入 Stack 中，需要參數才能把指定數字
- ADD_TWO_VALUES 將兩個值相加，他直譯器的 Stack 上 pop 所有要用的值，沒有參數
- PRINT_ANSWER 列印 Stack 最上面，並把他 pop 掉

我們不用關心語法、編譯錯誤、還有指令怎麼產生的，我們單純就去寫 `7+5` 

我們可以生出這樣的指令集

```py
#7+5

what_to_execute = {
    "instructions": [("LOAD_VALUE", 0),  # the first number
                     ("LOAD_VALUE", 1),  # the second number
                     ("ADD_TWO_VALUES", None),
                     ("PRINT_ANSWER", None)],
    "numbers": [7, 5] }
```

Python 的直譯器是一個 Stack Machine，所以我們要用 Stack 的方式去跑

![image](https://hackmd.io/_uploads/Hkr_mAtbeg.png)

現在我們就先定義前面三個指令吧

```py=
class Interpreter:
    def __init__(self):
        self.stack = []

    def LOAD_VALUE(self, number):
        self.stack.append(number)

    def PRINT_ANSWER(self):
        answer = self.stack.pop()
        print(answer)

    def ADD_TWO_VALUES(self):
        first_num = self.stack.pop()
        second_num = self.stack.pop()
        total = first_num + second_num
        self.stack.append(total)
```

寫出了這些指令後，直譯器還需要一個能把所有東西結合在一起然後執行的方法 => `run_code`，她們我們前面定義的 `what-to-execute` 作為參數，把對應的方法填上去

```py=
    def run_code(self, what_to_execute):
        instructions = what_to_execute["instructions"]
        numbers = what_to_execute["numbers"]
        for each_step in instructions:
            instruction, argument = each_step
            if instruction == "LOAD_VALUE":
                number = numbers[argument]
                self.LOAD_VALUE(number)
            elif instruction == "ADD_TWO_VALUES":
                self.ADD_TWO_VALUES()
            elif instruction == "PRINT_ANSWER":
                self.PRINT_ANSWER()
```

然後再幫他開個物件

完整版:


```py=
what_to_execute = {
    "instructions": [("LOAD_VALUE", 0),  # the first number
                     ("LOAD_VALUE", 1),  # the second number
                     ("ADD_TWO_VALUES", None),
                     ("PRINT_ANSWER", None)],
    "numbers": [7, 5] }

class Interpreter:
    def __init__(self):
        self.stack = []

    def LOAD_VALUE(self, number):
        self.stack.append(number)

    def PRINT_ANSWER(self):
        answer = self.stack.pop()
        print(answer)

    def ADD_TWO_VALUES(self):
        first_num = self.stack.pop()
        second_num = self.stack.pop()
        total = first_num + second_num
        self.stack.append(total)
    def run_code(self, what_to_execute):
        instructions = what_to_execute["instructions"]
        numbers = what_to_execute["numbers"]
        for each_step in instructions:
            instruction, argument = each_step
            if instruction == "LOAD_VALUE":
                number = numbers[argument]
                self.LOAD_VALUE(number)
            elif instruction == "ADD_TWO_VALUES":
                self.ADD_TWO_VALUES()
            elif instruction == "PRINT_ANSWER":
                self.PRINT_ANSWER()
    interpreter = Interpreter()
    interpreter.run_code(what_to_execute)
```                


## Variables 

為了要讓直譯器多了變數的功能，我們又新增了一條指令

- STORE_NAME 載入變數數值，需要參數
- LOAD_NAME  載入變數名稱，需要參數

為了要追蹤變數名稱對應哪個值，我們新增了一個字典 `environment`，然後會發現指令參數可能是 `numbers` 的索引，也有可能是 `names` 的索引，直譯器可以透過直接執行來判斷是哪種參數，但我們今天把指令和參數 mmap 放在另一個方法中

```py=
    what_to_execute = {
        "instructions": [("LOAD_VALUE", 0),
                         ("STORE_NAME", 0),
                         ("LOAD_VALUE", 1),
                         ("STORE_NAME", 1),
                         ("LOAD_NAME", 0),
                         ("LOAD_NAME", 1),
                         ("ADD_TWO_VALUES", None),
                         ("PRINT_ANSWER", None)],
        "numbers": [1, 2],
        "names":   ["a", "b"] }
class Interpreter:
    def __init__(self):
        self.stack = []
        self.environment = {}

    def STORE_NAME(self, name):
        val = self.stack.pop()
        self.environment[name] = val

    def LOAD_NAME(self, name):
        val = self.environment[name]
        self.stack.append(val)

    def parse_argument(self, instruction, argument, what_to_execute):
        """ Understand what the argument to each instruction means."""
        numbers = ["LOAD_VALUE"]
        names = ["LOAD_NAME", "STORE_NAME"]

        if instruction in numbers:
            argument = what_to_execute["numbers"][argument]
        elif instruction in names:
            argument = what_to_execute["names"][argument]

        return argument

    def run_code(self, what_to_execute):
        instructions = what_to_execute["instructions"]
        for each_step in instructions:
            instruction, argument = each_step
            argument = self.parse_argument(instruction, argument, what_to_execute)

            if instruction == "LOAD_VALUE":
                self.LOAD_VALUE(argument)
            elif instruction == "ADD_TWO_VALUES":
                self.ADD_TWO_VALUES()
            elif instruction == "PRINT_ANSWER":
                self.PRINT_ANSWER()
            elif instruction == "STORE_NAME":
                self.STORE_NAME(argument)
            elif instruction == "LOAD_NAME":
                self.LOAD_NAME(argument)
interpreter = Interpreter()
interpreter.run_code(what_to_execute)
```

## 優化 run_code

可以看到 run_code 已經變得很冗長了，所以這邊我們用了 python 的動態方法去查找，這樣就可以用 python 的 `getattr` 去找ㄌ

```py=
    def execute(self, what_to_execute):
        instructions = what_to_execute["instructions"]
        for each_step in instructions:
            instruction, argument = each_step
            argument = self.parse_argument(instruction, argument, what_to_execute)
            bytecode_method = getattr(self, instruction)
            if argument is None:
                bytecode_method()
            else:
                bytecode_method(argument)
```

## Real Python ByteCode

Byte Code 是 Python Complier 把 AST Complie 之後給 Cpython VM 所執行的東西，而顧名思義就是以 Byte 為主的內容，正常情況下我們是看不懂他在幹嘛的但是今天我們可以用 python 內部的 `dis` 去看一隻 python 程式的 Byte Code 具體會長怎樣

```
流程圖:
原始碼 => AST => Bytecode => 執行（在 CPython VM中）
```

可以用 `cond` 這個函數物件去看跟他有關的 code object，而`cond.__code__.co_code` 是他的 byte code，一般情況下會長這樣

```py=
>>> cond.__code__.co_code  # the bytecode as raw bytes
b'd\x01\x00}\x00\x00|\x00\x00d\x02\x00k\x00\x00r\x16\x00d\x03\x00Sd\x04\x00Sd\x00
   \x00S'
>>> list(cond.__code__.co_code)  # the bytecode as numbers
[100, 1, 0, 125, 0, 0, 124, 0, 0, 100, 2, 0, 107, 0, 0, 114, 22, 0, 100, 3, 0, 83, 
 100, 4, 0, 83, 100, 0, 0, 83]
```

這時候我們完全看不出這些 bytecode 在幹嘛的，

所以我們可以用 `dis` 去看這些 byte code 在幹嘛

這邊隨便寫一個會輸出 Meow 三次的程式

```py=
import dis

def test():
    for i in range(3):
        print('meow meow meow')

dis.dis(test)
```

執行之後他的 Byte Code 會長這樣

```py=
  4           0 LOAD_GLOBAL              0 (range)
              2 LOAD_CONST               1 (3)
              4 CALL_FUNCTION            1
              6 GET_ITER
        >>    8 FOR_ITER                12 (to 22)
             10 STORE_FAST               0 (i)

  5          12 LOAD_GLOBAL              1 (print)
             14 LOAD_CONST               2 ('meow meow meow')
             16 CALL_FUNCTION            1
             18 POP_TOP
             20 JUMP_ABSOLUTE            8
        >>   22 LOAD_CONST               0 (None)
             24 RETURN_VALUE
```

從這些 Byte Code 推回去的話是先從 `globals` 找到 `range` 這個 function
然後 `LOAD_GLOBAL` 是用來載入 `range` 、`print` 這些全域定義，之後 `LOAD_CONST` 看迴圈要執行幾次，知道幾次之後再 `CALL_FUNCTION` 呼叫函數，表示有 1 個參數 (3)，會變成 `range(3)`

`GET_ITER FOR_ITER` 就是把這些東西轉換成可以被 for loop 所使用的東西而已

`STORE_FAST` 代表把剛剛的 `ITER` 存到區域變數 i

之後就是用 `LOAD_GLOBAL` CALL `print` 跟載入我們給的值 `Meow Meow Meow` 接著在 CALL 這個 function

這邊`POP_TOP`意思是說 `print` return 0 ，所以把他從 STACK 上丟掉
`JUMP_ABSOLUTE `代表跑回去一開始的迴圈
最後的 `LOAD_CONST` 代表迴圈結束後預設的 return value 是 0，然後把 value Return 回來

那 `range` 、`print` etc.. 這些 function 都是 built_in function，所以都隸屬於 `globals`

我們如果要看 byte code 中數字代表指令的意思，也可以用 `dis.opname[?]` 去看

```py=
>>> dis.opname[100]
'LOAD_CONST'
>>> dis.opname[125]
'STORE_FAST'
```

##  比較和循環


程式中一定會有迴圈跟判斷條件，python 的 byte code 也有類似 goto 的東西可以做跳轉

```py=
if x < 5
```

會變成

```py=
4           4 LOAD_FAST                0 (x)
            6 LOAD_CONST               2 (5)
            8 COMPARE_OP               0 (<)
           10 POP_JUMP_IF_FALSE        8 (to 16)
```

如果是 False 就跳到第 16 行

然後 while 跟前面 for 不太一樣，在這邊會跟上面比較相近

```py=
def f():
    x = 1
    while x < 5:
        x = x + 1
    return x
```

=>

```py=
3           0 LOAD_CONST               1 (1)
            2 STORE_FAST               0 (x)

4           4 LOAD_FAST                0 (x)
            6 LOAD_CONST               2 (5)
            8 COMPARE_OP               0 (<)
           10 POP_JUMP_IF_FALSE       14 (to 28)

5     >>   12 LOAD_FAST                0 (x)
           14 LOAD_CONST               1 (1)
           16 BINARY_ADD
           18 STORE_FAST               0 (x)

4          20 LOAD_FAST                0 (x)
           22 LOAD_CONST               2 (5)
           24 COMPARE_OP               0 (<)
           26 POP_JUMP_IF_TRUE         6 (to 12)

6     >>   28 LOAD_FAST                0 (x)
           30 RETURN_VALUE
```

## Frames

我們知道 python VM 是一個 Stack Machine，在 Stack 上做一些操作，那最後的 `Return Value` 到底去了哪呢?

要回答前面那個問題，我們要先知道 frame 這個東西

首先 Frame 是執行每一個 function 時，所創建的結構，他存了很多 function 執行的資料，一個 code object 中會有很多 frame，可以把它想像成一個小包廂，每一次 call function 的時候，他就會創造一個新的 frame 來執行所需的東西

然後 frame 上的東西有

- 這個函式有哪些變數（local variables）

- 下一行要執行什麼（程式計數器）

- 上層呼叫者是誰（上一層 frame）

假設我今天 call 自己 10 次，這時會有 11 個 frame

```py=
def bar(y):
    x=y+3
    return x

def foo():
    a=1
    b=2
    return a + bar(b)

foo()
```

現在有三個 frame，當 bar return ，跟他對應的 frame 就會從 Stack 上 pop 掉

![image](https://hackmd.io/_uploads/r1cnOxq-lx.png)



回到前面 Byte Instruction 的 RETURN_VALUE 告訴直譯器 frame 間傳遞一個值，他會先把 Stack 最上面的 frame 數據給丟出，然後把整個 frame 丟掉，最後把剛剛最上面的那個數據壓入 Stack 上

## ByteRun

上面都是 python Interpreter 的基礎，往下來看 ByteRun ㄅ

ByteRun 有種對象

- VirtualMachine 類
    - 管理高層結構，frame 調用 Stack，指令到操作的 mmap，比前面 Inteprter 物件更複雜的版本
- Frame 類
    - 每個 Frame 類都有一個 code object ，管理其他一些必要的狀態資訊，像是全域和區域命名空間，指向調用他的 frame pointer 和最後執行的 Byte 指令
-  Function 類
    -  call function 時創造一個新的 frame
    -  管理和控制新的 frame
- Block 類
    - 包中程式碼區塊的三個屬性 (這邊不會用到)

## The VirtualMachine Class

VirtualMachine 就是用來執行 byte code 的 VM

VirtualMachine 保存呼叫 Stack，例外狀態，在 frame 中傳遞回傳值

他的進入點是方法 `run_code`，他在編譯後的 code object 為參數，以建立一個 frame 為開始，然後執行這個 frame，這個 frame 可能會在創建新的 frame，當第一個 frame 回傳時，執行結束

```py=
class VirtualMachineError(Exception):
    pass

class VirtualMachine(object):
    def __init__(self):
        self.frames = []   # The call stack of frames.
        self.frame = None  # The current frame.
        self.return_value = None
        self.last_exception = None

    def run_code(self, code, global_names=None, local_names=None):
        """ An entry point to execute code using the virtual machine."""
        frame = self.make_frame(code, global_names=global_names, 
                                local_names=local_names)
        self.run_frame(frame)
```

## frames

他是一個屬性的集合，沒有任何方法，他的屬性包含了 code object、區域、全域變數，還有命名空間等

前一個 frame 的引用，一個資料 Stack，一個 Block Stack，


```py=
class Frame(object):
    def __init__(self, code_obj, global_names, local_names, prev_frame):
        self.code_obj = code_obj
        self.global_names = global_names
        self.local_names = local_names
        self.prev_frame = prev_frame
        self.stack = []
        if prev_frame:
            self.builtin_names = prev_frame.builtin_names
        else:
            self.builtin_names = local_names['__builtins__']
            if hasattr(self.builtin_names, '__dict__'):
                self.builtin_names = self.builtin_names.__dict__

        self.last_instruction = 0
        self.block_stack = []
```

前面只宣告了 Frame 上的東西，現在我們要寫他的操作方式，包含了

- 創建新的 frame 
- push 去 Stack 的方法
- pop 出 Stack 的方法
- 還有運行 frame (run_frame)

```py=
class VirtualMachine(object):
    [... snip ...]

    # Frame manipulation
    def make_frame(self, code, callargs={}, global_names=None, local_names=None):
        if global_names is not None and local_names is not None:
            local_names = global_names
        elif self.frames:
            global_names = self.frame.global_names
            local_names = {}
        else:
            global_names = local_names = {
                '__builtins__': __builtins__,
                '__name__': '__main__',
                '__doc__': None,
                '__package__': None,
            }
        local_names.update(callargs)
        frame = Frame(code, global_names, local_names, self.frame)
        return frame

    def push_frame(self, frame):
        self.frames.append(frame)
        self.frame = frame

    def pop_frame(self):
        self.frames.pop()
        if self.frames:
            self.frame = self.frames[-1]
        else:
            self.frame = None

    def run_frame(self):
        pass
        # we'll come back to this shortly
```

## Function

Funtion 在直譯器中的細節不是很重要，只有在`__call__`這個方法被調用時，才會創建一個新的 `Frame` 並執行他

```py=
class Function(object):
    """
    Create a realistic function object, defining the things the interpreter expects.
    """
    __slots__ = [
        'func_code', 'func_name', 'func_defaults', 'func_globals',
        'func_locals', 'func_dict', 'func_closure',
        '__name__', '__dict__', '__doc__',
        '_vm', '_func',
    ]

    def __init__(self, name, code, globs, defaults, closure, vm):
        """You don't need to follow this closely to understand the interpreter."""
        self._vm = vm
        self.func_code = code
        self.func_name = self.__name__ = name or code.co_name
        self.func_defaults = tuple(defaults)
        self.func_globals = globs
        self.func_locals = self._vm.frame.f_locals
        self.__dict__ = {}
        self.func_closure = closure
        self.__doc__ = code.co_consts[0] if code.co_consts else None

        # Sometimes, we need a real Python function.  This is for that.
        kw = {
            'argdefs': self.func_defaults,
        }
        if closure:
            kw['closure'] = tuple(make_cell(0) for _ in closure)
        self._func = types.FunctionType(code, globs, **kw)

    def __call__(self, *args, **kwargs):
        """When calling a Function, make a new frame and run it."""
        callargs = inspect.getcallargs(self._func, *args, **kwargs)
        # Use callargs to provide a mapping of arguments: values to pass into the new 
        # frame.
        frame = self._vm.make_frame(
            self.func_code, callargs, self.func_globals, {}
        )
        return self._vm.run_frame(frame)

def make_cell(value):
    """Create a real Python closure and grab a cell."""
    # Thanks to Alex Gaynor for help with this bit of twistiness.
    fn = (lambda x: lambda: x)(value)
    return fn.__closure__[0]
```

對資料 Stack 上也增加方法，Byte Code 操作的 Stack 總是在當前的 Frame Stack

```py=
class VirtualMachine(object):
    [... snip ...]

    # Data stack manipulation
    def top(self):
        return self.frame.stack[-1]

    def pop(self):
        return self.frame.stack.pop()

    def push(self, *vals):
        self.frame.stack.extend(vals)

    def popn(self, n):
        """Pop a number of values from the value stack.
        A list of `n` values is returned, the deepest value first.
        """
        if n:
            ret = self.frame.stack[-n:]
            self.frame.stack[-n:] = []
            return ret
        else:
            return []
```

在運行 frame 之前，我們還需要兩個方法

第一個方法，用 `parse_byte_and_args` 來看說他是否有參數，如果有，就 parse 他的參數

```py=
def parse_byte_and_args(self):
    f = self.frame  # 取得目前執行中的 frame（也就是當前的執行環境）
    opoffset = f.last_instruction  # 當前 bytecode 的偏移位置
    byteCode = f.code_obj.co_code[opoffset]  # 取得該位置的 bytecode（opcode 數值）
    f.last_instruction += 1  # 移動到下一個 bytecode 位置
    byte_name = dis.opname[byteCode]  # 取得該 bytecode 對應的名稱，例如 LOAD_CONST

    if byteCode >= dis.HAVE_ARGUMENT:
        # 有額外參數的指令（參數為兩個 byte 構成，低位在前，高位在後）
        arg = f.code_obj.co_code[f.last_instruction:f.last_instruction+2]  # 抓取 2 個 byte 的參數
        f.last_instruction += 2  # 向前推進 2 byte

        # 將兩個 byte 合成一個整數值（little-endian）
        arg_val = arg[0] + (arg[1] * 256)

        # 根據指令類型，把參數值轉換成對應的內容
        if byteCode in dis.hasconst:
            # 若是常數相關指令（如 LOAD_CONST），從 co_consts 取出實際值
            arg = f.code_obj.co_consts[arg_val]
        elif byteCode in dis.hasname:
            # 若是變數名稱相關指令（如 LOAD_NAME），從 co_names 取出變數名稱
            arg = f.code_obj.co_names[arg_val]
        elif byteCode in dis.haslocal:
            # 若是區域變數（如 LOAD_FAST），從 co_varnames 取出變數名稱
            arg = f.code_obj.co_varnames[arg_val]
        elif byteCode in dis.hasjrel:
            # 若是跳躍指令（如 JUMP_FORWARD），計算跳轉後的位移
            arg = f.last_instruction + arg_val
        else:
            # 其他指令，參數就保留為整數值
            arg = arg_val

        # 把參數包裝成 list 回傳（因為 dispatch 接受 list 作為 *args）
        argument = [arg]
    else:
        # 沒有額外參數的 bytecode（例如 RETURN_VALUE）
        argument = []

    return byte_name, argument  # 回傳指令名稱和對應參數
t
```

第二個方法是用 `dispathch` ， 用 `getattr` 來找，如果一條指令叫做 `FOO_BAR`，那他對應的方法就是 `byte_FOO_BAR`，然後每個指令都有 return None 或加上一個 String => why

```py=
class VirtualMachine(object):
    [...]

    def dispatch(self, byte_name, argument):
        """根據 bytecode 指令名稱分派對應的方法來執行。
        如果指令不支援，則拋出錯誤；如果執行時發生例外，也會記錄。"""

        # 我們需要追蹤執行結束的原因，例如是正常 return、跳出 loop 還是 exception。
        why = None
        try:
            # 根據 byte_name 找到對應的方法（例如 byte_LOAD_CONST）。
            bytecode_fn = getattr(self, 'byte_%s' % byte_name, None)

            if bytecode_fn is None:
                # 如果不是已定義的指令函式，試著以 UNARY_ 或 BINARY_ 處理
                if byte_name.startswith('UNARY_'):
                    self.unaryOperator(byte_name[6:])  # 處理一元運算，例如 UNARY_NEGATIVE
                elif byte_name.startswith('BINARY_'):
                    self.binaryOperator(byte_name[7:])  # 處理二元運算，例如 BINARY_ADD
                else:
                    # 若不符合任何可處理的 bytecode，拋出錯誤
                    raise VirtualMachineError(
                        "unsupported bytecode type: %s" % byte_name
                    )
            else:
                # 執行對應的 bytecode 方法，可能會回傳為何要中斷執行（例如 'return' 或 'exception'）
                why = bytecode_fn(*argument)
        except:
            # 如果在執行指令時發生例外，記錄錯誤並設為中止原因
            self.last_exception = sys.exc_info()[:2] + (None,)
            why = 'exception'

        return why

    def run_frame(self, frame):
        """執行一個 frame（等同一個函式呼叫範圍）直到它結束為止。
        如果中途發生例外就拋出，否則回傳執行結果。"""

        # 把這個 frame 加入執行堆疊中
        self.push_frame(frame)

        while True:
            # 從 bytecode 中解析出目前要執行的指令與參數
            byte_name, arguments = self.parse_byte_and_args()

            # 執行該 bytecode 指令，取得為何要結束執行（或 None）
            why = self.dispatch(byte_name, arguments)

            # 如果有中斷原因，並且目前有 block（如 try-finally, for, while 等），就處理 block stack
            while why and frame.block_stack:
                why = self.manage_block_stack(why)

            # 如果已經有明確的結束原因（如 return 或 exception），就跳出迴圈
            if why:
                break

        # 執行完後，把這個 frame 從堆疊中移除
        self.pop_frame()

        # 如果結束原因是例外，就重新拋出例外（轉為 Python 的例外）
        if why == 'exception':
            exc, val, tb = self.last_exception
            e = exc(val)
            e.__traceback__ = tb
            raise e

        # 否則就回傳執行結果（由 RETURN_VALUE 設定）
        return self.return_value

```


## Block (在這邊沒啥重要ㄉ)

Block 負責保障迴圈或異常廚李後的資料 Stack 是不是處於正確狀態

為了追蹤這些額外的信息，解釋器設置了一個 flag 來指示它的狀態。我們使用一個變數 why 實現這個標誌，它可以 None 或者是下面幾個字串這一， "continue" , "break" , "excption" , return 。他們指示對塊栈和資料栈進行什麼操作。回到我們迭代器的例子，如果塊栈的栈頂是一個 loop 塊， why 是 continue ,迭代器就應該保存在資料栈上，不是如果 why 是 break ,迭代器就會被彈出。

```py=
Block = collections.namedtuple("Block", "type, handler, stack_height")

class VirtualMachine(object):
    [... snip ...]

    # Block stack manipulation
    def push_block(self, b_type, handler=None):
        level = len(self.frame.stack)
        self.frame.block_stack.append(Block(b_type, handler, stack_height))

    def pop_block(self):
        return self.frame.block_stack.pop()

    def unwind_block(self, block):
        """Unwind the values on the data stack corresponding to a given block."""
        if block.type == 'except-handler':
            # The exception itself is on the stack as type, value, and traceback.
            offset = 3  
        else:
            offset = 0

        while len(self.frame.stack) > block.level + offset:
            self.pop()

        if block.type == 'except-handler':
            traceback, value, exctype = self.popn(3)
            self.last_exception = exctype, value, traceback

    def manage_block_stack(self, why):
        """ """
        frame = self.frame
        block = frame.block_stack[-1]
        if block.type == 'loop' and why == 'continue':
            self.jump(self.return_value)
            why = None
            return why

        self.pop_block()
        self.unwind_block(block)

        if block.type == 'loop' and why == 'break':
            why = None
            self.jump(block.handler)
            return why

        if (block.type in ['setup-except', 'finally'] and why == 'exception'):
            self.push_block('except-handler')
            exctype, value, tb = self.last_exception
            self.push(tb, value, exctype)
            self.push(tb, value, exctype) # yes, twice
            why = None
            self.jump(block.handler)
            return why

        elif block.type == 'finally':
            if why in ('return', 'continue'):
                self.push(self.return_value)

            self.push(why)

            why = None
            self.jump(block.handler)
            return why
        return why
```

## Instructions

剩下就是指令了，像是`byte_LOAD_FAST , byte_BINARY_MODULO` 等等，這邊只有一小段
```py=
class VirtualMachine(object):
    [... snip ...]

    ## Stack manipulation

    def byte_LOAD_CONST(self, const):
        self.push(const)

    def byte_POP_TOP(self):
        self.pop()

    ## Names
    def byte_LOAD_NAME(self, name):
        frame = self.frame
        if name in frame.f_locals:
            val = frame.f_locals[name]
        elif name in frame.f_globals:
            val = frame.f_globals[name]
        elif name in frame.f_builtins:
            val = frame.f_builtins[name]
        else:
            raise NameError("name '%s' is not defined" % name)
        self.push(val)

    def byte_STORE_NAME(self, name):
        self.frame.f_locals[name] = self.pop()

    def byte_LOAD_FAST(self, name):
        if name in self.frame.f_locals:
            val = self.frame.f_locals[name]
        else:
            raise UnboundLocalError(
                "local variable '%s' referenced before assignment" % name
            )
        self.push(val)

    def byte_STORE_FAST(self, name):
        self.frame.f_locals[name] = self.pop()

    def byte_LOAD_GLOBAL(self, name):
        f = self.frame
        if name in f.f_globals:
            val = f.f_globals[name]
        elif name in f.f_builtins:
            val = f.f_builtins[name]
        else:
            raise NameError("global name '%s' is not defined" % name)
        self.push(val)

    ## Operators

    BINARY_OPERATORS = {
        'POWER':    pow,
        'MULTIPLY': operator.mul,
        'FLOOR_DIVIDE': operator.floordiv,
        'TRUE_DIVIDE':  operator.truediv,
        'MODULO':   operator.mod,
        'ADD':      operator.add,
        'SUBTRACT': operator.sub,
        'SUBSCR':   operator.getitem,
        'LSHIFT':   operator.lshift,
        'RSHIFT':   operator.rshift,
        'AND':      operator.and_,
        'XOR':      operator.xor,
        'OR':       operator.or_,
    }

    def binaryOperator(self, op):
        x, y = self.popn(2)
        self.push(self.BINARY_OPERATORS[op](x, y))

    COMPARE_OPERATORS = [
        operator.lt,
        operator.le,
        operator.eq,
        operator.ne,
        operator.gt,
        operator.ge,
        lambda x, y: x in y,
        lambda x, y: x not in y,
        lambda x, y: x is y,
        lambda x, y: x is not y,
        lambda x, y: issubclass(x, Exception) and issubclass(x, y),
    ]

    def byte_COMPARE_OP(self, opnum):
        x, y = self.popn(2)
        self.push(self.COMPARE_OPERATORS[opnum](x, y))

    ## Attributes and indexing

    def byte_LOAD_ATTR(self, attr):
        obj = self.pop()
        val = getattr(obj, attr)
        self.push(val)

    def byte_STORE_ATTR(self, name):
        val, obj = self.popn(2)
        setattr(obj, name, val)

    ## Building

    def byte_BUILD_LIST(self, count):
        elts = self.popn(count)
        self.push(elts)

    def byte_BUILD_MAP(self, size):
        self.push({})

    def byte_STORE_MAP(self):
        the_map, val, key = self.popn(3)
        the_map[key] = val
        self.push(the_map)

    def byte_LIST_APPEND(self, count):
        val = self.pop()
        the_list = self.frame.stack[-count] # peek
        the_list.append(val)

    ## Jumps

    def byte_JUMP_FORWARD(self, jump):
        self.jump(jump)

    def byte_JUMP_ABSOLUTE(self, jump):
        self.jump(jump)

    def byte_POP_JUMP_IF_TRUE(self, jump):
        val = self.pop()
        if val:
            self.jump(jump)

    def byte_POP_JUMP_IF_FALSE(self, jump):
        val = self.pop()
        if not val:
            self.jump(jump)

    ## Blocks

    def byte_SETUP_LOOP(self, dest):
        self.push_block('loop', dest)

    def byte_GET_ITER(self):
        self.push(iter(self.pop()))

    def byte_FOR_ITER(self, jump):
        iterobj = self.top()
        try:
            v = next(iterobj)
            self.push(v)
        except StopIteration:
            self.pop()
            self.jump(jump)

    def byte_BREAK_LOOP(self):
        return 'break'

    def byte_POP_BLOCK(self):
        self.pop_block()

    ## Functions

    def byte_MAKE_FUNCTION(self, argc):
        name = self.pop()
        code = self.pop()
        defaults = self.popn(argc)
        globs = self.frame.f_globals
        fn = Function(name, code, globs, defaults, None, self)
        self.push(fn)

    def byte_CALL_FUNCTION(self, arg):
        lenKw, lenPos = divmod(arg, 256) # KWargs not supported here
        posargs = self.popn(lenPos)

        func = self.pop()
        frame = self.frame
        retval = func(*posargs)
        self.push(retval)

    def byte_RETURN_VALUE(self):
        self.return_value = self.pop()
        return "return"
```

## 動態型別: Complier 不知道的內容

Python 是一種動態語言，動態的意思是很多工作都在運行的時候完成，我們以下面的 `mod` 當例子，我們可以看到 Byte code 中，a 和 b 這兩個變數先被載入，最後 Byte Code `VINAY_MODULO` 完成這個運算

```py=
>>> def mod(a, b):
...    return a % b
>>> dis.dis(mod)
  2           0 LOAD_FAST                0 (a)
              3 LOAD_FAST                1 (b)
              6 BINARY_MODULO
              7 RETURN_VALUE
>>> mod(19, 5)
4
```

可以計算出 19 % 5 = 4，那假設我們今天用不同類的參數呢?

```
>>> mod("by%sde", "teco")
'bytecode'
```

在這邊，我用 python 中 Format String 的語法去跑他

會發現都會產生 `BINARY_MODULO`，但是結果卻差很多，也可以看到，不管我傳是整數還是字串，Python 的 ByteCode 都是一樣的，具體原因是 Python 的 Complier 並不是決定 `a%b` 是啥意思，他只是負責產生 `BINARY_MODULO` 而已，真正的邏輯是交給 `runtime`

Python 直譯器根據物件類型，去決定如何執行 `%` =>

Python 解釋器在執行 BINARY_MODULO 時會做以下事：

- 檢查 Stack top 兩個值的型別

- 如果是 int 就執行數值模運算

- 如果是 str 就執行格式化

- 如果是自定義物件，就呼叫 obj.__mod__() 方法

## Reference

https://blog.louie.lu/2017/04/11/python-%E5%BA%95%E5%B1%A4%E9%81%8B%E4%BD%9C-01-%E8%99%9B%E6%93%AC%E6%A9%9F%E5%99%A8%E8%88%87-byte-code/ 
[Python interpreter](https://hackmd.io/@naup96321/Bk1Cb_moC)

https://qingyunha.github.io/taotao/

https://naupjjin.github.io/2024/08/22/Python-interpreter/

https://blog.codingconfessions.com/p/cpython-vm-internals