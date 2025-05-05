<h3 id="옵저버-패턴observer-pattern">옵저버 패턴(Observer Pattern)</h3>
<p>옵저버 패턴은 한 객체의 상태가 바뀌었을 때, 그 객체에 의존하는 여러 객체들이 자동으로 통보받아 갱신되도록 하는 패턴이다.
주체(Subject) : 상태를 가지고 있고, 변경 시 알림을 보냄
옵저버(Observer) : 주체의 변경을 감지하고, 반응하는 객체들</p>
<p><strong>장점</strong>
느슨한 결합 : Subject는 Observer가 누구인지 모름, 이벤트만 발생
확장성 : 옵저버를 얼마든지 추가 가능
재사용성 : 이벤트만 있으면 다양한 로직을 붙일 수 있음</p>
<p><strong>사용 예시</strong>
플레이어 체력 변화 → UI 체력바 갱신
퀘스트 완료 → 알림, 경험치 증가 등 여러 반응
몬스터 죽음 → 스코어 증가, 이펙트 재생 등</p>
<h3 id="옵저버-패턴-예시">옵저버 패턴 예시</h3>
<blockquote>
<p>Subject(이벤트 발행자)</p>
</blockquote>
<pre><code class="language-cs">public class HealthSystem : MonoBehaviour
{
    public event Action&lt;int&gt; OnHealthChanged;
    private int health = 100;
    public void TakeDamage(int damage)
    {
        health -= damage;
        Debug.Log(&quot;Health: &quot; + health);
        // 옵저버들에게 알림
        OnHealthChanged?.Invoke(health);
    }
}</code></pre>
<blockquote>
<p>Observer(이벤트 수신자)</p>
</blockquote>
<pre><code class="language-cs">public class HealthUI : MonoBehaviour
{
    [SerializeField] private HealthSystem healthSystem;
    private void OnEnable()
    {
        if (healthSystem != null)
        {
            healthSystem.OnHealthChanged += UpdateHealthBar;
        }
    }
    private void OnDisable()
    {
        if (healthSystem != null)
        {
            healthSystem.OnHealthChanged -= UpdateHealthBar;
        }
    }
    private void UpdateHealthBar(int newHealth)
    {
        Debug.Log(&quot;Update UI: &quot; + newHealth);
        // 여기서 실제 UI 체력바를 갱신
    }
}</code></pre>
<blockquote>
<p>몬스터가 죽을 때 여러 반응이 필요할 때
public event Action OnDeath;</p>
</blockquote>
<pre><code class="language-cs">void Die()
{
    OnDeath?.Invoke(); // 여러 컴포넌트가 반응 가능
}</code></pre>
<pre><code class="language-cs">monster.OnDeath += PlayDeathSound;// 옵저버 연결
monster.OnDeath += AddScore;// 옵저버 연결
monster.OnDeath += ShowEffect;// 옵저버 연결</code></pre>