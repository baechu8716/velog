<h3 id="object">object</h3>
<p>유니티에서 오브젝트(Object)란 씬(Scene)안에 존재하는 모든 요소이다.
3D모델, 캐릭터, UI, 카메라, 빛, 빈 오브젝트 모두 해당한다.
Unity에서 &quot;GameObject&quot;가 모든 오브젝트의 기본 단위이고 여기에 Component(컴포넌트)를 붙여 기능을 추가한다.
모든 오브젝트는 transform컴포넌트를 가지고 있다.</p>
<h3 id="prefab">Prefab</h3>
<p>프리팹이란 재사용 가능한 게임 오브젝트의 원본 탬플릿이다.
씬(Scene)에서 직접 만들지 않고, 미리 만들어두고 복제해서 사용하는 오브젝트(설계도)이다.</p>
<p>프리팹의 장점
한 번 만들면 반복 사용 가능하여 재사용성이 좋다.
프리팹 하나를 수정하면 복제된 모든 오브젝트도 반영 가능하여 유지보수가 쉽다.
스크립트에서 런타임 중 Instantiate()로 생성 가능해서 동적 생성이 가능하다.
UI, 파티클, 총알 등 자주 쓰이는 요소 관리에 편하다.</p>
<p>주의점</p>
<p>프리팹을 수정하면 모든 인스턴스가 영향을 받는다.
프리팹 안에 프리팹도 가능하지만, 의존성이 꼬이면 유지보수가 어렵다.
씬에 있는 오브젝트를 프리팹으로 바꾸려면 Project로 드래그하면 된다.</p>
<blockquote>
<p>자주 사용되는 메서드</p>
</blockquote>
<pre><code class="language-cs">Instantiate(prefab) // 복사에서 씬에 생성
Destroy(gameObject) // 오브젝트 삭제</code></pre>
<h3 id="오브젝트-풀object-pool">오브젝트 풀(Object Pool)</h3>
<p>유니티에서 프리팹을 사용하여 총알을 생성하고 n초가 지나면 파괴시킨다고 가정할 때 생성과 반복을 반복하면 분명 컴퓨터에 부담이 되는 행위이고 메모리 단편화가 일어난다. 그래서 이를 해결하기 위해 오브젝트 풀로 미리 많이 사용할 오브젝트를 미리 저장해두고 빌리고 반납 하는 방식을 사용한다.</p>
<blockquote>
<p>유니티 간단한 오브젝트 풀</p>
</blockquote>
<pre><code class="language-cs">public class ObjectPool : MonoBehaviour
{
    [SerializeField] List&lt;PooledObject&gt; pool = new List&lt;PooledObject&gt;();
    [SerializeField] PooledObject prefab;
    [SerializeField] int size;
    private void Awake()
    {
        for (int i = 0; i &lt; size; i++)
        {
            PooledObject instance = Instantiate(prefab);
            instance.gameObject.SetActive(false);
            pool.Add(instance);
        }
    }
    public PooledObject GetPool(Vector3 position, Quaternion rotation)
    {
        if (pool.Count == 0)
        {
            return Instantiate(prefab, position, rotation);
        }
        PooledObject instance = pool[pool.Count - 1];
        pool.RemoveAt(pool.Count - 1);
        instance.returnPool = this;
        instance.transform.position = position;
        instance.transform.rotation = rotation;
        instance.gameObject.SetActive(true);
        return instance;
    }
    public void ReturnPool(PooledObject instance)
    {
        instance.gameObject.SetActive(false);
        pool.Add(instance);
    }
}
public class PooledObject : MonoBehaviour
{
    public ObjectPool returnPool;
    [SerializeField] float returnTime;
    private float timer;
    private void OnEnable()
    {
        timer = returnTime;
    }
    private void Update()
    {
        timer -= Time.deltaTime;    
        if (timer &lt;= 0)
        {
            ReturnPool();
        }
    }
    public void ReturnPool()
    {
        if (returnPool == null)
        {
            Destroy(gameObject);
        }
        else
        {
            returnPool.ReturnPool(this);
        }   
    }
}</code></pre>