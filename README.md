# spring-specification
specification
Dạ, tôi có thể viết specification cho entity User có các thuộc tính name, firstname, email và roles. Để làm được điều này, tôi cần giả sử rằng bạn đã định nghĩa quan hệ many-to-many giữa User và Role bằng cách sử dụng annotation @ManyToMany trong Spring Data JPA¹. Ví dụ:

```java
// Entity User
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String firstname;

    private String email;

    // Quan hệ many-to-many với Role
    @ManyToMany(fetch = FetchType.LAZY)
    @JoinTable(name = "user_roles",
            joinColumns = @JoinColumn(name = "user_id"),
            inverseJoinColumns = @JoinColumn(name = "role_id"))
    private Set<Role> roles = new HashSet<>();

    // Các getter và setter
}
```

```java
// Entity Role
@Entity
public class Role {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Quan hệ many-to-many với User (không sở hữu quan hệ)
    @ManyToMany(mappedBy = "roles", fetch = FetchType.LAZY)
    private Set<User> users = new HashSet<>();

   // Các getter và setter
}
```

Sau khi định nghĩa quan hệ many-to-many, tôi có thể viết specification cho entity User như sau:

```java
// Tạo một specification cho entity User
public class UserSpecification {

   // Tạo một specification để tìm user có name chứa keyword 
   public static Specification<User> hasName(String keyword) {
        return (root, query, cb) -> {
            // Sử dụng phương thức like của CriteriaBuilder 
            return cb.like(root.get("name"), "%" + keyword + "%");
        };
   }

   // Tạo một specification để tìm user có firstname chứa keyword 
   public static Specification<User> hasFirstname(String keyword) {
        return (root, query, cb) -> {
            // Sử dụng phương thức like của CriteriaBuilder 
            return cb.like(root.get("firstname"), "%" + keyword + "%");
        };
   }

   // Tạo một specification để tìm user có email chứa keyword 
   public static Specification<User> hasEmail(String keyword) {
        return (root, query, cb) -> {
            // Sử dụng phương thức like của CriteriaBuilder 
            return cb.like(root.get("email"), "%" + keyword + "%");
        };
   }

   // Tạo một specification để tìm user có role chứa keyword 
   public static Specification<User> hasRole(String keyword) {
        return (root, query, cb) -> {
            // Sử dụng phương thức join của Root để kết nối với bảng role 
            Join<User, Role> roleJoin = root.join("roles");
            // Sử dụng phương thức like của CriteriaBuilder trên thuộc tính name của role 
            return cb.like(roleJoin.get("name"), "%" + keyword + "%");
        };
   }
}
```

Để sử dụng các specification này trong truy vấn, bạn cần sử dụng phương thức findAll() của JpaRepository và truyền vào các specification như là tham số². Ví dụ:

```java
// Lấy ra danh sách user có firstname bắt đầu bằng "Nguyen" và role là "ADMIN"
List<User> users = userRepository.findAll(
     UserSpecification.hasFirstname("Nguyen")
         .and(UserSpecification.hasRole("ADMIN"))
);
```

Hy vọng đoạn mã này giúp bạn hiểu cách viết specification cho entity khác.😊

Nguồn: Cuộc hội thoại với Bing, 8/3/2023(1) Spring Data JPA Many To Many Relationship Mapping Example. https://attacomsian.com/blog/spring-data-jpa-many-to-many-mapping Đã truy nhập 8/3/2023.
(2) Many-To-Many Relationship in JPA | Baeldung. https://www.baeldung.com/jpa-many-to-many Đã truy nhập 8/3/2023.
(3) java - Spring Data JPA Specification for a ManyToMany Unidirectional .... https://stackoverflow.com/questions/31841471/spring-data-jpa-specification-for-a-manytomany-unidirectional-relationship Đã truy nhập 8/3/2023.
(4) ManyToMany (Java(TM) EE 7 Specification APIs). https://docs.oracle.com/javaee/7/api/javax/persistence/ManyToMany.html Đã truy nhập 8/3/2023.



