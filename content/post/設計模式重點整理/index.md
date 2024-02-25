---
title: 設計模式重點整理
date: 2024-02-25
categories:
    - Design Pattern
tags:
    - Design Pattern
---

## Design Pattern 設計模式

解決重複性問題的模板

<hr />

## OOP 基本原則 - SOLID

### S: Single Responsibility Principle (SRP) 單一職責

一個類別只負責一件事

### O: Open/Close Principle (OCP) 開放 / 封閉原則

開放新增功能，封閉修改其他不相關的功能

### L: Liskov Substitution Principle (LSP) Liskov 替換原則

子類別應該要可以替換父類別，而不影響程式架構

### I: Interface Segregation Principle (ISP) 介面隔離原則

把不同功能從介面中分離出來

### D: Dependency Inversion Principle (DIP) 依賴反轉原則

高階模組不應該依賴低階模組，兩者都應該依賴抽象

<hr />

## Singleton 單例模式

保證一個類別只會產生一個物件，而且要提供存取該物件的統一方法

<hr />

## Simple Factory 簡單工廠模式

定義一個簡單工廠，傳入不同參數取得不同的類別物件

<hr />

>  [7天學會設計模式：設計模式也可以這樣學](https://www.books.com.tw/products/0010750585)