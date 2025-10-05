# logx

封装了Zap和lumberjack的日志库


```go
package test

import (
	"errors"
	"testing"

	"github.com/colin-404/logx"
)

func TestLoger(t *testing.T) {
	//no json
	logx.Println("test println")
	logx.Printfln("test printf: %v", "test")
	logx.Printf("test printf: %v", "test")

	//json
	logx.Info("test info json")

	logOpts := &logx.Options{
		//log path 日志文件路径,默认：./default.log
		LogFile: "logs/test.log",
		//log size 日志文件大小，单位：MB,默认：5
		MaxSize: 10,
		//log age 日志文件保存时间，单位：天,默认：3
		MaxAge: 30,
		//log backups 日志文件备份数量,默认：3
		MaxBackups: 10,

		//time format 日志时间格式,默认：EpochNanos
		TimeFormat: logx.TimeFormats.EpochNanos,
	}
	loger := logx.NewLoger(logOpts)
	logx.InitLogger(loger)

	err := errors.New("error")

	// info
	logx.Infof("logx: %v", err)

	// add msg to info log
	logx.Infomf("logx", "test: %v", err)
}


```

日志格式
```
2025/09/27 12:18:58 loger_test.go:12: test println
2025/09/27 12:18:58 loger_test.go:13: test printf: test
2025/09/27 12:18:58 loger_test.go:14: test printf: test{"level":"info","timestamp":"2025-09-27T12:18:58+08:00","source":"test/loger_test.go:15","msg":"test info json"}
{"level":"info","timestamp":"2025-09-27T12:18:58+08:00","source":"test/loger_test.go:37","msg":"info"}
{"level":"info","timestamp":"2025-09-27T12:18:58+08:00","source":"test/loger_test.go:40","msg":"infof: error"}
{"level":"info","timestamp":"2025-09-27T12:18:58+08:00","source":"test/loger_test.go:43","msg":"infomf","info":"test: error"}
```