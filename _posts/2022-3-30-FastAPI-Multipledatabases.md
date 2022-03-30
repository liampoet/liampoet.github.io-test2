---
layout: post
title: FastAPI에 2개 이상의 DB연동하기
category: Blog
tags: [Git, FastAPI, pydantic, sqlalchemy]
---
FastAPI로 관리자 웹을 만들다가 기존에 연결해 두었던 DB에 한개를 추가로 더 연결해야하는 상황이 생겼다.생각보다 reference가 많이 없었는데, FastAPI의 제작자이신 [tiangolo](https://github.com/tiangolo/fastapi/issues/2592)님의 issues에서 이 문제를 찾을 수 있었다.<br>
Multipledatabases에 대해 다양한분들의 의견이 잘 정리되어있어서 이를 나의 FastAPI에 적용해보았다.
<br>
<br>
### 1. settings.py
---------------------------------------

<script src="https://gist.github.com/liampoet/7db7cc280b3a03655e611a814a112062.js"></script>

[pydantic - BaseSettings](https://pydantic-docs.helpmanual.io/usage/settings/)<br>
[FastAPI - pydantic BaseSettings예제](https://fastapi.tiangolo.com/advanced/settings/)

functools - @lru_cache : 
LRU(Least Recently Used)캐싱을 사용하기위해 @lru_cache를 사용했다.
@lru_cache 데코레이터는 functools 내장 모듈로 부터 불러올 수 있으며, @lru_cache 를 아무 함수 위에 선언하면 사용하면
그 함수에 넘어온 인자를 키(key)로 그리고 함수의 호출 결과를 값(value)으로 LRU캐싱이 적용된다.
<br>
<br>
### 2. database.py
---------------------------------------

<script src="https://gist.github.com/liampoet/ba25801f94f48afd1549c81a067be4f3.js"></script>

SqlAlchemy은 강버섯님의 블로그에 간단하고 쉽게 연결방법이 나와있다<br>
[강버섯님의 SqlAlchemy을 이용한 간단한 DB연결](https://pydantic-docs.helpmanual.io/usage/settings/)<br>
<br>
<br>
### 3. model.py
---------------------------------------

<script src="https://gist.github.com/liampoet/28401f8a253f048b8be2665bbdcf68f0.js"></script>
<br>
<br>
### 4. route.py
---------------------------------------

<script src="https://gist.github.com/liampoet/59c6d958cbc4e968ccf0b0634ccdabfd.js"></script><br>

이렇게만 설정을 해주면 간단하게 2개 이상의 DB를 연결할 수 있다.! 전체적인 코드는 아래 링크에서 확인할 수 있다.<br>
[FastAPI-Multipledatabases](https://github.com/liampoet/FastAPI-multiple_databases.git)
