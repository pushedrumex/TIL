### @Primary : 타입의 빈이 여러개 있을 떄 기본 선택

```java
@Configuration
public class AppConfig {

    @Bean
    @Primary
    public DiscountPolicy rateDiscountPolicy() {
        return new RateDiscountPolicy();
    }

    @Bean
    public DiscountPolicy fixDiscountPolicy() {
        return new FixDiscountPolicy();
    }
}
```

### @Qualifier: 특정 빈을 지정해서 주입하는 방법 → @Primary 보다 우선순위가 높음
```java
@Bean
@Qualifier("mainPolicy")
public DiscountPolicy rateDiscountPolicy() { … }

@Bean
@Qualifier("subPolicy")
public DiscountPolicy fixDiscountPolicy() { … }

@Autowired
@Qualifier("mainPolicy")
private DiscountPolicy discountPolicy;
```