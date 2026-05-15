# 가야동 주민 관리 시스템 (Gaya-dong Resident Management System)

## 📝 프로젝트 개요
주민센터 행정 업무의 효율성을 극대화하기 위해 주민 정보 관리 및 맞춤형 복지 혜택 대상자를 조회·관리하는 행정 자동화 시스템입니다.</br>
Oracle DB와 Pro*C(Embedded SQL) 환경을 구축하여 대량의 주민 데이터를 안정적으로 처리하고 동적 쿼리를 통한 맞춤형 필터링 알고리즘을 구현했습니다.

## 📅 프로젝트 기간
* 2018.11.12 ~ 2018.12.15

## 💻 기술 스택
* **Language**: C
* **Database & Tool**: Oracle DB, Pro*C (Embedded SQL)

## 📂 파일 및 디렉터리 구조

### 1. 메인 시스템 및 DB 인프라 (`main`)
* **`main.test.pc`**: 시스템 초기 구동 및 Oracle DB 커넥션 검증 모듈
* **`main.txt`**: TUI 화면 제어를 위해 외부 텍스트 템플릿 파일(`scr_select.txt`)을 동적으로 읽어오는 UI 파일 입출력 로직

### 2. 소득분위 분류 시스템 (`information`)
* **`new_select_xy.c`**: 주민 테이블(`JUMIN`) 스키마 호스트 변수 맵핑 및 다차원 `CASE WHEN` 절을 활용한 실시간 소득분위 산정 모듈

### 3. 복지 및 보호 대상자 관리 시스템 (`disorder`)
* **`main.pc`**: 보호관찰 대상자(`probation`) 조회 및 타겟 마케팅/복지 안내를 위한 다중 상태값 동적 업데이트(DML) 모듈

## 🛠️ 핵심 기능 및 구현 상세

### 🔐 1. 안정적인 Oracle DB 커넥션 및 예외 처리
* 호스트 변수(`uid`, `pwd`)를 선언하여 Oracle DB에 보안 접속 환경을 구현했습니다.
* `sqlca.sqlcode` 핸들러를 구성하여 연결 실패 또는 Null 값 처리 예외(`-1405`) 발생 시 시스템 오작동을 방지하는 예외 처리 아키텍처를 도입했습니다.

### ♿ 2. 맞춤형 복지 대상자 추출 및 상태 머신 업데이트
* 특정 복지 조건 및 보호관찰 등급(`J_PROT_RANK <> '0'`)을 만족하는 대상자를 실시간 필터링합니다.
* 사용자가 시스템을 통해 복지 대상자에게 연락을 취하면 연락 횟수가 증가(`p_contact_f = p_contact_f + 1`)하며, 누적 안내 횟수가 5회 이상인 주민은 중점 관리 대상자(`p_manage = 'YES'`)로 일괄 전환하는 데이터 배치 업데이트 로직을 설계했습니다.

### 🗄️ 3. Oracle 동적 쿼리 기반 실시간 소득분위 산정 알고리즘
* 주민의 이름과 주민등록번호 조합을 입력받아 DB 내 저장된 소득 금액 자산(`j_sal`)에 따라 1분위부터 5분위까지 실시간 분류하는 스케일러블한 쿼리를 구축했습니다.

```c
/* 이름 및 주민등록번호 기반 소득분위 실시간 분류 및 Dynamic SQL 실행 파트 */
sprintf(dynstmt, 
    "SELECT j_name, j_social_num, j_sal, "
    "CASE WHEN j_sal BETWEEN 0 AND 100 THEN '1분위' "
    "     WHEN j_sal BETWEEN 101 AND 1000 THEN '2분위' "
    "     WHEN j_sal BETWEEN 1001 AND 10000 THEN '3분위' "
    "     WHEN j_sal BETWEEN 10001 AND 100000 THEN '4분위' "
    "     ELSE '5분위' END AS 소득분위 "
    "FROM jumin "
    "WHERE j_name LIKE '%%%s%%' AND j_social_num LIKE '%%%s%%'", 
    no_temp1, no_temp2);

EXEC SQL PREPARE S FROM :dynstmt;
