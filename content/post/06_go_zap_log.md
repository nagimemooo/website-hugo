---
title: "[Go 06] 使用 zap 框架印出 log"
date: 2020-05-18T23:51:23+08:00
lastmod: 2020-05-18T23:51:23+08:00
author: Nagi
cover: /img/go.png
categories: [" 後端 go"]
tags: ["golang", "log"]
draft: false
---

本章內容：
* 實作使用高性能zap log框架
* 擁有log level配置 (常用debug/warn/info/error)與程式碼位置
* 可以選擇印出在console或是文件（要外掛lumberjack去分割）
* zap有suger函式可以增加易用性，但犧牲效能
<!--more-->


> 雖然目前還未有高性能之需求，網路上也有很多不同的ＬＯＧ框架可以選擇，有興趣可以看這篇[在Github中stars数最多的Go日志库集合](https://my.oschina.net/u/168737/blog/1536117 "在Github中stars数最多的Go日志库集合")，看了各框架說明介紹，這個框架實作上看來蠻容易的，今天就還試試看．

直接上實作完之程式碼  

```
package logger

import (
	"mywork/demo/internal/config"
	"os"
	"strings"
	"time"
	"go.uber.org/zap"
	"go.uber.org/zap/zapcore"
	"gopkg.in/natefinch/lumberjack.v2"
)


var Logger *zap.Logger
var Sugar *zap.SugaredLogger

func InitLogger() {

	logWriter := []zapcore.WriteSyncer{zapcore.AddSync(os.Stdout)}

	if config.Configuration.Logger.File != "" {
		hook := setFileWriter(config.Configuration.Logger.File)
		logWriter = append(logWriter, hook)
	}
	encoderConfig := setEncoder()

	level := getLogLevel(config.Configuration.Logger.Level)

	core := zapcore.NewCore(encoderConfig,
		zapcore.NewMultiWriteSyncer(logWriter...),
		level)

	Logger = zap.New(core, zap.AddCaller()) //印出log的位置與號碼
	defer Logger.Sync()
	Logger.Debug("debug print is ok")
	Logger.Info("InitLogger Msg:",
		zap.String("url", "uedf"),
		zap.Int("attempt", 3),
		zap.Duration("backoff", time.Second),
	)
	Sugar = Logger.Sugar()
	Sugar.Infof("InitLogger Success! level = %s ", level.String())

}

func setEncoder() zapcore.Encoder {
	encoderConfig := zap.NewProductionEncoderConfig()
	encoderConfig.EncodeTime = zapcore.ISO8601TimeEncoder
	encoderConfig.EncodeLevel = zapcore.CapitalLevelEncoder
	return zapcore.NewConsoleEncoder(encoderConfig)
}

func setFileWriter(filePath string) zapcore.WriteSyncer {
	lumberJackLogger := &lumberjack.Logger{
		Filename:   filePath,
		MaxSize:    1,
		MaxBackups: 5,
		MaxAge:     30,
		Compress:   false,
	}
	return zapcore.AddSync(lumberJackLogger)
}

func getLogLevel(lv string) zapcore.Level {
	lv = strings.ToLower(lv)
	if level, ok := levelMap[lv]; ok {
		return level
	}
	return zapcore.InfoLevel
}

var levelMap = map[string]zapcore.Level{
	"debug":  zapcore.DebugLevel,
	"info":   zapcore.InfoLevel,
	"warn":   zapcore.WarnLevel,
	"error":  zapcore.ErrorLevel,
}



```