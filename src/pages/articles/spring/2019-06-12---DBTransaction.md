---
title: 스프링 DB연동 및 Transaction
date: "2019-06-12"
layout: post
draft: false
path: "/posts/DB/"
category: "SPRING"
tags:
  - "SPRING"
description: "스프링에서 DB를 연동하는 방법과 Transaction에 대해 알아보자."
---

>최범균님의 *스프링 프로그래밍 입문* 이라는 책을 통해 공부한 부분을 블로그에 정리하고자 합니다.
>잘못되거나 부족한 부분이 있으면 언제든 댓글로 가르침 부탁드립니다.


## 목차
1. 스프링 JdbcTemplate의 필요성
2. JDBC, DataSource, Connection Pool
3. JdbcTemplate를 이용한 쿼리
4. Transaction


## 1. 스프링 JdbcTemplate의 필요성
### 1.1 JDBC 란?  
Java DataBase Connectivity의 약자로, DB 종류에 관계없이 자바에서 DB와 연동하기 위한 API.  
DB에 맞는 JDBC 드라이버를 사용해서 JAVA 어플리케이션와 DB를 연결시킨다.  
(ex : MYSQL:com.mysql.jdbc.Driver, 오라클:orcle.jdbc.driver.OracleDriver ..)

### 1.2 JDBC를 이용한 DB 연동
자바에서 JDBC를 사용해서 DB를 연동시키는 방법은 보통 다음과 같은 과정을 거친다.
1) JDBC 드라이버 로드
2) Connection객체에 DB 연결
3) PreparedStatement에 쿼리 입력 및 실행
4) ResultSet에 저장

코드는 아래와 같다.

```
Connection con = null;
Class.forName("com.mysql.jdbc.Driver");
con = DriverManager.getConnection("jdbc:[url]:[포트]", 계정ID, 계정비밀번호);
PreparedStatement stmt = con.prepareSatement("쿼리");
ResultSet rs = psmt.executeQuery();
```

이 과정은 매번 db와 연동할 때 거쳐야 하는 중복 코드이다.  
따라서 스프링은 이를 줄이기 위해 jdbcTemplate 클래스을 제공한다.


## 2. JdbcTemplate, DataSource, Connection Pool
### 2.1 JdbcTemplate 사용을 위한 3가지 모듈 설정 (pom.xml에 추가)  
 - spring-jdbc : JdbcTemplate 제공
 - tomcat-jdbc : DB 커넥션풀 제공
 - mysql-connector-java : MYSQL에 맞는 JDBC 드라이버 제공(DB가 MYSQL일 경우만)

### 2.2 JdbcTemplate 사용에 필요한 객체 설정
 1) DataSource 객체 - connection 객체를 관리하는 역할, applicationContext에 설정
 2) JdbcTemplate 클래스 - dataSource 객체를 참조하는 속성을 가진다, applicationContext에 설정
 3) Mapper 클래스 - 쿼리 결과인 resultSet을 자바 list로 매핑하기 위해 필요

### + DB 커넥션풀 (Connection Pool) 이란?  
자바에서 DB에 접근하기 위해서는 커넥션 객체가 필요하다. 이때 많은 클라이언트가 DB에 접근 요청을 날릴 경우 매번 커넥션 객체를 만들어야 하기에 부하가 커진다. 이를 해결하기 위해 일정 개수의 커낵션 객체들을 미리 만들어두고 필요할때마다 꺼내쓰고 다시 돌려놓는 기법을 의미한다.

### + mybatis를 사용할 경우 아래 3가지를 applicationContext에 설정해야 한다.  
1) DataSource 객체  
2) mybatis제공하는 SqlSessionFactoryBean 객체(DataSource, sql Mapper 속성)  
3) sqlSessionTemplate 객체 (SqlSessionFactoryBean을 생성자에서 참조)


## 3. JdbcTemplate를 이용한 쿼리
### 3.1 JdbcTemplate 객체 생성
JdbcTemplate는 보통 DAO에서 사용되며 DataSource 객체를 파라미터로 하여 객체를 생성할 수 있다.  
```
public class MemberDao {

  private  JdbcTemplate jdbcTemplate;

  public setDataSource(DataSource dataSource){
    this.jdbcTemplate = new JdbcTemplate(dataSource);
  }

}
```
### 3.2 query 메서드를 이용한 조회 쿼리
query 메서드는 JdbcTemplate 클래스에서 제공하는 SELECT 쿼리 실행 메서드이다. String으로 sql 쿼리를 파라미터로 입력하면 조회 결과를 ResultSet에 담고 Mapper를 이용해 자바 객체로 변환하고, 마지막에 List로 return 해준다. 다음과 같이 3가지 방식으로 메서드를 제공한다.

```
List<T> query(String sql, RowMapper<T> rowMapper)
List<T> query(String sql, Object[] args, RowMapper<T> rowMapper)
List<T> query(String sql, RowMapper<T> rowMapper, Object ... args)
```

