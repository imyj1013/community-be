# **Community Backend (FastAPI + MySQL + AI Summary)**

## 개요

이 백엔드는 FastAPI 기반으로 구현된 커뮤니티 서비스 서버입니다.
사용자는 회원가입, 로그인, 프로필 수정, 비밀번호 변경, 게시글 CRUD, 댓글 CRUD, 좋아요 기능 등을 이용할 수 있습니다.
또한 BART Transformer 모델을 이용한 **게시글 요약 자동 생성 기능**과 **이미지 업로드 기능**이 포함되어 있습니다.

---

# **시연영상**

https://drive.google.com/file/d/16yCrMHfZRHSoyG99GfKu6IIlS5KXJN1I/view?usp=sharing 

---
| 로그인 | 회원가입 | 개인정보수정 |
|---|---|---|
| <img width="1119" height="785" alt="Image" src="https://github.com/user-attachments/assets/e0b407b0-bd36-45e1-96bd-edb3297c08e2" /> | <img width="1118" height="786" alt="Image" src="https://github.com/user-attachments/assets/61b9f54f-c7c4-4eda-ae24-84c9012b1a7c" /> | <img width="1121" height="786" alt="Image" src="https://github.com/user-attachments/assets/471465b7-250b-4955-a4a7-ecb09e8f11aa" /> |

| 비밀번호수정 | 게시글목록 | 게시글상세조회 |
|---|---|---|
| <img width="1119" height="786" alt="Image" src="https://github.com/user-attachments/assets/b0efd852-90d5-4ba5-93f7-1b689efa0d7b" /> | <img width="1109" height="780" alt="Image" src="https://github.com/user-attachments/assets/d7ec5bea-31c4-4f36-a953-1e29de66df33" /> | <img width="1122" height="789" alt="Image" src="https://github.com/user-attachments/assets/e2d8acfb-ec6d-4832-a258-54991e540dfc" /> |

---

## 프로젝트 구조

```
backend/
 ├── app/
 │   ├── main.py
 │   ├── db.py
 │   ├── utils.py        # 인증/유효성 검사/비밀번호 해시 처리
 │   ├── models/         # DB 처리 로직
 │   ├── controllers/    # API 로직
 │   ├── entity/         # SQLAlchemy 모델 (User, Post, Comment, Like)
 │   ├── routes/         # 라우터 모음
 ├── create_table.py
 ├── download_model.py
 ├── requirements.txt
```

---

## 실행 방법

### 1) 가상환경 생성 및 활성화

```bash
conda create -n env_community python=3.10
conda activate env_community
```

### 2) 패키지 설치

```bash
pip install -r requirements.txt
```

### 3) MySQL 실행 후 DB 환경변수 수정

`db.py` 내부에 본인의 MySQL 정보 수정.

### 4) 테이블 생성

```bash
python create_table.py
```

### 5) 서버 실행

```bash
uvicorn app.main:app --reload
```

---

## 주요 기능

### 인증 & 세션 관리

* FastAPI SessionMiddleware 사용
* 로그인 시 세션에 `sessionID`, `user_id`, `email` 저장
* 모든 인증 필요한 API는 세션 기반으로 검증

### 사용자 기능

* 회원가입 (이메일/닉네임 중복 검사)
* 로그인 / 로그아웃
* 프로필 수정 (닉네임 + 프로필 사진)
* 비밀번호 변경
* 회원 탈퇴 (CASCADE 적용으로 연관 데이터 자동 삭제)

### 게시글 기능

* 게시글 생성(이미지 업로드 가능)
* 게시글 요약 자동 생성 (BART 모델 사용)
* 게시글 목록 조회 (인피니티 스크롤)
* 게시글 상세조회
* 게시글 수정 / 삭제

### 댓글 기능

* 댓글 작성 / 수정 / 삭제
* user_id 기반으로 본인 여부 판단
* 댓글 작성자 프로필 이미지 지원

### 좋아요 기능

* 좋아요 생성 / 취소
* 게시글 좋아요 수 자동 반영

### 이미지 업로드

* FastAPI UploadFile로 이미지 수신
* `/image/*` 형태로 정적 파일 제공
* UUID 기반 파일명 저장

### 게시글 자동 요약 기능

* Transformer 모델(BART)을 이용해 게시글 본문을 받아 summary를 생성한다.
