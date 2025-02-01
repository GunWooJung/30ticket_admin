# 🏠 30 Ticket 관리자 페이지

📍 **연극 티켓 자동 할인 플랫폼**

![관리자페이지](https://github.com/GunWooJung/READMEImage/blob/main/30ticketadmin.jpg)

30 Ticket은 연극 관계자가 설정한 할인률과 할인 시간대에 맞춰 자동으로 할인 정보를 갱신하는 플랫폼입니다. 이를 통해 관객들에게 실시간으로 연극 티켓 할인 혜택을 제공하고, 티켓 판매를 촉진합니다.

## 💸 할인률 로직

Spring Scheduler를 통해서 1분마다 DB에 할인 적용 가격 업데이트가 수행됩니다.

![할인률 로직](https://github.com/GunWooJung/READMEImage/blob/main/%ED%95%A0%EC%9D%B8%EB%A5%A0.JPG)

## 🎯 담당 기능(관리자 페이지의 Admin과 Seller 중 Admin 담당)

- **로그인 페이지**: 사용자가 사이트 관리자(Admin) 및 티켓 판매자(Seller)로 로그인할 수 있는 기능을 구현했습니다.
- **사이트 관리자 기능(Admin)**: 관리자는 회원 관리, 공지 사항 관리, 기대평 및 공연평 관리를 할 수 있습니다.
- **프론트엔드 및 백엔드**: 관리자 페이지(Admin)의 프론트엔드 및 백엔드 구현을 담당했습니다. 권한의 분리와 보안 향상을 위해, 사용자 관련 서버와 분리하여 관리자 페이지는 별도의 서버를 두었습니다.
- **SSL 인증서 구축**: 웹사이트 보안을 강화하기 위해 SSL 인증서를 구축하여 종단 간 암호화를 통해 사용자와 서버 간의 안전한 데이터 전송을 보장합니다.

## 📄 기술 스택

- **프론트엔드**: HTML, CSS, JavaScript, jQuery, Thymeleaf
- **백엔드**: Spring Boot, MySQL, Naver Cloud Platform
- **보안**: SSL 인증서

---

## ✨ 주요 기능

✅ **로그인** – 사이트 관리자(Admin), 티켓 판매자(Seller) 탭에서 선택하여 로그인할 수 있습니다. Http Session을 기반으로 로그인 상태를 유지합니다.  
✅ **회원 관리** – 모든 회원 및 티켓 판매자(Seller) 정보를 조회하고 검색할 수 있습니다.  
✅ **공지 사항** – 사이트 공지 사항을 등록, 수정, 삭제할 수 있습니다. 또한, 숨김 여부를 통해 임시로 노출되지 않도록 할 수 있습니다.  
✅ **기대평, 관람평** – 기대평 및 관람평을 조회하고 삭제할 수 있습니다.

---

## ⚡ DB 성능 개선 관련 내용

![오늘의 할인](https://github.com/GunWooJung/READMEImage/blob/main/%EC%98%A4%EB%8A%98.JPG)

#### 1️⃣ **테이블 파티셔닝을 통한 조회 성능 향상**

오늘의 할인 공연 조회 시, `where` 조건으로 공연 날짜를 확인합니다. 오늘의 공연만 조회하면 되기 때문에 오늘(today partition), 과거(past partition), 미래(future partition)로 파티션을 나누면 성능을 향상시킬 수 있습니다.

![파티셔닝](https://github.com/GunWooJung/READMEImage/blob/main/%ED%8C%8C%ED%8B%B0%EC%85%94%EB%8B%9D.JPG)

**결과 분석** : 동일한 조건에서 파티셔닝 전후로 평균 API 응답시간이 **534ms → 457ms**로 줄었습니다.  
이러한 성능 향상은 조인 시 오늘 파티션(today partition)이랑만 조인되기 때문입니다.

**단점** : 매일 자정에 00:00에 Scheduler를 통해 오늘 날짜에 맞게 테이블 파티션을 재구성하는 과정에서 테이블 전체에 잠금이 걸립니다.

---

## 🏗️ 시스템 아키텍처

![시스템 아키텍처](https://github.com/GunWooJung/READMEImage/blob/main/30ticket.JPG)

---

## 📺 시연 영상 및 문서

📌 **시연 영상**: [YouTube 링크](https://youtu.be/eqWKif0CrNo)  
📌 **E-R 다이어그램**:

![ERD](https://github.com/GunWooJung/READMEImage/blob/main/erd.png)
