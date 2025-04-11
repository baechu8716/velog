<p>버그 fix
<a href="https://github.com/baechu8716/OOP_ConsoleProject">https://github.com/baechu8716/OOP_ConsoleProject</a></p>
<p>기존 코드에는 인벤토리에 아이템 사용 여부 판단이 안되서 인벤토리를 키고 끄기만 해도 적 포켓몬이 공격하던 상황이 발생했다
해당 버그를 수정하기 위해 인벤토리를 열때 아이템을 사용했는지 판단하는 bool변수를 생성하여 전투중 아이템을 사용했을때만 적 포켓몬이 공격하도록 고쳤고 인벤토리를 열고 닫을때는 전투가 정상적으로 진행되도록 고쳤다</p>
<pre><code class="language-cs">using Project_P.GameObjects.Items;
using Project_P.Monsters;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.NetworkInformation;
using System.Text;
using System.Threading.Tasks;

namespace Project_P
{
    public class ItemInventory
    {
        public List&lt;Item&gt; items;
        public Stack&lt;string&gt; stack;
        public int selectIndex;
        public ItemInventory()
        {
            items = new List&lt;Item&gt;();
            stack = new Stack&lt;string&gt;();
        }
        public void UseItem(int index)
        {
            items[index].Use();
        }
        public void Add(Item item)
        {
            var existingItem = items.Find(i =&gt; i.name == item.name);
            if (existingItem != null)
            {
                existingItem.count += item.count;
            }
            else
            {
                items.Add(item);
            }
        }
        public void Remove(Item item)
        {
            items.Remove(item);
        }

        public void RemoveAt(int index)
        {
            items.RemoveAt(index);
        }

        public void Open()
        {

            stack.Push(&quot;Menu&quot;);
            while(stack.Count &gt; 0)
            {
                Console.Clear();
                switch (stack.Peek())
                {
                    case &quot;Menu&quot;:
                        Menu();
                        break;
                    case &quot;UseMenu&quot;:
                        UseMenu();
                        break;
                    case &quot;DropMenu&quot;:
                        DropMenu();
                        break;
                    case &quot;UseConfirm&quot;:
                        UseConfirm();
                        break;
                    case &quot;DropConfirm&quot;:
                        DropConfirm();
                        break;
                    default:
                        break;
                }
            }

        }

