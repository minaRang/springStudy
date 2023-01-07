## 1주차 답안

###미나

1. 스프링 컨테이너의 생성 과정에 대해 설명

- ApplicationContext(스프링 컨테이너면서 인터페이스), 스프링 컨테이너는 XML이나 애노테이션 기반의 자바 설정 클래스(AppConfig)로 생성 가능
  ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class); -> 인터페이스의 구현체

  스프링 컨테이너는 BeanFactory, ApplicationContext로 구분한다. 하지만 일반적으로 ApplicationContext를 스프링 컨테이너라 부른다.

  스프링 컨테이너 생성과정

  1. 스프링 컨테이너 생성
     new AnnotationConfigApplicationContext(AppConfig.class(구성정보)); -> 스프링 컨테이너가 생성 되고 안에 스프링 빈 저장소가 (빈이름 , 빈객체)가 생성된다. -> 구성 정보를 활용 할 수 있다.

  2. 스프링 빈등록
     스프링 컨테이너가 파라미터로 넘어온 설정 클래스 정보를 사용해서 스프링 빈을 등록한다.

     - 빈이름은 기본적으로 메서드 이름을 사용하지만 이름을 직접 부여 할 수도 있다.(@Bean(name = "이름"))
     - 빈 이름은 항상 다른이름을 부여해야함. 같은 이름을 부여하면 무시되거나 기존 빈을 덮어 버릴 수 있다.

  3. 스프링 빈 의존관계 설정
     - 스프링 컨테이너는 설정 정보를 참고해서 의존관계를 주입(DI)한다.
     - 단순히 자바 코드를 호출 하는 것과는 다름(싱글톤 컨테이너 참고)

2. BeanFactory와 ApplicationContext에 대해 설명(둘다 스프링 컨테이너)
   <<interface>> BeanFactory <- <<interface>> ApplicationContext <- AnnotationConfigApplicationContext

   1. BeanFactory

      - 스프링 컨테이너의 최상위 인터페이스
      - 스프링 빈을 관리하고 조회하는 역할!
      - getBean()제공
      - 대부분의 기능은 BeanFactory가 제공하는 기능

   2. ApplicationContext - BeanFactory 기능을 모두 상속 - Bean을 관리하고 검색하는 기능 + 편리한 부가기능 제공 1. MessageSource - 메세지소스를 활용한 국제화 기능 2. EnvironmentCapable - 환경변수(로컬, 개발, 운영 등을 구분해서 처리) 3. ApplicationEventPublisher - 어플리케이션 이벤트(이벤트를 발행하고 구독하는 모델을 편리 하게 지원) 4. ResourceLoader - 편리한 리소스 조회(파일, 클래스패스, 외부 등에서 리소스 조회)
      -> 즉 ApplicationContext는 BeanFactory를 상속받아 관리기능 + 부가기능을 제공하고 실제로 BeanFactory를 사용 할 일은 거의 없다.

3. 싱글톤 패턴과 싱글톤 컨테이너의 차이점
   싱글톤 패턴 - 클래스의 인스턴스가 1개만 생성 되는 디자인 패턴
   -> private 생성자를 사용해서 외부에서 new 키워드를 사용하지 못하도록 해야한다. - 문제점
   -> 코드가 길어진다.
   -> 의존관계상 클라이언트가 구체 클래스에 의존한다. -> DIP 위반
   -> 클라이언트가 구체 클래스에 의존해서 OCP 원칙을 위반
   -> 테스트하기 어렵고, 내부 속성 변경 및 초기화 어려움
   -> private 생성자로 자식 클래스를 만들기 어려움
   -> 즉, 유연성이 떨어져서 안티패턴으로 불림

   싱글톤 컨테이너 - 스프링 컨테이너가 싱글톤 패턴의 문제점을 해결하면서 싱글톤으로 관리 -> 스프링 빈(싱글톤으로 관리되는 빈)
   -> 스프링 컨테이너는 싱글톤 패턴을 적용하지 않아도, 인스턴스를 싱글톤으로 관리
   -> 스프링 컨테이너는 싱글톤 컨테이너의 역할(싱글톤 객체를 생성하고 관리하는 기능 = 싱글톤 레지스트리) - 장점  
    -> 싱글톤 패턴을 위한 지저분한 코드 X
   -> DIP, OCP, 테스트, private 생성자로부터 자유롭게 싱글톤 사용가능

