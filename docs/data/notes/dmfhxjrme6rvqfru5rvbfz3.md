
### Goroutine
Each execution path of our Go program is a *goroutine*. 
- Therefore, we always have at least 1 (the `main` goroutine).

If we prepend a function invocation with the keyword `go`, that function won't wait to finish executing before moving on to the next line.

The following will run `count("sheep")` in the background, then immediately execute `count("fish")`, thereby creating a *goroutine*, which runs concurrently.
```go
go count("sheep")
count("fish")

// 1 sheep
// 1 fish
// 2 sheep
// 2 fish
// etc.
```

imagine both of our `count()` functions are called in a background goroutine:
```go
func main() {
  go count("sheep")
  go count("fish")
}
```
- in this case, we would not see any logs in the console, since the `go` keyword tells the `count()` function to run in the background and move to the next line. Since that was the last line, the `main()` function finishes and our program exits.
  - if we added `time.Sleep(time.Second * 2)` as the last line in our `main()` function, then we would see logs for 2 seconds before the program exits.
- a better solution here is to use a *WaitGroup* (which is basically just a counter):
```go
import "sync"

func main() {
  var wg sync.WaitGroup
  // increment the WG by 1 to incicate that we have 1 goroutine 
  // to wait for before `main()` finishes executing
  wg.Add(1)

  go func() {
    count("sheep")
    // decrement the WG when the goroutine finishes.
    wg.Done()
  }

  // wait until the counter is 0 (ie. wait until all goroutines have finished)
  wg.Wait()
}
```

Goroutines are very efficient
- we can make 1000s of simultaneously running goroutines.
- however, ultimately we are constrained by how many cores our [[CPU|hardware.cpu]] has.

### Channel
A *channel* is a means for goroutines to communicate with each other.
- ex. we have a value in one goroutine that we want to pass to the `main` goroutine.
  - to do this, we can modify our `count()` function to accept a channel as an argument:
```go
func main() {
  // make the channel
  c := make(chan string)
  go count("sheep", c)

  // iterate over the range (spec: length, as if it were an array) of a channel
  for msg := range c {
    fmt.Println(msg)
  }

  /* The above for-loop is syntactic sugar for the long-form: 
  * for msg := range c {
  *   // receive the message from the channel and set it to `msg`
  *   msg, open := <- c
  *
  *   // if channel is not open, break out of for loop (stop receiving messages)
  *   if !open {
  *     break
  *   }
  *
  *   fmt.Println(msg)
  * }
  */
}
func count(animalType string, c chan string) {
  for i: 0; i <= 5; i++ {
    // send the value of `animalType` over the channel
    c <- animalType
    time.Sleep(time.Millisecond * 500)
  }

  // close the channel once the for loop has finished
  close(c)
}
```
- note: sending and receiving messages through channels are blocking operations; code execution will stop until a value is sent/received through the channel.
- note: channels have a type (here, `string`), meaning the only messages we can pass through those channels are strings.
  - we can even type channels as channels (ie. channels that only accept messages with the type `channel`)
    - spec: `c chan chan`, or `c chan channel` maybe?
- note: naturally, only senders of messages should close channels, since they are the ones that know whether or not the data flow has finished.

We can use channels to synchronize goroutines.
- ex. Imagine we have 2 goroutines, and gr1 depends on gr2 (gr1 receives a message from gr2 via a channel)
![](/assets/images/2022-12-20-11-25-19.png)
  - here, execution of gr1 will pause at line 7 as it waits for gr2 to reach its line 4, where it will send a message through the channel.

Channels must have a goroutine ready to receive a message from them *before* anything can be sent through them.
- in other words, if we are within the `main` goroutine and our code executes `c <- "hello"` before it executes `msg := <- c`, our program will exit in error, since we are trying to send something to the `c` channel *before* anything is set up to listen to it (recall: sending/receiving from a channel is a blocking operation until it can be completed)

Channel references can be specified to restrict us from only ever receiving/sending messages:
- ex. we make a function that takes 2 channels:
  - 1. a channel of jobs to do (from which we will only ever receive messages; `<-chan`)
  - 2. a channel to send results to (to which we will only ever send messages; `chan<-`)
```go
func worker(jobs <-chan int, results chan<- int) {
  for n := range jobs
}
```
- if we try and send a message to the `jobs` channel, we will get a compile-time error

#### Buffered Channel
A buffered channel can be filled without a corresponding receiver, and it will not block until that buffer is full.
- Buffered channels have a fixed capacity set when they are initialized.

```go
func main() {
  // make a buffered channel of strings with a capacity of 2
  c := make(chan string, 2)
  c <- "hello"
  c <- "world"

  msg := <- c
  fmt.Println(msg) // hello

  msg = <- c
  fmt.Println(msg) // world
}
```

#### `select`
the `select` keyword allows us to receive a message from whatever channel has one:

ex. imagine we have 2 goroutines that each do some calculation then return it to the `main` function where it then gets logged to the console. We set up each function to do the calculation, then send the data through their own respective channel. Back in the `main` function, we create a loop to log to the console each time a new message arrives in the channel):
```go
main() {
  for {
    select {
      case msg1 := <- c1:
        fmt.Println(msg1)
      case msg2 := <- c2:
        fmt.Println(msg2)
    }
  }
}
```

### Worker Pool
A worker pool is a queue of jobs to be done, from which multiple concurrent workers can pull jobs and perform them.

```go
func main() {
  // no real reason why we have a buffer of 100; it's just a nice round and large enough number
  jobs := make(chan int, 100)
  results := make(chan int, 100)

  // create a worker as a concurrent goroutine
  go worker(jobs, results)

  for i := 0; i < 100; i++ {
    // fill up the jobs channel with numbers from 0-99
    // since it's a buffered channel, it's not going to block
    jobs <- i
  }
  close(jobs)

  for j := 0; j < 100; j++ {
    // receive each fibonnaci number from the `results` channel and print to console
    fmt.Println(<-results)
  }
}

func worker(jobs <-chan int, results chan<- int) {
  // as long as there are jobs on the `jobs` channel, the calculation will continue to run
  for n := range jobs {
    results <- fibonnaci(n)
  }
}

func fibonacci(n int) int {
  if n <= 1 {
    return n
  }

  return fibonacci(n - 1) + fibonacci(n - 2)
}
```


* * *

[[see: Concurrency main article|general.concurrency]]
