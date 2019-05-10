Go Scheduler
================================

Go Scheduler helps you to manage functions that should be executed every N seconds/minutes/hours etc.

See it in action:

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
	worker.Add(ctx, parseLatestPosts, time.Second*5)
	worker.Add(ctx, collectStatistics, time.Second*10)

	time.AfterFunc(time.Minute*1, worker.Stop)

	quit := make(chan os.Signal, 1)
	signal.Notify(quit, os.Interrupt, os.Interrupt)

	<-quit
}

func parseLatestPosts(ctx context.Context) {
	time.Sleep(time.Second * 1)
	fmt.Printf("latest posts parsed successfuly at %s\n", time.Now().String())
}

func collectStatistics(ctx context.Context) {
	time.Sleep(time.Second * 5)
	fmt.Printf("stats updated at %s\n", time.Now().String())
}

```