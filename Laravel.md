Основные правила разработки PHP-Laravel

 1. Использование интерфейсов Laravel. Создавать свои только при необходимости.
 2. Стараться как можно реже использовать count и with для моделей

    ```
    class MyController extends Controller {
    	public function __invoke(): void
    	{
    		// The load is increasing (This is not best practice)
    		Model::with('relation1', 'relation2', 'relation3');
    		
    		// This is not best practice
    		if (Model::count()) {}
    
    		// This is best practice
    		if (Model::exists()) {}
    	}
    }
    ```
 3. Request имеющий более 2 fields описывается отдельным обьектом (DTO + Validator). Метод authorize():boolean описывается только если нет Policy для Controller -> "\_\_constructor".

    ```
    class MyRequest extends FormRequest {
    
       public function rules(): array
       {
          return ['field' => 'string'];
       }
    
      /**
      * This method return field from entity / current object
      * @return string Return field
      */
      public function getField(): string
      {
         return $this->field;
      }
    }
    ```
 4. Метод Controller не может содержать больше 12 строк кода. Для реализации объемного кода используется сервисный слой. Сервисный слой должен иметь unit test или быть достаточно простым. Сервисный слой в controller может быть использован либо через агрегацию, либо через композицию. Слишком длинное название сервисного слоя решается через [alias](https://www.php.net/manual/en/language.namespaces.importing.php).

    ```
    use \MyServiceForAction as MyService;
    
    class MyController extends Controller {
       public function __invoke(MyService $service): void
       {
          return;
       }
    }
    ```
 5. Разрешено использовать DDD.
 6. Запрещена модификация Request.
 7. В маршрутах необходимо указывать аннотацию к методу контролера.

    ```
    /** @see HomeController::index(); */
    Route::get('home', 'HomeController@home');
    ```
 8. Маршруты должны соблюдать следующую логику (Контролеры тоже)

    ```
    // get many
    Route::get('entities', 'EntityController@index');
    
    // get one
    Route::get('entity', 'EntityController@show');
    
    // method
    Route::get('entity/method', 'EntityController@method');
    
    // big method
    Route::get('entity/method', 'EntityMethodController'); // __invoke()
    
    
    // Tree hierarchy
    Route::post('entity', 'EntityController')
    Route::post('entity/object', 'EntityObjectController')
    Route::post('entity/object/field', 'EntityObjectFieldController')
    ```
 9. Внедрение интерфейса должно быть оправданным
    * Будет более одного класса
    * Не известно какой класс будет в итоге
    * Есть бешеная необходимость
    * Описание взаимодействия API
10. К каждому методу оставлять достойный комментарий (Не мемуары).
11. Запрещено писать класс ради класса.
12. Не забывать использовать очистку память (НЕ unset);

    ```
    // 1
    $this->value = null;
    // 2
    unset($this->value);
    
    // using only unset is SO BAD!
    // php does not have a garbage collector.
    // unset only removes a reference to an object.
    // And he hangs in memory.
    ```
13. Соглашение об именах Eloquent для имен таблиц.
14. Использование транзакции базы данных.
15. Запрещено отправлять в jobs модели
