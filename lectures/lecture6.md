# Объектно ориентированное программирование. Классы. объекты



<a name="oop"/>

Объектно ориентированное программирование (ООП) — это метод программирования, при использовании которого главными элементами программ являются объекты. В языках программирования понятие объекта реализовано как совокупность свойств (структур данных, характерных для данного объекта), методов их обработки (подпрограмм изменения их свойств) и событий, на которые данный объект может реагировать и, которые приводят, как правило, к изменению свойств объекта. Объединение данных и свойственных им процедур обработки в одном объекте, называется инкапсуляцией и является одним из важнейших принципов ООП.

Есть четыре основные характеристики ООП:

- [Инкапсуляция(Сокрытие реализации)](#incapsulation)
- [Наследование](#nasledovanie)
- [Полиморфизм(Различные реализации)](#polim)
- Абстракция(Выделение важного)

<a name="classes&objects"/>

## Классы и объекты

Основными понятиями ООП являются **класс** и **объект**.

Говоря о классах можно провести аналогию с биологическим значением этого слова. Класс объединяем различные виды живых организмов, имеющих схожее строение и поведение. Для примера: Бабочка-капустница относится к классу Насекомые.

В контексте ООП можно рассматривать класс как шаблон, описывающий **возможные состояния** и **поведение** объекта.

**Объект**, в контексте ООП, явлется реализацией класса. Именно объект имеет какое либо реальное состояние описанное в классе.

Для примера можно рассмотреть собаку.

Класс **Собака** описывает возможные состояния и поведение:

    Собака :
      состояния :
        имя
        порода
        окраска
        ...
      поведение :
        потребление прищи
        виляние хвостом
        лай
        ...

Объект **Собака** реализует вышеописанный класс:

    Собака :
      состояния :
        имя - RS232
        порода - Дворняга
        окраска - Самая красивая
      поведение :
        потребление прищи - Очень любит сосиски
        виляние хвостом - Частота вилянием хвоста имеет экспоненциальную зависимость от расстония до любимого хозяина
        лай - Вмеру звонкий

<a name="classes"/>

### Классы в Java

Если сравнить программный объект в Java с предметов из реального мира, то они имеют очень схожие характеристики, у них также есть состояние и поведение. Программное состояние хранят в **полях**, а поведение отображается через **методы**.

Пример создания класса в Java, приводится ниже:

    public class Dog{
       String name;
       String breed;
       String color;

       void hungry() {
       }

       void bark() {
       }

       void tailEffect() {

       }
    }

Поля класса : **name**, **breed**, **color**.

Методы : **hungry**, **bark**, **tailEffect**.

### Поля класса

Полем класса называется переменная определённая внутри класса, но не внутри метода. Объявление поля класса в общем случае выглядит следующим образом :

    модификаторы тип имя;

### Методы

Метод представляет из себя набор операторов, которые выполняют опредеоённые действия.
Общее определение методов выглядит следующим образом :

    модификаторы тип возвращаемого значения название (параметры) { тело метода }

### Конструктор класса

При обсуждении вопроса класса, одной из наиболее важных подтем в Java является конструктор. Каждый класс имеет конструктор. Если мы не напишем его или, например, забудем, компилятор создаст его по умолчанию для этого класса.

Каждый раз, когда в Java создается новый объект, будет вызываться по меньшей мере один конструктор. Главное правило является то, что они должны иметь то же имя, что и класс, который может иметь более одного конструктора.

Добавим конструтор в наш класс собаки.

    public class Dog{
       String name;
       String breed;
       String color;

       public Dog(String name) {
          this.name = name;
       }

       void hungry() {
       }

       void bark() {
       }

       void tailEffect() {

       }
    }

### Создание объекта

Для создания экземпляра класса в Java используется оператор **new**.

    public class Dog{

       public Dog(String name){
          System.out.println("Передаваемое имя:" + name );
       }

       public static void main(String []args){
          // Создание объекта myPuppy.
          Dog myDog = new Dog( "RS232" );
       }
    }

### Доступ к переменным экземпляра и методам в Java

После получени объекта класса мы моем полуить доступ к его переменным и методам. Для получения доступа к переменным и методам используется оператор '**.**' .

    /* Сначала создайте объект */
    Dog dog = new Dog("RS232");

    /* Теперь вызовите переменную следующим образом */
    dog.name;

    /* Теперь Вы можете вызвать метод класса */
    dog.bark();

### Java пакет (package)
При разработке приложений сотни классов и интерфейсов будет написано, поэтому категоризации этих классов является обязательным, а также это делает жизнь намного проще.

### Операторы импорта (import)
Если задать полное имя, которое включает в себя пакет и имя класса, то компилятор может легко найти исходный код или классы. В Java импорт это способ задать правильное место для компилятора, чтобы найти конкретный класс.

Например, следующая строка будет просить компилятор загрузить все классы, доступные в каталоге «java_installation/java/io»:

    import java.io.*;

### Правила объявления классов, операторов импорта и пакетов в исходном файле
Поле того что мы узнали что такое объект и класс, научились создавать свои классы и получать их экземпляры необходимо рассмотреть правила декларации исходного файла. Эти правила в Java имеют важное значение при объявлении классов, операторов импорта и операторов пакета в исходном файле.

- **В исходном файле может быть только один публичный класс (public class);**
- **Исходный файл может иметь несколько "непубличных" классов**
- **Название публичного класса должно совпадать с именем исходного файла, который должен иметь расширение .java в конце. Например: имя класса public class Dog{}, то исходный файл должен быть Dog.java;**
- **Если класс определен внутри пакета, то оператор пакет должно быть первым оператором в исходном файле.**
- **Если присутствуют операторы импорта, то они должны быть написаны между операторами пакета и объявлением класса. Если нет никаких операторов пакета, то оператор импорта должен быть первой строкой в исходном файле.**


## Модификаторы доступа и инкапсуляция

Все члены класса в языке Java - поля и методы, свойства - имеют модификаторы доступа. Выше мы уже сталкивались с модификатором public. Модификаторы доступа позволяют задать допустимую область видимости для членов класса, то есть контекст, в котором можно употреблять данную переменную или метод.

В Java используются следующие модификаторы доступа:

 - **public**: публичный, общедоступный класс или член класса. Поля и методы, объявленные с модификатором public, видны другим классам из текущего пакета и из внешних пакетов.

- **private**: закрытый класс или член класса, противоположность модификатору public. Закрытый класс или член класса доступен только из кода в том же классе.

- **protected**: такой класс или член класса доступен из любого места в текущем классе или пакете или в производных классах, даже если они находятся в других пакетах

- **default (package-private).** Модификатор по умолчанию. Отсутствие модификатора у поля или метода класса предполагает применение к нему модификатора по умолчанию. Такие поля или методы видны всем классам в текущем пакете.

      public class Program{

          public static void main(String[] args) {

              Person kate = new Person("Kate", 32, "Baker Street", "+12334567");
              kate.displayName();     // метод public
              kate.displayAge();      // метод имеет модификатор по умолчанию
              kate.displayPhone();    // метод protected
              //kate.displayAddress();  // ошибка, метод private

              System.out.println(kate.name);      // модификатор по умолчанию
              System.out.println(kate.address);   // модификатор public
              System.out.println(kate.age);       // модификатор protected
              //System.out.println(kate.phone);   // ошибка, модификатор private
          }
      }
      class Person{

          String name;
          protected int age;
          public String address;
          private String phone;

          public Person(String name, int age, String address, String phone){
              this.name = name;
              this.age = age;
              this.address = address;
              this.phone = phone;
          }
          public void displayName(){
              System.out.printf("Name: %s \n", name);
          }
          void displayAge(){
              System.out.printf("Age: %d \n", age);
          }
          private void displayAddress(){
              System.out.printf("Address: %s \n", address);
          }
          protected void displayPhone(){
              System.out.printf("Phone: %s \n", phone);
          }}


В данном случае оба класса расположены в одном пакете - пакете по умолчанию, поэтому в классе Program мы можем использовать все методы и переменные класса Person, которые имеют модификатор по умлчанию, public и protected. А поля и методы с модификатором private в классе Program не будут доступны.

Если бы класс Program располагался бы в другом пакете, то ему были бы доступны только поля и методы с модификатором public.

**Модификатор доступа должен предшествовать остальной части определения переменной или метода.**

<a name="incapsulation"/>
### Инкапсуляция

Казалось бы, почему бы не объявить все переменные и методы с модификатором **public**, чтобы они были доступны в любой точке программы вне зависимости от пакета или класса? Возьмем, например, поле age, которое представляет возраст. Если другой класс имеет прямой доступ к этому полю, то есть вероятность, что в процессе работы программы ему будет передано некорректное значение, например, отрицательное число. Подобное изменение данных не является желательным. Либо же мы хотим, чтобы некоторые данные были достуны напрямую, чтобы их можно было вывести на консоль или просто узнать их значение. В этой связи рекомендуется как можно больше ограничивать доступ к данным, чтобы защитить их от нежелательного доступа извне (как для получения значения, так и для его изменения). Использование различных модификаторов гарантирует, что данные не будут искажены или изменены не надлежащим образом. Подобное сокрытие данных внутри некоторой области видимости называется инкапсуляцией.

Так, как правило, вместо непосредственного использования полей, как правило, используют методы доступа. Например:

    public class Program{

        public static void main(String[] args) {

            Person kate = new Person("Kate", 30);
            System.out.println(kate.getAge());      // 30
            kate.setAge(33);
            System.out.println(kate.getAge());      // 33
            kate.setAge(123450);
            System.out.println(kate.getAge());      // 33
        }
    }

    class Person{

        private String name;
        private int age;

        public Person(String name, int age){
            this.name = name;
            this.age = age;
        }
        public String getName(){
            return this.name;
        }
        public void setName(String name){
            this.name = name;
        }
        public int getAge(){
            return this.age;
        }
        public void setAge(int age){
            if(age > 0 && age < 110)
                this.age = age;
        }
    }

И затем вместо непосредственной работы с полями name и age в классе Person мы будем работать с методами, которые устанавливает и возвращают значения этих полей. Методы setName, setAge и наподобие еще называют мьютейтерами (mutator), так как они изменяют значения поля. А методы getName, getAge и наподобие называют аксессерами (accessor), так как с их помощью мы получаем значение поля.

Причем в эти методы мы можем вложить дополнительную логику. Например, в данном случае при изменении возраста производится проверка, насколько соответствует новое значение допустимому диапазону.


### Статические члены и модификатор static

Кроме обычных методов и полей класс может иметь статические поля, методы, константы и инициализаторы. Например, главный класс программы имеет метод main, который является статическим:

    public static void main(String[] args) {

    }

Для объявления статических переменных, констант, методов и инициализаторов перед их объявлением указывается ключевое слово **static**.

### Статические поля

При создании объектов класса для каждого объекта создается своя копия нестатических обычных полей. А статические поля являются общими для всего класса. Поэому они могут использоваться без создания объектов класса.

Например, создадим статическую переменную:

    public class Program{

        public static void main(String[] args) {

            Person tom = new Person();
            Person bob = new Person();

            tom.displayId();    // Id = 1
            bob.displayId();    // Id = 2
            System.out.println(Person.counter); // 3

            // изменяем Person.counter
            Person.counter = 8;

            Person sam = new Person();
            sam.displayId();    // Id = 8
        }
    }

    class Person{

        private int id;
        static int counter=1;

        Person(){
            id = counter++;
        }
        public void displayId(){

            System.out.printf("Id: %d \n", id);
        }
    }


Класс Person содержит статическую переменную counter, которая увеличивается в конструкторе и ее значение присваивается переменной id. То есть при создании каждого нового объекта Person эта переменная будет увеличиваться, поэтому у каждого нового объекта Person значение поля id будет на 1 больше чем у предыдущего.

Так как переменная counter статическая, то мы можем обратиться к ней в программе по имени класса:

    System.out.println(Person.counter); // получаем значение
    Person.counter = 8;                 // изменяем значение

Консольный вывод программы:

    Id = 1
    Id = 2
    Id = 8

### Статические константы
Также статическими бывают константы, которые являются общими для всего класса.

    public class Program{

        public static void main(String[] args) {

            double radius = 60;
            System.out.printf("Radisu: %f \n", radius);             // 60
            System.out.printf("Area: %f \n", Math.PI * radius);     // 188,4
        }
    }
    class Math{
        public static final double PI = 3.14;
    }

Стоит отметить, что на протяжении всех предыдущих тем уже активно использовались статические константы. В частности, в выражении:

    System.out.println("hello");

out как раз представляет статическую константу класса System. Поэтому обращение к ней идет без создания объекта класса System.

### Статические инициализаторы

Статические инициализаторы предназначены для инициализации статических переменных, либо для выполнения таких действий, которые выполняются при создании самого первого объекта. Например, определим статический инициализатор:

    public class Program{

        public static void main(String[] args) {

            Person tom = new Person();
            Person bob = new Person();

            tom.displayId();    // Id = 105
            bob.displayId();    // Id = 106
        }
    }

    class Person{

        private int id;
        static int counter;

        static{
            counter = 105;
            System.out.println("Static initializer");
        }
        Person(){
            id=counter++;
            System.out.println("Constructor");
        }
        public void displayId(){

            System.out.printf("Id: %d \n", id);
        }
    }

Статический инициализатор определяется как обычный, только перед ним ставится ключевое слово **static**. В данном случае в статическом инициализаторе мы устанавливаем начальное значение статического поля counter и выводим на консоль сообщение.

В самой программе создаются два объекта класса Person. Поэтому консольный вывод будет выглядеть следующим образом:

    Static initializer
    Constructor
    Constructor
    Id: 105
    Id: 106

Стоит учитывать, что вызов статического инициализатора производится только перед созданием самого первого объекта класса.

### Статические методы

Статические методы также относятся ко всему классу в целом. Например, в примере выше статическая переменная counter была доступна извне, и мы могли изменить ее значение вне класса Person. Сделаем ее недоступной для изменения извне, но доступной для чтения. Для этого используем статический метод:

    public class Program{

        public static void main(String[] args) {

            Person.displayCounter();    // Counter: 1

            Person tom = new Person();
            Person bob = new Person();

            Person.displayCounter();    // Counter: 3
        }
    }
    class Person{

        private int id;
        private static int counter = 1;

        Person(){
            id = counter++;
        }
        // статический метод
        public static void displayCounter(){

            System.out.printf("Counter: %d \n", counter);
        }
        public void displayId(){

            System.out.printf("Id: %d \n", id);
        }
    }
Теперь статическая переменная недоступна извне, она приватная. А ее значение выводится с помощью статического метода displayCounter. Для обращения к статическому методу используется имя класса:

    Person.displayCounter().

При использовании статических методов надо учитывать ограничения: в статических методах мы можем вызывать только другие статические методы и использовать только статические переменные.

Вообще методы определяются как статические, когда методы не затрагиют состояние объекта, то есть его нестатические поля и константы, и для вызова метода нет смысла создавать экземпляр класса. Например:

    public class Program{

        public static void main(String[] args) {

            System.out.println(Operation.sum(45, 23));          // 68
            System.out.println(Operation.subtract(45, 23));     // 22
            System.out.println(Operation.multiply(4, 23));      // 92
        }
    }
    class Operation{

        static int sum(int x, int y){
            return x + y;
        }
        static int subtract(int x, int y){
            return x - y;
        }
        static int multiply(int x, int y){
            return x * y;
        }
    }
В данном случае для методов sum, subtract, multiply не имеет значения, какой именно экземпляр класса Operation используется. Эти методы работают только с параметрами, не затрагивая состояние класса. Поэтому их можно определить как статические.

### Объекты как параметры методов

Объекты классов, как и данные примитивных типов могут передаваться в методы. Однако в данном случае есть одна особенность - при передаче объектов в качестве значения передается копия ссылки на область в памяти, где рассположен этот объект. Рассмотрим небольшой пример. Пусть у нас есть следующий класс Person:

    public class Program{

        public static void main(String[] args) {

            Person kate = new Person("Kate");
            System.out.println(kate.getName());     // Kate
            changeName(kate);
            System.out.println(kate.getName());     // Alice
        }
        static void changeName(Person p){
            p.setName("Alice");
        }
    }

    class Person{

        private String name;

        Person(String name){
            this.name = name;
        }
        public void setName(String name){
            this.name = name;
        }
        public String getName(){

            return this.name;
        }
    }

Здесь в метод changeName передается объект Person, у которого изменяется имя. Так как в метод будет передаваться копия ссылки на область памяти, в которой находится объект Person, то переменная kate и параметр p метода changeName будут указывать на один и тот же объект в памяти. Поэтому после выполнения метода у объекта kate, который передается в метод, будет изменено имя с "Kate" на "Alice".

От этого случая следует отличать другой случай:

    public class Program{

        public static void main(String[] args) {

            Person kate = new Person("Kate");
            System.out.println(kate.getName());     // Kate
            changePerson(kate);
            System.out.println(kate.getName());     // Kate - изменения не произошло
                                                    // kate хранит ссылку на старый объект
        }
        static void changePerson(Person p){
            p = new Person("Alice");    // p указывает на новый объект
            p.setName("Ann");
        }
        static void changeName(Person p){
            p.setName("Alice");
        }
    }
    class Person{

        private String name;

        Person(String name){
            this.name = name;
        }
        public void setName(String name){
            this.name = name;
        }
        public String getName(){

            return this.name;
        }
    }

В метод changePerson также передается копия ссылки на объект Person. Однако в самом методе мы изменяем не отдельные значения объекта, а пересоздаем объект с помощью конструктора и оператора new. В результате в памяти будет выделено новое место для нового объекта Person, и ссылка на этот объект будет привоена параметру p:

    static void changePerson(Person p){
        p = new Person("Alice");    // p указывает на новый объект
        p.setName("Ann");           // изменяется новый объект
    }

То есть после создания нового объекта Person параметр p и переменная kate в методе main будут хранить ссылки на разные объекты. Переменная kate, которая передавалась в метод, продолжит хранить ссылку на старый объект в памяти. Поэтому ее значение не меняется


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


<a name="nasledovanie"/>

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

<a name="polim"/>

### Полиморфизм

### Переопределение методов

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
