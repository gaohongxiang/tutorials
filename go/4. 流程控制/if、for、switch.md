## for

带range子句的for语句
```
for i, e := range 被迭代的变量 {}

for i := range 被迭代的变量 {}
```

迭代变量i代表当次迭代对应的元素值的索引值。迭代变量e代表当次迭代对应的某一个元素值。

当for语句执行时，range关键字右边的表达式（range表达式）会先被求值。range表达式的结果可以是数组、数组的指针、切片、字符串、字典、或者允许接收操作的通道中的某一个，并且结果值只能有一个。

当只有一个迭代变量时，如果被迭代的变量是数组、数组的指针、切片和字符串，那么它们的元素值都是无处安放的，迭代变量代表当次迭代对应的元素值的索引值。

range表达式只会在for语句开始执行时被求值一次，无论后面会有多少次迭代。range表达式的求值结果会被复制，即被迭代对象是range表达式结果值的副本而不是原值。

```
func main() {
	//数组
	nub := [...]int{1,2,3,4,5,6}
	for i, e := range nub {
		if i == len(nub)-1 {
			nub[0] += e
		}else {
			nub[i+1] += e
		}
	}
	fmt.Println(nub) //[7 3 5 7 9 11]
}
```
数组是值类型的，range表达式的结果是原数组的副本，跟原数组无关了。
```
func main() {
	//切片
	nub := []int{1,2,3,4,5,6}
	for i, e := range nub {
		if i == len(nub)-1 {
			nub[0] += e
		}else {
			nub[i+1] += e
		}
	}
	fmt.Println(nub) //[22 3 6 10 15 21]
}
```
切片是引用类型的，range表达式的结果跟原数组依然关联。改变一个，另一个也会改变。

## switch语句

#### 类型问题

Go语言中只有类型相同的值才能进行判等操作。

case表达式的所有子表达式的结果值都是要与switch表达式的结果值判等的，因此，它们的类型必须相同或者能够统一到switch表达式的结果类型。否则这条switch语句就无法通过编译。

switch表达式若是无类型值，那么会自动转换。如整数常量会转换为int类型，浮点数会转换为float64类型。case表达式如是无类型值，那么会转换为switch表达式的类型。

例1
```
func main() {
	v := [...]int8{1,2,3}
	switch v[1] {
		case 1,2:
			fmt.Println("1 or 2")
		case 3:
			fmt.Println("3")
	}
}
```
switch表达式的结果值是int8类型的，case表达式中子表达式的结果类型是无类型的常量值，它们的类型会被自动转换为switch表达式的结果类型即int8类型（上述的整数值在int8的范围内，可以转换）。所以，此switch语句可以通过编译。

例2
```
func main() {
	v := [...]int8{1,2,3}
	switch 2 {
		case v[0],v[1]:
			fmt.Println("1 or 2")
		case v[2]:
			fmt.Println("3")
	}
}
```
switch表达式的结果值是无类型的常量值，被自动转换为此常量默认类型 int类型 的值（如果是浮点数默认为float64类型），而case表达式中子表达式的结果类型是int8，它们的类型不同，因此为不能通过编译

对由字面量直接表示的子表达式而言，同一条switch语句的所有case表达式的子表达式的结果值不能重复。

#### case表达式结果值重复问题

switch语句在case子句的选择上具有唯一性，因此，对由字面量直接表示的case子表达式而言，不允许它们的结果值存在相等的情况。
```
func main() {
	v := [...]int8{1,2,3}
	switch v[1] {
		case 1, 2:
			fmt.Println("1 or 2")
		case 2, 3:
			fmt.Println("2 or 3")
	}
}
```
上例中字面量表示的case子表达式中存在值相同的情况，无法通过编译

```
func main() {
	v := [...]int8{1,2,3}
	switch v[1] {
		case v[0], v[1]:
			fmt.Println("1 or 2")
		case v[1], v[2]:
			fmt.Println("2 or 3")
	}
}
```
上例中case表达式不是字面量表示的，虽然值存在相同的情况，但可以通过编译，第一条符合判等的表达式会被选中。

#### case子句的编写顺序问题

case子句的编写顺序很重要，最上面的case子句的子表达式总是会被最先求值，最先判等。