# DBMS 01

~~~ 
set linesize 300;  -> 항상 설정하기.
~~~

#### 첫번째 예제

> system/manager에서 scott/tiger 계정 만들어 사용하기

~~~ 예제1
conn system/manager(system에 접속)
create user scott identified by tiger;
(새로운 scott/tiger 계정 생성)
grant connect,resource to scott;
(접속 가능하게 만들어줌)
conn scott/tiger
~~~



> customer table만들기

~~~ table 만들기
create table customer(
       id varchar2(10) primary key,
       pass varchar2(10),
       name varchar2(15) not null,
       point number,
       regdate date);
       
select * from tab;->table보여줌
desc customer -> 안에 어떤 컬럼이 존재하는지 보여줌

~~~

> insert 하기

~~~ insert
insert into customer values('jang','1234','장동건',1000, sysdate);
~~~



~~~ 
create table test(
       num1 number,
       num2 number(3), 
       num3 number(3,2),
       num4 number(5,2),
       num5 number(10,3),
       num6 integer);

insert into test values(125.88,125.88,125.88,125.88,12345,125.88);          => error남

insert into test values(125.88,125.88,5.88,125.88,1234.12345,125.88);


     NUM1       NUM2       NUM3       NUM4       NUM5       NUM6
--------- ---------- ---------- ---------- ---------- ----------
   125.88        126       5.88     125.88   1234.123        126
   
   => 
   
   
 select user from dual; 지금 사용하고 있는 계정 알려줌
~~~

> create table product(pdno number primary key,pdname varchar2(10),pdsubname varchar2(10) not null,factno varchar2(5),pddate date,pdcost number,pdprice number,pdamount number);
>
> ​			

