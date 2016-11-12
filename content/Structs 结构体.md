# 结构体

> [structs.md](https://github.com/rust-lang/rust/blob/master/src/doc/book/structs.md)
> <br>
> commit 6ba952020fbc91bad64be1ea0650bfba52e6aab4

结构体是一个创建更复杂数据类型的方法。例如，如果我们正在进行涉及到 2D 空间坐标的计算，我们将需要一个`x`和一个`y`值：

```rust
let origin_x = 0;
let origin_y = 0;
```

结构体让我们组合它们俩为一个单独，统一的数据类型：

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let origin = Point { x: 0, y: 0 }; // origin: Point

    println!("The origin is at ({}, {})", origin.x, origin.y);
}
```

这里有许多细节，让我们分开说。我们使用了`struct`关键字后跟名字来定义了一个结构体。根据传统，结构体使用大写字母开头并且使用驼峰命名法：`PointInSpace`而不要写成`Point_In_Space`。

像往常一样我们用`let`创建了一个结构体的实例，不过我们用`key: value`语法设置了每个字段。这里顺序不必和声明的时候一致。

最后，因为每个字段都有名字，我们可以访问字段通过圆点记法：`origin.x`。

结构体中的值默认是不可变的，就像 Rust 中其它的绑定一样。使用`mut`使其可变：

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let mut point = Point { x: 0, y: 0 };

    point.x = 5;

    println!("The point is at ({}, {})", point.x, point.y);
}
```

上面的代码会打印`The point is at (5, 0)`。

Rust 在语言级别不支持字段可变性，所以你不能像这么写：

```rust
struct Point {
    mut x: i32,
    y: i32,
}
```

可变性是绑定的一个属性，不是结构体自身的。如果你习惯于字段级别的可变性，这开始可能看起来有点奇怪，不过这样明显地简化了问题。它甚至可以让你使变量只可变一段临时时间：

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let mut point = Point { x: 0, y: 0 };

    point.x = 5;

    let point = point; // now immutable

    point.y = 6; // this causes an error
}
```

你的结构体仍然可以包含`&mut`指针，它会给你一些类型的可变性：

```rust
struct Point {
    x: i32,
    y: i32,
}

struct PointRef<'a> {
    x: &'a mut i32,
    y: &'a mut i32,
}

fn main() {
    let mut point = Point { x: 0, y: 0 };

    {
        let r = PointRef { x: &mut point.x, y: &mut point.y };

        *r.x = 5;
        *r.y = 6;
    }

    assert_eq!(5, point.x);
    assert_eq!(6, point.y);
}
```

## 更新语法（Update syntax）
一个包含`..`的`struct`表明你想要使用一些其它结构体的拷贝的一些值。例如：

```rust
struct Point3d {
    x: i32,
    y: i32,
    z: i32,
}

let mut point = Point3d { x: 0, y: 0, z: 0 };
point = Point3d { y: 1, .. point };
```

这给了`point`一个新的`y`，不过保留了`x`和`z`的值。这也并不必要是同样的`struct`，你可以在创建新结构体时使用这个语法，并会拷贝你未指定的值：

```rust
# struct Point3d {
#     x: i32,
#     y: i32,
#     z: i32,
# }
let origin = Point3d { x: 0, y: 0, z: 0 };
let point = Point3d { z: 1, x: 2, .. origin };
```

## 元组结构体
Rust有像另一个[元组](Primitive Types 原生类型.md#tuples)和结构体的混合体的数据类型。元组结构体有一个名字，不过它的字段没有。他们用`struct`关键字声明，并元组前面带有一个名字：

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

这里`black`和`origin`并不相等，即使它们有一模一样的值：

```rust
let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

使用结构体几乎总是好于使用元组结构体。我们可以这样重写`Color`和`Point`：

```rust
struct Color {
    red: i32,
    blue: i32,
    green: i32,
}

struct Point {
    x: i32,
    y: i32,
    z: i32,
}
```

现在，我们有了名字，而不是位置。好的名字是很重要的，使用结构体，我们就可以设置名字。

不过有种情况元组结构体非常有用，就是当元组结构体只有一个元素时。我们管它叫*新类型*（*newtype*），因为你创建了一个与元素相似的类型：

```rust
struct Inches(i32);

let length = Inches(10);

let Inches(integer_length) = length;
println!("length is {} inches", integer_length);
```

如你所见，你可以通过一个解构`let`来提取内部的整型，就像我们在讲元组时说的那样，`let Inches(integer_length)`给`integer_length`赋值为`10`。

## 类单元结构体（Unit-like structs）
你可以定义一个没有任何成员的结构体：

```rust
struct Electron;

let x = Electron;
```

这样的结构体叫做“类单元”因为它与一个空元组类似，`()`，这有时叫做“单元”。就像一个元组结构体，它定义了一个新类型。

就它本身来看没什么用（虽然有时它可以作为一个标记类型），不过在与其它功能的结合中，它可以变得有用。例如，一个库可能请求你创建一个实现了一个特定特性的结构来处理事件。如果你并不需要在结构中存储任何数据，你可以仅仅创建一个类单元结构体。
