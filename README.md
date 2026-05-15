# 가야동 주민 관리 시스템
<b>주민 센터에서 주민들의 정보를 조회하여 필요한 정보와 해당되는 복지혜택을 제공 및 관리하는 시스템 설계 및 개발. 개인정보와 장애인 혜택 대상을 조회 할 수 있는 기능 구현과 필요한 테이블 레코드 작성을 설계함.</b>

## 프로젝트 목적과 내용
주민 센터에서 주민들의 정보를 조회하여 필요한 정보와 해당되는 복지혜택을 제공 및 관리하는 시스템 설계 및 개발. 개인정보와 장애인 혜택 대상을 조회 할 수 있는 기능 구현과 필요한 테이블 레코드 작성을 설계함.
    
## 프로젝트 기간
2018.11.12 ~ 2018.12.15

## 기술 스택
💻 Language & Frameworks
- C

🗄 Database
- MySQL

## 프로젝트
### main
#### main.test.pc

```c
EXEC SQL BEGIN DECLARE SECTION;
    VARCHAR uid [80];
    VARCHAR pwdl201;
EXEC SQL END DECLARE SECTION;
  
#define getch( ) _getch()
 
void main()
{
    DB_connect();
    Show_tuple();
    P_search();
    Update_pro();
    Show_tuple();
    getch();
}
  
void DB_connect()
{
    strcpy((char *)uid.arr,"xxxxxx@//sedb.deu.ac.kr:1521/orcl");
    uid. len = (short) strlen((char *)uid.arr);
    strcpy((char *)pwd.arr,"xxxxxxx") ;
    pwd. len = (short) strlen((char *)pwd.arr);
      
    EXEC SQL CONNECT :uid IDENTIFIED BY :pwd;
       
    // connection이 실패했을경우의 처리부분
    if (salca.sqlcode != O && salca.sqlcode != -1405){
        printf("\7Cconnect error: %s", sqlca.sqlerrm.sqlerrmc);
        getch();
        exit (-1);
    }
    printf("Oracle Connect SUCCESS by %s\n", uid.arr);
}
```
#### main.txt
```c
void print_screen(char fname[])
{
    FITE *fp;
    char line[100];

    if ((fp = fopen(fname, "r")) == NULL) {
        printf("file open error\n");
        getch();
        exit(-1);
    }
    while(1)
    {
        if((fgets(line, 99, fp) == NULL) {
            break;
        }
    }
    fclose(fp);
}
```

### information
#### new_select_xy.c
1. 튜플
```c
void Get_tuple()
{
/* ------------------------------------------------------------------------------------ */
    Retrieve the current maximum employee number
/* ------------------------------------------------------------------------------------ */
/* EXEC SQL BEGIN DECLARE SECTION; */
    
/* varchar name[100]; */
struct { unsigned short len; unsigned char arr[100]; } name;
/* varchar social[150]; */
struct { unsigned short len; unsigned char arr[150]; } social;
/* varchar spouse [150]; */
struct { unsigned short len; unsigned char arr[150]; } spouse;
/* varchar sal[150]; */
struct { unsigned short len; unsigned char arr[150]; } sal;
/* varchar fam[100]; */
struct ‹ unsigned short len; unsigned char arr[1001; } fam;
/* varchar protrank[100]; */
struct ‹ unsigned short len; unsigned char arr[100]; & protrank;
/* varchar tel[200]; */
struct { unsigned short len; unsigned char arr[2001; } tel;
/* varchar address[400]; */
struct { unsigned short len; unsigned char arr[400]; } address;
/* varchar child[100]; */
struct { unsigned short len; unsigned char arr[100]; } child;
/* varchar disrank[100]; */
struct { unsigned short len; unsigned char arr[100]; } disrank;
```
이름, 주민번호, 배우자 주민번호, 재산, 가족, 보호관찰등급, 전화번호, 주소, 자녀, 장애등급으로 변수 선언.

