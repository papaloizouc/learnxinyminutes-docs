---
language: Go
filename: learngo-ru.go
contributors:
    - ["Sonia Keys", "https://github.com/soniakeys"]
translators:
    - ["Artem Medeusheyev", "https://github.com/armed"]
lang: ru-ru
---

Go - это язык общего назначения, целью которого является удобство, простота,
конкуррентность. Это не тренд в компьютерных науках, а новейший и быстрый
способ решать насущные проблемы.

Концепции Go схожи с другими императивными статически типизированными языками.
Быстро компилируется и быстро исполняется, имеет легкие в понимании конструкции
для создания масштабируемых и многопоточных программ.

Может похвастаться отличной стандартной библиотекой и большим комьюнити, полным
энтузиастов.

```go
// Однострочный комментарий
/* Многострочный
   комментарий */

// Ключевое слово package присутствует в начале каждого файла.
// Main это специальное имя, обозначающее исполняемый файл, нежели библиотеку.
package main

// Import предназначен для указания зависимостей этого файла.
import (
    "fmt"      // Пакет в стандартной библиотеке Go
    "net/http" // Да, это web server!
    "strconv"  // Конвертирование типов в строки и обратно
)

// Объявление функции. Main это специальная функция, служащая точкой входа для
// исполняемой программы. Нравится вам или нет, но Go использует фигурные
// скобки.
func main() {
    // Println выводит строку в stdout.
    // В данном случае фигурирует вызов функции из пакета fmt.
    fmt.Println("Hello world!")

    // Вызов другой функции из текущего пакета.
    beyondHello()
}

// Функции содержат входные параметры в круглых скобках.
// Пустые скобки все равно обязательны, даже если параметров нет.
func beyondHello() {
    var x int // Переменные должны быть объявлены до их использования.
    x = 3     // Присвоение значения переменной.
    // Краткое определение := позволяет объявить перменную с автоматической
    // подстановкой типа из значения.
    y := 4
    sum, prod := learnMultiple(x, y)        // функция возвращает два значения
    fmt.Println("sum:", sum, "prod:", prod) // простой вывод
    learnTypes()                            // < y minutes, learn more!
}

// Функция имеющая входные параметры и возврат нескольких значений.
func learnMultiple(x, y int) (sum, prod int) {
    return x + y, x * y // возврат двух результатов
}

// Некотрые встроенные типы и литералы.
func learnTypes() {
    // Краткое определение переменной говорит само за себя.
    s := "Learn Go!" // тип string

    s2 := `"Чистый" строковой литерал
