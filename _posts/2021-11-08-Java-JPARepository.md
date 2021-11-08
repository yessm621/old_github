---
layout: post
title:  "JPA Repository"
date:   2021-11-08 10:00:00 0100
categories: Java JPA
---
<br>


**ItemRepository.java**

```java
package com.shop.shop.repository;

import com.shop.shop.entity.Item;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ItemRepository extends JpaRepository<Item, Long> {

}
```

<br>

## JpaRepository
---

JpaReporitory 는 2개의 제네릭 타입을 사용하는데 첫번째에는 엔티티 타입 클래스, 두번재는 기본키 타입을 넣어줌

JpaReporitory 는 기본적인 CRUD 및 페이징 처리를 위한 메소드가 정의돼 있음

- JpaReporitory 에서 지원하는 메소드 예시

    ![jpaRepository](https://user-images.githubusercontent.com/79130276/140668825-9087ab17-8b1b-43ae-86cd-1d09c0e49de1.png)

<br>

**application-test.properties**

```java
# Datasource 설정
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.url=jdbc:h2:mem:test
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
```

<br>

**ItemRepositoryTest.java**

```java
package com.shop.shop.repository;

import com.shop.shop.entity.Item;
import com.shop.shop.entity.ItemSellStatus;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.TestPropertySource;

import java.time.LocalDateTime;

@SpringBootTest
@TestPropertySource(locations = "classpath:application-test.properties")
class ItemRepositoryTest {

    @Autowired
    ItemRepository itemRepository;

    @Test
    @DisplayName("상품 저장 테스트")
    public void createItemTest() {
        Item item = new Item();
        item.setItemNm("테스트 상품");
        item.setPrice(10000);
        item.setItemDetail("테스트 상품 상세 설명");
        item.setItemSellStatus(ItemSellStatus.SELL);
        item.setStockNumber(100);
        item.setRegTime(LocalDateTime.now());
        item.setUpdateTime(LocalDateTime.now());
        Item savedItem = itemRepository.save(item);
        System.out.println(savedItem.toString());
    }
}
```

<br>

**@SpringBootTest**

통합 테스트를 위해 스프링 부트에서 제공하는 어노테이션

실제 애플리케이션을 구동할 때처럼 모든 Bean 을 IoC 컨테이너에 등록

애플리케이션의 규모가 크면 속도가 느려질 수 있다.

<br>

**@TestPropertySource(locations = "classpath:application-test.properties")**

테스트 코드 실행 시 application.properties 에 설정해둔 값보다 application-test.properties 에 같은 설정이 있다면 더 높은 우선순위를 부여

<br>

**@Autowired**

ItemRepository 를 사용하기 위해 어노테이션을 이용하여 Bean 을 주입

<br>

**@Test**

테스트할 메소드위에 선언하여 테스트 대상으로 지정

<br>

**@DisplayName("상품 저장 테스트")**

테스트 코드 실행 시 지정한 테스트명이 노출됨

<br>
<br>