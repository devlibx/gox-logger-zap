### How to use

```go
package main

import (
	"github.com/devlibx/gox-base/logger"
	"github.com/devlibx/gox-base/logger/tag"
	"github.com/devlibx/gox-logger-zap"
)

func main() {
	mainLogger := goxzap.NewZapLogger(goxzap.BuildZapLogger(logger.Config{
		Stdout:     true,
		Level:      "info",
		OutputFile: "stdout",
	}))
	mainLogger.Info("This is a error on parent level", tag.NewStringTag("userId", "user1"))

	// Sub logger
	subLogger := mainLogger.With(tag.NewInt("key", 10))
	subLogger.Error("logger which will also print stack trace")
	subLogger.Info("regular info log")
	subLogger.Debug("regular info debug")
}
```

```shell
2021-04-24T14:28:00.921+0530    info    This is a error on parent level {"userId": "user1", "logging-call-at": "main.go:15"}
2021-04-24T14:28:00.922+0530    error   logger which will also print stack trace        {"key": 10, "logging-call-at": "main.go:19"}
github.com/devlibx/gox-logger-zap.(*zapLogger).Error
        /Users/harishbohara/go/pkg/mod/github.com/devlibx/gox-logger-zap@v0.0.2/zap_logger.go:135
main.main
        /Users/harishbohara/go/src/awesomeProject/main.go:19
runtime.main
        /usr/local/Cellar/go/1.16.2/libexec/src/runtime/proc.go:225
2021-04-24T14:28:00.922+0530    info    regular info log        {"key": 10, "logging-call-at": "main.go:20"}
```