---
layout: post
current: post
cover:
navigation: True
title: Git 설정하기 (1) - 개행 설정하기
date: 2022-09-10
tags: ["Git"]
class: post-template
subclass: 'post'
author: on-Sync
---

# 눈에 보이지 않는 변경사항

> Editor 는 보통 문단관련 예약어를 동작으로써 적용합니다.   
> 실제로 사용한 간격(Whitespace) 예약어가 Space(`\s`) 인지 Tab(`\t`) 인지 바로 확인하기 힘듭니다.  
> 이 경우엔 개발자가 직접 간격을 이동하면서 확인할 수는 있습니다.   
> 사실 둘 중 무엇이든 코드동작에 차이점도 없기에 무관합니다.   
> 하지만 개행(End of Line)과 같이 코드동작에 영향을 준다면 말이 달라집니다.  
> 개행은 Operation System 에 맞지 않을 경우 File Read 와 Execute 가 실패하게 만들며,   
> Editor 화면으로는 CRLF(`\r\n`) 인지 LF(`\n`) 인지 확인할 수 없습니다.  
> 특히 코드작업을 하다보면 습관적으로 `Ctrl + S` 로 저장하게 되는데,   
> 이후 편리상 `git add .` 과 같은 Recursive 한 옵션으로 무책임하게 Commit 을 추가한다면   
> 아무도 모르게 잘못된 개행정보가 Git 에 적용될 수 있습니다.

## 개행에 대해서

> 위 문제 해결에 앞서서 개행에 관련된 주변지식을 공유합니다.

### EOL

- `End of line` 을 뜻한다.
- 어셈블리 명령 처리부터 코드를 작성하는 텍스트 편집기까지 문자데이터 관리에 사용된다.  
- 이들은 CRLF 로 표현된다.

### CRLF 란 ?

- CRLF 는 `Carriage Return` 과 `Line Feed` 를 말한다.  
- 이들은 옛 타자기 사용방법에서 유래됐다.  
- 기존 타자기는 한 줄을 작성한 뒤, 커서를 앞단으로 이동시킨 후에 아랫줄로 이동했다.  
- Carriage Return 는 커서를 앞단으로 이동시키는 행위 또는 키를 말한다.  
- Line Feed 는 아랫줄로 한칸 이동하는 행위 또는 키를 말한다.

### 운영체제별 CRLF 가 다른 이유

> CRLF 는 Text 문서의 줄변환(EOL) 예약어로 사용된다.  
> 이때 CR 은 `/r` , LF 는 `/n` 로 표현된다.  
> 하지만 이들 중 CR 가 줄변환 예약어로 사용되는 것은 운영체제별로 상이하다.  
> DOS 계열의 경우 CRLF 을 전부 표시하여 사용한다.  
> UNIX 계열의 경우 LF 만을 표시하여 사용한다.  
> 이러한 이유는 장치의 발전과 관련된다.  
> 시대적으로 빠른 UNIX 는 저장공간을 줄이기 위해 LF 만을 차용헀고,  
> 이후 나온 DOS 는 CP/M 의 방식을 따르는데,   
> CP/M 은 시리얼라인(직렬통신)으로 터미널을 연결했기에 화면전환이 느렸다.  
> 때문에 느린 스크롤 속도와 보조를 맞추는 목적으로 CR 을 추가했다.  
> 즉 이들의 파생인 Linux, Windows 도 각각의 이전 `관례`를 유지하고 있다.

## 설정방법

> 다음은 개행문제를 회피하기 위한 설정방법입니다.   

#### 1. core.autocrlf (default=false)

- `checkout` , `commit` 을 기점으로 `eol` 방식을 __변환합니다.__
- 교차플랫폼으로 협업할 경우 사용됩니다.

```shell
# 설정 종류
git config --global core.autocrlf true
git config --global core.autocrlf input
git config --global core.autocrlf false
```

| 설정    | 동작                                | 교차플랫폼 협업 | 사용자                |
|-------|-----------------------------------|----------|--------------------|
| true  | `checkout` 은 CRLF , `commit` 은 LF | O        | Windows 계열         |
| input | `commit` 만 LF                     | O        | Unix 계열            |
| false | X                                 | X        | Windows 또는 Unix 계열 |

#### 2. core.eol (default=native)

- `checkout` , `commit` 을 기점으로 `eol` 방식을 __지정합니다.__
- 단일플랫폼으로 협업할 경우 `강제` 를 목적으로 사용됩니다.

```shell
# 설정 종류
git config --global core.eol native
git config --global core.eol crlf
git config --global core.eol lf
```

| 설정     | 동작                     |
|--------|------------------------|
| native | 사용자 시스템의 `eol` 방식을 사용  |
| crlf   | `crlf` 방식을 사용          |
| lf     | `lf` 방식을 사용            |

#### 3. core.safecrlf (default=native)

- `checkout` , `commit` 을 기점으로 `eol` 방식을 __제어합니다.__
- `crlf -> lf -> crlf` 변환 후의 데이터 차이로 `eol` 교차사용을 판단합니다.
- 교차사용 확인시 중단 유무를 설정합니다.
- _* false 일 경우, `autocrlf` 의 설정을 수행합니다._

```shell
# 설정 종류
git config --global core.safecrlf true
git config --global core.safecrlf warn
git config --global core.safecrlf false
```

| 설정    | 동작           |
|-------|--------------|
| true  | 중단           |
| warn  | 경고 후 진행      |
| false | X            |
