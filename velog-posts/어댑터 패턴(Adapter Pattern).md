<h3 id="어댑터-패턴adapter-pattern">어댑터 패턴(Adapter Pattern)</h3>
<p>어댑터 패턴은 구조적 디자인 패턴 중 하나로, 서로 호환되지 않는 인터페이스를 가진 클래스들이 함께 작동할 수 있도록 중간에 어댑터를 두어 인터페이스를 변환하는 패턴이다.
한 클래스의 인터페이스를 클라이언트가 원하는 다른 인터페이스로 변환하여 호환성을 제공한다.</p>
<p><strong>장점</strong>
기존 코드를 수정하지 않고 새로운 인터페이스에 맞춰 재사용 가능하다.
시스템 간의 느슨한 결합을 유지한다.
코드의 유연성과 확장성이 증가한다.</p>
<p><strong>단점</strong>
어댑터 클래스를 추가로 작성해야 하므로 코드 복잡도가 약간 증가할 수 있다.
성능상 약간의 오버헤드가 발생할 가능성이 있다.</p>
<h4 id="어댑터-패턴-구성-요소">어댑터 패턴 구성 요소</h4>
<p>Target : 클라이언트가 기대하는 인터페이스
Adaptee : 기존에 존재하는 클래스 또는 시스템으로, 클라이언트가 직접 사용할 수 없는 인터페이스를 가짐
Adpater : Target인터페이스를 구현하고, Adaptee의 기능을 호출하여 호환성을 제공함
Client : Target 인터페이스를 사용하는 주체</p>
<h4 id="어댑터-패턴-코드-예시">어댑터 패턴 코드 예시</h4>
<blockquote>
<ol>
<li>클라이언트가 기대하는 인터페이스 (Target)</li>
</ol>
</blockquote>
<pre><code class="language-cs">public interface ILight
{
    void SwitchOn();
    void SwitchOff();
}</code></pre>
<blockquote>
<p>2.외부에서 제공된 클래스 (Adaptee)</p>
</blockquote>
<pre><code class="language-cs">public class Lamp
{
    public void TurnOn()
    {
        // 램프가 켜짐
    }
    public void TurnOff()
    {
        // 램프가 꺼짐
    }
}</code></pre>
<blockquote>
<ol start="3">
<li>어댑터 클래스</li>
</ol>
</blockquote>
<pre><code class="language-cs">public class LamAdapter : ILight
{
    private Lamp _lamp
    public LampAdapter(Lamp lamp)
    {
        _lamp = lamp;
    } 
    public void SwitchOn()
    {
        _lamp.TurnOn();
    }
    public void SwitchOff()
    {
        _lamp.TurnOff();
    }
}</code></pre>
<blockquote>
<ol start="4">
<li>사용 코드(Client)</li>
</ol>
</blockquote>
<pre><code class="language-cs">public class LightController : MonoBehaviour
{
    private ILight light;
    void Start()
    {
        Lamp externalLamp = new Lamp(); // 외부램프
        light = new LampAdapter(externalLamp); // 어댑터를 통해 외부 램프 사용
        light.SwitchOn();
        light.SwitchOff();
    }
}</code></pre>