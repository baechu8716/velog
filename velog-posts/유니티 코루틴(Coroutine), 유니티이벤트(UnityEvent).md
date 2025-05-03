<h3 id="코루틴coroutine">코루틴(Coroutine)</h3>
<p>코루틴은 유니티에서 시간을 기다리면서 코드 실행을 잠시 멈추고, 다시 이어서 실행할 수 있게 해주는 함수이다.
Unity의 Update()함수는 매 프레임마다 실행된다. 하지만 &quot;몇 초 기다렸다가 무언가 하고 싶을 때&quot;나 &quot;프레임 단위로 어떤 동작을 반복하고 싶을 때&quot; 코루틴으로 프레임을 나눠서 처리하거나 지연 동작을 만들기 위한 기능이다.</p>
<h3 id="코루틴-예제">코루틴 예제</h3>
<blockquote>
</blockquote>
<pre><code class="language-cs">IEnumerator WaitAndPrint()
{
    Debug.Log(&quot;시작&quot;);
    yield return new WaitForSeconds(2f); //2초 대기
    Debug.Log(&quot;2초 후&quot;);
}</code></pre>
<h3 id="핵심-동작-구조">핵심 동작 구조</h3>
<p><strong>IEnumerator 리턴 타입</strong>
코루틴 함수는 반드시 Ienumerator를 반환해야 한다.
yield return을 이용하여 멈춤 포인트를 지정한다.</p>
<p><strong>yield return 키워드</strong>
null : 다음 프레임까지 기다림
new WaitForSeconds(t) : t초 동안 기다림
new WaitForEndOfFrame() : 렌더링 후에 실행
new WaitForFixedUpdate() : FixedUpdate 다음에 
다른 코루틴 : 해당 코루틴이 끝날 때까지 대기</p>
<p><strong>실행 흐름</strong></p>
<ol>
<li>Coroutine routine = StartCoroutine(ex:WaitAndPrint())으로 코루틴 시작</li>
<li>yield return이 나올 때 까지 실행</li>
<li>해당 조건이 만족할 때까지 자동으로 기다림</li>
<li>조건을 만족하면 그 이후 코드부터 다시 실행</li>
<li>반복적으로 yield가 있으면 계속 &quot;멈췄다가 다시 실행&quot;</li>
<li>중지하기위해 StopCoroutine(routine);</li>
</ol>
<hr />
<p><strong><em>코루틴은 멀티쓰레드가 아닌 단일 스레드에서 작동. yield로 실행을 잠시 멈췄다가 이어서 할 뿐 다른 작업을 병렬로 처리하지 않는다.</em></strong></p>
<hr />
<h3 id="unity이벤트">Unity이벤트</h3>
<p>유니티 이벤트(UnityEvent)는 Unity에서 제공하는 직렬화 가능한 C# 이벤트 시스템이다.
System.Action, delegate, event등 C#의 이벤트처럼 작동하지만, 인스펙터창에서 직접 함수 연결이 가능하다.
<strong>인스펙터에서 함수 연결이 가능(정적 바인딩)</strong>해 디자이너나 기획자도 이벤트에 함수 연결이 가능하며 코드 수정없이 다양한 동작 설정이 가능하다.
A 오브젝트에서 B오브젝트의 기능을 직접 호출하지 않고, 이벤트를 통해 간접 호출 가능해 모듈 간 의존성을 줄여 유지보수가 편해진다.(<strong>느슨한 결합</strong>)
오브젝트마다 다르게 설정 가능하므로 다양한 기능을 공유 가능해 <strong>재사용성이 증가</strong>한다.</p>
<blockquote>
<pre><code class="language-cs">public class MyButton : MonoBehaviour
{
    public UnityEvent onClicked;  // 인스펙터에서 연결 가능
    public void Click()
    {
        Debug.Log(&quot;버튼 클릭됨&quot;);
        onClicked?.Invoke();  // 연결된 이벤트 호출
    }
}</code></pre>
</blockquote>
<pre><code>
### Unity이벤트 구성 
UnityEvent : 기본 이벤트, 매개변수 없음
UnityEvent&lt;데이터타입&gt; : int, float, GameObject, string 등 하나의 인자 전달 가능
UnityEvent&lt;T&gt; : 제네릭 이벤트, 매개변수 1개
Invoke() : 이벤트 호출
AddListener() : 코드에서 함수 연결(동적바인딩)
RemoveListener : 리스너 제거

&gt;예제
```cs
public class OnDamaged : MonoBehaviour
{
    public UnityEvent onDamaged;
    public void Damage(int damage)
    {
        OnDamaged.Invoke();
    }
}</code></pre><p>Player 오브젝트에 OnDamaged 스크립트 연결
OnDamaged이벤트 항목에 원하는 함수 드래그 연결(HP감소, 사운드 재생, 애니메이션 실행등)</p>