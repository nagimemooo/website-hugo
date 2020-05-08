---
title: "[ Go 03 ] 使用 zap 框架印出 log"
date: 2020-05-08T23:51:23+08:00
lastmod: 2020-05-08T23:51:23+08:00
author: Nagi
cover: /img/go.png
categories: ["Go"]
tags: ["golang", "log"]
# showcase: true
draft: false
---

本章內容：
* 使用高性能zap框架配置
* 擁有log level配置 (常用debug/warn/info/error)
* 可以選擇印出在console或是文件（要外掛lumberjack去分割）
* zap有suger函式可以增加易用性，但犧牲效能
<!--more-->


雖然目前還未有高性能之需求，但這個框架還蠻易用的．

直接上程式碼

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