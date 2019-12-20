

# DBMS 03

## 1. 조인

- 정규화된 테이블이나 혹은 일반적으로 작성된 여러 테이블의 컬럼을 이용해서 데이터를 조회하는 것을 조인이라고 한다.

- 조인은 관계형 데이터베이스에 반드시 알아야 하는 개념

- 기본키와 외래키의 관계를 이용해서 테이블을 조인.

  외래키를 가지고 기본키 테이블에서 값을 비교하여 작업이 진행된다.

- 조인을 하는 경우 무조건 where절에 조인조건을 정의해야 한다.

- 테이블을 여러개 사용하는 경우 모든 테이블들의 조인조건을 정의해야 하며 select절에서 사용하지 않고 조건으로만 사용한다 하더라도 조인조건은 정의해야한다.



###  [조인 방법]

1. from절에 조회하고 싶은 데이터가 저장된 테이블들을 모두 명시

2. 조인을 하는 경우 컬럼이 어떤 테이블의 컬럼인지 명확하게 정의하기 위해 "테이블명.컬럼명"으로 액세스 한다.

3. from절에 테이블명을 정의할 때 alias를 함께 추가하여 alias를 통해 액세스하도록 한다.

    select alias1.컬럼명, alias2.컬럼명....

   from 테이블1 alias1, 테이블2 alias2

4. where절에는 반드시 조인조건을 추가하며 조인조건에는 두 테이블 값을 비교하기 위해 정의하는 것이므로 외래키와 기본키를 정의한다

   외래키테이블(child테이블)에 정의된 컬럼값을 기본키테이블(parent테이블)에서 비교하여 정확하게 일치하는 경우 값을 가져온다.(기본조인)

   > 만약에 테이블이 두개이면 조인 조건1개, 테이블이 3개이면 조인조건 2개



### 1) 조인종류

- equi join(inner join) :  두 테이블에서 정확하게 일치하는 컬럼에 대한 데이터만 조회
- outer join : 두 개 이상의 테이블에 조인을 적용했을 때 join조건을 만족하지 않아도 데이터를 조회하고 싶을 경우 사용조인조건에 (+)를 추가한다.
- join조건을 만족하지 않아도 한쪽 테이블의 모든 데이터를 출력하고 싶을 때 사용하는 조인방식으로 정보가 부족산 테이블 컬럼에 + 추가
- select 테이블 alias,.컬럼명
- self - join: 두개 이상의 테이블에서 조인하지 않고 같은 테이블의 컬럼들을 이용해서 조인(하나를 가상테이블ㅇ)

관리자별 인원수 구하기

SQL> select m.ename, count(e.empno)
  2  from emp e , emp m
  3  where e.mgr = m.empno
  4  group by m.ename;

ENAME                COUNT(E.EMPNO)
-------------------- --------------
JONES                             2
FORD                              1
CLARK                             1
SCOTT                             1
BLAKE                             5
KING                              3

//self조인이지만 관리자가 없는 사람들도 보여주려면



## 2.뷰와 커리



뷰는 반드시 alias를 주어야한다.

SQL> conn system/manager
Connected.
SQL> grant create view to scott;

Grant succeeded.

SQL> conn scott/tiger;
Connected.
SQL>  create view countdata
  2   as
  3   select deptno, avg(sal) empcount
  4   from emp
  5   group by deptno;

View created.

//뷰는 가상테이블



## 3. 서브쿼리

- SQL문에서 삽입된 query

- select문에서 주로 사용하고 select문에 삽입된 select문, 바깥쪽의 query를 main query, 안쪽에 삽입된 query를 sub qurery라 한다.

- 작성 방법

  sub query는 괄호로 묶어 주어야한다.

  sub query는 메인query가 실행되기전 한번 실행되며 그 실행 결과를 메인쿼리를 사용한다.

  

  ### [서브쿼리 종류]

  #### 1.단일행 서브쿼리- 서브쿼리의 결과가 1행 1열인 서브쿼리 

  

  '>',>=,<,<= 와 같은 연산자

  [실습]

  10번 부서의 평균 급여보다 급여를 많이 받는 사원들을 조회

  전체 평균보다 높은 급여를 받는 사원의 목록(ename,sal)

  SQL> select ename, sal
    2  from emp
    3  where sal>(select avg(sal)
    4             from emp)
    5  ;

  ENAME                       SAL
  -------------------- ----------
  JONES                      2975
  BLAKE                      2850
  CLARK                      2450
  SCOTT                      3000
  KING                       5000
  FORD                       3000

  6 rows selected.

  smith와 같은 job을 가지고 있는 사원의 목록(ename,job,hiredate)