create table product(

​			PDNO number primarykey,

​			

create table factory(facno varchar2(5) primary key,
        facname varcher2(14) not null,
        facloc varchar2(13));
   facname varchar2(14) not null,

## 기본 select

### 구현하는 법

> select 컬럼명1, 컬럼명2,...
>
> from 테이블
>
> -> 모든 열 참조 시 '*' 사용

#### 1. 일반적인 select



- sql 문은 대소문자 구분 x

- ; sql문의 종료를 의미

  -> ; 입력하기 전에 여러 줄로 sql문 작성 가능

- 컬럼에 null저장 가능

   null은 0이나 space만 입력해 놓은 값과 다른값

   오라클에서 null은 아무 값도 없는 거슬 의미

   사용할 수 없고 저장되어 있지 않은 상태

- 컬럼명 대신 alias를 정의해서 사용할 수 있다.

    select 컬럼명 alias명or 컬럼명 as alias명 or 컬럼명 "alias명" (alias명에 공백이 있는 경우 사용)

- 여러 컬럼을 합쳐서 하나의 컬럼으로 조회할 수있다.

   ||연산자 이용

- 오라클의 문자열과 날짜 데이터는  ''로 표현한다.

- 연산의 결과로 컬럼을 생성할 수 있다.

  (+*/0, 함수...) -> 단 null이 포함되어있는 컬럼은 연산 할 수 없다.

- 중복이 있는 경우 select문에 distinct를 추가 할 수 있다.

#### 2. select 절에 조건 추가하기

​	select [distinct] 컬럼명, 컬럼명,.......[alias명]

​	from 테이블명

​	where 조건

 - 검색 결과를 제한(조건에 만족하는 데이터만 조회하겠다는 의미)

 - where 절은 from 절 다음에 정의 

 - where절은 조건식이 true인 데이터를 조회하기 때문에 조건식이 true가 되도록 정의

 - where절에 사용할 수 있는 비교 연산자

   - > ,>=,<,<=,=,(<>,!=):같지 않다.

- where절에서 조건과 함께 비교하는 값을 추가해야 하는 경우 : 문자와 날짜는 작은 따옴표로 묶어주여야 한다.

- sql 은 대소문자를 구별하지 않지만, 값을 비교할 때는 대소문자 일치 시켜야함.

- 두 개이상의 조건이 있는 경우 사용할 수 있는 연산자

  - and연산자,

  - or연산자 : 조건이 모두 다른 컬럼인 경우

  - between A and B : and연산과 동일 : 같은 컬럼에서 조건을 비교하는 경우

    ~~~ between
    SQL>  select ename,sal,comm
      2   from emp
      3   where sal between 2000 and 5000;
    
    ENAME                       SAL       COMM
    -------------------- ---------- ----------
    JONES                      2975
    BLAKE                      2850
    CLARK                      2450
    SCOTT                      3000
    KING                       5000
    FORD                       3000
    
    6 rows selected.
    
    ~~~

    

  - in연산자 : or연산자의 의미와 동일

    ​                    컬럼명 in(비교할 값, 값,.....)

    ​					같은 컬럼에서 값을 여러 개 비교해야 하는 경우

  - not연산자:부정

  - null값에 대한 비교

    is null : null인 데이터를 조회

    is not null : null이 아닌 데이터를 조회

  - like연산자 : 대표문자와 함께 사용

    ​       			 조건비교를 위해 입력한 값이 문자열에 포함되어 있는 것을 찾는 경우

    ​					%: 모든 문자열 

    ​					_: 한 자리 문자를 의미

    ~~~like연산자
    
    SQL> select ename,sal,comm
      2  from emp
      3  where ename like 'A%'; A로 시작하는 모든 것
    
    ENAME                       SAL       COMM
    -------------------- ---------- ----------
    ALLEN                      1600        300
    ADAMS                      1100
    
     where ename like '%A'; A로 끝나는 모든 것
     where ename like '%A%'; A를 포함하는것
     
    
    SQL> select ename,sal,comm
      2  from emp
      3  where ename like '_A%';//두번째에 A가 있는거
    
    ENAME                       SAL       COMM
    -------------------- ---------- ----------
    WARD                       1250        500
    MARTIN                     1250       1400
    JAMES                       950
    ~~~



#### 3. 데이터 정렬

``` skj
select[distinct] 컬럼명, 컬럼명,...[alias명]
form 테이블명
where조건
order by 컬럼명 정렬기준(asc,desc)
```

- asc : 오름차순 정렬(작->큰)
- desc : 내림차순 정렬(작->큰)

~~~
SQL>  select ename,sal,job
  2   from emp
  3   order by job asc;

ENAME                       SAL JOB
-------------------- ---------- ------------------
SCOTT                      3000 ANALYST
FORD                       3000 ANALYST
MILLER                     1300 CLERK
JAMES                       950 CLERK
SMITH                       800 CLERK
ADAMS                      1100 CLERK
BLAKE                      2850 MANAGER
JONES                      2975 MANAGER
CLARK                      2450 MANAGER
KING                       5000 PRESIDENT
TURNER                     1500 SALESMAN

ENAME                       SAL JOB
-------------------- ---------- ------------------
MARTIN                     1250 SALESMAN
WARD                       1250 SALESMAN
ALLEN                      1600 SALESMAN

14 rows selected.

SQL>  select ename,sal,job
  2   from emp
  3   order by job desc;

ENAME                       SAL JOB
-------------------- ---------- ------------------
ALLEN                      1600 SALESMAN
WARD                       1250 SALESMAN
TURNER                     1500 SALESMAN
MARTIN                     1250 SALESMAN
KING                       5000 PRESIDENT
BLAKE                      2850 MANAGER
JONES                      2975 MANAGER
CLARK                      2450 MANAGER
MILLER                     1300 CLERK
SMITH                       800 CLERK
ADAMS                      1100 CLERK

ENAME                       SAL JOB
-------------------- ---------- ------------------
JAMES                       950 CLERK
FORD                       3000 ANALYST
SCOTT                      3000 ANALYST

14 rows selected.
~~~



cl scr : 전체 화면 지움

floor, trunc버림

ceil 올림