2. 데이터베이스 실행
```c
/* 사용자 입력 */
clrser();
    
print_screen ("scr_select.txt");
    
/* 실행시킬 SQL 문장*/
sprintf(dynstmt, "SELECT J_NAME, J_SOCIAL_NUM, J_SPOUSE_NUM, J_SAL, J_FAM, J_PROT_RANK, J_TEL, J_ADDRESS, J_CHILD, J_DIS_RANK FROM JUMIN") ;
    
/* select 문장이 제대로 구성되어 있는지 화면에 찍어봄 */
//printf("dynstmt:%s\n", dynstmt);
    
/* EXEC SQL PREPARE S2 FROM : dynstmt ; */
```

### disorder
#### main.pc
1. 튜플
```c
void Show_tuple()
{
/* ------------------------------------------------------------------------------------ */
    Retrieve the current maximum employee number
/* ------------------------------------------------------------------------------------ */
EXEC SQL BEGIN DECLARE SECTION;
    
    varchar name [100]; 
    varchar num[100]; 
    varchar grade [100]; 
    varchar cf [100]; 
    varchar cyn [100]; 
    varchar special[100];
        
    char dynstmt [1000];
EXEC SQL END DECLARE SECTION;
    
    int x, y, count=0, i ;
    /* Register sal_error() as the error handler. */ EXEC SQL WHENEVER SQLERROR DO sal_error("\70RACLE ERROR: \n"') ;
        
    gotoxy (1,1);
    print_screen("scr_select.txt");
    
    /* 실행시킬 SOL 문장*/
        sprintf(dynstmt, "SELECT p_name, P_social_num, P_grade, P_contact_f, P_contact_pa, p_manage FROM probation");
        
    EXEC SQL PREPARE S FROM : dynstmt ;
```

2. 장애인 혜택대상 조회
```c
void P_search(){
EXEC SQL BEGIN DECLARE SECTION;
    varchar p_name [100];
    varchar p_num[100];
    varchar p_grade [100];
    varchar p_cf[100];
    varchar p_cyn [100];
    varchar P_special [100];
    
    char dynstmt2 [1000];
EXEC SQL END DECLARE SECTION;
        
    int x, y, count=0, i ;
    /* Register sql_error() as the error handler. */
    EXEC SQL WHENEVER SQLERROR DO sal_error("\ 70RACLE ERROR: \n");
// print_screen("'scr_select.txt");
    
    gotoxy (22,6);
    gets (no_temp2);
    
/* 실행시킬 SOL 문장*/
sprintf(dunstmt2, "SELECT o name, o social num, o arade, o contact f, o contact pa, o manade EROM probation where p name LIKE regeseg'". no temp2):
EXEC SOL PREPARE S FROM : dynstmt2 :
```

3. 데이터 베이스 조회
```c
void P_search(){
EXEC SQL BEGIN DECLARE SECTION;
    varchar p_tel [100];
   
    char telbp [1000];
EXEC SQL END DECLARE SECTION;
        
    /* Register sql_error() as the error handler. */
    EXEC SQL WHENEVER SQLERROR DO sal_error("\ 70RACLE ERROR: \n");
    
    gotoxy (22,6);
    
/* 실행시킬 SOL 문장*/
sprintf(telbp, "SELECT J_TEL FROM jumin where J_NAME LIKE '99%5%%' and J_PROT_RANK <> '0'", no_temp2);

EXEC SOL PREPARE 53 FROM :telbp:
```