- SQL>  select ename,job, hiredate
    2   from emp
    3   where job =(select job
    4               from emp
    5               where ename = 'SMITH');

  ENAME                JOB                HIREDATE
  -------------------- ------------------ --------
  SMITH                CLERK              80/12/17
  ADAMS                CLERK              83/01/12
  JAMES                CLERK              81/12/03
  MILLER               CLERK              82/01/23]

- 10번 부서에 JOB과 같은 JOB을 가지고 있는 사원의 목록

#### 2. 다중행 서브쿼리





- 섭즈쿼리의 실행 결과과 열 하나의 행이 여려개인 경우

-  = 연산자와 같은 비교연산자를 사용할 수 없다.

- in,any ,all

- SQL>  select sal
    2    from emp
    3  where deptno =10;

  



#### 3. 다중컬럼 서브쿼리

    - 두개 이상의 컬럼과 다중행을 반환하는 서브쿼리
    - 메인쿼리 비교 컬럼의 갯수,종류가 서브쿼리의 반환결과와 동일

...

where(컬럼1,컬럼2) in (select 컬럼1, 컬럼2

​                                                     .......)

[실습]

- 각 부서별로 최소급여 받는 사원의 정보를 출력

  (사원명, 부서코드, 급여.입사일)



SQL>  select ename,deptno,sal,hiredate
  2   from emp
  3  where(deptno,sal) in (select deptno,min(sal)
  4                        from emp
  5                        group by deptno);

ENAME                    DEPTNO        SAL HIREDATE
-------------------- ---------- ---------- --------
SMITH                        20        800 80/12/17
JAMES                        30        950 81/12/03
MILLER                       10       1300 82/01/23



#### 4.상관형 서브쿼리(상호연관서브쿼리)

- 메인쿼리의 값이 서브쿼리에서 사용되는 경우
- 메인쿼리 한 row에 대해 서브쿼리가 한번씩 실행된다.
- 메인쿼리의 값이 어떤 값이냐에 따라 서브쿼리의 결과가 달라진다.()

[실행]

1. 메인쿼리에서 비교할 값을 가져온다.
2. 메인쿼리에서 받은 값을 이용해서 서브쿼리 실행
3. 서브쿼리의 실행결과로 메인쿼리가 실행된다.
4. 메인 쿼리의 레코드 수 만큼 반복된다.
5. 

[실습]

소속부서의 급여 평균보다 급여가 많은 사원들의 정보 출력

(ename,deptno,sal)

select ename,deptno,sal
from emp outer
where sal>(select avg(sal)
           from emp e
           where e.deptno = outer.deptno);



 select ename, deptno, sal
 from emp
 where sal>(select avg(sal)
           from emp
           where = 현재 작업중인 row의 부서코드



#### 5. from 절에서 사용하는 서브쿼리(inline view)

- from 절에 서브쿼리를 추가해서 사용
- 서브쿼리 결과를 가상 테이블로 사용하겠다는 의미
- from 절에 추가되는 서브쿼리는 alias를 정의해야 한다.
- from절이 추가되는 서브쿼리의 내부의 컬럼은 실제 컬럼처럼 메인쿼리에서 사용해야 하므로 컬럼명이 존재하거나 alias를 정의해야한다.
- 

select 컬럼명1,......

from (select 컬럼

​		  from 테이블명	

​		 	where...

​		 group by ....) alias

[실습]



select deptcode, countdata
from(select deptno as deptcode,count(empno) as countdata=>이것으로 가상
     from emp
     group by deptno)

SQL> select deptcode, countdata
  2  from(select deptno as deptcode,count(empno) as countdata
  3       from emp
  4       group by deptno) myTable;

  DEPTCODE  COUNTDATA
---------- ----------
        30          6
        20          6
        10          3


소속부서의 급여 평균보다 급여가 많은 사원들의 정보 출력

=>조인과 from 절에 추가하는 서브쿼리를 이용해서 작업

SQL>  select e.ename,e.deptno,e.sal, d.avgsal
  2   from emp e,(select deptno,avg(sal) avgsal
  3               from emp
  4               group by deptno) d
  5   where e.deptno = d.deptno
  6         and e.sal > d.avgsal;

ENAME                    DEPTNO        SAL     AVGSAL
-------------------- ---------- ---------- ----------
BLAKE                        30       2850 1566.66667
ALLEN                        30       1600 1566.66667
FORD                         20       3000 2029.16667
SCOTT                        20       3000 2029.16667
JONES                        20       2975 2029.16667
KING                         10       5000 2916.66667

6 rows selected.











insert all
           into member values('lee','1234','인천')
           into member values('kang','1234','안산')
           into member values('hong','1234','수원')
select * from dual;



insert into member values('JJang',null,null);

