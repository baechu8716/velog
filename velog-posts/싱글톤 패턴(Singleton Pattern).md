<h3 id="싱글톤-패턴singleton-pattern">싱글톤 패턴(Singleton Pattern)</h3>
<p>싱글톤 패턴은 생성 패턴중 하나로, 특정 클래스의 인스턴스가 오직 하나만 생성되도록 보장하고, 그 인스턴스에 전역적으로 접근할 수 있는 방법을 제공하는 패턴이다.
유니티에서 싱글톤 패턴은 게임 전반에서 공유되어야 하는 관리 객체(예 : 게임매니저, 오디오매니저, 데이터매니저 등)를 구현할때 자주 사용된다.</p>
<p><strong>주요 특징</strong></p>
<ul>
<li>단일 인스턴스 : 클래스의 인스턴스가 하나만 존재</li>
<li>전역 접근 : 어디서든 해당 인스턴스에 접근 가능</li>
<li>지연 초기화 : 인스턴스가 필요할 떄 생성</li>
<li>생성 제어 : 외부에서 임의로 인스턴스로 생성하지 못하도록 생성자를 private로 설정</li>
</ul>
<p><strong>장점</strong></p>
<ol>
<li>전역 접근 지점 제공으로 코드 간 공유 용이</li>
<li>메모리 효율성 : 단일 인스턴스만 유지</li>
<li>상태 관리 용이 : 단일 상태 유지</li>
</ol>
<p><strong>단점</strong></p>
<ol>
<li>전역 상태 문제 : 전역 접근 가능성으로 인해 의도치 않은 상태 변경 위험</li>
<li>테스트 어려움 : 단일 인스터로 인해 테스트시 상태 초기화가 까다로움</li>
<li>강한 결합 : 싱글톤에 의존하는 코드가 많아지면 결합도가 높아짐</li>
<li>멀티스레딩 문제 : 스레드 안전성을 보장하려면 추가 구현 필요</li>
</ol>
<h4 id="싱글톤-패턴-구성-요소">싱글톤 패턴 구성 요소</h4>
<p>Singleton 클래스</p>
<ul>
<li>private static 인스턴스 변수</li>
<li>private 생성자(외부에서 new로 생성 방지)</li>
<li>public static 메서드(인스턴스 접근 제공, Instance속성)</li>
</ul>
<p>Client : 싱글톤 인스턴스를 사용하는 코드</p>
<h3 id="싱글톤-예제-코드">싱글톤 예제 코드</h3>
<p>게임 매니저를 싱글톤으로 구현해 점수를 관리하고, 모든 씬에서 접근 가능하도록 설정</p>
<ul>
<li>단일 인스턴스 보장</li>
<li>씬전환 시에도 유지(DontDestroyOnLoad 사용)</li>
<li>다른 스크립트에서 쉽게 접근 가능<blockquote>
<pre><code class="language-cs">public class GameManager : MonoBehaviour
{
  // 1. 정적 인스턴스 변수
  private static GameManager _instance;
  public static GameManager Instance
  {
      get
      {
          // 인스턴스가 없으면 생성
          if (_instance == null)
          {
              // 씬에서 기존 인스턴스 찾기
              _instance = FindObjectOfType&lt;GameManager&gt;();
              // 없으면 새로 생성
              if (_instance == null)
              {
                  GameObject singletonObject = new GameObject(typeof(GameManager).Name);
                  _instance = singletonObject.AddComponent&lt;GameManager&gt;();
                  DontDestroyOnLoad(singletonObject);
              }
          }
          return _instance;
      }
  }
  // 2. 게임 상태 (예: 점수)
  private int score = 0;
  // 3. Awake에서 인스턴스 관리
  private void Awake()
  {
      // 이미 인스턴스가 존재하면 이 객체 파괴
      if (_instance != null &amp;&amp; _instance != this)
      {
          Destroy(gameObject);
          return;
      }
      _instance = this;
      DontDestroyOnLoad(gameObject); // 씬 전환 시 유지
  }
  // 4. 점수 관련 메서드
  public void AddScore(int points)
  {
      score += points;
      Debug.Log($&quot;Score: {score}&quot;);
  }
  public int GetScore()
  {
      return score;
  }
  // 5. 게임 리셋
  public void ResetGame()
  {
      score = 0;
      Debug.Log(&quot;Game Reset&quot;);
  }
}
// 클라이언트: 점수 UI 업데이트
public class ScoreUI : MonoBehaviour
{
  private void Update()
  {
      // GameManager 싱글톤 인스턴스에 접근
      int currentScore = GameManager.Instance.GetScore();
      // UI에 점수 표시 (예: TextMeshPro, UI.Text 등)
      Debug.Log($&quot;Current Score: {currentScore}&quot;);
  }
}
// 클라이언트: 플레이어 점수 증가
public class PlayerController : MonoBehaviour
{
  private void OnTriggerEnter(Collider other)
  {
      // 예: 코인에 닿으면 점수 증가
      if (other.CompareTag(&quot;Coin&quot;))
      {
          GameManager.Instance.AddScore(10);
          Destroy(other.gameObject);
      }
  }
}</code></pre>
</blockquote>
</li>
</ul>