4. 업데이트
```c
for (;;)
{
    if(atoi(temp) ==1){
        sprintf(dynstmt22, "update probation set p_contact_f=p_contact_f+1, P_contact_pa = 'YES' where p_name like '988588' and rownum = 1", no_temp2);
        EXEC SQL EXECUTE IMMEDIATE : dynstmt22;
        EXEC SQL COMMIT WORK ;
       
    stropy (temp, "0");
    }
        
    if(atoi(temp)==2){
    gotoxy (52,23);
        printf ("연락을 취소합니다");
    }
    if( intcontact_f=5)
    {    
    sprintf(dynstmt23, "SELECT P_contact_f FROM probation");
    EXEC SQL EXECUTE IMMEDIATE : dynstmt23;
        
    sprintf(dynstmt24, "update probation set p_manage = 'YES' where P_contact_f >= 5");
    EXEC SQL EXECUTE IMMEDIATE :dynstmt24;
    EXEC SQL COMMIT WORK ;
    
    y = 10 ;
    break;
```
이름, 주민등록번호, 장애여부를 이용하여 월 1회 제공되는 혜택을 받아야 하는 대상 조회한다. 연락처를 누르고 숫자를 입력하면 연락 정보 여부를 알 수 있다.

#### scr_select.txt
1. 기본화면
```c
for (;;) {
    EXEC SQL FETCH c_ cursor INTO :name, :num, :grade, :cf, :cyn, :special;

    name.arr [name.len] = '\0';
    num.arr [num.len] = '\0' ;
    grade.arr[grade.len] = '\0' ;
    cf.arr[cf.len] = '\0';
    cyn.arr[cyn. len] = '\0' ;
    special.arr[special.len] = '\0';

    gotoxy (x, y) ;
    printf("'%-9s | 8-15s | 8-55 | 8-55 18-55 |%-5s I', name.arr, num.arr, grade.arr, cf.arr, cyn.arr, special.arr);
    
    y++;
    
            gotoxy(0,10): //이전 화면 부분 클리어
    }
    /* Close the cursor. */ 
    EXEC SQL CLOSE c_cursor;
    EXEC SQL COMMIT ;
}
```
2. 조회화면
```c
for (;:)
    {
        EXEC SQL FETCH C_cursor2 INTO :p_name, :p_num, :p_grade, :p_cf, :p_cyn, :p_special ;

    p_name.arr[p_name.len] = '\0';
    p_num.arr[p_num. len] = '10' ;
    p_grade.arr[p_grade. len] = '\0' ;
    p_cf.arr[p_cf.len] = '\0';
    p_cyn.arr[p_cyn.len] = '10' ;
    P_special.arr[p_special.len] = '\0';

    gotoxy (x, y);
    printf("g-95 | %-155 | %-5s | %-5s | %-5s | l", l", p_name.arr, p_grade.arr, p_grade.arr, p_cf.arr, p_cyn.arr, p_special.arr);

    y++;
    count++;
    if( count == PAGE_NUM) {
        printf("\n                                        hit any key\n");
        count = 0;
        getch ();

            gotoxy(0,10); //이전 화면 부분 클리어
                for(i= 0; i < PAGE_NUM; i++){
        printf("                                           \n"');
        }

        y = 10 ;
    if ( count ==0)
        { printf ("검색 결과가 없습니다"); }
    }
}

printf("                                        (END) \n");
```
3. 연락처 조회화면
```c
for (;;)
    {
        EXEC SQL FETCH c_cursor3 INTO :p_tel;
    P_tel.arrIp_tel. len] = '10';

    gotoxy (14,21);
    printf("%-20s" , P_tel.arr);
    
    if( count == PAGE_NUM) {
        printf("\n                                hit any key\n");
        count = 0;
        getch ();
        
            gotoxy(0, 10): //이전 화면 부분 클리어
            for (i= 0; i < PAGE_NUM; i++){
        printf("                                                        \n");
        }
    
    y = 10 ;
    if (count ==0)
        { printf("검색 결과가 없습니다); }
    }

    printf("                                   (END) \n");
    
    * Close the cursor. */ 
    EXEC SQL CLOSE c_cursor3;
    EXEC SQL COMMIT :
```

