## Модульное тестирование

---

### Виды автоматического тестирования,
применяемые у нас

<br>

- модульное тестирование |
- функциональное тестирование |
- GUI тестирование |

---

## Регрессио́нное тести́рование 

(англ. regression testing, от лат. regressio — движение назад)

---

## Модульное тестирование

(англ. unit testing)

---

## Функциональное тестирование

---

## GUI тестирование

---

## Инструменты модульного тестирования
(применяемые у нас)

- PhpUnit
- Codeception

---

## Цель

- изолировать отдельные части программы 
- показать, что по отдельности эти части работоспособны

---

## Поощрение изменений

- рефакторинг
- расширение

---

## Упрощение интеграции

подход «снизу вверх»

---

## Документирование кода

«живой пример»

---

## Отделение интерфейса от реализации

через mock-объекты

---

## Подход TDD

Test-Driven Development

---

@title[Пример теста]

<p><span class="slide-title">Пример</span></p>

```php
namespace unit\base;

use skewer\base\SysVar;

class SysVarTest extends \PHPUnit_Framework_TestCase
{
    /**
     * @var SysVar
     */
    protected $object;

    /**
     * Sets up the fixture, for example, opens a network connection.
     * This method is called before a test is executed.
     */
    protected function setUp()
    {
        $this->object = new SysVar;
    }

    /**
     * Tears down the fixture, for example, closes a network connection.
     * This method is called after a test is executed.
     */
    protected function tearDown()
    {
    }

    /**
     * Проверка метода запроса значений    
     * @covers \skewer\base\SysVar::get()
     */
    public function testGet()
    {

        $this->assertSame(null, SysVar::get(''), 'Неверный результат для пустого значения!');

        $this->assertSame(null, SysVar::get(5), 'Неверный результат для некорректных входных параметров. Type:int');

        $sVarValue = 'test1';
        $sVarKey = 'test2';

        SysVar::set($sVarKey, $sVarValue);
        $this->assertSame($sVarValue, SysVar::get($sVarKey), 'Неверный результат!');

    }

    /**
     * Данные для проверки сохранения
     */         
    public function providerGet() {
        return [
            ['int', 123, '123'],
            ['str', 'qwe', 'qwe']
        ];
    }
    
    /**
     * @covers \skewer\base\SysVar::get
     * @dataProvider providerGet
     */         
    public function testGetVal( $name, $in, $out ) {
        SysVar::set($name, $in);
        $this->assertSame($out, SysVar::get($name));   
    }

}
```

@[1](namespace должен соответствовать расположению тестируемого класса)
@[5](SysVar (имя тестируемого класса) + Test)
@[12-27](методы выполныемые до и после КАЖДОГО теста)
@[19-46](обычный тест)
@[31](covers - указание на проверяемый метод)
@[33](test + Get(имя метода))
@[36](типовое "утверждение" (expected, actual, error_text))
@[43](использование других методов класса)
@[48-56](провайдер данных для теста)
@[58-65](тест, работающий с провайдером данных)
@[53](один набор данных)
@[60](имя метода с данными)
@[62](метод принимает переменные)
@[58-65](тест, работающий с провайдером данных)

---

```bash
$: vendor/bin/codecept run tests/codeception/unit/components/Cron/ApiTest.php
Codeception PHP Testing Framework v2.3.7
Powered by PHPUnit 4.8.36 by Sebastian Bergmann and contributors.
Тестовая база [_394_sapskii3] создана


Unit Tests (8) ---------------------------------------------------------------------------------------------------------
✔ ApiTest: Run task | #0 (0.00s)
✔ ApiTest: Run task | #1 (0.00s)
✔ ApiTest: Set test urls (0.00s)
✔ ApiTest: Get test urls (0.00s)
------------------------------------------------------------------------------------------------------------------------


Time: 1.04 seconds, Memory: 21.50MB

OK (8 tests, 8 assertions)

Тестовая база [_394_sapskii3] удалена
```
---

## Для запуска

1. прописать в constants.generated.php
```
define('SITE_ID', 394);
```
2. запустить
```
vendor/bin/codecept run tests/codeception/unit/
```
---

### Утилиты для запуска

---

### utils\testAll

```
#!/bin/bash

clear
vendor/bin/codecept run tests/codeception/unit/
```

---

### utils\set_rights

```
sudo chown -R canape:www-data .
sudo chmod -R 0775 .
```

---

### Вопросы?
