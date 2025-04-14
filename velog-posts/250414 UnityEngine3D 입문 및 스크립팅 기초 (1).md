<h3 id="게임-엔진">게임 엔진</h3>
<p>게임 엔진은 게임 개발을 위한 소프트웨어 프레임워크로, 게임의 핵심 기능(그래픽 렌더링, 물리 연산, 사운드 처리, 입력 관리, 애니메이션, 에셋 관리, 스크립팅 등) 을 제공하여 개발자가 게임을 더 효율적으로 만들 수 있게 도와준다.</p>
<p>대표적인 게임 엔진</p>
<ul>
<li>Unity : 2D/3D 게임 개발에 강력하다. C#언어를 사용하며 초보자 친화적이다.</li>
<li>Unreal Engine : 고품질 3D 그래픽, C++ 및 Blueprint을 사용하며 대규모 프로젝트에 적합하다.</li>
<li>Godot : 오픈소스이고, 2D/3D를 지원한다. GDScript를 사용한다.</li>
<li>CryEngine : 고사양 그래픽에 특화되어있으며, 대형 AAA게임에 사용된다.</li>
</ul>
<h3 id="unity-엔진">Unity 엔진</h3>
<p><img alt="" src="https://velog.velcdn.com/images/yangb062/post/7a763d84-fd71-4999-bf89-134f24ca70a5/image.png" /></p>
<p>유니티(Unity)는 3D 및 2D 게임의 개발 환경을 제공하는 게임 엔진이다.
게임 뿐만아니라 3D 애니메이션과 건축 시간화, 가상현실(VR) 등 인터랙티브 콘텐츠 제작을 위한 통합 제작 도구이다.</p>
<h3 id="unity-인터페이스-구성">Unity 인터페이스 구성</h3>
<p><img alt="" src="https://velog.velcdn.com/images/yangb062/post/342c02d6-29a0-4fac-b9c0-8175b90170e1/image.png" />
1.메인메뉴
각종 메뉴와 설정들이 있다</p>
<p>2.하이어라키(Hierarchy)
게임 공간상에 배치되는 오브젝트들에 대한 목록과 계층 구조(상속 관곌)를 표시한다.</p>
<p>3.프로젝트(Project)
해당 프로젝트 내에서 사용하는 모든 파일과 에셋을 표시한다.</p>
<p>4.콘솔(Console)
에디터 운용 / 테스트 진행 중 발생하는 모든 로그 및 에러의 이력을 확인 가능하다.</p>
<p>5.씬뷰(SceneView)
게임 내 공간을 개발자가 시각적으로 탐색하고 편집할 수 있는 공간이다.</p>
<p>6.게임뷰(Gameview)
Run(실행)시 실제로 게임 화면으로 렌더링 되는 화면이다.
게임 내의 카메라를 통해 렌더링</p>
<p>7.인스펙터(Inspector)
선택 된 오브젝트에 대한 정보를 보고 편집할 수 있다.
오브젝트에 추가된 기능에 따라 다양한 설정 메뉴들이 있다.</p>
<p>8.툴바 - 레이어 메뉴(Layer Menu)
에디터 내에서 레이어가 설정된 오브젝트에 대한 가시여부 설정이 가능하다.</p>
<p>9.툴바 - 레이아웃 메뉴(Layout Menu)
화면의 인터페이스를 미리 설정된 레이아웃대로 변경할 수 있다.</p>
<p>10.툴바 - 플레이 모드(Play Mode)
게임의 플레이테스트를 진행. 왼쪽부터 Run(play), 일시정지, 1프레임 넘기기</p>
<h3 id="라이프-사이클">라이프 사이클</h3>
<p>유니티는 라이프 사이클을 통해 프레임에서 이뤄져야 할 동작, 즉 연산들을 수행하고 플레이어에게 그래픽으로 결과를 보여준다.
라이프 사이클을 잘 이해 한다면 게임의 가장 기초적인 최적화를 어떻게 수행할지 알게 된다.</p>
<h4 id="유니티-라이프사이클-주요-단계">유니티 라이프사이클 주요 단계</h4>
<p><strong>1. 초기화 단계 (Initialization)</strong></p>
<ul>
<li>Awake() :
  스크립트 인스턴스가 처음 생성될 때 호출.
  오브젝트가 활성화되기 전에 실행되며, 주로 초기화 작업(변수 설정, 참조 연결 등)에 사용
  다른 오브젝트의 참조를 가져오거나 초기 설정에 유용
  오브젝트가 비활성화 상태여도 호출</li>
<li>OnEnable():
  오브젝트가 활성화될 때마다 호출
  스크립트가 비활성화되었다가 다시 활성화될 때도 실행
  주로 활성화 시 필요한 작업에 사용</li>
<li>Start():
  첫 프레임 업데이트 직전에 호출
  Awake()이후 실행되며, 모든 오브젝트가 초기화된 후 로직을 실행할 때 유용
  오브젝트가 활성화된 상태에서만 호출</li>
<li><em>2. 업데이트 단계 (Update Loop)*</em></li>
<li>FixedUpdate() : 
   고정된 시간 간격(기본 0.02초)으로 호출
   물리 연산 관련 로직(ex:Rigidbody 움직임)에 적합
   프레임 속도와 무관하게 일정한 간격으로 실행</li>