        public void Menu()         
        {   
            if(items.Count &gt; 0)
            {
                Print();
                Console.WriteLine(&quot;1. 사용한다.&quot;);
                Console.WriteLine(&quot;2. 버린다.&quot;);
                Console.WriteLine(&quot;돌아가려면 0번&quot;);
                ConsoleKey input = Console.ReadKey(true).Key;
                switch (input)
                {
                    case ConsoleKey.D1:
                        stack.Push(&quot;UseMenu&quot;);
                        break;
                    case ConsoleKey.D2:
                        stack.Push(&quot;DropMenu&quot;);
                        break;
                    case ConsoleKey.D0:
                        stack.Pop();
                        break;
                    default: break;

                }
            }     
            else
            {
                Console.WriteLine(&quot;아이템이 없습니다.&quot;);
                Console.WriteLine(&quot;E키를 눌러 종료.&quot;);
                if (Console.ReadKey(true).Key == ConsoleKey.E)
                {
                    stack.Clear();
                }
            }
        }
        public void UseMenu()
        {
            Print();
            Console.WriteLine(&quot;사용할 아이템의 번호를 선택해주세요&quot;);
            Console.WriteLine(&quot;돌아가려면 0번&quot;);

            ConsoleKey input = Console.ReadKey(true).Key;
            if (input == ConsoleKey.D0)
            {
                stack.Pop();
            }
            else
            {
                int select = (int)input - (int)ConsoleKey.D1;
                if (select &lt; 0 || select &gt;= items.Count)
                {
                    Console.WriteLine(&quot;범위 내의 아이템을 선택하세요.&quot;);
                    Console.ReadKey(true);
                }
                else
                {
                    selectIndex = select;
                    stack.Push(&quot;UseConfirm&quot;);
                }
            }
        }
        public void DropMenu()
        {
            Print();
            Console.WriteLine(&quot;버릴 아이템의 번호를 선택해주세요&quot;);
            Console.WriteLine(&quot;돌아가려면 0번&quot;);

            ConsoleKey input = Console.ReadKey(true).Key;
            if (input == ConsoleKey.D0)
            {
                stack.Pop();
            }
            else
            {
                int select = (int)input - (int)ConsoleKey.D1;
                if (select &lt; 0 || select &gt;= items.Count)
                {
                    Console.WriteLine(&quot;범위 내의 아이템을 선택하세요.&quot;);
                    Console.ReadKey(true);
                }
                else
                {
                    selectIndex = select;
                    stack.Push(&quot;DropConfirm&quot;);
                }
            }
        }
        public void UseConfirm()
        {
            Item selectItem = items[selectIndex];
            Util.TextColor($&quot;해당 {selectItem.name}을 정말로 사용하시겠습니까? ( Y / N )&quot;, ConsoleColor.Red);

            ConsoleKey input = Console.ReadKey(true).Key;
            switch(input)
            {
                case ConsoleKey.Y:
                    selectItem.Use();
                    stack.Clear();
                    Console.ReadKey(true);
                    break;
                case ConsoleKey.N:
                    stack.Pop();
                    break;
                default:
                    break;
            }
        }
        public void DropConfirm()
        {
            Item selectItem = items[selectIndex];
            Util.TextColor($&quot;해당 {selectItem.name}을 버리시면 다시 주울 수 없습니다.&quot;, ConsoleColor.Red);
            Util.TextColor($&quot;정말로 버리시겠습니까? ( Y / N )&quot;, ConsoleColor.Red);

            ConsoleKey input = Console.ReadKey(true).Key;
            switch (input)
            {
                case ConsoleKey.Y:
                    Console.WriteLine($&quot;몇개 버리시겠습니까? 현재 아이템 개수 : {selectItem.count}&quot;);
                    bool isTrue = false;
                    do
                    {
                        if(int.TryParse(Console.ReadLine(),out int useCount))
                        {
                            if (useCount &gt; 0 &amp;&amp; useCount &lt; selectItem.count)
                            {
                                selectItem.count -= useCount;
                                isTrue = true;
                            }
                            else
                            {
                                Console.WriteLine($&quot;1부터 {selectItem.count}사이의  값을 입력하세요.&quot;);
                            }
                        }
                        else
                        {
                            Console.WriteLine(&quot;유효한 숫자를 입력해주세요&quot;);
                        }
                    }
                    while (!isTrue);
                    Console.WriteLine($&quot;{selectItem.name}을 버렸습니다. 남은 아이템 개수 : {selectItem.count}&quot;);
                    if (selectItem.count &lt;= 0)
                    {
                        items.Remove(selectItem);
                    }
                    Console.ReadKey(true);
                    stack.Pop();
                    break;
                case ConsoleKey.N:
                    stack.Pop();
                    break;
                default:
                    break;
            }
        }

        public void Print()
        {
            if (items != null &amp;&amp; items.Count &gt; 0)
            {
                Console.WriteLine(&quot;==================아이템 목록==================&quot;);
                for (int i = 0; i &lt; items.Count; i++)
                {
                    Console.WriteLine($&quot;[{i + 1}] 이름 : {items[i].name} 갯수 :{items[i].count}개\n&quot; +
                        $&quot;아이템 정보 : {items[i].info}&quot;);
                }
                Console.WriteLine(&quot;==============================================&quot;);
            }
        }
    }
}</code></pre>
<pre><code class="language-cs">using Project_P.Monsters;

namespace Project_P.Scenes
{
    public class BattleScene : BaseScene
    {
        public int turnCount;
        private Stack&lt;string&gt; stack;
        public Monster enemy;
        private Monster playerMonster;
        int index = -1;
        private bool isItemUsed = false;
        public string previousScene;

        public BattleScene()
        {
            name = &quot;Battle&quot;;
            turnCount = 0;
            stack = new Stack&lt;string&gt;();
        }
        public override void Enter()
        {
            enemy = GameManager.CurrentEnemy;
            Console.Clear();
        }

        public override void Render()
        {
            if (stack.Count == 0)
            {
                stack.Push(&quot;Meet&quot;);
            }
            switch (stack.Peek())
            {
                case &quot;Meet&quot;:
                    Meet();
                    break;
                case &quot;SelectMenu&quot;:
                    SelectMenu();
                    break;
                case &quot;SelectConfirm&quot;:
                    SelectConfirm();
                    break;
                case &quot;Battle&quot;:
                    Battle();
                    break;
                case &quot;Inventory&quot;:
                    GameManager.Player.Iteminventory.Open();
                    stack.Pop();
                    isItemUsed = true;
                    break;
                default: break;
            }
        }

