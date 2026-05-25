---
title: "不完全 Minecraft 局域网服务器搭建指南"
date: 2023-10-16T22:47:35+08:00
description: 
image: ./images/4featureimg/
comments: true
math: true
categories: 
  - 技术
tags:
  - Minecraft
draft: true
---

## 前言

起因是突然想和室友玩 Minecraft，然而 Minecraft 自带的局域网联机效果实在太烂了，而且由于本身处于同一局域网下，所以就进行一个服务器的搭。

首先：我既不懂 Java 也不懂 Minecraft ，本篇文章参考自[Minecraft Wiki](https://zh.minecraft.wiki/w/%E6%95%99%E7%A8%8B/%E6%9E%B6%E8%AE%BE%E6%9C%8D%E5%8A%A1%E5%99%A8)，如有错误也请麻烦指出，我什么都会做的。

本指南仅适用于 Windows 下的 Java 版本。

## 准备

1. 首先，准备一台高于以下推荐配置的PC/服务器：

|需求|玩家|CPU|RAM|ROM|
|---|---|---|---|---|
|最低|2-4|Intel Core 2 Duo/AMD Athlon 64 x2|2GB|> 150 MB|
|推荐|2-4|Intel Core 2 Duo/AMD Athlon 64 x2|3GB|> 300 MB|

> 需要注意的是 Wiki 上的这个配置推荐似乎是上古时代的，现代的 PC 应该是随便跑了


至于系统似乎没什么要求，常见的 Windows 或是 Linux 都没啥问题，我这里使用的是 Windows 11。


2. 下载Java

我首先推荐在 shell 中输入 `java -version` 避免重复安装。

倘若先前没有安装过 Java，那么可以去[Java 官网](https://www.java.com/zh-CN/download/)下载

> 推荐安装的是 JDK，但是装 JRE 或者 OpenJDK 之类的都是可以的，详见[这里](https://zh.minecraft.wiki/w/Tutorial:%E6%9E%B6%E8%AE%BE%E6%9C%8D%E5%8A%A1%E5%99%A8#Java)


3. 下载服务器核心