###희준

1.  SOLID 원칙 정리하기

    SOLID란 로버트 마틴이 좋은 객체 지향 설계의 5가지 원칙을 정리

    1. SRP - 단일책임 원칙
       - 하나의 클래스는 하나의 책임만 가져야한다.(변경이 있을 때 파급 효과가 적으면 SRP를 따른 것)
    2. OCP - 개방 폐쇄 원칙
       - 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.(다형성을 활용해서 인터페이스로 역할과 구현을 분리)
       - 자바에선 AppConfig같은 DI컨테이너를 이용해서 OCP원칙을 지킬 수 있게 한다.
    3. LSP - 리스코프 치환원칙
       - 객체는 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.
    4. ISP - 인터페이스 분리 원칙
       - 여러 개의 범용 인터페이스보다 특정 클라이언트를 위한 여러개의 인터페이스로 분리(역할도 명확해지고 SRP를 따르게 됨)
    5. DIP - 의존관계 역전 원칙
       - 역할에 의존하게 만드는 것 즉, 구현 클래스보단 인터페이스에 의존해야한다.

2.  아래의 싱글톤 패턴 생성코드에서 2번째 라인 static 으로 선언하는 이유 정리하기

    public class SingletonService {
    private static final SingletonService instance = new SingletonService();
    public static SingletonService getInstance() {
    return instance;
    }
    private SingletonService() {
    }

        public void logic() {
        System.out.println("싱글톤 객체 로직 호출");
        }

    }

    -> static을 붙이면 정적타입으로 선언해서 static영역에 할당되기 때문에 프로그램 종료시 까지 메모리가 할당 된채로 종료되서 하나의 객체를 계속해서 참조 할 수 잇다. -> 싱글톤 패턴으로 사용 가능하다. - 즉 static 변수와 메소드는 객체가 생성 되기 전에 이미 할당 되어 있어서 객체 생성 없이 사용 가능하다.

3.  의존관계 주입방법 4가지와 이중 생성자주입방식이 권장되는 이유 설명하기

    1. 불변
       - 대부분의 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 의존관계를 변경할 일도 없고 변경해서도 안된다.
       - 수정자 주입을 사용하면 set메서드를 public으로 열어야 한다.
       - 누군가 실수로 변경할 수 있기 때문에 올바른 설계가 아니다.
       - 생성자 주입은 객체를 생성 할 때 딱 1번만 호출 되므로 불변하게 설계 가능
    2. 누락
       - 생성자 주입을 사용하면 데이터를 누락 했을 때 컴파일 오류가 발생해서 누락을 쉽게 알 수 있다.
    3. final 키워드
       - 생성자 주입을 사용하면 필드에 final 키워드를 사용해서 생성자에 값이 설정되지 않는 오류를 컴파일 시점에 막아준다.
       - 수정자 주입 및 나머지 주입 방식은 생성자 이후에 호출되므로 final 키워드를 사용 할 수 없다.

    <종합 정리> - 생성자 주입 방식은 프레임 워크에 의존하지 않고 자바 언어의 특징을 살릴 수 있는 방법 - 기본적으로 생성자 주입방식을 사용하고, 필수 값이 아닌 경우에만 수정자 방식을 옵션으로 넣는다. - 필드 주입은 사용하지 않는 것이 좋다.

###정우

1. 객체지향의 5원칙 중 OCP에 대해 설명
   소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
   말이 굉장히 모호 한데 결국 인터페이스 구현한 새로운 클래스를 사용해서 **다형성**을 이용해야 한다.
   EX) MemberRepository m = new MemoryMemberRepository();
   -> MemberRepository m = new JdbcMemberRepository();

구현 객체를 변경하려면 클라이언트 코드를 변경해야 한다. 즉 다형성을 활용 했지만 OCP를 지키지 못했다.
따라서 객체를 생성하고 연관관계를 맺어 주는 별도의 조립과 설정자가 필요한데 AppConfig를 이용해서 FixDiscountPolicy -> RateDiscountPolicy로 변경해서 코드에 주입한다면 클라이언트 코드는 변경하지 않아도 된다.
즉, 소프트웨어 요소를 새롭게 확장해도 사용 영역의 변경은 닫혀있기 때문에 OCP를 준수 한 코드다.

