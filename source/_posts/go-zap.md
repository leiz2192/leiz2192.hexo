---
title: Go Zap
date: 2025-05-04 22:25:16
tags: log
categories: go
---

# 安装

```bash
go get go.uber.org/zap
```

# 配置

```go
type Config struct {
	FileName     string        `toml:"file-name"`
	Format       string        `toml:"format"`
	MaxSize      toml.Size     `toml:"max-size"`
	MaxBackups   int           `toml:"max-backups"`
	Level        zapcore.Level `toml:"level"`
}

func (c *Config) NewLogger(atomicLevel *zap.AtomicLevel) (*zap.Logger, error) {
    // 日志轮转
    lumberJackLogger := &lumberjack.Logger{
        Filename:   c.FileName,
        MaxSize:    int(c.MaxSize),
        MaxBackups: c.MaxBackups,
        Compress:   true,
    }

    encoder, err := newEncoder(c.Format)
    if err != nil {
        return nil, err
    }
    atomicLevel.SetLevel(c.Level)
    core := zapcore.NewCore(encoder, zapcore.AddSync(lumberJackLogger), atomicLevel)
    return zap.New(core, zap.AddCaller()), nil
}

func newEncoder(format string) (zapcore.Encoder, error) {
    config := newEncoderConfig()
    switch format {
        case "json":
            return zapcore.NewJSONEncoder(config), nil
        case "console":
            return zapcore.NewConsoleEncoder(config), nil
        case "logfmt":
            return zaplogfmt.NewEncoder(config), nil
        default:
            return nil, fmt.Errorf("unknown logging format: %s", format)
    }
}

func newEncoderConfig() zapcore.EncoderConfig {
    config := zap.NewProductionEncoderConfig()
    // 定义时间格式
    config.EncodeTime = func(ts time.Time, encoder zapcore.PrimitiveArrayEncoder) {
        encoder.AppendString(ts.UTC().Format(TimeFormat))
    }
    // 定义时间间隔精度
    config.EncodeDuration = func(d time.Duration, encoder zapcore.PrimitiveArrayEncoder) {
        val := float64(d) / float64(time.Millisecond)
        encoder.AppendString(fmt.Sprintf("%.3fms", val))
    }
    config.LevelKey = "lvl"
    return config
}
```

# 调用

```go
if cmd.Logger, logErr = config.Logging.NewLogger(&cmd.atomicLevel); logErr != nil {
    return fmt.Errorf("unable to configure logger: %w", logErr)
}

go func() {
    // 启动http服务，用于动态调整日志级别
    http.HandleFunc("/log/level", cmd.atomicLevel.ServeHTTP)
    http.ListenAndServe("localhost:6060", nil)
}()
```

# 动态调整级别

```bash
# 查看当前日志级别
curl -X GET localhost:6060/log/level
# 修改日志级别
curl -X PUT -H "Content-Type: application/x-www-form-urlencoded" localhost:6060/log/level?level=debug
```

# 参考

1、[https://pkg.go.dev/go.uber.org/zap](https://pkg.go.dev/go.uber.org/zap)