        public override void Update()
        {
            if (stack.Count == 0)
            {
                stack.Push(&quot;Meet&quot;);
            }
            switch (stack.Peek())
            {
                case &quot;Meet&quot;:
                    if (input == ConsoleKey.D1)
                    {
                        stack.Pop();
                        stack.Push(&quot;SelectMenu&quot;);
                    }
                    else if (input == ConsoleKey.D2)
                    {
                        Console.WriteLine(&quot;도망갑니다...&quot;);
                        Console.ReadKey(true);
                        stack.Pop();
                        GameManager.ChangeScene(GameManager.prevSceneName);
                    }
                    break;
                case &quot;SelectMenu&quot;:
                    ;
                    if (input == ConsoleKey.D0)
                    {
                        Console.WriteLine(&quot;전투를 종료합니다...&quot;);
                        stack.Pop();
                        GameManager.ChangeScene(GameManager.prevSceneName);
                    }
                    else
                    {
                        int select = (int)input - (int)ConsoleKey.D1;

                        if (select &lt; 0 || select &gt;= GameManager.Player.Inventory.monsters.Length)
                        {
                            Console.WriteLine(&quot;잘못된 입력입니다. 1~6 또는 0을 선택하세요.&quot;);
                            Console.ReadKey(true);
                        }
                        else if (GameManager.Player.Inventory.monsters[select] == null)
                        {
                            Console.WriteLine(&quot;빈 슬롯입니다. 다시 선택해 주세요&quot;);
                            Console.ReadKey(true);
                        }
                        else
                        {

                            index = select;
                            stack.Push(&quot;SelectConfirm&quot;);
                        }
                    }
                    break;
                case &quot;SelectConfirm&quot;:
                    if (input == ConsoleKey.Y)
                    {
                        if (index &gt;= 0 &amp;&amp; index &lt; GameManager.Player.Inventory.monsters.Length)
                        {
                            Monster selectedMonster = GameManager.Player.Inventory.monsters[index];
                            if (selectedMonster != null &amp;&amp; selectedMonster.CurHP &gt; 0)
                            {
                                playerMonster = selectedMonster;
                                playerMonster.Position = new Vector2(1, 1);
                                Console.WriteLine($&quot;{playerMonster.Name}. 너로 정했다!&quot;);
                                enemy.Position = new Vector2(60, 1);
                                Console.ReadKey(true);
                                stack.Push(&quot;Battle&quot;);
                            }
                            else if (selectedMonster?.CurHP &lt;= 0)
                            {
                                Console.WriteLine(&quot;해당 몬스터가 전투 불능 상태입니다.&quot;);
                                Console.ReadKey(true);
                            }
                        }
                    }
                    else if (input == ConsoleKey.N)
                    {
                        stack.Pop();
                    }
                    else { Console.WriteLine(&quot;잘못된 선택입니다.&quot;); Console.ReadKey(true); }
                    break;
                case &quot;Battle&quot;:
                    if (!isItemUsed) 
                    {
                        if (input == ConsoleKey.D1)
                        {
                            playerMonster.UseSkill(0, enemy);
                            ProcessTurn(false);
                            Console.ReadKey();
                        }
                        else if (input == ConsoleKey.D2)
                        {
                            playerMonster.UseSkill(1, enemy);
                            ProcessTurn(false);
                            Console.ReadKey();
                        }
                        else if (input == ConsoleKey.D3)
                        {
                            playerMonster.UseSkill(2, enemy);
                            ProcessTurn(false);
                            Console.ReadKey();
                        }
                        else if (input == ConsoleKey.D4)
                        {
                            playerMonster.UseSkill(3, enemy);
                            ProcessTurn(false);
                            Console.ReadKey();
                        }
                    }
                    if (input == ConsoleKey.E)
                    {
                        stack.Push(&quot;Inventory&quot;);
                    }
                    else if (isItemUsed)
                    {
                        ProcessTurn(true);
                        Console.ReadKey(true);
                        isItemUsed = false;
                    }
                    break;
                default: break;


            }
        }

        public override void Result()
        {

        }

