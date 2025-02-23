---

title: rust
author: codefish
date: 2025-2-23 22:08:07
categories: rust
tags: [rust ownership]
top_img: /img/basketball.jpg
cover: /img/basketball.jpg

---

### 基础概念
cargo 是rust的包管理工具
rustc 是rust的编译器
rustdoc 是rust的文档生成工具
rustfmt 是rust的代码格式化工具
rust-analyzer 是rust的智能补全工具

#### cargo 
cargo 是rust的包管理工具
cargo build 编译项目
cargo run 运行项目
cargo check 检查项目
cargo fmt 格式化代码
cargo clippy 检查代码
cargo doc 生成文档
cargo build --release 编译项目（优化）
cargo clean 清理项目
cargo update 更新依赖
cargo add 添加依赖
cargo remove 删除依赖


### 1. 简单的hello

```rust
use ferris_says::say;
use std::io::{stdout, BufWriter};

fn main() {
    let out = "Hello fellow Rustaceans!";
    let width = 24;

    let mut writer = BufWriter::new(stdout());
    say(out, width, &mut writer).unwrap();
}
```

`
cargo run 
`
#运行src/main.rs

也可以手动执行特定文件
`cargo run -- --src main1.rs`
或者
`cargo run ./src/main1.rs`

--bin 指定运行哪个二进制文件



std::io::{stdout, BufWriter};
std::io 为rust自带的标准库

提供 
1. 标准输入输出：如 stdin()、stdout()、stderr()
文件操作：如 File 类型用于读写文件
3. 缓冲读写：如 BufReader、BufWriter
错误处理：如 Error 类型
其他工具：如 Cursor、Seek 等

```rust
   let name = "Rust";
     println!("Hello, {}!", name);
```
println! 是rust的标准库提供的宏，用于打印信息到控制台
println! 后面的感叹号表示它是一个宏，用于格式化输出。宏是 Rust 中非常强大的工具，可以处理比普通函数更复杂的逻辑。


### 2. 第一个关键点, 所以权系统
```rust
use std::io;

fn main() {
    println!("Guess the number!");

    println!("Please input your guess.");1

    // let apples = 5; // immutable
    // let mut bananas = 5; // mutable
    
    let mut guess = String::new(); //创建一个mut的变量为一个null的string

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");
    // &mut guess as the argument to read_line to tell it what string to store the user input in.
    println!("You guessed: {}", guess);
}

```
关键内容在`&mut guess` 这是一个对guess的引用，& 表示引用，mut 表示可变引用。, 如果传递的是不可变引用(&guess)，则不能修改guess的值。, 

**Rust 的所有权系统确保内存安全。通过传递可变引用（&mut guess），read_line 可以借用 guess 并修改它，而不会获取 guess 的所有权。
这样，guess 的所有权仍然在 main 函数中，read_line 只是临时借用它。**

声明数字类型
`let guess_number: u32 = 42;`



### 3. Mut 声明

#### 1.关键点

1. 是否可变
2. 是否可修改

声明变量的时候, 使用mut, 代表是**可变变量**, 可以修改, 使用可变变量的时候, &mut是可以修改原本的值, 而&value只能访问

Eg: 

```js
// 变量声明
let x = 5;          // 不可变变量
// x = 6;          // 错误！不能修改不可变变量

let mut y = 5;      // 可变变量
y = 6;              // 正确，可以修改

// 引用
let mut value = String::from("hello");
let ref1 = &value;         // 不可变引用
let ref2 = &mut value;     // 可变引用，可以通过这个引用修改 value
```



**注意**

增加引用之后就要使用引用值, 没增加引用的话可以直接使用, **当一个值被可变借用时，在这个借用的生命周期内，我们不能直接修改原值。**

❌

```rust
fn misxTable(){
  let mut value = 1;
  let mutValue = &mut value;
  value = 2;
}
// # 第三行报错
`value` is assigned to here but it was already borrowed
```

