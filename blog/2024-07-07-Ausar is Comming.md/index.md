---
slug: "Ausar is Comming"
title: "Ausar is Comming"
authors: ausar
tags: [hello, docusaurus]
---

## Introduction

Ausar is a light-weight script language written in pure Go with a dynamic type system. Its grammars are a deliberately simplified subset of the standard Go Spec. The original intention for this repo is to implement an expression engine as the core lib for the business rule engine and formula engine. 

### Why I Implement this Repo?

#### Close Relationship with My Working Scenario

I must emphasize that this repo is not a reimplementation of so many existed expresion engine libraries. I am working in a securities company with the high concern with the NUMERICAL ACCURACY SECURITY. For instance, the execution result of expr `0.1+0.2` must be `0.3` for my working scenarios. However, the mainstream of the existed open-source libararies fail to achieve this with taking the float number as its underlying number type. For float number, the execution result is ridiculous if the arithmetic operations are repeated with tremendous times. 

#### Every Number should Be Decimal

To solve the issue as demonstrated above, Ausar takes the [Decimal](https://github.com/shopspring/decimal) as the underlying type for numbers. Every inputted number would be transformed into the decimal type. Meanwhile, with the help of the famous [Go Decimal library](https://github.com/shopspring/decimal), every arithmetic operations of numbers would be converted into the corresponding decimal methods. For instance, the expr `0.1+0.2*0.3` would be eventually transformed into `Decimal(0.1).Add(Decimal(0.2).Mul(Decimal(0.3)))` during the execution phase. 

#### From an Expression Engine to a Simplified Language
The original purpose of the repo is to implement a simple expression executor for the Business rule engine. As same as the equivalent Rust version: [expression_engine_rs](https://github.com/ashyanSpada/expression_engine_rs) which I used to implement, the original version only supports some basic expressions like unary, binary, ternary expression and so on. The only complicated case is the chain expression with expressions separated by the punctuation ";" indicating that the expressions can be executed sequenctially. 

Things became complicated when the formula engine scenario arised. Let me show a brief example as below:


``` go
func max(a, b) {
    if a > b {
        return a;
    };
    return b;
}

wA := 0.5;
wB := 0.4;

hA := 0.6;
hB := 0.9;

// size is the multiplication result of the maximum one of wA and wB with the maximum one of hA and hB. 
size := max(wA,wB)*max(hA,hB);
```
This secnario requires more sophisticated features like function declarations and basic control flow. Eventually, the intention upgraded to the implementation of a light-weight script language. 

#### Enthusiasm to The Compiler Design

I love compiler design with the genuine passion to decode the string into the corresponding abstact syntax tree. I used to spend 7 to 8 months in the implementation of some well-known automated parsing algorithms, like LL, LR, Early, and PEG. Thankfully, I eventually achieved these goals. I had the aspiration to apply the knowledge into some realistic scenarios. This repo combines the techniques of handwritten parsers in my early work and automated parsers (mainly PEG) of my recent work.