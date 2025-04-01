<p>지금까지 배운 내용으로 객체지향적 TextRPG를 만들어 보았다.
게임을 만들려면 첫번째로 중요한 것은 <strong>Git</strong>이다.
Git에 레포지토리를 새로 만들고 Visual Studio에서 코드를 작성하고 저장할때마다 <strong>커밋</strong>해주었다.
그다음으로 객체지향은 설계가 필요하기 때문에 간이기획과 프로젝트 구조 설계를 하였다.</p>
<h3 id="textrpg">TextRPG</h3>
<ol>
<li>상황 설명이 주어진다.</li>
<li>선택지를 제시한다.</li>
<li>선택지 입력을 대기한다.</li>
<li>선택한 선택지에 따라서 결과를 출력한다.</li>
<li>다음으로 넘어가기 대기</li>
<li>다음 상황으로 전환한다</li>
</ol>
<p>게임에 필요한 기능들</p>
<ol>
<li>게임시작</li>
<li>게임종료</li>
<li>플레이어</li>
<li>몬스터</li>
<li>아이템</li>
<li>인벤토리</li>
<li>플레이어가 선택한 상황</li>
</ol>
<hr />
<p>메인클래스</p>
<pre><code class="language-cs">namespace TextRPG
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Game.Start();
            Game.Run();
            Game.End();
        }
    }
}</code></pre>
<hr />
<p>게임 클래스
게임에 필요한 정보와 기능들을 구현해놓은 클래스이다.</p>
<pre><code class="language-cs">namespace TextRPG
{
    public static class Game
    {
        // 게임에 필요한 정보들
        private static bool gameOver;

        private static Dictionary&lt;string, Scene&gt; sceneDic; //여러씬들을 관리하고 빠르게 찾기 위해 
        private static Scene curScene;
        private static Player player;
        private static Inventory inventory;
        private static Monster monster;
        public static Player Player { get { return player; } }
        public static Inventory Inventory { get { return inventory; } }
        public static Monster Monster { get { return monster; } }

        //게임에 필요한 기능들
        //게임시작
        public static void Start() // 게임의 초기,기본 설정
        { 
            //게임 시작시 필요한 작업들
            sceneDic = new Dictionary&lt;string, Scene&gt;();
            inventory = new Inventory();
            sceneDic.Add(&quot;Title&quot;, new TitleScene()); // Title, TitleScene추가
            sceneDic.Add(&quot;Town&quot;, new TownScene());
            sceneDic.Add(&quot;Shop&quot;, new ShopScene());
            sceneDic.Add(&quot;Field&quot;, new FieldScene());

            //처음 시작할 씬을 선정
            curScene = sceneDic[&quot;Title&quot;];

            //플레이어 추가 및 초기 스탯 부여
            player = new Player();
            player.Power = 10;
            player.Speed = 8;

            inventory.AddItem(new HpPotion(&quot;체력포션&quot;,&quot;포션&quot;, 10));
        }

        // 게임종료
        public static void End()
        {
            // 게임 종료시에 필요한 작업들
        }

        // 게임동작
        public static void Run()
        {
            // 게임 동작시에 필요한 작업들
            while(gameOver == false)
            {
                Console.Clear();
                curScene.Render();
                Console.WriteLine();
                curScene.Choice();
                Console.WriteLine();
                curScene.Input();
                Console.WriteLine();
                curScene.Result();
                Console.WriteLine();
                curScene.Wait();
                curScene.Next();

            }
        }
        public static void ChangeScene(string sceneName) // 기존 방식
        {
            curScene = sceneDic[sceneName];
        }

        public static void ChangeScene(Scene newScene)
        {
            curScene = newScene;
        }

        public static void GameOver(string reason)
        {
            Console.Clear();
            Console.WriteLine(&quot;************************&quot;);
            Console.WriteLine(&quot;*      게임 오버..     *&quot;);
            Console.WriteLine(&quot;************************&quot;);
            Console.WriteLine();
            Console.WriteLine(reason);

            gameOver = true;
        }

        public static void PrintInfo()
        {
            Console.WriteLine(&quot;************************&quot;);
            Console.WriteLine(&quot; 플레이어&quot;);
            Console.WriteLine($&quot; 힘 : {player.Power}\t 속도 : {player.Speed}&quot;);
            Console.WriteLine(&quot;************************&quot;);
        }
    }
}</code></pre>
<hr />
<p>타이틀 씬
<strong>게임 시작시 시작되는 씬</strong>
게임시작시 초기설정한 마을 씬으로, 게임종료시 게임을 종료하는 기능을 구현했다.</p>
<pre><code class="language-cs">namespace TextRPG.Scenes
{
    class TitleScene : Scene // Ctrl + . 추상클래스 구현 or 우클릭 빠른 작업 및 리팩토링
    {

        public override void Render()
        {
            Console.WriteLine(&quot;************************&quot;);
            Console.WriteLine(&quot;*      레전드 RPG       *&quot;);
            Console.WriteLine(&quot;************************&quot;);
            Console.WriteLine();
        }

        public override void Result() { } // 필요없는경우

        public override void Choice()
        {
            Console.WriteLine(&quot;1. 게임시작&quot;);
            Console.WriteLine(&quot;2. 게임종료&quot;);
        }

        public override void Wait() { }
        public override void Next()
        {
            // TODO : 다음 씬으로 전환 구현 필요 (TODO : 보기-작업목록 선택시 할일 표시)
            switch(input)
            {
                case ConsoleKey.D1:
                    Game.ChangeScene(&quot;Town&quot;);
                    break;
                case ConsoleKey.D2:
                    Game.GameOver(&quot;게임종료&quot;);
                    break;
            }
        }
    }
}</code></pre>
<hr />
<p>*<em>상황별 씬 *</em>
플레이어가 주어진 상황에서 선택에 따라 결과가 달라지도록 구현했다.
<img alt="" src="https://velog.velcdn.com/images/yangb062/post/3d356fd2-d003-41cf-947e-a3591b2b8f3a/image.png" /></p>
<hr />
<p><strong>인벤토리와 아이템</strong>
인벤토리는 리스트로 구현해보았고, 아이템은 Hp포션과 기능을 구현해보았다. (사용은 미구현..)
<img alt="" src="https://velog.velcdn.com/images/yangb062/post/fa72e9ee-6d7e-44b8-bdb2-0a9e31dc7d5a/image.png" /></p>
<hr />
<p><strong>몬스터와 플레이어</strong>
몬스터와 플레이어 둘 다 공격한다와 공격받는다의 공통점이 있어 인터페이스로 기능들을 묶어서 상속시켜보았고 몬스터가 죽을 시 아이템을 드랍하는 메서드를 생성하여 인벤토리에 추가하는 기능을 만들어 보았다.
<img alt="" src="https://velog.velcdn.com/images/yangb062/post/e5b54088-0228-4de3-93b9-780bc2123065/image.png" />
<img alt="" src="https://velog.velcdn.com/images/yangb062/post/f80284f9-4448-4621-89fa-0908745f4873/image.png" /></p>
<p><code>공격기능을 타겟을 지정하고 싶어서 어떻게 하는지 궁굼해서 찾아보니 인터페이스에서 선언한 함수에 매개변수를 인터페이스이름으로 해서(기능을 구현하는 클래스들은 인터페이스를 상속받을테니 타겟 지정 가능) 타겟을 만든다는 사실을 알았다..</code></p>
<hr />
<p><strong>전투</strong>
플레이어가 몬스터를 공격하면 서로 한대씩 주고받아 어느쪽이 먼저 피가 0이 되는지에 따라 결과가 바뀐다. <del>(약간 턴제 느낌?)</del></p>
<pre><code class="language-cs">using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using TextRPG.Monsters;