2. 싱글톤 방식을 사용시 주의 할 점에 대해 설명
   싱글톤 패턴, 싱글톤 컨테이너처럼 객체 인스턴스를 하나만 생성해서 공유하는 싱글톤 방식은 여러 클라이언트가 하나의 같은 객체를 공유하기 때문에 stateful하게 설계하면 안된다.

즉, 무상태(stateless)로 설계를 해야한다.

무상태란 공유하는 필드가 없는 것을 말한다. 필드를 사용하지 않고 자바에서는 공유되지 않는 지역변수나 파라미터, ThreadLocal등을 사용해야 한다.

stateful일 때의 문제점 예시 코드

public class StatefulService {
private int price; //상태를 유지하는 필드

    public void order(String name, int price) {
        System.out.println("name = " + name + " price = " + price);
        this.price = price; //여기가 문제!
    }

    public int getPrice() {
        return price;
    }

}
stateful일 때의 문제점 테스트 코드
userA가 10,000원을 주문하고, userB가 20,000원을 주문 했을 때 getPrice() 메소드를 사용해 userA의 주문 금액을 확인하면 20,000원이라는 결과가 나왔다.
즉, 필드가 공유되기 때문에 price값이 20,000원으로 바뀌게 되었다.

class StatefulServiceTest {

    @Test
    void statefulServiceSingleton() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
        StatefulService statefulService1 = ac.getBean(StatefulService.class);
        StatefulService statefulService2 = ac.getBean(StatefulService.class);

        //ThreadA: A사용자 10000원 주문
        int userAPrice = statefulService1.order("userA", 10000);
        //ThreadB: B사용자 20000원 주문
        int userBPrice = statefulService2.order("userB", 20000);

        //ThreadA: 사용자A 주문 금액 조회

// int price = statefulService1.getPrice();
System.out.println("price = " + userAPrice);

// assertThat(statefulService1.getPrice()).isEqualTo(20000);
}

    static class TestConfig {

        @Bean
        public StatefulService statefulService() {
            return new StatefulService();
        }
    }

}

3. 의존 관계 주입 방법 네가지에 대해 설명

의존관계 주입은 크게 4가지 방법이 있다.

생성자주입
수정자(setter 주입)
필드 주입
일반 메서드주입
생성자 주입
생성자를 통해서 의존 관계를 주입 받는 방법

특징
생성자 호출시점에 딱 한번만 호출되는 것이 보장된다.
불변, 필수(final 필드 선언시 반드시 값을 설정해야함) 의존관계에 사용한다.
@Autowired // 생성자 주입은 생성자를 Bean에 등록 할 때 의존성 주입을 같이 한다. / 수정자로 주입하면 주입이 되기 때문에 굳이 생성자를 만들지 않아도 된다.
public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
System.out.println("1. OrderServiceImpl.OrderServiceImpl");
this.memberRepository = memberRepository;
this.discountPolicy = discountPolicy;
}
!!! 생성자가 딱 1개만 있으면 @Autowired를 생략해도 자동으로 주입이 된다. 단 스프링 빈일 때만 해당
수정자 주입(setter 주입)
setter라는 필드의 값을 수정자 메서드를 통해 의존관계를 주입

특징
선택, 변경 가능성이 있는 의존관계에 사용(set을 통해 변경가능하기 때문에)
자바빈 프로퍼티(필드 값 직접변경이 아닌 set, get 메서드를 사용) 규약의 수정자 메서드를 사용
@Autowired(required = false) // required = false (주입할 대상이 없어도 동작하게 하려면) 지정 수정자는 Bean등록 후 의존관계 주입 단계에서 일어난다. 변경가능하거나 선택적으로 주입 할 수 있다.
public void setMemberRepository(MemberRepository memberRepository) {
System.out.println("memberRepository = " + memberRepository);
this.memberRepository = memberRepository;
}
필드 주입
필드에 주입하는 방법(@Autowired private MemberRepository memberRepository;)

특징
코드가 간결하다 but 외부에서 변경이 불가능해 순수 자바코드로 테스트하기 힘들다.
즉, DI 프레임워크가 없으면 아무것도 할수 없다.
테스트 코드나 스프링 설정을 목적으로 하는 @Configuration에서만 사용
일반 메서드 주입
일반 메서드를 통해 주입

