# Домашняя работа по теме "JVM. Организация памяти, сборщики мусора, VisualVM".

## Код для исследования
```java
public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
```
## Описание
В стэке создается frame под названием `main()`
### 1
Примитивный тип данных `int` хранится прямо в стэке frame своего метода. Создается переменная типа `int` со значением 1.
### 2
Cсылочный тип данных `Object`. Ссылочные типы данных хранятся в стэке. В heap создается место под `Object`. `new` означает, что было выделено место в хипе под следующий класс `Object`. `new` возвращает адрес в памяти (адрес в heap). `new Object()` означает возврат адреса в heap. `Object o` означает, что в стэке создается переменная `о`, которая будет хранить в себе ссылку на объект типа `Object` (в эту переменную записывается эта ссылка).
### 3
`Integer` – это класс, который создается в heap. Туда передается значение `2`, а ссылка на этот объект в памяти передается в переменную `ii`.
### 4
Вызывается метод `printAll`. Появляется новый frame, который находится над frame `main()`. Туда передаются все переменные `o, i, ii`, которые были созданы.
Переменная `int i` создастся повторно. В этом frame `printAll` эта переменная создастся заново. Это будет другая переменная, в неё скопируются данные из этой записи `int i = 1`, которую передали сюда `o, i, ii`. Эти переменные `Object o`, `Integer ii` тоже создадутся.
### 5
Здесь создаётся новая переменная ссылочного типа. В heap создается объект `Integer` со значением `700`. Здесь у нас в стэке во frame `printAll` (из строки `private static void printAll(Object o, int i, Integer ii)` создается переменная `uselessVar`.
### 6
Вызывается метод `println`, который есть у объекта `out`. Объект `out` находится в классе `System`, то есть появляется ещё один frame. Каждый вызов метода создает новый frame. В этот frame передается одно значение `o.toString() + i + ii`. Эти все части вместе вычисляются и сводятся до одного выражения и передаются в метод `println`. После этого метод `System.out.println(o.toString() + i + ii)` завершает свою работу, а frame закрывается. Программа возвращается во frame `printAll`.
### 7
Создается новый frame `println`, в который передается строка `finished`. Создается переменная, в которую эта строка записывается. Переменная выводится на экран, метод завершается, frame закрывается. Метод `main` завершает свою работу и frame `main()` тоже завершается.