может содержать переносы строк` // тоже тип данных string

    // символ не из ASCII. Исходный код Go в кодировке UTF-8.
    g := 'Σ' // тип rune, это алиас для типа uint32, содержит юникод символ

    f := 3.14195 // float64, 64-х битное число с плавающей точкой (IEEE-754)
    c := 3 + 4i  // complex128, внутри себя содержит два float64

    // Синтаксис var с инициализациями
    var u uint = 7 // беззнаковое, но размер зависит от реализации, как и у int
    var pi float32 = 22. / 7

    // Синтаксис приведения типа с кратким определением
    n := byte('\n') // byte алиас для uint8

    // Массивы (Array) имеют фиксированный размер на момент компиляции.
    var a4 [4]int           // массив из 4-х int, проинициализирован нулями
    a3 := [...]int{3, 1, 5} // массив из 3-х int, ручная инициализация

    // Slice имеют динамическую длину. И массивы и slice-ы имеют каждый свои
    // преимущества, но slice-ы используются гораздо чаще.
    s3 := []int{4, 5, 9}    // по сравнению с a3 тут нет троеточия
    s4 := make([]int, 4)    // выделение памяти для slice из 4-х int (нули)
    var d2 [][]float64      // только объявление, память не выделяется
    bs := []byte("a slice") // конвертирование строки в slice байтов

    p, q := learnMemory() // объявление p и q как указателей на int.
    fmt.Println(*p, *q)   // * извлекает указатель. Печатает два int-а.

    // Map как словарь или хеш теблица из других языков является ассоциативным
    // массивом с динамически изменяемым размером.
    m := map[string]int{"three": 3, "four": 4}
    m["one"] = 1

    delete(m, "three") // встроенная функция, удаляет элемент из map-а.

    // Неиспользуемые переменные в Go являются ошибкой.
    // Нижнее подчеркивание позволяет игнорировать такие переменные.
    _, _, _, _, _, _, _, _, _ = s2, g, f, u, pi, n, a3, s4, bs
    // Вывод считается использованием переменной.
    fmt.Println(s, c, a4, s3, d2, m)

    learnFlowControl() // идем далее
}

// У Go есть полноценный сборщик мусора. В нем есть указатели но нет арифметики
// указатеей. Вы можете допустить ошибку с указателем на nil, но не с его
// инкрементацией.
func learnMemory() (p, q *int) {
    // Именованные возвращаемые значения p и q являются указателями на int.
    p = new(int) // встроенная функция new выделяет память.
    // Выделенный int проинициализирован нулем, p больше не содержит nil.
    s := make([]int, 20) // Выделение единого блока памяти под 20 int-ов,
    s[3] = 7             // назначение одному из них,
    r := -2              // опредление еще одной локальной переменной,
    return &s[3], &r     // амперсанд обозначает получение адреса переменной.
}

func expensiveComputation() int {
    return 1e6
}

func learnFlowControl() {
    // If-ы всегда требуют наличине фигурных скобок, но круглые скобки
    // необязательны.
    if true {
        fmt.Println("told ya")
    }
    // Форматирование кода стандартизировано утилитой "go fmt".
    if false {
        // все тлен
    } else {
        // жизнь прекрасна
    }
    // Использоване switch на замену нескольким if-else
    x := 1
    switch x {
    case 0:
    case 1:
        // case-ы в Go не проваливаются, т.е. break по умолчанию
    case 2:
        // не выполнится
    }
    // For, как и if не требует круглых скобок
    for x := 0; x < 3; x++ { // ++ это операция
        fmt.Println("итерация", x)
    }
    // тут x == 1.

    // For это единственный цикл в Go, но у него несколько форм.
    for { // бесконечный цикл
        break    // не такой уж и бесконечный
        continue // не выполнится
    }
    // Как и в for, := в if-е означает объявление и присвоение значения y,
    // затем проверка y > x.
    if y := expensiveComputation(); y > x {
        x = y
    }
    // Функции являются замыканиями.
    xBig := func() bool {
        return x > 100 // ссылается на x, объявленый выше switch.
    }
    fmt.Println("xBig:", xBig()) // true (т.к. мы присвоили x = 1e6)
    x /= 1e5                     // тут х == 10
    fmt.Println("xBig:", xBig()) // теперь false

    // Метки, куда же без них, их все любят.
    goto love
love:

    learnInterfaces() // О! Интерфейсы, идем далее.
}

// Объявление Stringer как интерфейса с одним мметодом, String.
type Stringer interface {
    String() string
}

// Объявление pair как структуры с двумя полями x и y типа int.
type pair struct {
    x, y int
}

// Объявление метода для типа pair. Теперь pair реализует интерфейс Stringer.
func (p pair) String() string { // p в данном случае называют receiver-ом
    // Sprintf - еще одна функция из пакета fmt.
    // Обращение к полям p через точку.
    return fmt.Sprintf("(%d, %d)", p.x, p.y)
}

func learnInterfaces() {
    // Синтаксис с фигурными скобками это "литерал структуры". Он возвращает
    // проинициализированную структуру, а оператор := присваивает ее в p.
    p := pair{3, 4}
    fmt.Println(p.String()) // вызов метода String у p, типа pair.
    var i Stringer          // объявление i как типа с интерфейсом Stringer.
    i = p                   // валидно, т.к. pair реализует Stringer.
    // Вызов метода String у i, типа Stringer. Вывод такой же что и выше.
    fmt.Println(i.String())

    // Функции в пакете fmt сами всегда вызывают метод String у объектов для
    // получения строкового представления о них.
    fmt.Println(p) // Вывод такой же что и выше. Println вызывает метод String.
    fmt.Println(i) // тоже самое

    learnErrorHandling()
}

func learnErrorHandling() {
    // Идиома ", ok" служит для обозначения сработало что-то или нет.
    m := map[int]string{3: "three", 4: "four"}
    if x, ok := m[1]; !ok { // ok будет false, потому что 1 нет в map-е.
        fmt.Println("тут никого")
    } else {
        fmt.Print(x) // x содержал бы значение, если бы 1 был в map-е.
    }
    // Идиома ", err" служит для обозначения была ли ошибка или нет.
    if _, err := strconv.Atoi("non-int"); err != nil { // _ игнорирует значение
        // выведет "strconv.ParseInt: parsing "non-int": invalid syntax"
        fmt.Println(err)
    }
    // Мы еще обратимся к интерфейсам чуть позже, а пока...
    learnConcurrency()
}

// c это тип данных channel (канал), объект для конкуррентного взаимодействия.
func inc(i int, c chan int) {
    c <- i + 1 // когда channel слева, <- являтся оператором "отправки".
}

// Будем использовать функцию inc для конкуррентной инкрементации чисел.
func learnConcurrency() {
    // Тот же make, что и в случае со slice. Он предназначен для выделения
    // памяти и инициализации типов slice, map и channel.
    c := make(chan int)
    // Старт трех конкуррентных goroutine. Числа будут инкрементированы
    // конкуррентно и, может быть параллельно, если машина правильно
    // сконфигурирована и позволяет это делать. Все они будут отправлены в один
    // и тот же канал.
    go inc(0, c) // go начинает новую горутину.
    go inc(10, c)
    go inc(-805, c)
    // Считывание всех трех результатов из канала и вывод на экран.
    // Нет никакой гарантии в каком порядке они будут выведены.
    fmt.Println(<-c, <-c, <-c) // канал справа, <- обозначает "получение".

    cs := make(chan string)       // другой канал, содержит строки.
    cc := make(chan chan string)  // канал каналов со строками.
    go func() { c <- 84 }()       // пуск новой горутины для отправки значения
    go func() { cs <- "wordy" }() // еще раз, теперь для cs
    // Select тоже что и switch, но работает с каналами. Он случайно выбирает
    // готовый для взаимодействия канал.
    select {
    case i := <-c: // полученное значение можно присвоить переменной
        fmt.Printf("это %T", i)
    case <-cs: // либо значение можно игнорировать
        fmt.Println("это строка")
    case <-cc: // пустой канал, не готов для коммуникации.
        fmt.Println("это не выполнится.")
    }
    // В этой точке значение будет получено из c или cs. Одна горутина будет
    // завершена, другая останется заблокированной.

    learnWebProgramming() // Да, Go это может.
}

// Всего одна функция из пакета http запускает web-сервер.
func learnWebProgramming() {
    // У ListenAndServe первый параметр это TCP адрес, который нужно слушать.
    // Второй параметр это интерфейс типа http.Handler.
    err := http.ListenAndServe(":8080", pair{})
    fmt.Println(err) // не игнорируйте сообщения об ошибках
}

// Реализация интерфейса http.Handler для pair, только один метод ServeHTTP.
func (p pair) ServeHTTP(w http.ResponseWriter, r *http.Request) {
    // Обработка запроса и отправка данных методом из http.ResponseWriter
    w.Write([]byte("You learned Go in Y minutes!"))
}
```

## Что дальше

Основа всех основ в Go это [официальный веб сайт](http://golang.org/).
Там можно пройти туториал, поиграться с интерактивной средой Go и почитать
объемную документацию.

Для живого ознакомления рекомендуется почитать исходные коды [стандартной
библиотеки Go](http://golang.org/src/pkg/). Отлично задокументированная, она
является лучшим источником для чтения и понимания Go, его стиля и идиом. Либо
можно, кликнув на имени функции в [документации](http://golang.org/pkg/),
перейти к ее исходным кодам.
