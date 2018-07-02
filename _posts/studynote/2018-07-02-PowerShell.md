---
layout: post
title:  "[PowShell] 기본 프린터 설정하기"
date:   2018-07-02
author: souk0721
categories: studynote
tags: [powshell, printer]
comments: true
---


# Mastering Markdown - 마크다운 파일 작성법

  - 깃헙에 제대로 글 올리기도 전에 난관에 부딪혔다. 할줄 아는건 # 뿐.
  - 마크다운 파일 작성이 아직 어색해 가이드를 가져와 연습하기로 한다.
[원문: GitHub에서 제공하는 Markdown Guide](https://guides.github.com/features/mastering-markdown/)


### 볼드체, 이탤릭체, 취소선

\*\*bold\*\*  **bold**  
\_\___이렇게도 bold 표현이 가능하다__\_\_

\*italic\*    *italic*  
\__이렇게도 이탤릭 표현이 가능하다_\_

\~\~취소선\~\~ ~~취소선~~   
`<del>`<del>이렇게도 취소선 표현이 가능하다</del>`</del>`   
취소선은 너무 남발하면 뭔가 나무위키 + 오덕...스러워지는것 같다.

### 외부 URL 링크

앞에 https가 붙어 있는 링크의 경우, 자동으로 활성화된다.
https://guides.github.com/features/mastering-markdown/

링크 앞에 설명을 붙이고 싶으면 아래와 같은 포멧을 사용한다.  
`[링크 설명](https://www.URLwhatever.com)`

[원문: GitHub에서 제공하는 Markdown Guide](https://guides.github.com/features/mastering-markdown/)

### 숫자/기호로 List 작성

예상 가능한 숫자 리스트는 물론
1. 이런
2. 형식을
3. 띤다


별 표시 \*로 줄을 시작하면
* 이렇게
* 리스트 기호가 작성된다. 어썸하다.

혹은, 하이픈 -을 사용하면
- 이렇게 리스트가 분류된다.
  - 추가로, `*`나 `-` 앞에 Tab or 공백 두 칸을 추가하면
    - 짜잔! 하위 리스트가 생성된다.

### 이미지 표시하기

\!\[이미지 설명\]\(이미지 주소\) 를 하면 아래와 같이 이미지가 표시된다.

![구글 로고를 가져와보기로 한다.](https://www.google.co.kr/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png)  


### 헤더 티어 Header Tier 구분

해시 기호 `#`로 줄을 시작하면 헤더를 1~6단계까지 지정할 수 있다.  
굵은 글씨로 표시되며, 글씨 크기가 달라진다.

# # 하나
#### #### 넷
###### ###### 여섯  

~~근데 소심해서 한개보다는 세네개 정도 쓰게 되는 것 같다.~~

### 인용 문구
인용 문구를 사용하고 싶을 때에는 줄 앞에 부등호 `>` 기호를 사용하여 표현한다.

> 지금 적극적으로 실행되는 괜찮은 계획이   
> 다음 주의 완벽한 계획보다 낫다.

> A good plan, violently executed now,    
> is better than a perfect plan next week.

> - 조지 S. 패튼(George S. Patton) -

### Code 표현하기

마크다운에서는 코드를 표현하는 다양한 방법을 제공한다.    
알아보기 쉽고 깔끔하게, 또 언어를 지정하면 syntax 강조 기능까지 쓸 수 있다!

- 간단한 인라인 코드는 앞뒤로 \` 기호를 붙이면 `var example = true` 와 같이 강조된다. ~~`낙서도 가능하다`~~


- 코드가 여러 줄인 경우, 줄 앞에 공백 네 칸을 추가하면 일반 글과 구분된다.
      if (isAwesome){
        return true
      }

- 또는, 코드 맨윗줄과 맨아랫줄을 \`\`\`로 감싸도 같은 결과가 나온다.
```
if (isAwesome){
  return true
}
```

- 더욱더 어썸한 건, \`\`\` 옆에 언어를 지정해주면 syntax가 예뻐진다!  
```java
// ```java로 시작
if (isAwesome) {
  return true
}
// ```로 끝
```

### Task List - 체크리스트

줄 앞에 `- [x]`를 써서 완료된 리스트 표시,     
줄 앞에 `- [ ]`를 써서 미완료된 리스트 표시.

- [x] 이것은 완료된 항목이다.
- [x] 이 안에서도 **강조** 외에 여러 기능을 사용할 수 있다. `true`
- [ ] 이것은 완료되지 않은 항목이다.

### Table - 테이블(표) 만들기

하이픈 `-`으로 첫 번째 행을 구분한다. 첫째 줄 외엔 -를 다시 써봐도 소용없다.   
파이프`|`로 열을 구분한다.

```
항목 1 | 항목 2 | 항목 3
----- | ----- | -----
내용을 | 추가할 | 수있다
여기서도 | *강조 기능이* | **작동한다**
```

항목 1 | 항목 2 | 항목 3
----- | ----- | -----
내용을 | 추가할 | 수있다
여기서도 | *강조 기능이* | **작동한다**

### 다른 유저 참조

여타 SNS와 마찬가지로, `@nickname` 하면 다른 유저나 팀을 참조할 수 있다.
