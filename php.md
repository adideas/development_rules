 1. Соблюдение PSR (12).
 2. Стараться использовать === вместо == (в примере float != int)

    ```
    var_dump(15/3 === 5.0); // bool(false)
    ```
 3. Аккуратно с inArray (Type Error)

    ```PHP
    $array = [ false, true, 1 ];
    
    if( in_array( 'text', $array ) ){
    	echo 'Find!';
    }
    // 'text' == true 
    ```
 4. Везде описываем тип

    ```PHP
    var_dump(0 == 'foobar');
    // 7.4 bool(true)
    // 8+ bool(false)
    
    // THIS EXAMPLE ERROR (if int valid)
    function test($test) {
     return 0 == $test;
    }
    // THIS EXAMPLE VALID
    function test(int $test) {
     return 0 == $test;
    }
    
    // EXAMPLES ERROR
    var_dump('123' == '       123'); // bool(true)
    var_dump('1e3' == '1000'); // bool(true)
    var_dump('+74951112233' == '74951112233'); // bool(true)
    var_dump('00000020' == '0000000000000000020'); // bool(true)
    var_dump('0.235' == '+.235'); // bool(true)
    var_dump('0.2e-10' == '2.0E-11'); // bool(true)
    
    // WTF?
    var_dump('0X1D' != '29E0'); // bool(true) => int(29) != float(29)
    var_dump(29 == 29.0); // true
    var_dump(floatval(29) == intval(29)); // true
    var_dump(29 === 29.0); // false
    var_dump(floatval(29) === intval(29)); // false
    //
    var_dump((0xafebac == 11529132) != ('0xafebac' == '11529132')); // bool(true)
    var_dump('0xafebac' != '0XAFEBAC'); // bool(true) => int(11529132) != int(11529132)
    var_dump('0xeb' != '+235e-0'); // true int(235) != float(235)
    //
    var_dump(6/5 === 1.2000000000000004); // bool(false)
    var_dump(6/5 === 1.20000000000000004); // bool(true)
    ```
 5. ЗАПРЕЩЕНО ИСПОЛЬЗОВАТЬ (AND, OR, XOR) ТОЛЬКО (&& || !$x)

    ```
    // and | or | xor
    $a = true;
    $b = false;
    $c = $a and $b;
    
    var_dump($c);
    // bool(true)
    
    $a = true;
    $b = false;
    $c = $a && $b;
    
    var_dump($c);
    // bool(false)
    
    // EXAMPLE 2
    $var = false OR true;
    $var2 = false || true;
    var_dump( $var, $var2 ); // bool(false), bool(true)
    // EXAMPLE 3
    $var = true && false;
    $var2 = true and false;
    var_dump( $var, $var2 ); // bool(false), bool(true)
    ```
 6. Запрещено инкрементировать строки

    ```PHP
    $a = 'fact_2';
    echo ++$a; // fact_3
    
    $a = 'Привет';
    echo ++$a; // Привет
    ```
 7. Запрещено использовать FOREACH с ссылкой (Или делать unset)

    ```PHP
    $array = [ 'a', 'b', 'c' ];
    
    foreach( $array as & $item ){ }
    foreach( $array as   $item ){ }
    
    print_r( $array ); // Array([0] => a [1] => b [2] => b)
    
    // HOW FIXED
    $array = [ 'a', 'b', 'c' ];
    
    foreach( $array as & $item ){ }
    unset( $item );
    foreach( $array as   $item ){ }
    
    print_r( $array ); // Array([0] => a [1] => b [2] => c)
    ```
 8. На всякий случай с плавающей точкой лучше работать так

    ```PHP
    echo intval( (0.1 + 0.7) * 10 ); // 7
    echo intval( bcadd(0.1, 0.7, 1)  * 10 ); // 8
    ```
 9. SOLID.
