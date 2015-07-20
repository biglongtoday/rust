##全类型

大多数类型都有一个特定的大小，以字节为单位，是在编译时可知。例如，一个32位大的 i32 或四个字节。然而，也有一些用于表达的类型，但没有指定的大小。这些被称为全类型或动态大小类型。有一个例子是[T]。这个类型代表了序列中一定数量的 T。但是我们不知道有多少，所以不知道大小。　　　　

Rust 使用许多这样的类型，但是他们有一些限制。有如下三种： 　　　　

1. 我们只能通过指针操作一个全类型的实例。一个 &[T] 就可以了，但[T]不可以。　　
2. 变量和参数没有动态大小类型。　　
3. 只有 struct 的最后一个字段可能有一个动态大小类型；其他字段没有。动态大小类型。　　　

所以，为什么要找麻烦呢？好吧，因为 [T] 只能在指针后使用如果我们没有语言支持全类型，就不可能写出下面代码：

    impl Foo for str {

或

    impl<TFoo for [T] {

相反，你必须这样写：

    impl Foo for &str {

这意味着，只能实现引用,而不是指针的其他类型。impl str，所有指针，包括（在某种程度上,首先有一些bug要修复）用户定义的定制智能指针，可以使用这个 impl。

###?Sized

如果你想写一个接受一个动态大小类型的函数，你可以使用特殊的约束，?Sized：

    struct Foo<T: ?Sized> {
    f: T,
    }

这个？，读作“T可能的大小”，意味着这个绑定很特别：它让我们匹配更多的种类,而不是更少。就像每一个 T 隐式包含 T: Sized，并且？可以撤销默认。