namespace TextRPG.Scenes
{
    public class CombatScene : Scene
    {
        private Monster currentMonster;
        private Inventory inventory;
        private bool isCombat = true;
        public CombatScene (Monster currentMonster)
        {
            this.currentMonster = currentMonster;
            this.inventory = Game.Inventory;
        }

        public override void Render()
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine($&quot;{currentMonster.Name}과 전투 시작&quot;);
            Console.ResetColor();
            Console.WriteLine(&quot;무엇을 하시겠습니까?&quot;);
        }


        public override void Choice()
        {
            Console.WriteLine(&quot;1. 공격한다.&quot;);
            Console.WriteLine(&quot;2. 인테토리 열기.&quot;);
            Console.WriteLine(&quot;3. 도망간다.&quot;);
        }


        public override void Result()
        {

                switch (input)
                {
                case ConsoleKey.D1:
                    Console.WriteLine(&quot;전투시작!&quot;);
                    if (isCombat = true)
                    {
                        Combat();
                    }
                    Console.WriteLine($&quot;{currentMonster.Name}의 싸늘한 시체다.. 공격하지말자.&quot;);
                    break;
                case ConsoleKey.D2:
                    Console.WriteLine(&quot;인벤토리를 엽니다&quot;);
                    Console.Clear();
                    inventory.PrintAll();
                    break;
                case ConsoleKey.D3:
                    Console.WriteLine(&quot;도망칩니다.&quot;);
                    break;
                default:
                        Console.WriteLine(&quot;잘못 입력 하셨습니다.&quot;);
                        break;
                }


        }