        public void Meet()
        {
            Console.WriteLine($&quot;{GameManager.curScene.name} 이전씬 {GameManager.prevSceneName}&quot;);
            Console.WriteLine($&quot;야생의 {enemy.Name}(이)가 나타났다!&quot;);
            Thread.Sleep(500);
            enemy.Position = new Vector2(1, 2);
            enemy.Print(true);
            Console.SetCursorPosition(1, 30);
            Console.WriteLine(&quot;어떻게 할까?&quot;);
            Console.WriteLine(&quot;1. 싸운다&quot;);
            Console.WriteLine(&quot;2. 도망간다.&quot;);
        }

        private bool AreAllMonstersFainted()
        {
            foreach (var monster in GameManager.Player.Inventory.monsters)
            {
                if (monster != null &amp;&amp; monster.CurHP &gt; 0)
                    return false;

            }
            return true;
        }

        public void SelectMenu()
        {

            if (AreAllMonstersFainted())
            {
                Console.WriteLine(&quot;모든 몬스터가 전투불능 상태입니다.&quot;);
                Console.ReadKey(true);
                stack.Clear();
                GameManager.ChangeScene(GameManager.prevSceneName);
                return;
            }
            Console.WriteLine(&quot;어느 몬스터를 내보내겠습니까?&quot;);
            GameManager.Player.Inventory.PrintAll();
            Console.WriteLine(&quot;전투종료를 하시려면 0번&quot;);


        }
        public void SelectConfirm()
        {
            Console.WriteLine($&quot;{GameManager.Player.Inventory.monsters[index].Name}를(을) 내보내시겠습니까?&quot;);
            Console.WriteLine(&quot;Y / N&quot;);
        }

        public void Battle()
        {
            playerMonster.Print();
            enemy.Print(true);

            Console.SetCursorPosition(0, 27);
            Console.WriteLine($&quot;턴 {turnCount + 1}: {playerMonster.Name} vs {enemy.Name}&quot;);

            Console.SetCursorPosition(0, 29);
            Console.WriteLine($&quot;{playerMonster.Name}&quot;);
            Console.WriteLine($&quot;HP : {playerMonster.CurHP} / {playerMonster.MaxHP} &quot;);

            Console.SetCursorPosition(60, 29);
            Console.WriteLine($&quot;{enemy.Name}&quot;);
            Console.SetCursorPosition(60, 29);
            Console.WriteLine($&quot;HP : {enemy.CurHP} / {enemy.MaxHP}&quot;);

            Console.SetCursorPosition(0, 34);
            playerMonster.SkillList();
            Console.WriteLine(&quot;사용할 기술을 선택하세요(1~4) | E : 인벤토리 열기&quot;);
        }

        public void ProcessTurn(bool isItemUsed)
        {
            Random randomInt = new Random();
            if (enemy.CurHP &lt;= 0)
            {
                Console.SetCursorPosition(0, 44);
                Console.WriteLine($&quot;{enemy.Name}을(를) 쓰러뜨렸습니다!&quot;);
                Thread.Sleep(500);
                Console.SetCursorPosition(0, 45);
                playerMonster.LevelUp(enemy);
                Console.ReadKey(true);
                stack.Clear();
                if (enemy.Name == &quot;칠색조&quot;)
                {
                    GameManager.GameOver(&quot;보스 몬스터를 잡았습니다. 게임 클리어!&quot;);
                }
                else
                {
                    GameManager.ChangeScene(GameManager.prevSceneName);
                }
                this.isItemUsed = false;
                return;
            }
            if (!isItemUsed)
            {

            }
            enemy.UseSkill(randomInt.Next(3), playerMonster);
            if (playerMonster.CurHP &lt;= 0 || (playerMonster.Skills[0].CurPP == 0
                &amp;&amp; playerMonster.Skills[1].CurPP == 0
                &amp;&amp; playerMonster.Skills[2].CurPP == 0
                &amp;&amp; playerMonster.Skills[3].CurPP == 0))
            {
                Console.SetCursorPosition(0, 44);
                Console.WriteLine($&quot;{playerMonster.Name}이(가) 전투 불능이 되었습니다!&quot;);
                Thread.Sleep(500);
                Console.ReadKey(true);
                stack.Pop();
                stack.Push(&quot;SelectMenu&quot;);
                this.isItemUsed = false;
                return;
            }
            turnCount++;
            this.isItemUsed = false;
        }
    }
}</code></pre>