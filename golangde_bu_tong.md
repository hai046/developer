# golang的不同点

##随便说说

先看个例子里面注释：
```
package main
import (
	"fmt"
	"io/ioutil"
	"path"
	"os"
)//多个import,如果没有不要导入  因为声明了 不使用  golang会报错

func readFile(f string) (string, error) {//声明想java倒过来  哈哈 ,开始有点不习惯

	file, er := os.Open(f);
	defer file.Close(); //defer关键字NB之处: 他在执行该方法最后执行,然后在返回值.你可以把他当成java里面的finally部分代码.
	if er != nil {    //"{ "必须在这后,不然报错,语法规定
		panic("open file err")//抛异常退出
	}

	content, err := ioutil.ReadAll(file); //哈哈哈 多返回值 nb不?!!
	if err != nil {
		panic(err)
	}

	return string(content), nil; //content转成string,多值返回
}

func findFile(dir string) (string, error) {
	fs, err := ioutil.ReadDir(dir);
	if err != nil {
		panic("open file err")
	}


	for _, info := range fs {//多值返回  如果用"_"就不用处理,如果不是那么必须处理,golang里面 不能声明了 不使用,

		f := path.Join(dir, info.Name())

		if info.IsDir() {
			fmt.Println("find dir", f)
			findFile(f)
		}else {
			cont, _ := readFile(f);
			fmt.Println("find file \n", cont)
		}
	}

	return "", nil;
}

func main() {

	dir := "/Users/haizhu/Documents/web_workspace/jiemo-golang/src/test"//声明  也可这样写  var dir string=""
	findFile(dir)
}


```

1，大括号 "{" 必须在if for 等关键字最后  不能在行首位

2，defer 功效是在method 都执行玩但是在返回值前处理

3，多返回值

4，声明多少使用多少，不能多声明不使用

5，