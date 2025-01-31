# 자바 프로그램의 개발과 구동
- 현실 세계에서 소프트웨어, 즉 프로그램은 개발자가 개발 도구를 이용해 개발하고 운영체제를 통해 물리적 컴퓨터인 하드웨어 상에서 구동된다.
- 자바가 만들어 주는 가상 세계도 이와 마찬가지다.
- `자바 개발 도구인 JDK를 이용해 개발된 프로그램은 JRE에 의해 가상의 컴퓨터인 JVM 상에서 구동된다.`
<img width="818" alt="image" src="https://user-images.githubusercontent.com/26343023/162250314-50f34d09-6f20-45a9-88be-e7250344425a.png">

### 프로그램 메모리 사용방식
- 코드 실행 영역과 데이터 저장 역역을 구분해서 사용한다.

### 객체 지향 프로그램의 메모리 사용 방식
- 데이터 저장 영역을 다시 세 개의 영역으로 분할해서 사용한다.
  - 스태틱(static) 영역: 클래스들의 놀이터
  - 스택(stack) 영역: 메서드들의 놀이터
  - 힙(heap) 영역: 객체들의 놀이터

# 자바에 존재하는 절차적/구조적 프로그래밍의 유산
- 자바 키워드 중 절반 이상이 절차적/구조적 프로그래밍 언어에서 유래됐다.
  - continue, for, default, switch, boolean, ...

# main() 메서드 호출과정
``` java
public class Start {
  public static void main(String[] args) {
    System.out.println("hi");
  }
}
```
1. `JRE`는 프로그램에 main() 메서드가 있는지 확인.
2. Start의 main()을 확인 한 JRE는 프로그램 실행을 위한 사전 준비를 한다. => JVM에 전원을 넣어 부팅한다.
3. 부팅된 `JVM`은 목적 파일을 받아 실행한다.
4. JVM은 가장먼저 `전처리 과정`을 수행한다.
- 모든 자바 프로그램이 반드시 포함하게 되는 패키지(java.lang)를 static 영역에 배치.
- import 된 패키지도 static 영역에 배치.
- 프로그램 상의 모든 클래스를 static 영역에 배치.
5. 여는 중괄호를 만날 때마다 스택 프레임을 생성한다.
- 함수의 스택 프레임에는 반환타입, 매개변수, 지역변수 순서로 쌓인다.
6. 실행을 위한 데이터가 모두 저장되면 첫 명령문 부터 `수행`한다.
7. 닫는 중괄호를 만나게되면 스택 프레임이 소멸된다.

이 예제에서 Heap 영역은 사용되지 않았다.

# main 메서드에서 square 메서드의 지역변수에 접근할 수 없다.
``` java
public class Start {
  public static void main(String[] args) {
    int k = 4;
    int m;
    
    m = square(k);
  }
  
  private static int square(int k) {
    int result;
    k = 25;
    result = k;
    return result;
  }
}
```

- 예제 코드가 억지스러운게 맞다.
- main은 square의 지역변수에 절대 접근할 수 없다.
- 메서드를 블랙박스화 하기 때문에
  - 입력 값들(인자 값들)과 반환값에 의해서만 메서드 사이에서 값이 전달될 뿐 서로 내부의 지역 변수를 볼 수 없다는 것을 의미한다.

- 접근을 못하는게 이치에 맞다. 메서드는 서로의 고유 공간인데 무단 침입하면 문제가 발생할 수 있다.
- 포인터 문제.
  - square에서 main의 지역변수 m에 정확히 접근하려면 주소를 알고, 접근할 수 있어야 한다.
  - Java는 포인터가 존재하지 않는다.

# Call By Value
- 메서드 호출 시 전달되는 인자는 변수 자체가 아닌, 변수가 저장한 값만을 복사해서 전달한다.
- 참조변수와 같은 경우는 주소를 복사해서 전달한다.

# 전역변수 함부로 쓰지마라
- 전역 변수는 공유 변수. 여러 쓰레드에서 동시에 수정하면 문제가 발생한다.
- 전역 변수는 피할 수 있다면 피해라.
- 전역 변수는 읽기 전용 상수로 사용되는 것은 추천이다.

# 멀티 스레드 / 멀티 프로세스

### 멀티 프로세스
- 프로세스 당 각각 T 메모리 구조를 갖는다. (스태틱, 스택, 힙)

### 멀티 스레드
- 하나의 T 메모리만 사용한다.
- 스태틱과 힙은 공유한다.
- 각 스레드마다 스택 영역을 분할해서 사용한다.
