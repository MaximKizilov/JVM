# Задача "Понимание JVM"

## Описание
Просмотрите код ниже и опишите (текстово или с картинками) каждую строку с точки зрения происходящего в JVM

Не забудьте упомянуть про:
- ClassLoader'ы,
- области памяти (стэк (и его фреймы), хип, метаспейс)
- сборщик мусора

## Код для исследования
```java

public class JvmComprehension { // Загружает

    public static void main(String[] args) { // создается фрейм стека
        int i = 1;                      // создается локальная переменная в фрейме стека
        Object o = new Object();        // Загружает Object в bootstrap. new Object() создается в heap (Эдем), ссылка Object o в стеке
        Integer ii = 2;                 // Загружает Integer в bootstrap, создается в heap, new Integer(2) создается в heap (Эдем), Integer ii в стеке
        printAll(o, i, ii);             // создается фрейм в стеке вызова метода, с параметрами ссылающимся на объекты кучи
        System.out.println("finished"); // удаляется из стека фрейм printAll, создается фрейм в стеке вызова метода вызываем метод println с ссылкой на стринговый объект в куче
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // Integer загружен раннее, new Integer(700) создается в heap, Integer uselessVar в стеке, удалится сборщиком по окончанию работы метода
        System.out.println(o.toString() + i + ii);  // Загружает System, PrintStream в bootstrap, создается фрейм в стеке вызова метода вызываем метод println с ссылкой на стринговый объект в куче
    }
}

```