10. KISS, YAGNI, APO, бритва Оккама.
11. GOF minimal
12. Константа всегда должна быть ЗАГЛАВНЫМИ БУКВАМИ. И находится в своем месте.
13. Константа может быть определена двумя способами. (Использование define запрещено!).

    ```PHP
    const A = 'this a';
    public static string $A = 'this a';
    ```
14. Константы либо импортируются в объект, либо находятся на своих местах (в своих объектах).

    ```PHP
    // importing a constant
    use const My\CONSTANT;
    ```
15. Объект может быть расширен импортом функции. Функция может быть переименована через alias.

    ```PHP
    use function My\functionName as funcInThisScope;
    ```
16. Каждый метод или функция должна иметь типы аргументов и должна иметь тип возвращаемого значения.
17. Строгое соблюдение нейминга. Примеры ниже!
    * *A group of variables must be named according to a certain pattern*

      ```PHP
      $array = [
       'before_field_first' => '',
       'after_field_first' => '',
       'field_second_before' => '', // this is not good! -> before_field_second
       'field_second_after' => '' // this is not good! -> after_field_second
      ];
      ```
    * *Nested variables must have scope*

      ```PHP
      $array = [
       'col' => [
          'a' => 'value'
       ],
       'row' => [
          'row_a' => 'value' // this is not good! -> a {$array['row']['a']}
       ]
      ];
      ```
    * *A group of methods must be named according to a certain pattern*

      ```PHP
      class Eye {
       public function open();
       public function closeEye(); // this is not good! -> close();
       public function closeThis(); // this is not good! -> close();
       public function closeThisEye(); // this is not good! -> close();
      }
      ```
    * *Its good*

      ```PHP
      // THIS IS GOOD
      class EyeFacade {
        /**
        * @return EyeImpl | EyeService | EyeBuilder ...
        */
        public function myFabricMethod();
      }
      
      class EyeImpl {
        public function open();
      }
      
      EyeFacade::open();
      ```
    * *Class names must contain their purpose*

      ```PHP
      // THIS IS NOT GOOD! -> rename class Eye {}
      class EyeClass {
        public function open() {}
      }
      ```
    * *Class names must contain their purpose*

      ```PHP
      // THIS IS NOT GOOD! -> rename class Eye {}
      // this class is not Facade, because is Instance Object.
      
      class EyeFacade {
        public function open() {}
      }
      ```
    * *Class names must be in CamelCase format*

      ```PHP
      // THIS IS NOT GOOD! -> class Eye {}
      class EYE {}
      ```
    * *Class names must contain their purpose*

      ```PHP
      // THIS IS NOT GOOD! 
      // Because EyeStrategy is not stategy! is DTO or Flyweight.
      class Eye {
        private EyeStategy strategy; // -> private TYPE dto;
      
        public function __constructor() {
          $this->strategy = new EyeDTO(); // extends EyeStategy
        }
      }
      ```
    * *The name of the variables must carry their purpose in accordance with the scope. (amelCase format)*

      ```PHP
      // its good
      $userName = '';
      
      // its not good -> $userName
      $user_name = '';
      
      // its not good
      $a = '?';
      
      // its no good
      function getUserName(): string
      {
        $a = '';
        return $a;
      }
      
      // its good
      function getUserName(): string
      {
        $name = '';
        return $name;
      }
      ```
18. Понимание "Временная сложность алгоритма".

    ```PHP
    // O(n)
    for($i = 0; $i < $n; $i++) \\ O(n)
    
    // O(n * m) => O(n^2)
    for($i = 0; $i < $n; $i++) \\ O(n)
        for($i = 0; $i < $n; $i++) \\ O(m)
    ```
19. Класс не может быть наследован более 3х раз.

    ```PHP
    class First {} // ITS GOOD
    
    class Second extends First {} // ITS GOOD
    
    class Third extends Second {} // ITS GOOD
    
    class Fourth extends Third {} // ITS NOT GOOD -> only 3x extends
    ```
