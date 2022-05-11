# JVM и как я его понимаю

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

## Описание работы кода
В работе программы используются три области памяти - metaspace, heap(хип, куча) и stack memory (стек).
Metaspace хранит данные о классах. В стеке для каждого метода вылеляются подобласти - фреймы, в которых хранятся переменные этого метода и значения переменных примитивных типов данных. Хип используется для создания и хранения значений переменных ссылочных типов данных. 

При старте программы вызывается ClassLoader, который ищет класс JvmComprehension и загружает его в Metaspace.
В классе JvmComprehension JVM ищет точку входа - метод main() и вызывает этот метод. В момент вызова метода main() в стеке
создается фрейм 1.

### Строка 1:
* Во фрейме 1 создается переменная "i" типа int.
* Переменной "i" присваивается значение "1", которое также хранится в этом фрейме.

### Строка 2:
* С помощью структуры new Object() в хипе создается новый объект типа Object.
* Во фрейме 1 создается переменная "o" типа Object. 
* Cсылка на объект типа Object из хипа присваивается переменной "o".

### Строка 3:
* В хипе создается новый объект типа Integer со значением "1".
* Во фрейме 1 создается переменная "ii" типа Integer.
* Cсылка на объект типа Integer из хипа присваивается переменной "ii".

### Строка 4:
* Вызывается метод printAll(). В момент вызова метода printAll() а стеке создается фрейм 2.
* Во фрейм 2 копируются переменные параметров метода printAll(). При этом значение
переменной "i" также копируется во фрейм 3, а переменным "o" и "ii" передаются ссылки на объекты, находящиеся в хипе;

### Строка 5:
* В хипе создается новый объект типа Integer со значением "700".
* Во фрейме 2 создается переменная "uselessVar" типа Integer.
* Cсылка на объект типа Integer из хипа присваивается переменной "uselessVar".

### Строка 6:
* Вызывается метод System.out.println(). В момент вызова метода System.out.println() в стеке создается фрейм 3.
* Во фрейм 3 копируются переменные параметров метода System.out.println(). 
* У переменной "o" вызывается метод toString(). В момент вызова метода toString() в стеке создается фрейм 4.
* Во фрейм 4 передается переменная "o" со ссылкой на объект в хипе.
* В хипе создается новый объект типа String со значением результата выполнения метода toSring() у переменной "o".
* Ссылка на объект типа String в хипе передается в параметры метода System.out.println() во фрейм 3.
* Метод toSring() завершается, фрейм 4 удаляется из стека.
* Значение переменной "i" копируется во фрейм 3, переменной "ii" передается ссылка на объект, находящийся в хипе.
* Метод System.out.println() завершается, переменные параметров удаляются из фрейма 3, фрейм 3 удаляется из стека.
* Завершается метод printAll(). Переменные параметров метода printAll() и переменная "uselessVar" удалаются из фрейма 2, фрейм 2 удаляется из стека.

### Строка 7:
* Вызывается метод System.out.println(). В момент вызова метода System.out.println() в стеке создается фрейм 5.
* В хипе создается новый объект типа String со значением "finished".
* Ссылка на объект типа String передается в переменную параметров метода System.out.println() во фрейме 5.
* Метод System.out.println() завершается, переменная параметров метода удаляется из фрейма 5, фрейм 5 удаляется из стека.
* Завершается выполнение метода main(). Переменные удаляются из фрейма 1. Фрейм 1 удаляется из стека.

В процессе выполнения программы периодически запускается сборщик мусора и очищает хип от неиспользуемых объектов.






