---
layout: single
title:  "Ch.5 Inventory & Equipment"
categories: UnitySystem
typora-root-url: ../
toc: true
author_profile: false
sidebar:
    nav: "docs"

---

User Inventory 

유저인벤토리에 관한 것에 대한 요약. 
여기서부터 정신 똑바로 차리자. 

아이템 데이터테이블 
규칙 : 종류(1) + 등급(1) + 넘버링(3) 
아이템의 ID를 보고 종류와 등급을 유추할 수 있음. 

Data Part

1. DataTableManager에서 파일 불러오기. 
2. 불러온 파일에서 데이터를 읽어서 ItemData List에 하나하나 넣기 
3. 아이템 데이터를 불러오기 위해서 GetItemData() 함수를 구현하기 



Game Package Manager

에셋스토어를 통해서 설치하면 상단에 Tools 메뉴가 생긴다. 
내부에서 원하는 기능을 설치해서 사용할 수 있음 
설치하면 Assets 하위에 GPM 폴더가 생긴다. 