특징
한번에 여러 필드를 주입 받을 수 있다.
but 일반적으로 사용하지 않는다.

###민욱

1. DI 컨테이너를 통해, 기존 자바 코드에 어떠한 문제를 해결할 수 있는지 설명

   AppConfig와 같은 DI 컨테이너를 도입하기 전에는 새로운 정책을 적용 하려고 할때 '클라이언트 코드'의 구현체도 함께 변경해야하고 클라이언트가 인터페이스 뿐만 아니라 구체 클래스도 의존하게 때문에 DIP를 위반하게 된다. - DI 컨테이너에 어플리케이션 전체 동작 방식을 구성하기 위해 구현 객체를 생성하고 연결 하는 한다. - 클라이언트 객체는 역할에만 집중 할 수 있다. 즉, 어플리케이션을 사용영역과 구성영역으로 분리 할 수 있다. - 또한 DIP와 OCP를 전부 지킬 수 있고, 역할을 나눔으로써 SRP도 지킬 수 있게 된다.

2. @Autowired의 역할은 @Bean과 무슨 차이가 있는지에 대해 설명

   @Bean은 스프링 컨테이너가 @Configuration이 붙은 클래스를 구성정보로 사용하고 @Bean이라 적힌 메소드를 모두 호출해서 반환 된 객체를 전부 스프링 컨테이너에 등록한다. -> 스프링 빈
   @Bean은 @Bean이 붙은 메소드 명을 스프링 빈의 이름으로 사용, 또한 의존관계도 직접 명시해야 한다.

   컴포넌트 스캔은 @Bean과 같은 스프링 설정정보가 없어도 자동으로 스프링 빈을 등록한다. - @Autowired는 의존관계를 자동으로 주입해주는 어노테이션이다. 생성자에서 여러 의존관계도 한번에 주입가능 - 컴포넌트 스캔을 사용하면 @Configuration이 붙은 설정정보도 자동으로 등록된다. excludeFilters를 사용하자. - @Component이 붙은 클래스를 스캔해서 스프링 빈으로 등록한다.
   순서대로 정리해보면 1. @ComponentScan -> 컴포넌트가 붙은 모든 클래스를 스프링 빈으로 등록(빈 이름은 맨앞글자 소문자이면서 클래스명) 2. 생성자에 @Autowired를 지정하면 스프링 컨테이너가 자동으로 타입이 같은 빈을 찾아서 주입한다.

3. 빈 생명주기 콜백 시, @PostConstruct, @PreDestroy가 권장되는 이유에 대해 다른 방법과 비교하며 설명

   먼저 스프링 빈의 이벤트 라이프 사이클을 알아야 한다.
   스프링 컨테이너 생성 -> 스프링 빈 생성 -> 의존관계 주입 -> 초기화 콜백 사용 -> 소멸전 콜백 -> 스프링 종료

   - 빈 생명주기는 콜백에는 크게 3가지 방법이 있다.
     1. 인터페이스 (InitializingBean, DisposableBean)
        afterPropertiesSet(), destroy() 메소드를 오버라이딩해서 콜백한다.
        -> 스프링 전용 인터페이스로 스프링에 의존, 메소드명 변경 불가, 외부 라이브러리에 적용 X
     2. 빈 등록 초기화, 소멸 메소드(@Bean(initMethod = "init", destroyMethod = "close"))
        -> 메서드 이름 자유롭다, 스프링 빈이 스프링 코드에 의존 X, 설정 정보를 사용하기 때문에 외부 라이브러리에 적용가능
        -> 또한 종료 메소드(Bean destroy)에는 close, shutdown이름의 메소드를 추론해서 호출, 사용하기 싫으면 destroy=""
     3. 어노테이션(@PostConstruct, @PreDestroy)
        - 어노테이션이 가장 많이 사용되는 이유는 편리하고 자바 표준 패키지를 사용하기 때문에 다른 컨테이너에서도 사용가능하다.
        - 또한 컴포넌트 스캔과 가장 잘 어울린다.
        - 단점으로 외부 라이브러리에 적용하지 못하기 때문에 그때는 2번인 메소드를 사용해야 한다.
