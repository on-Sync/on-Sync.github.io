---
layout: post
current: post
navigation: True
title: Computer Science (1) - 컴퓨터 과학은 약속입니다.
date: 2023-03-05
tags: ["Computer"]
class: post-template
subclass: 'post'
author: on-Sync
---

# 컴퓨터 과학의 기초

> 컴퓨터과학의 시작과 끝은 표현에 대한 약속입니다.
> 이는 컴퓨터를 이루는 하드웨어인 논리회로가 전기신호를 기반으로 구성되어 있다는 것에서 시작됩니다.
> 논리회로는 전기신호를 하드웨어가 인식하는 일정 역치에 따라 두 가지 상태로 분류합니다.
> 이를 전기적으로는 On/Off 인 두 가지의 상태로 분류하게 되는데, 우리는 이를 가장 작은 데이터 단위인 비트라고 부릅니다.
> 이는 두 가지 상태, 즉 두가지 경우의 수만 인식할 수 있다는 얘기와 같습니다.
> 컴퓨터과학에서는 이 두가지 상태를 나타내는 저장공간을 데이터의 가장 작은 단위인 Bit 라고 부릅니다.
> 이 비트를 이용하여 우리가 사는 자연계를 표현하기 위해서 여러 Bit 에 저장된 경우의 수를 데이터의 유형에 따라 해석방식하도록 약속하게 됩니다.

## 데이터의 유형

> 데이터의 유형은 크게 논리, 숫자, 문자로 나뉘게 됩니다.
> 위 세가지 유형의 해석방식을 간단하게 설명하면 다음과 같습니다.

### Boolean, 논리적 표현

> 논리적 표현은 단일 Bit 에 의해 해석되는 가장 작은 규모의 표현방식입니다.
> 논리값은 True/False 로 나뉘며, 논리적인 연산을 위해 사용됩니다. 
> 우리가 사용하는 하드웨어는 사실상 논리적인 연산을 기반으로 여러 유형의 데이터를 다루게 됩니다.

### Number, 수학적 표현

> 다음으로 Boolean 의 True/False 는 두 가지의 표현이므로 수학적으로 0 과 1 로 대체될 수 있습니다.
> 수학에서는 N 개의 표현에 의해 나타는 표기법을 N-진수라고 정의하는데, 컴퓨팅은 0 과 1 이므로 2진수라 표현할 수 있습니다.
> 즉 컴퓨터과학에서는 숫자를 2진수 기반으로 저장하여 사용하게 됩니다.

### Character, 언어적 표현

> 마지막으로 자연계의 문자는 나라별 지역별로 다양하기에 0 과 1 이란 두 가지 경우의 수로 표현하기에는 무리가 있습니다.
> 그렇기에 컴퓨터과학에서는 이를 2진수 기반으로 표현된 수에 대해 매칭하여 문자를 인식하도록 합니다.
> 우리는 이 숫자를 Code Point 라고 부르게 됩니다.
> Code Point 는 실제로 문자가 할당되거나 공백 또는 컴퓨팅 신호로 지정될 수 있습니다.
> 초기의 컴퓨터과학에서 이러한 문자의 매칭이 나라별로 지역별로 상이했습니다.
> 하지만 국제적인 표준을 정의하고 가장 많이 쓰는 표현을 분류하여 약속하게 됩니다.
> 우리는 이러한 언어적 표현의 약속을 문자의 집합(Character Set)이라 정의했고,
> 이러한 문자의 집합은 같은 Code Point 에 대해 서로 다른 해석결과가 나타날 수 있는 이유가 됩니다.

## 결론

> 이렇듯 컴퓨터의 데이터는 일련의 약속에 따른 해석방식에 따라 서로 상이한 결과를 갖게 됩니다.
> 그렇게 우리는 해당 데이터를 어떻게 처리할 지를 하드웨어적인 관점에서 이해할 필요가 있습니다.
> 다음 포스팅부터는 이 데이터 유형에 대한 하드웨어적인 제약사항과 이를 해결하기 위한 컴퓨터과학관점의 이야기를 이어나가겠습니다.