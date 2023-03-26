# Mocker

---
### Правила:
* У мокируемого объекта **_обязательно_** должен существовать пустой конструктор.
* От мокируемого интерфейса нельзя вызвать `thenImplemented()` если нет инстациированного объекта
* Все динамические и статические методы мокнутого объекта по умолчанию возвращают `null`
---
## API:
`@Mock` - аннотация вешается на поля класса, которые необходимо мокнуть.
Поле может иметь инстанциированный объект, тогда он будет использоваться
при вызове имплементированных функций.

### `Mocker` - основной оперируемый класс.
* `.init(Object obj) -> void` - мокает все аннотированные поля в `obj`.
* `.mock(Class<T> mocking, Object initialized) -> T` - создает мок класса `mocking`.
  * Имплементированные методы будут запускаться от `initialized`.
  * `initialized` может быть null только при условии, что `mocking` - интерфейс.
* `.mock(T instance) -> T` - создаёт мок класса `T`
  * Этим методом невозможно создать мок инерфейса.
* `.mock(Class<T> mocking) -> T` - создаёт мок класса `mocking`.
  * Создаёт экземпляр класса автоматически по пустому конструктору.
  * Этим методом невозможно создать мок интерфейса.
* `.when(R smt) -> IMockRT<R>` - инициализирует создание действия на метод.
  * В аргумент передаётся вызов функции.
    * Пример: `Mocker.when(someClass.someMethod("someValue", 123))`
---
### `IMockRT<R>` - интерфейс для создания действий на вызов метода.
Используется только в комбинации с `Mocker.when()`
* `.thenNull() -> void` - функция будет возвращать `null`
* `.thenReturn(R value) -> void` - функция будет возвращать `value`
* `.thenThrow(Throwable exception) -> void` - функция будет выкидывать `exception` 
* `.thenImplemented() -> void` - функция будет работать с поведением изначального класса.