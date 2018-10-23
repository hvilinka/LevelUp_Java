### Внутренние и вложенные классы


Классы могут быть вложенными (nested), то есть могут быть определены внури других классов. Частным случаем вложенных классов являются внутренние классы (inner class). Например, имеется класс Person, внутри которого определен класс Account:


    public class Program{

        public static void main(String[] args) {

            Person tom = new Person("Tom", "qwerty");
            tom.displayPerson();
            tom.account.displayAccount();
        }
    }
    class Person{

        private String name;
        Account account;

        Person(String name, String password){
            this.name = name;
            account = new Account(password);
        }
        public void displayPerson(){
            System.out.printf("Person \t Name: %s \t Password: %s \n", name, account.password);
        }

        public class Account{
            private String password;

            Account(String pass){
                this.password = pass;
            }
            void displayAccount(){
                System.out.printf("Account Login: %s \t Password: %s \n", Person.this.name, password);
            }
        }
    }
Внутренний класс ведет себя как обычный класс за тем исключением, что его объекты могут быть созданы только внутри внешнего класса.

Внутренний класс имеет доступ ко всем полям внешнего класса, в том числе закрытым с помощью модификатора private. Аналогично внешний класс имеет доступ ко всем членам внутреннего класса, в том числе к полям и методам с модификатором private.

Ссылку на объект внешнего класса из внутреннего класса можно получить с помощью выражения Внешний_класс.this, например, Person.this.

Объекты внутренних классов могут быть созданы только в том классе, в котором внутренние классы опеределены. В других внешних классах объекты внутреннего класса создать нельзя.

Еще одной особенностей внутренних классов является то, что их можно объявить внутри любого контекста, в том числе внутри метода и даже в цикле:

    public class Program{

        public static void main(String[] args) {

            Person tom = new Person("Tom");
            tom.setAccount("qwerty");
        }
    }
    class Person{

        private String name;

        Person(String name){
            this.name = name;
        }

        public void setAccount (String password){

            class Account{

                void display(){
                    System.out.printf("Account Login: %s \t Password: %s \n", name, password);
                }
            }
            Account account = new Account();
            account.display();
        }
    }

Статические вложенные классы
Кроме внутренних классов также могут статические вложенные классы. Статические вложенные классы позволяют скрыть некоторую комплексную информацию внутри внешнего класса:

    class Math{

        public static class Factorial{

            private int result;
            private int key;

            public Factorial(int number, int x){

                result=number;
                key = x;
            }

            public int getResult(){
                return result;
            }

            public int getKey(){
                return key;
            }
        }

        public static Factorial getFactorial(int x){

            int result=1;
            for (int i = 1; i <= x; i++){

                result *= i;
            }
            return new Factorial(result, x);
        }
    }
Здесь определен вложенный класс для хранения данных о вычислении факториала. Основные действия выполняет метод getFactorial, который возвращает объект вложенного класса. И теперь используем классы в методе main:

    public static void main(String[] args) {

        Math.Factorial fact = Math.getFactorial(6);
        System.out.printf("Факториал числа %d равен %d \n", fact.getKey(), fact.getResult());
    }

### Наследование

Одним из ключевых аспектов объектно-ориентированного программирования является наследование. С помощью наследования можно расширить функционал уже имеющихся классов за счет добавления нового функционала или изменения старого. Например, имеется следующий класс Person, описывающий отдельного человека:

    class Person {

        private String name;
        public String getName(){ return name; }

        public Person(String name){

            this.name=name;
        }

        public void display(){

            System.out.println("Name: " + name);
        }
    }

И, возможно, впоследствии мы захотим добавить еще один класс, который описывает сотрудника предприятия - класс Employee. Так как этот класс реализует тот же функционал, что и класс Person, так как сотрудник - это также и человек, то было бы рационально сделать класс Employee производным (наследником, подклассом) от класса Person, который, в свою очередь, называется базовым классом, родителем или суперклассом:


    class Employee extends Person{
    }

