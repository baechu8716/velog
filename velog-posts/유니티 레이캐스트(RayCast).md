<h3 id="레이케스트raycast">레이케스트(Raycast)</h3>
<p>레이캐스트(Raycast)는 보이지 않는 레이저를 쏴서 무언가와 부딪히는지를 감지하는 기능이다.
유니티에서 레이케스트는 시작점에서 방향으로 레이저를 쏴서 부딪히는 콜라이더를 감지하는 것이다.</p>
<h4 id="기본-문법">기본 문법</h4>
<pre><code class="language-cs">if (Physics.Raycast(시작지점, 방향, out RaycastHit hitinfo, 최대거리))
{
    Dubug.Log(hitinfo.collider.name);
}</code></pre>
<p>RaycastHit는 레이가 부딪힌 대상의 정보를 담은 구조체다.</p>
<ul>
<li>hitinfo.collider : 충돌한 콜라이더</li>
<li>hitinfo.point : 충돌한 지점</li>
<li>hitinfo.normal : 표면의 방향</li>
<li>hit.distance : 출발점에서 충돌지점까지의 거리</li>
<li>hit.rigidbody : 부딪힌 물체의 리지드바디</li>
<li>hit.transform : 부딪힌 물체의 트랜스폼</li>
</ul>
<h4 id="예제">예제</h4>
<p>마우스를 클릭한 위치에 레이 쏘기 (2D화면 =&gt; 3D세계)</p>
<pre><code class="language-cs">void Update()
{
    if (Input.GetMouseButtonDown(0))
    {
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        //ScreenPointToRay : 화면 좌표를 기준으로 카메라에서 3D 레이 쏘기
        if(Physics.Raycast(Ray, out RaycastHit hitinfo, 100f))
        {
            Debug.Log($&quot;충돌한 오브젝트 : {hitinfo.collider.name)
        }
     }
}</code></pre>
<p>레이어 마스크 활용(특정 오브젝트만 감지)</p>
<pre><code class="language-cs">LayerMask playerLayerMask // 인스펙터에서 설정
// 1 &lt;&lt; Layer번호 로 특정 레이어만 감지하도록 세팅 가능
if (Physics.Raycast(transform.position, transform.forward, out RaycastHit hit, 100f, playerLayerMask))
{
    Debug.Log(&quot;플레이어만 감지됨&quot;);
}</code></pre>