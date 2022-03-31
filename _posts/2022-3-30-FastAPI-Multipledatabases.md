---
layout: post
title: FastAPI에 2개 이상의 DB연동하기 - 1
category: Blog
tags: [Git, FastAPI, pydantic, sqlalchemy]
---
## 💡 Intro
- 나는 원래 flask를 주로 사용하는데 지인분의 추천으로 FastAPI를 알게 되었다. FastAPI에서 libuv(node.js 성능의 핵심)를 코어로 사용하는 uvloop가 매력적 이었고, [ASGI](https://asgi.readthedocs.io/en/latest/specs/main.html)를 한번 사용해보고싶어서 FastAPI를 처음 사용해보았다.
- FastAPI로 관리자 웹을 만들다가 기존에 연결해 두었던 DB에 한개를 추가로 더 연결해야하는 상황이 생겼다.생각보다 reference가 많이 없었는데, FastAPI의 제작자이신 [tiangolo](https://github.com/tiangolo/fastapi/issues/2592)님의 issues에서 이 문제를 찾을 수 있었다.
Multipledatabases에 대해 다양한분들의 의견이 잘 정리되어있어서 이를 나의 FastAPI에 적용해보았다.

<br>
<br>
## 🌩 그전에! FastAPI란? 
---------------------------------------
[공식 문서](https://fastapi.tiangolo.com/)에는 다음과 같이 설명되어져있다.

FastAPI is a modern, fast (high-performance), web framework for building APIs with Python 3.6+ based on standard Python type hints.
주요기능:

- Fast: NodeJS 및 Go와 비슷한 성능, 현존하는 파이썬 웹 프레임워크 중 가장 빠르다.
- Fast to code
- Fewer bugs
- Intuitive
- Easy
- Short: 적은버그와 코드중복을 최소화할 수 있고, 각 매개변수 선언에 여러기능을 제공해준다.
- Robust: 문서 자동화 및 쉬운 배포가 가능하다.
- Standards-based: 개방형 API 표준(OpenAPI&JSON)을 기반으로 한다.

이 뿐만아니라, 현재 아직 많은 레퍼런스가 나와있지는 않지만, 그것을 매꿔줄 매우매우 수준높은 공식문서가 잘 마련되어져있다. 또한 백엔드 엔지니어 입장에서 API를 사용할 수 있도록 만든 문서 작업이 생각보다 많은 시간이 소요되는데, FastAPI는 문서의 자동화를 제공해줌으로써 개발자가 문서 작업에 할애하는 시간을 줄이고, 오직 코드에만 집중하도록 해 업무 효율을 증진 시켜준다!
<br>
<br>
## 🌩 Multiple databases 설정하기 
---------------------------------------
1. **settings.py**
<script src="https://gist.github.com/liampoet/7db7cc280b3a03655e611a814a112062.js"></script>

**functools - @lru_cache** :<br> 
- LRU(Least Recently Used)캐싱을 사용하기위해 **@lru_cache**를 사용했다. @lru_cache 데코레이터는 functools 내장 모듈로 부터 불러올 수 있으며, @lru_cache 를 아무 함수 위에 선언하면 사용하면 그 함수에 넘어온 인자를 키(key)로 그리고 함수의 호출 결과를 값(value)으로 LRU캐싱이 적용된다.
<br>
<br>
2. **database.py**
<script src="https://gist.github.com/liampoet/ba25801f94f48afd1549c81a067be4f3.js"></script>
**sqlalchemy** :<br>
- SQL 문법 없이 개발 중인 언어로 데이터베이스에 접근할 수 있게 해주는 라이브러를 **ORM(Object Relational Mapping)** 이라고 한다. 그 중에서도 **sqlalchemy**은 python의 대표적인 ORM이다.
<br>
<br>
3. **model.py** 
<script src="https://gist.github.com/liampoet/28401f8a253f048b8be2665bbdcf68f0.js"></script>
<br>
<br>
4. **route.py**
<script src="https://gist.github.com/liampoet/59c6d958cbc4e968ccf0b0634ccdabfd.js"></script><br>
<br>
이렇게만 설정을 해주면 간단하게 2개 이상의 DB를 연결할 수 있다.! 전체적인 코드는 아래 링크에서 확인할 수 있다.<br>
[https://github.com/liampoet/FastAPI-multiple_databases.git](https://github.com/liampoet/FastAPI-multiple_databases.git)

**[참고자료]**
- [https://blog.neonkid.xyz/253](https://blog.neonkid.xyz/253)
- [pydantic - BaseSettings](https://pydantic-docs.helpmanual.io/usage/settings/)
- [FastAPI - pydantic BaseSettings예제](https://fastapi.tiangolo.com/advanced/settings/)

