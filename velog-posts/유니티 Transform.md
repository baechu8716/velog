<h3 id="transform">Transform</h3>
<p>Transform은 유니티에서 모든 GameObject가 필수적으로 가지고 있는 컴포넌트로, 객체의 위치(Position), 회전(Rotation), 크기(Scale)를 관리한다. 또한 객체간의 부모-자식 관계(Hierachy)를 정의하는 데도 사용된다.</p>
<h4 id="transform-주요-속성">Transform 주요 속성</h4>
<p><strong>위치관련</strong></p>
<ul>
<li>position(Vector3)
  월드 좌표계에서 객체의 위치를 절대적으로 설정함
  ex) transform.position = Vector3(x, y, z)</li>
<li>localPosition(Vector3)
  부모 객체 기준의 로컬 좌표
  ex) 부모가 (1,0,0)위치 자식이 (1,0,0)이면 월드 좌표는 (2,0,0)</li>
<li>transform.Translate(방향 * 속도)</li>
<li>Vector3.Lerp(a, b, t)
  t는 0~1사이의 보간값. t&lt;0.5면 a에 가깝고, t&gt;0.5면 b에 가깝다.
  t가 Time.deltaTime * 속도처럼 작으면 부드럽게 이동한다.
  Lerp는 속도가 일정하지 않고 처음에 빨랐다가 점점 느려진다.</li>
<li>Vector3.MoveTowards(a, b, 이동할 최대 거리)
  일정한 속도로 a에서 b로 이동</li>
</ul>
<p><strong>회전관련</strong></p>
<ul>
<li>.rotation
  Quaternion 타입으로 회전 설정(월드 기준)
  유니티는 짐벌락 현상을 막기위해 내부적으로 회전값을 쿼터니언으로 변환한다.</li>
<li>.Ratate(축(Vector3), 각도)
  축을 기준으로 회전</li>
<li>.RotateAround(지점, 축, 각도)
  지점을 기준으로 회전</li>
<li>.LookAt(지점)
  지점을 바라보도록 회전
  기본적으로 Y축을 위로 유지하려고 하며 회전 속도 조절 없이 즉시 회전</li>
</ul>
<p><strong>Quaternion 변환</strong></p>
<ul>
<li>오일러를 쿼터니언으로 변환
  Quaternion a = Quaternion.Euler(...) </li>
<li>쿼터니언을 오일러로 변환
  Vector3 b = transform.ratation.eulerAngles</li>
<li>방향 벡터를 쿼터니언으로 변환
Quaternion c = Quaternion.LookRotation(Vector3.forward)</li>
<li>쿼터니언을 방향 벡터로 변환
Vector3 d = transform.right</li>
</ul>
<h3 id="로컬-vs-월드-좌표">로컬 VS 월드 좌표</h3>
<p>World Position : 씬 전체 기준 위치
Local Position : 부모 오브젝트 기준 위치
transform.position : 월드 위치
transform.localPosition : 로컬 위치</p>