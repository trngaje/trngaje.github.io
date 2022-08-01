---
layout: posts
title:  "miyoo-mini mame 류 한글 게임 List 표시 방법"
categories : [miyoomini]
tags : [miyoomini]
---

### 개요

사용되는 프론트 엔드가 MinUI 로 예상되지만 오픈되어 있는 미유 미니용 소스가 없기 때문에
여러번의 시도로 방법을 확인 하고 정리 했습니다.

### List 표시 관련 영향을 주는 옵션 들

config.json 파일에서 "gamelist" 항목이 없는 경우
"rompath" 와 "imgpath" 와 "shorname" 값의 영향을 받아 게임 리스트 화면을 표시합니다.

    "rompath":"../../Roms/MAME2003",
    "imgpath":"../../Imgs/mame2003plus",
    "gamelist":"../../Roms/MAME2003/gamelist.xml",
    "shortname":1,
    "extlist":"zip"

config.json 파일에서 "gamelist" 항목이 있는 경우 "imgpath"의 경로는 무시되고 "gamelist.xml"의 정보에 따릅니다.

    "rompath":"../../Roms/MAME2003",
    "gamelist":"../../Roms/MAME2003/gamelist.xml",
    "shortname":1,
    "extlist":"zip"


"shortname" 은 0이면 파일명 그대로 짧게 표시되고, 1이면 길게 표시됩니다. 한글로 작성된 gamelist 파일이 없으면 영어로 길게 표시됩니다.
"extlist" 는 표시하는 파일 확장자명을 나타내며 mame류는 모두 zip 으로 압축되어 있어야하기 때문에 변동사항은 없습니다.

### 주의 사항

> 각 snap image 파일이 공통이라 ARCADE 폴더에 snap을 다 넣고 각기 서로다른 mame류 게임 리스트를 관리하고 싶겠으나 그렇게 하면 파일리스트가 일부만 출력되는 것을 확인할 수 있었다.

"gamelist" 파일을 각각의 롬 폴더에 넣어서 작성해 주어야 한다.

- MAME2003-PLUS 용

    "gamelist":"../../Roms/MAME2003/gamelist.xml",

- ARCADE 용

    "gamelist":"../../Roms/ARCADE/gamelist.xml",

> PC(windows) 환경에서는 한번 작성된 파일명이나 폴더명을 자유롭게 대소문자 변경이 어렵기 때문에 작성할때 한번에 원하는 대소문자로 맞추거나 gamelist.xml의 폴더명을 잘 맞추어서 적어주어야 합니다.
gamelist.xml 파일에서 아래 정보의 경로는 중요하니 대소문자 잘 맞추어서 일괄 변경해 주어야 합니다.
실재 참조 되는 경로는 'path' 'name' 'image' 정도 인듯 합니다. 그외 정보는 사용되는지 여부는 알 수 없습니다. 'path' 경로의 파일을 'name' 으로 변경해서 표시하게 되며 snap image는
우측으로 이동했을 때 'image' 경로의 파일을 표시해 줍니다.

	<game>
		<path>./720.zip</path>
		<name>720 도</name>
		<image>./snap/720.png</image>
		<video>./snap/720.mp4</video>
		<marquee>./marquee/720.png</marquee>
		<developer>아타리 게임즈</developer>
		<publisher>아타리 게임즈</publisher>
		<genre>스포츠 / 스케이트보드</genre>
		<players>1</players>
		<releasedate>1986</releasedate>
		<desc />
	</game>

### gamelist.xml 입수는

텐타클 팀이 작업한 gamelist.xml 파일은 아래 링크에서 받을 수 있습니다.
[https://github.com/losernator/romlistkr 링크](https://github.com/losernator/romlistkr)


### 한글 리스트 구동 화면

![](/images/2022-02-19/miyoo_mini_kor_list_1.png)

![](/images/2022-02-19/miyoo_mini_kor_list_2.png)

![](/images/2022-02-19/miyoo_mini_kor_list_3.png)