<li>Update():
   매 프레임마다 호출
   일반적인 게임 로직(입력 처리, 오브젝트 상태 변경 등)에 사용
   프레임 속도에 따라 호출 빈도가 달라짐</li>
<li>LateUpdate():
   Update()이후 호출
   모든 Update()로직이 처리된 후 실행되므로, 카메라 이동이나 후처리 로직에 유용
   캐릭터 이동 후 카메라가 따라가도록 설정</li>
<li><em>3. 물리 및 렌더링 단계*</em>
   물리 연산은 FixedUpdate와 연관되며, 내부적으로 물리 엔진이 처리<pre><code> 렌더링은 유니티 엔진이 프레임을 화면에 그리는 과정으로, 개발자가 직접 제어하지 않음</code></pre></li>
<li><em>4. 종료 단계 (Destruction)*</em></li>
<li>OnDisable() :
  오브젝트가 비활성화될 때 호출
  리소스 정리나 상태 저장에 사용</li>
<li>OnDestory() :
  오브젝트가 파괴될 때 호출
  씬 전환 시나 오브젝트 제거 시 정리 작업(ex:이벤트 구독 헤제)에 사용
  스크립트가 비활성화 상태라면 호출되지 않음</li>
<li>OnApplicationQuit() :
  에플리케이션이 종료될 때 호출
  게임 종료 전 데이터 저장등에 사용</li>
</ul>
<p><strong>추가 메서드 (상황별 호출)</strong></p>
<ul>
<li>OnValidate() :
  인스펙터에서 값이 변경될 때 호출(에디터에서만 동작).
  값 검증이나 테스트에 유용.</li>
<li>OnCollisionEnter/Stay/Exit :
  충돌 이벤트(물리 충돌) 발생 시 호출.</li>
<li>OnTriggerEnter/Stay/Exit:
  트리거 영역 진입/유지/퇴장 시 호출.</li>
</ul>
<p><code>주의점</code>
<code>Update() 매 프레임 호출되므로 무거운 연산은 피해야 한다.</code>
<code>Awake()와 Start()의 순서를 이해하고 초기화 순서에 맞게 코드를 작성해야 한다.</code>
`Update()나 Start()는 오브젝트가 활성화되어 있어야 호출된다</p>
<h3 id="직렬화serialization">직렬화(Serialization)</h3>
<p>직렬화는 유니티에서 데이터 구조나 게임 오브젝트의 상태를 보관하고 관리하는 기술이다.
이를 통해 스크립트의 멤버 변수를 유니티 에디터(인스펙터 창)에서 시각화하고 수정할 수 있다.</p>
<p>직렬화의 역할
데이터 보관 : 변수 값을 씬 파일이나 프리팹에 저장
익스펙터 표시 : 직렬화된 변수는 인스펙터 창에 노출되어 에디터에서 수정 가능
런타임 반영 : 에디터에서 변경할 값이 게임 실행 시 반영</p>
<p>조건
public 변수 : 기본적으로 직렬화됨
private/protected변수: [SerializedField] 어트리뷰트를 추가하면 직렬화 가능
지원 타입 : 기본타입(int, float, string, bool), 유니티 타입(GameObject, Transform), 사용자 정의 클래스나([SerializedField] 필요)</p>
<h3 id="어트리뷰트attribute">어트리뷰트(attribute)</h3>
<p>유니티에서 어트리뷰트는 변수나 클래스에 메타데이터(데이터에 대한 데이터)를 추가하여 유니티 에디터에서 특정 동작을 제어하거나 시각적 표시를 커스터마이징하는 역할을 한다. 주로 MonoBehavior 스크립트에서 사용되며, 인스펙터 창의 UI나 변수 동작을 변경하는 데 유용하다.</p>
<p>[Header(&quot;헤더 이름&quot;)] : 인스펙터 창에서 변수들을 기능별로 구분하기 위해 그룹화.
[Range(min, max)]int 또는 float 변수 위에 추가 : 변수 값을 특정 범위 내에서 스크롤바로 조정 가능하게 만듦.
[SerializeField] : private 변수도 인스펙터에 노출.
[Tooltip(&quot;설명&quot;)] : 변수에 마우스를 올리면 툴팁 표시.
[Space] : 변수 사이에 빈 공간 추가.
[TextArea] 또는 [TextArea(minLines, maxLines)] : string 변수를 인스펙터 창에서 텍스트 영역(Text Area)으로 표시하여 여러 줄 입력 가능.</p>
<blockquote>
<pre><code class="language-cs">[Header(&quot;Value&quot;)] // 어트리뷰트 구분하기 위한 용도
// 변수 : 정보
public bool boolValue;
public int intValue;
public float floatValue;
public Color color;
public Gradient gradient;
public Vector2 vector2;
public Vector2Int vector2Int;
public Vector3 position;
public Vector3Int vector3int;
public Rect rect;
public LayerMask layerMask;
public AnimationCurve curve;
public enum Type { None, Noraml, Rare }
public Type type;
public float jumpForce;
public int[] array;
public List&lt;int&gt; intList;
[Header(&quot;Reference&quot;)]
public GameObject ob;
public Rigidbody rigid;
public Collider collider;
[Range(0, 100)]
public float rate;
[TextArea(3,5)]
public string textField;
[SerializeField] int Value; </code></pre>
</blockquote>
<pre><code>![](https://velog.velcdn.com/images/yangb062/post/d65934a1-8675-48bb-a5c6-c0f572df0f83/image.png)</code></pre>