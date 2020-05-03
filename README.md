# Go异常处理
 
 ## 介绍
 
 用于Go语言中`error`的处理，避免在每次在返回值里面进行`err != nil`的判断，当`error`的错误信息可以直接展示并中断程序执行时，推荐使用`panic`的方法将错误抛出，在需要处理该`error`的调用层上进行捕获并处理。
 
 `exception`提供了抛出错误、捕获错误、推断错误及获取错误信息（code,msg,position）的方法。
 
 ## 示例
 ```go
// 抛出并捕获普通错误，所有普通error都由UnexpectedError捕获
err := errors.New("error")
exception.TryCatch(func() {
	exception.CheckError(err)
}, func(err exception.ErrorWrapper) {
	fmt.Printf("msg:%s  position:%s  code:%d\n", err.Error(), err.Position(), err.Code())
}, exception.UnexpectedError)


// 抛出并捕获exception构造错误，可使用error变量捕获
err2 := exception.New("root error", exception.RootError)
exception.TryCatch(func() {
	exception.CheckError(err2)
    // err2.Throw()
}, func(err exception.ErrorWrapper) {
	fmt.Printf("msg:%s  position:%s  code:%d\n", err.Error(), err.Position(), err.Code())
}, err2, exception.RootError) // 两种类型都可捕获


// 任何错误都可以使用RootError捕获
err3 := exception.New("unexpected error", exception.RootError)
exception.TryCatch(func() {
	exception.CheckError(err3)
    // err3.Throw()
}, func(err exception.ErrorWrapper) {
	fmt.Printf("msg:%s  position:%s  code:%d\n", err.Error(), err.Position(), err.Code())
}, exception.RootError)
```