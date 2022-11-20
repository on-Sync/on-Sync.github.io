---
layout: post
current: post
navigation: True
title: Software Engineering (1) - 전체 구조 요약
date: 2022-11-13
tags: ["Software Engineering"]
class: post-template
subclass: 'post'
author: on-Sync
---

# Software Engineering

> Software Engineering 에서 소프트웨서 개발은 다음과 같은 포함관계로 구체화된다.

- Software Development Process (절차)
    - Software Development Methodology (방법론)
        - Programming Paradigm (패러다임)
            - Software Design Pattern (패턴)
                - Programming Idiom (관용구)
                    - Programming Implementation (구현체) (i.g Concrete Algorithm)

### Software Development Process (절차)

> SDLC 을 조합하는 10가지 개발절차 모델

- 소프트웨어 개발 프로세스 모델의 이해
- 주머구구식 모델
- 선형 순차적 모델
- V모델 (Test by Step)
- 진화적 프로세스 모델 (N-Prototype)
- 나선형 모델
- 단계적 개발 모델 (Release)
- 통합 프로세스 모델 (Progressive)
- 애자일 프로세스 모델 (On needs basis, like YAGNI)

#### Software Development Life Cycle (개발 생명 주기, SDLC)

- 요구사항 정의 (R, Requirement)
- 요구사항 분석 (A, Analysis)
- 설계 (D, Design)
- 구현 (C, Coding)
- 테스트 (T, Test)
- 유지보수 (M, Maintenance)

### Software Development Methodology (방법론)

- 개발절차 모델의 구체화된 방법
    - (e.g, Agile`s Extreme Programming = XP)

#### 방법론의 구체화 대상

- 작업절차
- 작업방법
- 산출물
- 관리
- 기법
- 도구

### Programming Paradigm (패러다임)

- 개발언어 탄생(설계)의 기반이자 특징이 될 수 있다.
    - (e.g, Procedural = C, Object-oriented = Java )
- 방법론을 실현하기 위한 지향점 또는 이념.
    - (e.g, OOP, DDD, TDD, CBD ...)

### Software Design Pattern (패턴)

- 패러다임 내에서 소프트웨어의 설계를 위한 반복적인 유형을 말한다.
- 유지보수를 위해 재사용성에 중점을 둔다.
- __정답(구현)이 아닌 방향성(해결책)의 제시이며__, 책 또는 논문으로 공유되고 전파되어졌다.
- 크게 다음과 같이 분류할 수 있다.
    - GoF`s Patterns
        - 디자인 패턴 중 가장 유명한 제안으로 Design Patterns 란 이름의 서적으로 집필되었다.
        - 크게 생성(Assignment), 구조(Composition), 행동(Execution) 관점으로 분류된다.
            - 비슷한 서적인 Code Complete 에서 제시된 디자인 패턴과 다소 중첩된다.
    - Concurrency Patterns
        - 자원을 공유하는 방법을 제안한다.
    - Architectural Patterns
        - 다수의 소프트웨어를 연결/구조화 하는 방법을 제안한다.
    - 그외 패턴들
        - 문헌이 아닌 코드/프레임워크로 전파되어 사용되는 패턴
        - (e.g, Dependency Injection, Lazy Loading, Delegation ...)

### Idiom (관용구)

- Idiom 은 제시된 패턴(방향성)을 구현한 것 중 정형화된 작성 방법을 말한다.
- 동일한 패러다임을 지원하는 언어는 같은 패턴을 구사할 수 있지만, 같은 관용구는 언어의 전체적인 특성에 따라 다르게 표현될 수 있다.
- 관용구는 구현체 중 보편화된 작성 방법들이다.

### Implementation (구현체)

> i.e, Concrete Algorithm

- 논리 또는 구조에 대해 구체적으로 작성된 내용을 말한다.
- 주어진 전략이나 모델뿐만 아니라 사용자(개발자)에 따라 작성된 내용이 상이할 수 있다.
    - ___작성자의 개인적인 생각으론, 구현방법 또는 구현체가 상위 집합에서 제시된 개념에 벗어날 수록 유지보수 비용이 증가한다고 판단하고 있다.___