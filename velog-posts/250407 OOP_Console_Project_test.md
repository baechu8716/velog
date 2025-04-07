<p>앞으로 3일 동안 레벨 테스트로 콘솔게임을 만들기 전.지금 까지 배운 내용을 바탕으로 콘솔 게임을 머드게임 형태로 만드는 연습을 했다.</p>
<h3 id="연습-코드">연습 코드</h3>
<p><a href="https://github.com/baechu8716/OOPConsoleProject_test_1">https://github.com/baechu8716/OOPConsoleProject_test_1</a></p>
<h3 id="오류-해결">오류 해결</h3>
<p>인벤토리 메뉴 구현중 포션을 누르려고 y를 눌렀더니 오류가 발생했다.
<img alt="" src="https://velog.velcdn.com/images/yangb062/post/f616dda1-7942-421f-b594-1b6b603d5431/image.png" />
<img alt="" src="https://velog.velcdn.com/images/yangb062/post/d7cef897-39df-42f1-8b03-a5a3b1203d3b/image.png" /></p>
<blockquote>
<p>오류코드</p>
</blockquote>
<pre><code class="language-cs">public void Open()
{
    stack.Push(&quot;Menu&quot;);
    while (stack.Count&gt;0)
    {
        Console.Clear();
        switch(stack.Peek())
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
        public void UseConfirm()
        {
            Item selectItem = items[selectIndex];
            Console.WriteLine($&quot;{selectItem.name} 을/를 사용하시겠습니까? (y/n)&quot;);
            ConsoleKey input = Console.ReadKey(true).Key;
            switch (input)
            {
                case ConsoleKey.Y:
                    selectItem.Use();
                    Util.PressAnyKey($&quot;{selectItem.name}을/를 사용했습니다.&quot;);
                    Remove(selectItem);                    
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
            Console.WriteLine($&quot;{selectItem.name} 을/를 버리시겠습니까? (y/n)&quot;);
            ConsoleKey input = Console.ReadKey(true).Key;
            switch (input)
            {
                case ConsoleKey.Y:
                    Util.PressAnyKey($&quot;{selectItem.name}을/를 버렸습니다.&quot;);
                    Remove(selectItem);
                    break;
                case ConsoleKey.N:
                    stack.Pop();
                    break;
                default:
                    break;
            }
        }</code></pre>
<p>오류코드를 확인하였더니 아이템을 사용/드랍 확인중 Y를 누른다면 리스트 안 선택아이템을 제거하는 형식으로 되어있다.
하지만 Use/DropConfirm메소드를 스택에 쌓고 Pop을 통해 빼내주지 않았기 때문에 Use/DropConfirm메소드를 반복 실행하게 되는데 이때 Remove(selectItem);를 통해 리스트에 있는 아이템을 이미 제거하였기 때문에 
<code>System.ArgumentOutOfRangeException: 'Index was out of range. Must be non-negative and less than the size of the collection. (Parameter 'index')</code>에러가 발생했다
이를 해결하기 위에 Use/DropConfirm내 Y를 누를때 stack.Pop();을 추가해 문제를 해결했다</p>
<blockquote>
<p>해결</p>
</blockquote>
<pre><code class="language-cs">public void Open()
{
    stack.Push(&quot;Menu&quot;);
    while (stack.Count&gt;0)
    {
        Console.Clear();
        switch(stack.Peek())
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
        public void UseConfirm()
        {
            Item selectItem = items[selectIndex];
            Console.WriteLine($&quot;{selectItem.name} 을/를 사용하시겠습니까? (y/n)&quot;);
            ConsoleKey input = Console.ReadKey(true).Key;
            switch (input)
            {
                case ConsoleKey.Y:
                    selectItem.Use();
                    Util.PressAnyKey($&quot;{selectItem.name}을/를 사용했습니다.&quot;);
                    Remove(selectItem);  
                    stack.Pop();
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
            Console.WriteLine($&quot;{selectItem.name} 을/를 버리시겠습니까? (y/n)&quot;);
            ConsoleKey input = Console.ReadKey(true).Key;
            switch (input)
            {
                case ConsoleKey.Y:
                    Util.PressAnyKey($&quot;{selectItem.name}을/를 버렸습니다.&quot;);
                    Remove(selectItem);
                    stack.Pop();
                    break;
                case ConsoleKey.N:
                    stack.Pop();
                    break;
                default:
                    break;
            }
        }</code></pre>