        public override void Wait()
        {
            Console.WriteLine(&quot;계속하려면 아무키나 입력해주세요...&quot;); //없다면 결과출력후 바로 넘어가 결과를 못 볼수 있음
            Console.ReadKey();
        }
        public override void Next()
        {
            switch (input)
            {
                case ConsoleKey.D1:
                    break;
                case ConsoleKey.D3:
                    Game.ChangeScene(&quot;Field&quot;);
                    break;
            }
        }
        public void Combat()
        {
            while(isCombat)
            {
                Console.WriteLine($&quot;플레이어가 {currentMonster.Name} 공격&quot;);
                Game.Player.Attack(currentMonster);
                Console.WriteLine($&quot;{currentMonster.Name}의 남은 체력 {currentMonster.Hp}&quot;);
                Console.WriteLine(&quot;----------------------------------------------------------&quot;);
                Console.WriteLine($&quot;{currentMonster.Name}몬스터가 플레이어를 공격&quot;);
                currentMonster.Attack(Game.Player);   
                Console.WriteLine($&quot;플레이어의 남은 체력 {Game.Player.Hp}&quot;);
                Console.ReadLine();
                if (currentMonster.Hp &lt;= 0)
                {
                    Console.WriteLine(&quot;몬스터가 쓰려졌습니다! 필드로 돌아갑니다&quot;);
                    Console.WriteLine(&quot;몬스터가 아이템을 드랍합니다&quot;);
                    currentMonster.DropItem();
                    isCombat = false;
                    Game.ChangeScene(&quot;Field&quot;);
                }
                else if (Game.Player.Hp &lt;= 0)
                {
                    Console.WriteLine(&quot;플레이어가 쓰려졌습니다...&quot;);
                    isCombat = false;
                    Game.GameOver(&quot;플레이어가 몬스터와 전투하다 체력이 모두 소모하여 사망하였습니다.&quot;);
                }
            }  
        }
    }
}</code></pre>
<hr />
<p>최대한 객체지향적으로 만들어보기 위해<img alt="" src="https://velog.velcdn.com/images/yangb062/post/06f97cd1-005f-454c-8499-0065925f9ee5/image.png" />
이런식으로 폴더를 만들어 기능들은 분류해보았다.
강의를 들으면서 개념은 이해가 가겠는데 개념을 활용하는데에 매우 큰 어려움을 느끼고 있다.
실습 내용을 보면 오늘 개념을 배운건 알겠는데 어떻게 구현하는지 막막할때가 많았다.
많은 설계를 경험해서 부족한 점을 보완해야 할 것 같다.</p>