#### + RowMapper 인터페이스

```
public interface RowMapper<T> {
  T mapRow(ResultSet rs, int rowNum) throws SQLException;
}
```
RowMapper의 rowNum은 SQL 실행결과로 구한 ResultSet의 한 행을 자바 객체로 변환하는 역할을 한다.

#### ++ 예시
```
public Member selectMembers(){
  List<Member> result = jdbcTemplate.query(
    "SELECT * FROM TB_MEMBER WHERE EMAIL=? AND NAME=?"
    , new RowMapper<Member>(){
        @Override
        public Member mapRow(ResultSet rs, int rowNum){
          Member member = new Member(
            rs.getString("EMAIL"),
            rs.getString("NAME"),
          );
          return member;
        }
    }
    , email
    , name
  );
}
```
query 메서드의 3번째 인자로 들어감 파라미터 개수가 2개 이상이면(SQL에 물음표 개수) 콤마로 구분한다.


### 3.3 queryForObject를 이용한 1행 조회 쿼리
JdbcTemplate 클래스에서 제공하는 queryForObject 메서드는 결과가 1행인 경우에만 사용할 수 있다. query 메서드와는 크게 2개의 차이점이 있다.
  1. 첫번째는 방금 얘기했던대로 결과가 1행인 경우만 사용할 수 있다는 점이다. 결과가 0행이거나 2행 이상이면 exception을 발생시킨다.
  2. 두번째는 query는 List<객체> 타입으로 return 하지만 queryForObject 메서드는 자바 객체 타입으로 return한다. queryForObject는 4가지 형태로 제공된다.

```
// 쿼리 결과, 컬럼이 1개인 경우 (결과가 하나의 값인 경우)
T queryForObject(String sql, Class<T> requiredType)
T queryForObject(String sql, Class<T> requiredType, Object ... args)
// 쿼리 결과, 컬럼이 2개 이상인 경우
T queryForObject(String sql, RowMapper<T> rowMapper)
T queryForObject(String sql, RowMapper<T> rowMapper, Object ... args)

```

### 3.4 update 메서드를 이용한 변경 쿼리
JdbcTemplate 클래스가 제공하는 update() 메서드를 이용하면 INSERT, UPDATE, DELETE 쿼리를 모두 실행할 수 있다. 제공 형태는 아래 2가지이다.
```
int update(String sql)
int update(String sql, Object ... args)

```




## 4. Transaction
### 4.1 Transaction 이란?  
Transaction은 2개 이상의 쿼리를 작업할 때 이를 하나의 작업 단위로 묶어 주는 것.
스프링에서 제공하는 Transaction이 나오기 전까지는 프로시져를 통해 처리했다고 한다.

#### 4.2 Transaction 적용하는 2가지 방법
 1. @Transactional 어노테이션을 이용하는 방법
 2. `<tx:advice>` 태그를 이용하는 방법

#### 4.3 @Transactional 어노테이션
다음 2가지를 applicationContext에 설정해야 한다.
   1) TransactionManager - 스프링의 구현 기술에 관계없이 동일한 방식으로 transaction을 처리하게 해준다, dataSource를 속성을 참조한다.
   2) 어노테이션 활성화 - .xml 기준 : `<tx:annotation-driven>` 태그 / .java 기준 : @EnableTransactionManagement 애노테이션

그리고 사용하고 싶은 메소드 위에 `@Transactional` 애너테이션을 붙이기면 하면 된다.  

단,
 - RuntimeExeption일 때만 롤백이 적용되기 때문에 SQLException 처럼 RuntimeExeption이 아닐 경우 롤백되지 않는다.   
 - 따라서 `@Transactional(rollbackFor=SQLException.class)` 처럼 임의로 설정해줘야 한다.   
 - 반대로 롤백하고 싶지 않은 Exception이 있다면 noRollbackFor 속성을 사용하면 된다.  


#### 4.4 `<tx:advice>` 이용하는 방법
다음 3가지를 applicationContext에 설정해야 한다.  
1. transaction-manager
2. `<aop:pointcut>` 설정 - transaction 거는 포인트(클래스 단위) 설정
3. `<tx:advice id="포인터컷명">` 설정 - 해당 포인트컷에서 어떤 name을 가진 메소드에 트랜잭션을 포함시킬지 설정

예시,
```
<aop:config>
    <aop:pointcut id="basicTxAdvice" expression="execution(* com.**.* *Service.*(..))"/>
    <aop:advisor id="transactionAdvisor" pointcut-ref="basicTxAdvice" advice-ref="txAdvice"/>
</aop:config>

<tx:advice id="txAdvice"  transaction-manager="transactionManager">
  <tx:attributes>
    <tx:method name="save*"   rollback-for="Exception"/>
    <tx:method name="update*" rollback-for="Exception"/>
    <tx:method name="remove*" rollback-for="Exception"/>
  </tx:attributes>
</tx:advice>
```