此处第二个错误 fn misxTable(){
   |    ^^^^^^^^^ help: convert the identifier to snake case: `misx_table`

**这是 Rust 的命名规范警告。在 Rust 中，函数名应该使用蛇形命名法（snake_case）**

✅

```rust
fn correct_fn(){
    let mut value = 1;
    let mut_value = &mut value;
    *mut_value = 2; //解引用操作符号
    println!("mut_value: {}", mut_value);
  }
```



#### 2. 如何改变可变引用值

- 方法1: 使用解引用操作符号

```rust
fn current_eg(){
  let mut value = 1;
  let mut_value = &mut value;
  *mut_value = 2;
  println!("user * to change value mut: {}", mut_value)
}
// * 在此用作解决引用, 加次符号后, 可以修改mut_value的值, 但是value还是不能使用, 其他作用此处不做解释, 后面学到了再说
```

- 方法2: 通过块级作用域

```rust
fn othen_eg(){
  let mut value = 2;
  {
    let mut_vallue = &mut value;
    *mut_value = 3;
  }
  println("use this to change mut_value: {}", value)
}
// 此处利用*和块级别作用域名, 修改了value的值, 引用只在mut_value里面生效
```



#### Eg1: 实现一个简单的计算器

```rust
use std::io;

fn main(){
    print_fn()
}

fn handle_operation(code: &str, num1: i32, num2: i32) -> i32 {
    println!("code:{}", code);
    match code.trim() {
        "+" => add(num1, num2),
        "-" => sub(num1, num2),
        "*" => mul(num1, num2),
        "/" => div(num1, num2),
        _ => 0,
    }

}

fn add(a: i32, b:i32) -> i32 {
    // Rust 中函数的返回值可以是最后一个表达式（不加分号）
    a + b
}

fn sub(a: i32, b:i32) -> i32 {
    a - b
}

fn mul(a: i32, b: i32) -> i32 {
    a * b
}

fn div(a: i32, b: i32) -> i32 {
    a / b
}

fn judgeCode(code: &str) -> bool {
    match code.trim() {
        "=" => true, // 匹配任意一个
        _ => false,
    }
}

fn print_fn() {
    let mut pause = String::from("+");
    let mut switch_flag = false;
    let mut mul_number = String::new();
    let mut result = 0;
    
    while pause.trim() != "=" {
        println!("pause: {}", pause);
        match switch_flag {
            true => {
                println!("now need input a mul code(+-*/):");
                pause.clear(); 
                io::stdin().read_line(&mut pause).expect("读取失败");
                switch_flag = false;
            },
            false => {
                println!("now need input a number:");
                io::stdin().read_line(&mut mul_number).expect("读取失败");
                
                let number: i32 = mul_number.trim().parse().expect("请输入数字");
                
                result = handle_operation(&pause, result, number);
                
                switch_flag = true;
                mul_number.clear();
            },
        }
    }
    println!("最终结果: {}", result);
}
```

#####  知识点

1. 接受输入的时候需要trim
2. 函数最后一个不带;的表达式, 是返回值
3. 
4. 





#### 3. 如何执行强制类型转换



1. let number: i32 = mul_number.trim().parse().expect("请输入数字");

   ```rust
   let number: i32 = mul_number   // 从用户输入的字符串开始
       .trim()                    // 1. 去除首尾空白（包括换行符）
       .parse()                   // 2. 尝试解析为指定类型
       .expect("请输入数字");      // 3. 处理解析结果
   ```

   **等效于**

   ```rust
   let number **=** mul_number**.**trim()**.**parse**::**<*i32*>()**.**expect("请输入数字");
   ```

   

2. match转换

   ```rust
   let number:i32 =match guess_num.trim().parse() {
               Ok(num) => num,
               Err(_) => {
                   println!("请输入数字！");
                   continue;
               }
           };
   ```

   `match`处理错误更加友好, 不会中断

3. 1



*// 创建包含初始值 "+" 的字符串（已分配内存）*

let with_plus **=** String**::**from("+"); 

*// 创建空字符串（尚未分配堆内存）*

let empty **=** String**::**new();



**fn** print_fn() {

​    *// 当需要初始操作符时（推荐当前案例使用这个）*

​    let *mut* pause **=** String**::**from("+");

​    

​    *// 当需要全新空缓冲区时（适合接收用户即时输入）*

​    let *mut* input_buffer **=** String**::**new();

​    

​    *// ... 其余代码 ...*

}



#### 4.数据类型

```rust
let bool:bool = true;          // 布尔类型，true或false
    let int:i32 = 1;               // 32位有符号整数
    let float:f32 = 1.0;           // 32位浮点数
    let str:String = "hello".to_string(); // String类型（堆分配的UTF-8字符串）
    let char:char = 'h';           // Unicode标量值（4字节）
    let array:[i32;5] = [1,2,3,4,5]; // 固定长度数组（i32类型，5个元素）
    let tuple:(i32,f32,char) = (1,1.0,'h'); // 元组（可包含不同类型）
    let slice:&[i32] = &array[0..2]; // 不可变切片（数组的视图）
    let mut_slice:&mut [i32] = &mut array[0..2]; // 可变切片（需要数组可变）
```



#### 5.逻辑 if else

❌

```rust
fn main() {
    let number = 3;

    if number {
        println!("number was three");
    }
}
```

**if后面必须是bool**



✅

```rust
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```



###### 1. **三元替代**

```rust
const a = if(10) {10} else {-10}
```

if 和 else 后面的类型需要一致



###### 2.match

```rust
let number:i32 =match guess_num.trim().parse() {
            Ok(num) => num,
            Err(_) => {
                println!("请输入数字！");
                continue;
            }
        };
```



###### eg2

```rust
use rand::Rng;
use std::io;
use std::cmp::Ordering;
fn main(){



    // match number.cmp(&random_num){
    //     Ordering::Less => println!("too small"),
    //     Ordering::Greater => println!("too big"),
    //     Ordering::Equal => println!("you win"),
    // }
    
    loop {
        println!("gugess a number: (0-10)");
        let random_num = rand::thread_rng().gen_range(0..=10);
        let mut guess_num = String::new();
        io::stdin().read_line(&mut guess_num).expect("failed to read line");
        let number:i32 =match guess_num.trim().parse() {
            Ok(num) => num,
            Err(_) => {
                println!("请输入数字！");
                continue;
            }
        };

        match number.cmp(&random_num){
            Ordering::Less => {
                println!("too small: {}", random_num);
            },
            Ordering::Greater => {
                println!("too big: {}", random_num);
            },
            Ordering::Equal => {
                println!("you win");
                break;
            }
        }
    }

}

// 常见错误 stdin => stdio
// 类型转换  
// &mut 和 & 的区别 后者为引用
```

