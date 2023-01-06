## 1주차 문제

###미나
1. 스프링 컨테이너의 생성 과정에 대해 설명
2. BeanFactory와 ApplicationContext에 대해 설명
3. 싱글톤 패턴과 싱글톤 컨테이너의 차이점

###희준
1. SOLID 원칙 정리하기
2. 아래의 싱글톤 패턴 생성코드에서 2번째 라인 static 으로 선언하는 이유 정리하기

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
3. 의존관계 주입방법 4가지와 이중 생성자주입방식이 권장되는 이유 설명하기

###정우
1.
2.
3.

###민욱 
1.
2.
3.

