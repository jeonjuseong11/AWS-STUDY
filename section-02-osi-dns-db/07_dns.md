# DNS(도메인 네임 시스템)

---

## 1. DNS란?

- 사람이 입력하는 도메인명(google.com 등)을 서버 IP 주소로 변환하는 시스템임.
- 사용자는 도메인만 입력, 실제 통신은 DNS가 IP를 찾아줌.
- IPv4/IPv6 주소는 사람이 외우기 어려워서 도메인-주소 매핑이 필요함.

---

## 2. DNS 주요 용어

- **도메인:** 사람이 알아보기 쉬운 주소(ex. google.com)
- **서브도메인:** 도메인 앞에 붙는 추가명(www, mail 등)
- **Apex 도메인:** www 같은 것 없는 순수 도메인(example.com)
- **레코드(DNS Record):** 도메인과 데이터(IP 등) 매핑 정보 저장
  - A 레코드: IPv4 주소
  - MX 레코드: 메일 서버
  - CNAME, TXT 등 여러 종류 존재
- **존(Zone):** 하나의 도메인(혹은 그 하위)에 대한 레코드 집합
- **존 파일:** 존의 모든 레코드가 저장된 파일(텍스트 DB)
- **네임서버(NS):** 존 파일 바탕으로 DNS 쿼리에 응답하는 서버
  - Authoritative: 실제 원본 정보 보유
  - Non-Authoritative: 캐시로 응답하는 중간 서버
- **리졸버(Resolver):** 클라이언트 대신 DNS 쿼리 처리해주는 중계 서버(ISP 등에서 운영)

---

## 3. DNS 계층 구조

- 정보가 분산/계층 구조로 저장됨(단일 서버 불가)
- **루트 도메인(.):** 계층 최상위, IANA가 관리. 13개 루트 네임서버(A~M)가 전 세계에 분산됨. 리졸버에 주소 하드코딩.
- **TLD(Top Level Domain):** .com, .net, .kr, .edu 등. Registry(Verisign, KISA 등)에서 관리.
- **NS(Name Server):** 실제 도메인(A, MX 등) 레코드 보유.
- 실제로는 3억 개 넘는 도메인 정보가 이 계층 구조로 분산 관리됨.

---

## 4. DNS 쿼리 흐름 (예시: lecture.awsclassroom.kr)

1. 사용자가 브라우저에 도메인 입력 → 리졸버에 쿼리 요청
2. 리졸버가 루트 네임서버에 “.kr” NS 주소 질의
3. 루트 서버가 .kr TLD NS 주소 응답
4. 리졸버가 .kr NS에 awsclassroom.kr NS 주소 질의
5. .kr NS가 awsclassroom.kr 관리 NS(Route 53 등) 주소 응답
6. 리졸버가 awsclassroom.kr NS에 lecture.awsclassroom.kr의 A 레코드(IP) 질의
7. NS에서 최종 IP 획득 → 리졸버가 클라이언트에 전달 → 브라우저가 IP로 서버 접속

> 이 과정은 "NPC에게 계속 물어보고 최종 목적지 찾는 RPG 게임" 구조와 유사

---

## 5. 도메인 등록과 DNS 호스팅

- **도메인 등록:** 공식 Registrar(가비아, GoDaddy, AWS Route 53 등)에서 진행, ICANN 인증된 업체임
- **도메인 호스팅:** DNS 서비스 제공(가비아, Route 53 등). 도메인 등록 시 어떤 NS 서버(직접/호스팅업체) 쓸지 지정해야 함.
- 도메인/NS 정보는 TLD Registry DB에 등록 → 실제 쿼리는 이 NS로 향함
- 예) mydomain.com을 카페24에서 등록+호스팅 시 .com 레지스트리에 NS가 카페24로 기록됨
