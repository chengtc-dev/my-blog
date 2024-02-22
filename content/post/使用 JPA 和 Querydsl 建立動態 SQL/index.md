---
title: 使用 JPA 建立動態 SQL
date: 2024-01-26
categories:
    - JPA
tags:
    - JPA
    - Java
---

## JPA
JPA 是 Java Persistence API 的縮寫，是一種 Java EE 的標準規範，用於透過ORM（物件關聯映射）方式來管理 Java 應用程式中的資料庫。JPA 提供了對關聯資料庫的標準化存取，並且能夠將 Java 物件映射到資料表。

## 範例
1. 新增 Entity

```java
@Entity
@Table(name = "`products`")
@Getter @Setter @ToString @AllArgsConstructor @NoArgsConstructor
public class Product extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private BigDecimal price;
    private Long stock;
    private String description;
}
```

2. 建立 Specification 類別，用來建立動態查詢條件

```java
public class ProductSpecification {
    public static Specification<Product> containKeyword(String keyword) {
        return (root, query, criteriaBuilder) -> {
            String pattern = "%" + keyword + "%";
            return criteriaBuilder.or(
                    criteriaBuilder.like(root.get("name"), pattern),
                    criteriaBuilder.like(root.get("description"), pattern)
            );
        };
    }
}
```


3. 接著就可以在其他類別中直接使用，若要加入分頁功能，要先在 Repository 類別上繼承 JpaSpecificationExecutor<T>
 
```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long>,
                                           JpaSpecificationExecutor<Product> {}

public Page<Product> getProducts(ProductQueryParams productQueryParams) {
    Sort sort = Sort.by(productQueryParams.getOrderBy(), productQueryParams.getSortBy());
    Pageable pageable = PageRequest.of(productQueryParams.getPageNumber(), productQueryParams.getPageSize(), sort);
    Specification<Product> specification = Specification.where(null);

    if (!productQueryParams.getKeyword().isBlank()) {
        specification = specification.and(ProductSpecification.containKeyword(productQueryParams.getKeyword()));
    }

    return productRepository.findAll(specification, pageable);
}
```