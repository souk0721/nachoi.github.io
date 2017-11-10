---
layout: post
title:  "Date Formatting 날짜 형식 설정"
author: Yena Choi
categories: studynote
---

# Date Formatting - 날짜 형식 설정

- 블로그를 통해 작성한 글이 **06 Nov 2017** 와 같은 형식으로 업로드되는 것을 참을 수 없어서 **YYYY-MM-DD** 형식으로 바꾸고 싶어졌다. ~~역시 나는 한국인~~
- 구글에 이것저것 검색해보다 'jekyll post date format' 으로 찾아보니 좋은 사이트 페이지가 있어서 번역해서 정리해놓기로 한다.

[원문: CloudCannon Academy의 Learning 사이트 ](https://learn.cloudcannon.com/jekyll/date-formatting)

<br>

## Setup
우선 날짜 형식을 **ISO 8601** 로 설정한다.

```
---
layout: default
title: Date Formatting
date: 2017-11-06T12:34:00Z
---
```

이러한 형식을 바꾸려면 날짜 필터를 통해 변경해야 한다.

<br>

#### date_to_string

  \{\{ page.date \| date_to_long_string \}\} → **06 November 2017**

#### date_to_rfc822

  \{\{ page.date \| date_to_rfc822 \}\} → **Mon, 06 Nov 2017 12:34:00 +0900**
<br>
#### date_to_string

  \{\{ page.date \| date_to_string \}\} → **06 Nov 2017**

#### date_to_xmlschema

  \{\{ page.date \| date_to_xmlschema \}\} → **2017-11-06T12:34:00+09:00**

<br>

## Date Custom 설정

기본 설정된 형식 뿐 아니라, 직접 월 일 연도 등의 위치와 형식을 지정하여 자유롭게 Format 을 설정할 수 있다.

*(example)*

  \{\{ page.date \| date: "%m/%d/%Y" \}\} → **11/06/2017**

  \{\{ page.date \| date: "%-d %B %Y" \}\} → **06 November 2017**

이밖에 사용할 수 있는 표시자는(Placeholder) 아래와 같다. ([원문](https://learn.cloudcannon.com/jekyll/date-formatting/#date)
에 중복되는 형식 내용도 있고 해서 좀 헷갈린다. 일단 최대한 의미를 해치지 않는 선에서 해석했다.)


표시자 | 형식 | 예시
----- | ----- | -----
%a	|	 요일(간단히)	|	Sun
%A	|	요일(자세히)	|	Sunday
%b	|	월(간단히)	|	Jan
%B	|	월(자세히)	|	January
%c	|	기본 날짜와 시간 표현	|	Fri Jan 29 11:16:09 2016
%d	|	일(0 기재)	|	05
%-d	|	일(0 생략)	|	5
%D	|	DD-MM-YY	|	29/01/16
%e	|	일	|	3
%F	|	YYYY-MM-DD (ISO 8601 형식) 	|	2016-01-29
%H	|	시(24시간제)	|	16
%I	|	시(12시간제)	|	04
%j	|	일(해당 연도)	|	017
%k	|	시(24시간제, 0 생략)	|	4
%m	|	월	|	04
%M	|	분	|	09
%p	|	AM/PM (대문자)	|	AM
%P	|	AM/PM (소문자)	|	pm
%r	|	시간 (12시간제)	|	01:31:43 PM
%R	|	시간 (24시간제)	|	18:09
%T	|	시간 (24시간제, 초 포함)	|	18:09:13
%s	|	1970-01-01 00:00:00 UTC 부터 경과한 초	|	1452355261
%S	|	초	|	05
%U	|	해당 연도의 주차 (일~토 요일 적용)	|	03
%W	|	해당 연도의 주차 (월~일 요일 적용)	|	09
%w	|	요일 순서 (일~토: 0~6)	|	4
%x	|	기본 날짜 표현 (DD/MM/YY)	|	05/11/15
%X	|	기본 시간 표현 (HH/MM/SS)	|	17:15:31
%y	|	연도 (2자리)	|	16
%Y	|	연도 (4자리)	|	2016
%Z	|	타임존 이름	|	PST
%%	|	'%' 문자	|	%
