# Golang notes

## [all goroutines are asleep - deadlock](https://stackoverflow.com/questions/19892732/all-goroutines-are-asleep-deadlock)

issue:

```
package main

import "fmt"
import "sync"
import "strconv"

func worker(wg *sync.WaitGroup, cs chan string, i int ){
    defer wg.Done()
    cs<-"worker"+strconv.Itoa(i)    
}

func monitorWorker(wg *sync.WaitGroup, cs chan string) {
    defer wg.Done()
    for i:= range cs {
            fmt.Println(i)
     }
}
func main() {
    wg := &sync.WaitGroup{}
    cs := make(chan string)

    for i:=0;i<10;i++{
             wg.Add(1)
             go worker(wg,cs,i)
    } 

    wg.Add(1)
    go monitorWorker(wg,cs)
    wg.Wait()
}
```

solution1:

```
package main

import (
    "fmt"
    "strconv"
    "sync"
)

func worker(wg *sync.WaitGroup, cs chan string, i int) {
    defer wg.Done()
    cs <- "worker" + strconv.Itoa(i)
}

func monitorWorker(wg *sync.WaitGroup, cs chan string) {
    wg.Wait()
    close(cs)
}

func main() {
    wg := &sync.WaitGroup{}
    cs := make(chan string)

    for i := 0; i < 10; i++ {
        wg.Add(1)
        go worker(wg, cs, i)
    }

    go monitorWorker(wg, cs)

    for i := range cs {
        fmt.Println(i)

    }
}
```

reason:

Your `monitorWorker` never dies. When all the workers finish, it continues to wait on cs. This deadlocks because nothing else will ever send on cs and therefore `wg` will never reach 0. A possible fix is to have the monitor close the channel when all workers finish. If the for loop is in main, it will end the loop, return from main, and end the program.

train of thought:

Your program has three parts that need to synchronize. 

First, all of your workers need to send the data. 

Then your print loop needs to print that data. 

Then your main function needs to return thereby ending the program. 

In your example, all the workers send the data, all the data gets printed, but the message is never sent to main that it should return gracefully.

solution2:

```
package main

import (
    "fmt"
    "strconv"
    "sync"
)

func worker(wg *sync.WaitGroup, cs chan string, i int) {
    defer wg.Done()
    cs <- "worker" + strconv.Itoa(i)
}

func monitorWorker(wg *sync.WaitGroup, cs chan string) {
    wg.Wait()
    close(cs)
}

func printWorker(cs <-chan string, done chan<- bool) {
    for i := range cs {
        fmt.Println(i)
    }

    done <- true
}

func main() {
    wg := &sync.WaitGroup{}
    cs := make(chan string)

    for i := 0; i < 10; i++ {
        wg.Add(1)
        go worker(wg, cs, i)
    }

    go monitorWorker(wg, cs)

    done := make(chan bool, 1)
    go printWorker(cs, done)
    <-done
}
```

## [Mixed named and unnamed function parameters](https://stackoverflow.com/questions/36449724/mixed-named-and-unnamed-function-parameters)

参数声明方式需要变量名

## 变量声明方式

`var ch chan any` 的值是nil

`wg := make(chan any)` 的值是地址

