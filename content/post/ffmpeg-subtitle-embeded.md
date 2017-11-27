---
authors:
- younjin
categories:
- Tip
- ffmpeg 
- Subtitle
date: 2017-11-26T06:34:12-04:00
short: |
  ffmpeg 를 사용해서 자막을 영상에 임베드 하는 방법을 소개  
title: ffmpeg 를 사용한 자막 입히기 및 위치 조정 
package used: ffmpeg 를 사용한 자막 입히기 및 위치 조정 
---

간혹 이런 저런 목적으로 유튜브에 비디오를 자막과 함께 올리는데, 유튜브의 경우 특정 자막이 포함된 링크를 생성하는 것이 불가능하여 아예 자막을 영상에 구워서 업로드 하는 방법을  사용하고 있다. 관련해서 매우 많은 다양한 도구가 있지만, 편의상 사용하는 방법은 아래와 같다.

1. ClipGrab 과 같은 도구를 사용해서 원하는 영상을 다운로드 받는다. (저작권 문제는 논외) 
2. Youtube 에서 제공하는 "Creator Studio" 를 사용하여 원본 영상을 올린다.  
3. Creator Studio 에 보면, "새로운 자막을 생성" 옵션에서 자막을 신규로 만들어 넣을 수 있도록 기능을 제공하는데, 이걸 사용해서 자막을 제작 한다. 
4. 자막을 만들고 나면, "Action" 버튼에서 만들어진 자막을 srt 외 몇가지 포멧으로 다운로드 받을 수 있다. 
5. srt 로 다운 받는다. 
6. ffmpeg 로 ass 포멧으로 변환한다. 
7. .ass 자막 파일을 열어 자막의 위치를 원하는대로 수정한다. 
8. ffmpeg 로 자막을 비디오에 입힌다.


{{< responsive-figure src="http://cfile28.uf.tistory.com/image/2518AC3D591D263605112F" >}}