Dạ, tôi có thể viết specification cho entity User có các thuộc tính name, firstname, email và roles. Để làm được điều này, tôi cần giả sử rằng bạn đã định nghĩa quan hệ many-to-many giữa User và Role bằng cách sử dụng annotation @ManyToMany trong Spring Data JPA¹. Ví dụ:

```java
// Entity User
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String firstname;

    private String email;

    // Quan hệ many-to-many với Role
    @ManyToMany(fetch = FetchType.LAZY)
    @JoinTable(name = "user_roles",
            joinColumns = @JoinColumn(name = "user_id"),
            inverseJoinColumns = @JoinColumn(name = "role_id"))
    private Set<Role> roles = new HashSet<>();

    // Các getter và setter
}
```

```java
// Entity Role
@Entity
public class Role {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Quan hệ many-to-many với User (không sở hữu quan hệ)
    @ManyToMany(mappedBy = "roles", fetch = FetchType.LAZY)
    private Set<User> users = new HashSet<>();

   // Các getter và setter
}
```

Sau khi định nghĩa quan hệ many-to-many, tôi có thể viết specification cho entity User như sau:

```java
// Tạo một specification cho entity User
public class UserSpecification {

   // Tạo một specification để tìm user có name chứa keyword 
   public static Specification<User> hasName(String keyword) {
        return (root, query, cb) -> {
            // Sử dụng phương thức like của CriteriaBuilder 
            return cb.like(root.get("name"), "%" + keyword + "%");
        };
   }

   // Tạo một specification để tìm user có firstname chứa keyword 
   public static Specification<User> hasFirstname(String keyword) {
        return (root, query, cb) -> {
            // Sử dụng phương thức like của CriteriaBuilder 
            return cb.like(root.get("firstname"), "%" + keyword + "%");
        };
   }

   // Tạo một specification để tìm user có email chứa keyword 
   public static Specification<User> hasEmail(String keyword) {
        return (root, query, cb) -> {
            // Sử dụng phương thức like của CriteriaBuilder 
            return cb.like(root.get("email"), "%" + keyword + "%");
        };
   }

   // Tạo một specification để tìm user có role chứa keyword 
   public static Specification<User> hasRole(String keyword) {
        return (root, query, cb) -> {
            // Sử dụng phương thức join của Root để kết nối với bảng role 
            Join<User, Role> roleJoin = root.join("roles");
            // Sử dụng phương thức like của CriteriaBuilder trên thuộc tính name của role 
            return cb.like(roleJoin.get("name"), "%" + keyword + "%");
        };
   }
}
```

Để sử dụng các specification này trong truy vấn, bạn cần sử dụng phương thức findAll() của JpaRepository và truyền vào các specification như là tham số². Ví dụ:

```java
// Lấy ra danh sách user có firstname bắt đầu bằng "Nguyen" và role là "ADMIN"
List<User> users = userRepository.findAll(
     UserSpecification.hasFirstname("Nguyen")
         .and(UserSpecification.hasRole("ADMIN"))
);
```

Hy vọng đoạn mã này giúp bạn hiểu cách viết specification cho entity khác.😊

Ví dụ:

```java
// Tạo một specification để tìm user có role có id bằng keyword 
    public Specification<User> findUsersWithUploadables(Long userId) {
        return (root, query, criteriaBuilder) -> {
            final var join = (Join<Object, Object>) root.fetch(User_.UPLOADABLES, JoinType.LEFT);
            query.distinct(true);
            Predicate id = criteriaBuilder.equal(root.get("id"), userId);
            return criteriaBuilder.and(
                    id,
                    criteriaBuilder.equal(join.get(Uploadable_.OBJECT_TYPE), "USER"),
                    criteriaBuilder.equal(join.get(Uploadable_.OBJECT_FIELD), "AVATAR")
            );
        };
    }
```

Sau khi sử dụng specification này, khi bạn gọi phương thức user.getUploadables(), bạn sẽ chỉ nhận được các uploadables có objectType = 'USER' và objectField bằng 'AVATAR'
