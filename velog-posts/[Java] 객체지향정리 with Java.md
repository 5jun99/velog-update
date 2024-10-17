<p>하도 오랜만에 자바하니까 객체지향 다 까먹어버려서 다시 정리</p>
<h1 id="1-클래스">1. 클래스</h1>
<hr />
<p>클래스는 표현하고자하는 <strong>대상</strong>의 <strong>공통 속성을 모아놓은 것</strong>임.
속성으로는 <strong>멤버 변수, 메서드</strong> 가 있을 수 있음</p>
<h2 id="인스턴스">인스턴스</h2>
<p>인스턴스는 클래스를 기반으로 만들어지는 <strong>객체를</strong> 뜻함
흔한 예로 붕어빵과 붕어빵 틀이 잇으면 붕어빵이 객체 즉 인스턴스이고, 붕어빵 틀이 클래스임 붕어빵마다 모양은 같지만 안에 앙금이 다를 수 있음.</p>
<h1 id="2-메서드">2. 메서드</h1>
<p>쉽게 생각해서 <strong>작업</strong>이라고 생각하면 좋음. 비슷한 활동을 묶어놓는 거임.</p>
<blockquote>
<p>이름 규칙</p>
</blockquote>
<ul>
<li>클래스 : 명사형으로 작성하는 것이 좋음. 파스칼 규칙을 따름. 파스칼이란 단어의 첫글자를 전부 대문자로 작성하는 것 ex) FileEntity</li>
<li>메서드: 동사형으로 작성하는 것이 좋음. camel 규칙을 따름.
첫 문자는 소문자, 다음에 나오는 단어의 첫글자는 대문자로 작성하면 됨
ex) findBomb</li>
</ul>
<ul>
<li>정적 메서드 (static method)
클래스의 인스턴스를 생성을 따로 하지 않아도 사용할 수 있는 함수를 말함. 따라서 static 메서드를 호출할 시에는 classname.methodname 이런식으로 호출하면 되는 것임.
<em>static 메서드에서는 static이 아닌 메서드나 멤버 변수는 호출하거나 참조할 수 없다. 무조건 객체를 통해 참조해야하기 때문. 그렇기 때문에 정적 메서드에서는 this 를 쓰지 않는다. this 또한 참조해야하는 객체가 필요하기 때문</em></li>
<li>정적 변수: 비슷하게, 객체 생성 없이 static 영역에 저장되는 변수이다. 메모리 할당을 줄이기 위해 사용되는 목적은 같다. 때문에, 보통 상수 같은 값에 static을 많이 쓰고, 이런 이유로 public static final 제안자가 자주 붙는다.</li>
</ul>
<p><img alt="레퍼런스" src="https://velog.velcdn.com/images/9409velog/post/21bd2cc3-9fdf-4a62-9947-4815266ca2ba/image.png" /></p>
<h1 id="3-생성자-constructor">3. 생성자 (constructor)</h1>
<p>생성자는 <strong>초기화 메서드</strong>라고 생각하면 좋음. 객체를 생성할 적에 new 키워드 뒤에 호출됨.</p>
<ul>
<li>예시<pre><code class="language-java">public static void main(String[] args) {
      Dog dog = new Dog(&quot;코코&quot;, &quot;허스키&quot;);
      dog.cry();</code></pre>
</li>
</ul>
<ul>
<li>특징<ul>
<li>클래스명과 이름이 같아야 함</li>
<li>리턴값이 없음</li>
<li>생성자를 정의 안해도 기본 생성자가 있음 기본생성자는 매개변수와 내용이 없음</li>
<li>생성자는 여러번 정의될 수 있음. 매겨변수의 형태가 달라야 함.</li>
</ul>
</li>
</ul>
<h1 id="4-상속">4. 상속</h1>
<p>상속은 클래스를 재사용한다고 생각하면 좋음. 상속받은 클래스는 부모 클래스에서 변경된 부분만 따로 정의해줌</p>
<ul>
<li>특징</li>
</ul>
<ol>
<li>부모 클래스로에서 정의된 필드와 메소드를 물려 받음.</li>
<li>새로운 필드와 메소드를 추가할 수 있음.</li>
<li>부모 클래스스에서 물려받은 메소드를 수정(덮어쓰기)을 할 수 있음.</li>
</ol>
<ul>
<li><p>예시</p>
<p>Bird.java</p>
<pre><code class="language-java">public abstract class Bird {
    int x, y, z;
    void fly(int x, int y, int z){
        this.x = x;
        this.y = y;
        if (flyable(z)){
            this.z = z;
        }
        else System.out.println(z + &quot; 높이로는 날 수 없습니다.&quot;);
        printLocation();
    }</code></pre>
<p>Peacock.java</p>
<pre><code class="language-java">public class Peacock extends Bird{

    @Override
    public boolean flyable(int z) {
        return false;
    }
}</code></pre>
</li>
<li><p>super 메서드
부모 클래스에 있는 필드나 메소드, 생성자를 자식 클래스에서 참조하여 사용하고 싶을 때 사용하는 키워드임.</p>
</li>
<li><p>오버로딩
한 클래스 내에 같은 이름의 메서드를 여러개 정의하는 것
메서드 이름이 같지만, 매개변수의 개수나 타입이 달라야함</p>
</li>
<li><p>오버라이딩
부모 클래스로부터 상속받은 메서드를 재정의하는 경우</p>
</li>
</ul>
<ol>
<li>부모 클래스의 메소드와 이름이 같아야 함.</li>
<li>부모 클래스의 메소드와 매개변수가 같아야 함.</li>
<li>부모 클래스의 메소드와 반환타입이 같아야 함.</li>
</ol>
<h1 id="추가-다형성polymorphism">추가. 다형성(Polymorphism)</h1>
<p>하나의 객체가 여러가지 형태를 가질 수 있는 겂을 의미함. 
우리가 많이 볼 수 있는 객체 타임과 참조 변수 타입이 일치하는 경우와 다르게 불일치하는 경우이다. 예시와 함께 보자</p>
<pre><code class="language-java">static class Animal {
        String name;
        Animal(String name){
            this.name = name;
        }
        public void cry(){
            System.out.println(name + &quot;is crying&quot;);
        }

        public void walk(){
            System.out.println(name + &quot; is walking&quot;);
        }
    }
static class Giraffe extends Animal {
         Giraffe(String name) {
            super(name);
        }
        @Override
        public void cry() {
            super.cry();
            System.out.println(name + &quot; cannot cry.&quot;);
        }
    }

    //        Animal giraffe = new Giraffe(&quot;기린&quot;);   부모 클래스의 메서드만 접근 가능
//        Giraffe giraffe = new Giraffe(&quot;기린&quot;);  자식 클래스 부모클래스 메서드 모두 접근가능
//        Giraffe giraffe = new Animal(&quot;기린&quot;);   컴파일 오류
        Animal giraffe = new Giraffe(&quot;기린&quot;); //1)
        Giraffe giraffe1 = new Giraffe(&quot;기린2&quot;); //2)
</code></pre>
<p>코드를 보면 1번은 불일치, 2번은 일치하는 경우이다.
1번은 참조 변수가 상의 클래스인 Animal, 객체의 타입은 giraffe가 되는 것이다. 이를 활용하면 개별적인 클래스에 대한 함수나 코드를 구현하지 않고, 변수의 타입을 통일할 수 있다. 다만, 실 객체타입은 이제 자식의 클래스인 것이다.</p>
<h1 id="5-접근-제어자">5. 접근 제어자</h1>
<p>접근 제어자는 말그대로 외부에서의 접근을 제한하는 것이다.</p>
<ul>
<li>접근 제어자의 종류</li>
</ul>
<ol>
<li>private : 같은 클래스 내에서만 접근이 가능</li>
<li>default(nothing) : 같은 패키지 내에서만 접근이 가능</li>
<li>protected : 같은 패키지 내에서, 그리고 다른 패키지의 자손클래스에서 접근이 가능</li>
<li>public : 접근 제한이 전혀 없음</li>
</ol>
<p>접근제어자는 외부로부터의 데어터 부적절한 사용을 보호하기 위한 캡슐화를 도와주는 도구임.</p>
<h1 id="6-추상클래스-인터페이스">6. 추상클래스, 인터페이스</h1>
<h2 id="추상">추상</h2>
<p>추상 클래스는 추상 메서드를 선언한 클래스라고 생각하면 됨.
추상 메서드는 abstract 리턴타입 이름(매개변수); 이런식으로만 정의된 메서드임. 구현은 상속받는 클래서에서 정의되어야함.</p>
<h2 id="인터페이스">인터페이스</h2>
<p>인터페이스는 객체의 특정 행동의 특징만 정의하고 구현은 implements한 클래스에서 하는 것임.
구현과 마찬가지로 리턴타입 메서드 이름만 정의함. </p>
<h2 id="추상-클래스-vs-인터-페이스">추상 클래스 vs 인터 페이스</h2>
<p><img alt="" src="https://velog.velcdn.com/images/9409velog/post/cf9dd40d-476c-41a0-aef7-cde1c69dffb2/image.png" />
인터페이스는 변수는 static final만 사용가능, 접근제어자는 무조건 public이라 따로 선언하지도 않는다. 추가적으로 다중 implements가 가능하다.</p>
<p>인터페이스는 구현하려는 객체의 동작을 명세한다는 느낌이 강하다면, 추상클래스는 클래스를 상속받아 이용 하고 확장을 위한다고 생각하면 됨.</p>