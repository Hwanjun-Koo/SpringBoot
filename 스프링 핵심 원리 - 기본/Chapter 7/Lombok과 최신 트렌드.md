생성자 주입 방식을 사용하는게 대부분이다. 그러나 final 선언과 주입 코드를 매번 써줘야 하는 번거로움이 있다 -> 이 때 필요한 것이 롬복이다

```
@Component  
public class OrderServiceImpl implements OrderService{  
  
    private final MemberRepository memberRepository;  
    private final DiscountPolicy discountPolicy; //인터페이스만 의존하도록 변경  
  
    @Autowired  
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {  
        this.memberRepository = memberRepository;  
        this.discountPolicy = discountPolicy;  
    }
```
- 생성자가 1개면 `@Autowired` 생략할 수 있다
- 롬복의 `@RequieredArgsConstructor`로 생성자 메서드를 생략할 수 있다

```
@Component  
@RequiredArgsConstructor  
public class OrderServiceImpl implements OrderService{  
  
    private final MemberRepository memberRepository;  
    private final DiscountPolicy discountPolicy; //인터페이스만 의존하도록 변경
```
- final이 붙은 선언문을 보고 자동으로 생성자를 만들어준 것