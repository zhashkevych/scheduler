Go Scheduler
================================

## <a href="https://medium.com/@zhashkevych/simple-scheduling-library-golang-384ad72717f5">Blog Post</a>

Go Scheduler helps you to manage functions that should be executed every N seconds/minutes/hours etc.

See it in action:

## Example #1

```go
package main

import (
  "fmt"
  "time"
  "os"
  "os/signal"
  "context"
  "github.com/zhashkevych/scheduler"
)

func main() {
	ctx := context.Background()

	worker := scheduler.NewScheduler()
	worker.Add(ctx, parseSubscriptionData, time.Second*5)
	worker.Add(ctx, sendStatistics, time.Second*10)

	quit := make(chan os.Signal, 1)
	signal.Notify(quit, os.Interrupt, os.Interrupt)

	<-quit
	worker.Stop()
}

func parseSubscriptionData(ctx context.Context) {
	time.Sleep(time.Second * 1)
	fmt.Printf("subscription parsed successfully at %s\n", time.Now().String())
}

func sendStatistics(ctx context.Context) {
	time.Sleep(time.Second * 5)
	fmt.Printf("statistics sent at %s\n", time.Now().String())
}

```

## Example #2

```go
package main

import (
	"context"
	"fmt"
	"os"
	"os/signal"
	"time"

	"github.com/zhashkevych/scheduler"
)

func main() {
	ctx := context.Background()

	sc := scheduler.NewScheduler()
	sc.Add(ctx, testFunc, time.Second*2)

	quit := make(chan os.Signal, 1)
	signal.Notify(quit, os.Interrupt)

	<-quit
	worker.Stop()
}

func testFunc(ctx context.Context) {
	ctx, _ = context.WithTimeout(ctx, time.Second*5)

	i := 0
	for {
		time.Sleep(time.Millisecond * 100)
		i++
		fmt.Printf("%d ", i)

		select {
		case <-ctx.Done():
			fmt.Println()
			return
		default:
		}
	}
}
```