Чтобы объявить один класс наследником от другого, надо использовать после имени класса-наследника ключевое слово extends, после которого идет имя базового класса. Для класса Employee базовым является Person, и поэтому класс Employee наследует все те же поля и методы, которые есть в классе Person.

Использование классов:

    public class Program{

        public static void main(String[] args) {

            Person tom = new Person("Tom");
            tom.display();
            Employee sam = new Employee("Sam");
            sam.display();
        }
    }
    class Person {

        private String name;
        public String getName(){ return name; }

        public Person(String name){

            this.name=name;
        }

        public void display(){

            System.out.println("Name: " + name);
        }
    }
    class Employee extends Person{

    }
Производный класс имеет доступ ко всем методам и полям базового класса (даже если базовый класс находится в другом пакете) кроме тех, которые определены с модификатором private. При этом производный класс также может добавлять свои поля и методы:

    public class Program{

        public static void main(String[] args) {

            Employee sam = new Employee("Sam", "Microsoft");
            sam.display();  // Sam
            sam.work();     // Sam works in Microsoft
        }
    }
    class Person {

        private String name;
        public String getName(){ return name; }

        public Person(String name){

            this.name=name;
        }

        public void display(){

            System.out.println("Name: " + name);
        }
    }
    class Employee extends Person{

        private String company;

        public Employee(String name, String company) {

            super(name);
            this.company=company;
        }
        public void work(){
            System.out.printf("%s works in %s \n", getName(), company);
        }
    }

В данном случае класс Employee добавляет поле company, которое хранит место работы сотрудника, а также метод work.

Если в базовом классе определены конструкторы, то в конструкторе производного классы необходимо вызвать один из конструкторов базового класса с помощью ключевого слова super. Например, класс Person имеет конструктор, который принимает один параметр. Поэтому в классе Employee в конструкторе нужно вызвать констуктор класса Person. После слова super в скобках идет перечисление передаваемых аргументов. Таким образом, установка имени сотрудника делегируется конструктору базового класса.

При этом вызов конструктора базового класса должен идти в самом начале в конструкторе производного класса.

Переопределение методов
Производный класс может определять свои методы, а может переопределять методы, которые унаследованы от базового класса. Например, переопределим в классе Employee метод display:


    public class Program{

        public static void main(String[] args) {

            Employee sam = new Employee("Sam", "Microsoft");
            sam.display();  // Sam
                            // Works in Microsoft
        }
    }
    class Person {

        private String name;
        public String getName(){ return name; }

        public Person(String name){

            this.name=name;
        }

        public void display(){

            System.out.println("Name: " + name);
        }
    }
    class Employee extends Person{

        private String company;

        public Employee(String name, String company) {

            super(name);
            this.company=company;
        }
        @Override
        public void display(){

            System.out.printf("Name: %s \n", getName());
            System.out.printf("Works in %s \n", company);
        }
    }

Перед переопределяемым методом указывается аннотация @Override. Данная аннотация в принципе необязательна.

При переопределении метода он должен иметь уровень доступа не меньше, чем уровень доступа в базовом класса. Например, если в базовом классе метод имеет модификатор public, то и в производном классе метод должен иметь модификатор public.

Однако в данном случае мы видим, что часть метода display в Employee повторяет действия из метода display базового класса. Поэтому мы можем сократить класс Employee:


        private String company;

        public Employee(String name, String company) {

            super(name);
            this.company=company;
        }
        @Override
        public void display(){

            super.display();
            System.out.printf("Works in %s \n", company);
        }
    }

С помощью ключевого слова super мы также можем обратиться к реализации методов базового класса.

Запрет наследования
Хотя наследование очень интересный и эффективный механизм, но в некоторых ситуациях его применение может быть нежелательным. И в этом случае можно запретить наследование с помощью ключевого слова final. Например:

    public final class Person {
    }