## 문제점 & 수정사항
#### new_select_xy.c
1. 튜플
```c
void Get_tuple()
/* --------------------------------------------------------------------
Retrieve the current maximum employee number
 -------------------------------------------------------------------- /*
/* EXEC SQL BEGIN DECLARE SECTION; */

/* varchar j_name[100]; */
struct { unsigned short len; unsigned char arr[100]; } j_name;

/* varchar j_social_num[100]; */
struct { unsigned short len; unsigned char arr[100]; } j_social_num;

/* varchar j_sal[100]; */
struct { unsigned short len; unsigned char arr[100]; } j_sal;

/* varchar j_sal_quint[100]; */
struct { unsigned short len; unsigned char arr[100]; } j_sal_quint;

    char dynstmt [1000];

/* EXEC SQL END DECLARE SECTION; */
```

2. 소득분위
```c
/* 사용자 입력 */
cirser();

print_screen("scr_select.txt");

gotoxy (21,6);
1/printf("찾을 사람의 이름 입력하세요:");
gets (no_temp1);

gotoxy (57,6);
//printf("찾을 사람의 주민등록번호 입력하세요:");
gets (no_temp2);

/* 실행시킬 SOL 문창*/
sprintf(dynstmt, "SELECT j_name, j_social_num, j_sal, case when j_sal between O and 100 then '1분위'
when j_sal between 101 and 1000 then '2분위' when j_sal between 1001 and 10000 then '3분위' when j_sal between 10001 and 100000 then '4분위' when j_sal > 100000 then '5분위' end 소득분위 FROM jumin WHERE to_char(j_name) LIKE '%%%s%%' and j_social_num LIKE '%%%s%%' ", no_templ, no_temp2);

/* select 문장이 제대로 구성되어 있는지 화면에 찍어봄 */
//printf("dynstmt:%s\n", dynstmt);

/* EXEC SQL PREPARE S FROM : dynstmt; */
```
이름, 주민등록번호, 소득(금액) 등의 정보를 이용하여 소득분위 분류.</br>
( 월  0원 ≤ 1분위 ＜ 월 100만원 )</br>
( 월  100만원 ＜ 2분위 ≤ 200만원 )</br>
( 월  200만원 ＜ 3분위 ≤ 300만원 )</br>
( 월  300만원 ＜ 4분위 ≤ 400만원 )</br>
( 월  400만원 ＜ 5분위 ≤ 500만원 )</br>


#### scr_select.txt
```c
j_name.arrlj_name. len] = '10';
j_social_num.arr| j_social_num. len] = '10';
j_sal.arr[j_sal.len] = '\0';
            j_sal_quint.arrlj_sal_quint.len] ='\0';
gotoxy (x, y) ;
printf(" %-11s %-17s %-12s% %-10s", j_name.arr, j_social_num.arr, j_sal.arr, j_sal_quint.arr);

y++;
count+t；

if ( count = PAGE_NUM) {                    hit any key\n");
    printf("\n
    count = 0;
    getch ();
        gotoxy(0,10); //이전 화면 부분 클리어
        for (i= 0; i < PAGE_NUM; i++){
    printf("                                                    \n"');
    }

    y = 10 ;
            if(count==0)
                    {printf (검색 결과가 없습니다.");}
    }
    printf("                            (END) \n") ;
    /* Close the cursor. */
    /* EXEC SQL CLOSE c_cursor; */
```
수정사항
- 개인정보 조회 기능은 프로젝트에서 제외헀다.

## 실행 결과
<img src="docs/static/main_화면.jpg" width="650" height="400" /></p>
메인화면

<img src="docs/static/db_정보조회.png" height="400" /></p>
조회화면에서 데이터베이스에 들어있는 모든 정보를 조회할 수 있음.

<img src="docs/static/연락처지정.png" width="650" height="400" /></p>
주민 이름을 검색하면 정보를 볼 수 있고 연락을 지정할 수 있다.

<img src="docs/static/주민번호_소득분위.png" width="650" height="400" /></p>
이름과 주민등록번호를 입력하면 소득분위를 볼 수 있다.