Если бы класс Person был бы определен таким образом, то следующий код был бы ошибочным и не сработал, так как мы тем самым запретили наследование:


    class Employee extends Person{ {
    }

Кроме запрета наследования можно также запретить переопределение отдельных методов. Например, в примере выше переопределен метод displayInfo(), запретим его переопределение:

    public class Person {

        //........................

        public final void display(){

            System.out.println("Имя: " + name);
        }
    }

В этом случае класс Employee не сможет переопределить метод display.

Динамическая диспетчеризация методов
Наследование и возможность переопределения методов открывают нам большие возможности. Прежде всего мы можем передать переменной суперкласса ссылку на объект подкласса:

    Person sam = new Employee("Sam", "Oracle");

Так как Employee наследуется от Person, то объект Employee является в то же время и объектом Person. Грубо говоря, любой работник предприятия одновременно является человеком.

Однако несмотря на то, что переменная представляет объект Person, виртуальная машина видит, что в реальности она указывает на объект Employee. Поэтому при вызове методов у этого объектов будет вызывать та версия методов, которая определена в классе Employee, а не в Person. Например:


    public class Program{

        public static void main(String[] args) {

            Person tom = new Person("Tom");
            tom.display();
            Person sam = new Employee("Sam", "Oracle");
            sam.display();
        }
    }
    class Person {

        private String name;

        public String getName() { return name; }

        public Person(String name){

            this.name=name;
        }

        public void display(){

            System.out.printf("Person %s \n", name);
        }
    }

    class Employee extends Person{

        private String company;

        public Employee(String name, String company) {

            super(name);
            this.company = company;
        }
        @Override
        public void display(){

            System.out.printf("Employee %s works in %s \n", super.getName(), company);
        }
    }

Консольный вывод данной программы:

    Person Tom
    Employee Sam works in Oracle

При вызове переопределенного метода виртуального машина динамически находит и вызывает именно ту версию метода, которая определена в подклассе. Данный процесс еще называется dynamic method lookup или динамический поиск метода или динамическая диспетчеризация методов.

### Абстрактные классы

Кроме обычных классов в Java есть абстрактные классы. Абстрактный класс похож на обычный класс. В абстрактном классе также можно определить поля и методы, в то же время нельзя создать объект или экземпляр абстрактного класса. Абстрактные классы призваны предоставлять базовый функционал для классов-наследников. А производные классы уже реализуют этот функционал.

При определении абстрактных классов используется ключевое слово abstract:

    public abstract class Human{

        private String name;

        public String getName() { return name; }
    }
Но главное отличие состоит в том, что мы не можем использовать конструктор абстрактного класса для создания его объекта. Например, следующим образом:


    Human h = new Human();

Кроме обычных методов абстрактный класс может содержать абстрактные методы. Такие методы определяются с помощью ключевого слова abstract и не имеют никакого функционала:

    public abstract void display();

Производный класс обязан переопределить и реализовать все абстрактные методы, которые имеются в базовом абстрактном классе. Также следует учитывать, что если класс имеет хотя бы один абстрактный метод, то данный класс должен быть определен как абстрактный.

Зачем нужны абстрактные классы? Допустим, мы делаем программу для обсулживания банковских операций и определяем в ней три класса: Person, который описывает человека, Employee, который описывает банковского служащего, и класс Client, который представляет клиента банка. Очевидно, что классы Employee и Client будут производными от класса Person, так как оба класса имеют некоторые общие поля и методы. И так как все объекты будут представлять либо сотрудника, либо клиента банка, то напрямую мы от класса Person создавать объекты не будем. Поэтому имеет смысл сделать его абстрактным.


public class Program{

    public static void main(String[] args) {

        Employee sam = new Employee("Sam", "Leman Brothers");
        sam.display();
        Client bob = new Client("Bob", "Leman Brothers");
        bob.display();
    }
}
abstract class Person {

    private String name;

    public String getName() { return name; }

    public Person(String name){

        this.name=name;
    }

    public abstract void display();
}

class Employee extends Person{

    private String bank;

    public Employee(String name, String company) {

        super(name);
        this.bank = company;
    }

    public void display(){

        System.out.printf("Employee Name: %s \t Bank: %s \n", super.getName(), bank);
    }
}

class Client extends Person
{
    private String bank;

    public Client(String name, String company) {

        super(name);
        this.bank = company;
    }

    public void display(){

        System.out.printf("Client Name: %s \t Bank: %s \n", super.getName(), bank);
    }
}
Другим хрестоматийным примером является системы фигур. В реальности не существует геометрической фигуры как таковой. Есть круг, прямоугольник, квадрат, но просто фигуры нет. Однако же и круг, и прямоугольник имеют что-то общее и являются фигурами:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
// абстрактный класс фигуры
abstract class Figure{

    float x; // x-координата точки
    float y; // y-координата точки

    Figure(float x, float y){

        this.x=x;
        this.y=y;
    }
    // абстрактный метод для получения периметра
    public abstract float getPerimeter();
    // абстрактный метод для получения площади
    public abstract float getArea();
}
// производный класс прямоугольника
class Rectangle extends Figure
{
    private float width;
    private float height;

    // конструктор с обращением к конструктору класса Figure
    Rectangle(float x, float y, float width, float height){

        super(x,y);
        this.width = width;
        this.height = height;
    }

    public float getPerimeter(){

        return width * 2 + height * 2;
    }

    public float getArea(){

        return width * height;
    }
}


### Иерархия наследования и преобразование типов

В прошлой главе говорилось о преобразованиях объектов простых типов. Однако с объектами классов все происходит немного по-другому. Допустим, у нас есть следующая иерархия классов:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
public class Program{

    public static void main(String[] args) {

        Person tom = new Person("Tom");
        tom.display();
        Person sam = new Employee("Sam", "Oracle");
        sam.display();
        Person bob = new Client("Bob", "DeutscheBank", 3000);
        bob.display();
    }
}
// класс человека
class Person {

    private String name;

    public String getName() { return name; }

    public Person(String name){

        this.name=name;
    }

    public void display(){

        System.out.printf("Person %s \n", name);
    }
}
// служащий некоторой компании
class Employee extends Person{

    private String company;

    public Employee(String name, String company) {

        super(name);
        this.company = company;
    }
    public String getCompany(){ return company; }

    public void display(){

        System.out.printf("Employee %s works in %s \n", super.getName(), company);
    }
}
// класс клиента банка
class Client extends Person{

    private int sum; // Переменная для хранения суммы на счете
    private String bank;

    public Client(String name, String bank, int sum) {

        super(name);
        this.bank=bank;
        this.sum=sum;
    }

    public void display(){

        System.out.printf("Client %s has account in %s \n", super.getName(), bank);
    }

    public String getBank(){ return bank; }
    public int getSum(){ return sum; }
}
В этой иерархии классов можно проследить следующую цепь наследования: Object (все классы неявно наследуются от типа Object) -> Person -> Employee|Client.

Преобразование типов в языке Java
Суперклассы обычно размещаются выше подклассов, поэтому на вершине наследования находится класс Object, а в самом низу Employee и Client.

Объект подкласса также представляет объект суперкласса. Поэтому в программе мы можем написать следующим образом:

1
2
3
4
5
Object tom = new Person("Tom");
Object sam = new Employee("Sam", "Oracle");
Object kate = new Client("Kate", "DeutscheBank", 2000);
Person bob = new Client("Bob", "DeutscheBank", 3000);
Person alice = new Employee("Alice", "Google");
Это так называемое восходящее преобразование (от подкласса внизу к суперклассу вверху иерархии) или upcasting. Такое преобразование осуществляется автоматически.

Обратное не всегда верно. Например, объект Person не всегда является объектом Employee или Client. Поэтому нисходящее преобразование или downcasting от суперкласса к подклассу автоматически не выполняется. В этом случае нам надо использовать операцию преобразования типов.

1
2
3
4
5
6
Object sam = new Employee("Sam", "Oracle");

// нисходящее преобразование от Object к типу Employee
Employee emp = (Employee)sam;
emp.display();
System.out.println(emp.getCompany());
В данном случае переменная sam приводится к типу Employee. И затем через объект emp мы можем обратиться к функционалу объекта Employee.

Мы можем преобразовать объект Employee по всей прямой линии наследования от Object к Employee.

Примеры нисходящих перобразований:

1
2
3
4
5
Object kate = new Client("Kate", "DeutscheBank", 2000);
((Person)kate).display();

Object sam = new Employee("Sam", "Oracle");
((Employee)sam).display();
Но рассмотрим еще одну ситуацию:

1
2
3
4
5
6
Object kate = new Client("Kate", "DeutscheBank", 2000);
Employee emp = (Employee) kate;
emp.display();

// или так
((Employee)kate).display();
В данном случае переменная типа Object хранит ссылку на объект Client. Мы можем без ошибок привести этот объект к типам Person или Client. Но при попытке преобразования к типу Employee мы получим ошибку во время выполнения. Так как kate не представляет объект типа Employee.

Здесь мы явно видим, что переменная kate - это ссылка на объект Client, а не Employee. Однако нередко данные приходят извне, и мы можем точно не знать, какой именно объект эти данные представляют. Соответственно возникает большая вероятная столкнуться с ошибкой. И перед тем, как провести преобразование типов, мы можем проверить, а можем ли мы выполнить приведение с помощью оператора instanceof:

1
2
3
4
5
6
7
8
9
Object kate = new Client("Kate", "DeutscheBank", 2000);
if(kate instanceof Employee){

    ((Employee)kate).display();
}
else{

    System.out.println("Conversion is invalid");
}
Выражение kate instanceof Employee проверяет, является ли переменная kate объектом типа Employee. Но так как в данном случае явно не является, то такая проверка вернет значение false, и преобразование не сработает.

### Интерфейсы

Механизм наследования очень удобен, но он имеет свои ограничения. В частности мы можем наследовать только от одного класса, в отличие, например, от языка С++, где имеется множественное наследование.

В языке Java подобную проблему частично позволяют решить интерфейсы. Интерфейсы определяют некоторый функционал, не имеющий конкретной реализации, который затем реализуют классы, применяющие эти интерфейсы. И один класс может применить множество интерфейсов.

Чтобы определить интерфейс, используется ключевое слово interface. Например:

1
2
3
4
interface Printable{

    void print();
}
Данный интерфейс называется Printable. Интерфейс может определять константы и методы, которые могут иметь, а могут и не иметь реализации. Методы без реализации похожи на абстрактные методы абстрактных классов. Так, в данном случае объявлен один метод, который не имеет реализации.

Все методы интерфейса не имеют модификаторов доступа, но фактически по умолчанию доступ public, так как цель интерфейса - определение функционала для реализации его классом. Поэтому весь функционал должен быть открыт для реализации.

Чтобы класс применил интерфейс, надо использовать ключевое слово implements:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
public class Program{

    public static void main(String[] args) {

        Book b1 = new Book("Java. Complete Referense.", "H. Shildt");
        b1.print();
    }
}
interface Printable{

    void print();
}
class Book implements Printable{

    String name;
    String author;

    Book(String name, String author){

        this.name = name;
        this.author = author;
    }

    public void print() {

        System.out.printf("%s (%s) \n", name, author);
    }
}
В данном случае класс Book реализует интерфейс Printable. При этом надо учитывать, что если класс применяет интерфейс, то он должен реализовать все методы интерфейса, как в случае выше реализован метод print. Потом в методе main мы можем объект класса Book и вызвать его метод print. Если класс не реализует какие-то методы интерфейса, то такой класс должен быть определен как абстрактный, а его неабстрактные классы-наследники затем должны будут эти методы.

В тоже время мы не можем напрямую создавать объекты интерфейсов, поэтому следующий код не будет работать:

1
2
Printable pr = new Printable();
pr.print();
Одним из преимуществ использования интерфейсов является то, что они позволяют добавить в приложение гибкости. Например, в дополнение к классу Book определим еще один класс, который будет реализовывать интерфейс Printable:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
class Journal implements Printable {

    private String name;

    String getName(){
        return name;
    }

    Journal(String name){

        this.name = name;
    }
    public void print() {
        System.out.println(name);
    }
}
Класс Book и класс Journal связаны тем, что они реализуют интерфейс Printable. Поэтому мы динамически в программе можем создавать объекты Printable как экземпляры обоих классов:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
public class Program{

    public static void main(String[] args) {

        Printable printable = new Book("Java. Complete Reference", "H. Shildt");
        printable.print();      //  Java. Complete Reference (H. Shildt)
        printable = new Journal("Foreign Policy");
        printable.print();      // Foreign Policy
    }
}
interface Printable{

    void print();
}
class Book implements Printable{

    String name;
    String author;

    Book(String name, String author){

        this.name = name;
        this.author = author;
    }

    public void print() {

        System.out.printf("%s (%s) \n", name, author);
    }
}
class Journal implements Printable {

    private String name;

    String getName(){
        return name;
    }

    Journal(String name){

        this.name = name;
    }
    public void print() {
        System.out.println(name);
    }
}
Интерфейсы в преобразованиях типов
Все сказанное в отношении преобразования типов характерно и для интерфейсов. Например, так как класс Journal реализует интерфейс Printable, то переменная типа Printable может хранить ссылку на объект типа Journal:

1
2
3
4
5
Printable p =new Journal("Foreign Affairs");
p.print();
// Интерфейс не имеет метода getName, необходимо явное приведение
String name = ((Journal)p).getName();
System.out.println(name);
И если мы хотим обратиться к методам класса Journal, которые определены не в интерфейсе Printable, а в самом классе Journal, то нам надо явным образом выполнить преобразование типов: ((Journal)p).getName();

Методы по умолчанию
Ранее до JDK 8 при реализации интерфейса мы должны были обязательно реализовать все его методы в классе. А сам интерфейс мог содерать только определения методов без конкретной реализации. В JDK 8 была добавлена такая функциональность как методы по умолчанию. И теперь интерфейсы кроме определения методов могут иметь их реализацию по умолчанию, которая используется, если класс, реализующий данный интерфейс, не реализует метод. Например, создадим метод по умолчанию в интерфейсе Printable:

1
2
3
4
5
6
7
interface Printable {

    default void print(){

        System.out.println("Undefined printable");
    }
}
Метод по умолчанию - это обычный метод без модификаторов, который помечается ключевым словом default. Затем в классе Journal нам необязательно этот метод реализовать, хотя мы можем его и переопределить:

1
2
3
4
5
6
7
8
9
10
11
12
class Journal implements Printable {

    private String name;

    String getName(){
        return name;
    }
    Journal(String name){

        this.name = name;
    }
}
Статические методы
Начиная с JDK 8 в интерфейсах доступны статические методы - они аналогичны методам класса:

1
2
3
4
5
6
7
8
9
interface Printable {

    void print();

    static void read(){

        System.out.println("Read printable");
    }
}
Чтобы обратиться к статическому методу интерфейса также, как и в случае с классами, пишут название интерфейса и метод:

1
2
3
4
public static void main(String[] args) {

    Printable.read();
}
Приватные методы
По умолчанию все методы в интерфейсе фактически имеют модификатор public. Однако начиная с Java 9 мы также можем определять в интерфейсе методы с модификатором private. Они могут быть статическими и нестатическими, но они не могут иметь реализации по умолчанию.

Подобные методы могут использоваться только внутри самого интерфейса, в котором они определены. То есть к примеру нам надо выполнять в интерфейсе некоторые повторяющиеся действия, и в этом случае такие действия можно выделить в приватные методы:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
public class Program{

    public static void main(String[] args) {

        Calculatable c = new Calculation();
        System.out.println(c.sum(1, 2));
        System.out.println(c.sum(1, 2, 4));
    }
}
class Calculation implements Calculatable{

}
interface Calculatable{

    default int sum(int a, int b){
        return sumAll(a, b);
    }
    default int sum(int a, int b, int c){
        return sumAll(a, b, c);
    }

    private int sumAll(int... values){
         int result = 0;
         for(int n : values){
             result += n;
         }
         return result;
    }
}
Константы в интерфейсах
Кроме методов в интерфейсах могут быть определены статические константы:

1
2
3
4
5
6
7
interface Stateable{

    int OPEN = 1;
    int CLOSED = 0;

    void printState(int n);
}
Хотя такие константы также не имеют модификаторов, но по умолчанию они имеют модификатор доступа public static final, и поэтому их значение доступно из любого места программы.

Применение констант:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
public class Program{

    public static void main(String[] args) {

        WaterPipe pipe = new WaterPipe();
        pipe.printState(1);
    }
}
class WaterPipe implements Stateable{

    public void printState(int n){
        if(n==OPEN)
            System.out.println("Water is opened");
        else if(n==CLOSED)
            System.out.println("Water is closed");
        else
            System.out.println("State is invalid");
    }
}
interface Stateable{

    int OPEN = 1;
    int CLOSED = 0;

    void printState(int n);
}
Множественная реализация интерфейсов
Если нам надо применить в классе несколько интерфейсов, то они все перечисляются через запятую после слова implements:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
interface Printable {

    // методы интерфейса
}

interface Searchable {

    // методы интерфейса
}

class Book implements Printable, Searchable{

    // реализация класса
}
Наследование интерфейсов
Интерфейсы, как и классы, могут наследоваться:

1
2
3
4
interface BookPrintable extends Printable{

    void paint();
}
При применении этого интерфейса класс Book должен будет реализовать как методы интерфейса BookPrintable, так и методы базового интерфейса Printable.

Вложенные интерфейсы
Как и классы, интерфейсы могут быть вложенными, то есть могут быть определены в классах или других интерфейсах. Например:

1
2
3
4
5
6
class Printer{
    interface Printable {

        void print();
    }
}
При применении такого интерфейса нам надо указывать его полное имя вместе с именем класса:

1
2
3
4
5
6
7
8
9
10
11
12
public class Journal implements Printer.Printable {

    String name;

    Journal(String name){

        this.name = name;
    }
    public void print() {
        System.out.println(name);
    }
}
Использование интерфейса будет аналогично предыдущим случаям:

1
2
Printer.Printable p =new Journal("Foreign Affairs");
p.print();
Интерфейсы как параметры и результаты методов
И также как и в случае с классами, интерфейсы могут использоваться в качестве типа параметров метода или в качестве возвращаемого типа:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
public class Program{

    public static void main(String[] args) {

        Printable printable = createPrintable("Foreign Affairs",false);
        printable.print();

        read(new Book("Java for impatients", "Cay Horstmann"));
        read(new Journal("Java Dayly News"));
    }

    static void read(Printable p){

        p.print();
    }

    static Printable createPrintable(String name, boolean option){

        if(option)
            return new Book(name, "Undefined");
        else
            return new Journal(name);
    }
}
interface Printable{

    void print();
}
class Book implements Printable{

    String name;
    String author;

    Book(String name, String author){

        this.name = name;
        this.author = author;
    }

    public void print() {

        System.out.printf("%s (%s) \n", name, author);
    }
}
class Journal implements Printable {

    private String name;

    String getName(){
        return name;
    }

    Journal(String name){

        this.name = name;
    }
    public void print() {
        System.out.println(name);
    }
}
Метод read() в качестве параметра принимает объект интерфейса Printable, поэтому в этот метод мы можем передать как объект Book, так и объект Journal.

Метод createPrintable() возвращает объект Printable, поэтому также мы вожем возвратить как объект Book, так и Journal.

Консольный вывод:

Foreign Affairs
Java for impatients (Cay Horstmann)
Java Dayly News
