# Работа с файлами. Исключения.

**Исключение** - это проблема(ошибка) возникающая во время выполнения программы. Исключения могут возникать во многих случаях, например:

* Пользователь ввел некорректные данные.
* Файл, к которому обращается программа, не найден.
* Сетевое соединение с сервером было утеряно во время передачи данных.

**Обработка исключительных ситуаций (exception handling)** — механизм языков программирования, предназначенный для описания реакции программы на ошибки времени выполнения и другие возможные проблемы (исключения), которые могут возникнуть при выполнении программы и приводят к невозможности (бессмысленности) дальнейшей отработки программой её базового алгоритма.

## Синтаксис

В Java есть пять ключевых слов для работы с исключениями:
* **try** - данное ключевое слово используется для отметки начала блока кода, который потенциально может привести к ошибке.
* **catch** - ключевое слово для отметки начала блока кода, предназначенного для перехвата и обработки исключений.
* **finally** - ключевое слово для отметки начала блока кода, которое является дополнительным. Этот блок помещается после последнего блока 'catch'. Управление обычно передаётся в блок 'finally' в любом случае.
* **throw** - служит для генерации исключений.
* **throws** - ключевое слово, которое прописывается в сигнатуре метода, и обозначающее что метод потенциально может выбросить исключение с указанным типом.

Начнем с простого. Допустим мы пишем метод для вычисления площади прямоугольника:


    public static void main(String[] args) {
        System.out.println(getAreaValue(-1, 100));
    }

    public static int getAreaValue(int x, int y){
        if(x < 0 || y < 0) throw new IllegalArgumentException("value of 'x' or 'y' is negative: x="+x+", y="+y);
        return x*y;
    }

Здесь в методе getAreaValue мы бросаем исключение IllegalArgumentException с помощью ключевого слова throw. В данном случае в сигнатуре метода отсутствует throws IllegalArgumentException, это не сделано потому что исключение IllegalArgumentException является не проверяемым, о них мы ещё поговорим.

Общий вид конструкции для "поимки" исключительной ситуации выглядит следующим образом:

    try{
        //здесь код, который потенциально может привести к ошибке
    }
    catch(SomeException e ){ //в скобках указывается класс конкретной ожидаемой ошибки
        //здесь описываются действия, направленные на обработку исключений
    }
    finally{
        //выполняется в любом случае ( блок finnaly не обязателен)
    }

В нашем случае для площади прямоугольника:

    public static void main(String[] args) {

        int result = 0;

        try{
            result = getAreaValue(-1, 100);
        }catch(IllegalArgumentException e){
            Logger.getLogger(NewClass.class.getName()).log(new LogRecord(Level.WARNING, "В метод вычисления площади был передан аргумент с негативным значением!"));
            throw e;
        }
        System.out.println("Result is : "+result);
    }

    public static int getAreaValue(int x, int y){
        if(x < 0 || y < 0) throw new IllegalArgumentException("value of 'x' or 'y' is negative: x="+x+", y="+y);
        return x*y;
    }


Здесь мы поймали IllegalArgumentException и залогировали данное событие. Дело в том что "починить" такую поломку мы не можем, не будем же мы угадывать что хотел пользователь. По этому мы пробрасываем данное исключение дальше с помощью "throw e;". Такое часто можно встретить на серверах приложений(веб-серверах).

## finally

Иногда требуется гарантировать, что определенный участок кода будет выполняться независимо от того, какие исключения были возбуждены и перехвачены. Для создания такого участка кода используется ключевое слово finally. Даже в тех случаях, когда в методе нет соответствующего возбужденному исключению раздела catch, блок finally будет выполнен до того, как управление перейдет к операторам, следующим за разделом try. У каждого раздела try должен быть по крайней мере или один раздел catch или блок finally. Блок finally очень удобен для закрытия файлов и освобождения любых других ресурсов, захваченных для временного использования в начале выполнения метода. Ниже приведен пример класса с двумя методами, завершение которых происходит по разным причинам, но в обоих перед выходом выполняется код раздела finally.

    class FinallyDemo {
        static void procA() {
            try {
                System.out.println("inside procA");
                throw new RuntimeException("demo");
            } finally {
                System.out.println("procA's finally");
            }
        }
        static void procB() {
            try {
                System.out.println("inside procB");
                return;
            } finally {
                System.out.println("procB's finally");
            }
        }
        public static void main(String args[]) {
            try {
                procA();
            } catch (Exception e) {
            }
            procB();
        }
    }

В этом примере в методе procA из-за возбуждения исключения происходит преждевременный выход из блока try, но по пути «наружу» выполняется раздел finally. Другой метод procB завершает работу выполнением стоящего в try-блоке оператора return, но и при этом перед выходом из метода выполняется программный код блока finally. Ниже приведен результат, полученный при выполнении этой программы.

    inside procA
    procA's finally
    inside procB
    procB's finally

## Иерархия исключений

Все классы обрабатывающие ошибки являются наследниками класса** java.lang.Throwable**. Только объекты этого класса или его наследников могут быть "брошены" JVM при возникновении какой-нибудь исключительной ситуации, а также только эти объекты могут быть "брошены" во время выполнения программы с помощью ключевого слова throw.

Рассмотрим основные классы исключений. основные классы исключений

Здесь основными классами являются **Throwable**, **Error**, **Exception** и **RuntimeException**. Именно с ними связанна вся "магия".

Дело в том, что в java есть два типа исключений: checked и unchecked.

* **Checked** исключения, это те, которые должны обрабатываться блоком catch или описываться в сигнатуре метода. **Unchecked** могут не обрабатываться и не быть описанными.
* **Unchecked** исключения в Java - наследованные от RuntimeException, **checked** - от Exception (не включая unchecked).

Пример **unchecked** исключения - NullPointerException, **checked** исключения - IOException

(Вопрос собеседования)
То есть если в вашем коде есть участок который может бросить checked исключение то вы обязаны либо заключить его в конструкцию try/catch либо объявить в сигнатуре метода "throws SomeException" но в данном случае обработка исключения делегируется на уровень выше. В любом случае его нужно будет перехватить. В противном случае программа просто не скомпилируется.
Почему я выделил Throwable, Error, Exception и RuntimeException.
Throwable - проверяемое исключение
Error - не проверяемое
Exception - проверяемое
RuntimeException - не проверяемое

Все остальные исключения наследуют это свойство от данных классов. Например SQLException является наследником Exception и оно проверяемое. NullPointerException - наследник RuntimeException и оно не проверяемое.

Так почему не все исключения являются проверяемыми? Дело в том, что если проверять каждое место, где теоретически может быть ошибка, то ваш код сильно разрастется, станет плохо читаемым. Например в любом месте, где происходит деление чисел, нужно было бы проверять на ArithmeticException, потому что возможно деление на ноль. Эту опцию(пере отлавливать не проверяемые исключения) создатели языка оставили программисту на его усмотрение.

Вернемся к классам исключений:

1. RuntimeException
IndexOutOfBoundsException - выбрасывается, когда индекс некоторого элемента в структуре данных(массив/коллекция) не попадает в диапазон имеющихся индексов.
NullPointerException - ссылка на объект, к которому вы обращаетесь хранит null/
ClassCastException – Ошибка приведения типов. Всякий раз при приведении типов делается проверка на возможность приведения (проверка осуществляется с помощью instanceof.
ArithmeticException - бросается когда выполняются недопустимые арифметические операции, например деление на ноль.

2. Error
ThreadDeath - вызывается при неожиданной остановке потока посредством метода Thread.stop().
StackOverflowError - ошибка переполнение стека. Часто возникает в рекурсивных функциях из-за неправильного условия выхода.
OutOfMemoryError - ошибка переполнения памяти.
Создание своих классов исключений
Хотя встроенные исключения Java обрабатывают большинство частых ошибок, вероятно, вам потребуется создать ваши собственные типы исключений для обработки ситуаций, специфичных для ваших приложений. Это достаточно просто сделать: просто определите подкласс Exception (который, разумеется, является подклассом Throwable). Ваши подклассы не обязаны реализовывать что-либо — важно само их присутствие в системе типов, которое позволит использовать их как исключения.

    public class MyException extends Exception{
        public MyException(Throwable e) {
            initCause(e);
        }
    }
    public class NewClass {
        public static void main(String[] args) throws MyException {

            int result = 0;

            try{
                result = getAreaValue(-1, 100);
            }catch(IllegalArgumentException e){
                Logger.getLogger(NewClass.class.getName()).log(new LogRecord(Level.WARNING, "В метод вычисления площади был передан аргумент с негативным значением!"));
                throw new MyException(e);
            }
            System.out.println("Result is : "+result);
        }

        public static int getAreaValue(int x, int y){
            if(x < 0 || y < 0) throw new IllegalArgumentException("value of 'x' or 'y' is negative: x="+x+", y="+y);
            return x*y;
        }
    }

А вот получившейся стектрейс:

    Exception in thread "main" javatest.MyException
    at javatest.NewClass.main(NewClass.java:29)
    Caused by: java.lang.IllegalArgumentException: value of 'x' or 'y' is negative: x=-1, y=100
    at javatest.NewClass.getAreaValue(NewClass.java:37)
    at javatest.NewClass.main(NewClass.java:26)
    Java Result: 1

## Конструкция try-with-resources

В Java 7 ввели ещё одну конструкцию, призванную упростить жизнь программисту.
Если раньше приходилось писать так:

    static String readFirstLineFromFileWithFinallyBlock(String path)
                                                         throws IOException {
        BufferedReader br = new BufferedReader(new FileReader(path));
        try {
            return br.readLine();
        } finally {
            if (br != null) br.close();
        }
    }

То теперь это можно сделать так:

    static String readFirstLineFromFile(String path) throws IOException {
        try (BufferedReader br =
                       new BufferedReader(new FileReader(path))) {
            return br.readLine();
        }
    }

Но где же закрытие потока br? Дело в том, что в Java SE 7 поток BufferedReader реализует интерфейс java.lang.AutoCloseable и теперь в конструкции try-with-resources нет необходимости закрывать его.

# Работа с файлами

1. Как записывать в файл?

2. Как читать файл?

3. Как обновить файл?

4. Как удалить файл?

Подготовительные работы
Создадим простой проект, не обязательно Maven проект, так как нам не потребуется не каких дополнительных библиотек.

После того как вы создали проект, создайте класс WorkInFile.java и напишите туда стандартную конструкцию main:

public static void main(String[] args){
   //тут будем вызывать наши методы
}

Теперь создадим класс который будет иметь методы для работы с файлами, а назовем его FileWorker.java все методы в нем которые не есть private будут статическими для того чтобы мы получали к ним доступ без экземпляра этого класса.

Как записывать в файл?

В классе FileWorker.java создадим статический метод который будет осуществлять запись в файл и назовем этот метод write(String text; String nameFile):

    public static void write(String fileName, String text) {
        //Определяем файл
        File file = new File(fileName);

        try {
            //проверяем, что если файл не существует то создаем его
            if(!file.exists()){
                file.createNewFile();
            }

            //PrintWriter обеспечит возможности записи в файл
            PrintWriter out = new PrintWriter(file.getAbsoluteFile());

            try {
                //Записываем текст у файл
                out.print(text);
            } finally {
                //После чего мы должны закрыть файл
                out.close();
            }
        } catch(IOException e) {
            throw new RuntimeException(e);
        }
    }



    private static String text = "This new text \nThis new text2\nThis new text3\nThis new text4\n";
    private static String fileName = "C://a.txt";

    public static void main(String[] args) throws FileNotFoundException {

        //Запись в файл
        FileWorker.write(fileName, text);

    }
После чего мы получим новый файл “a.txt” со следующим содержимым:

This new text
This new text2
This new text3
This new text4


2. Как читать файл?

Теперь в классе FileWorker создадим метод для чтения файла, также статический:

    public static String read(String fileName) throws FileNotFoundException {
        //Этот спец. объект для построения строки
        StringBuilder sb = new StringBuilder();

        exists(fileName);

        try {
            //Объект для чтения файла в буфер
            BufferedReader in = new BufferedReader(new FileReader( file.getAbsoluteFile()));
            try {
                //В цикле построчно считываем файл
                String s;
                while ((s = in.readLine()) != null) {
                    sb.append(s);
                    sb.append("\n");
                }
            } finally {
                //Также не забываем закрыть файл
                in.close();
            }
        } catch(IOException e) {
            throw new RuntimeException(e);
        }

        //Возвращаем полученный текст с файла
        return sb.toString();
    }

StringBuilder – в чем разница между обычным String? В том что когда  вы в StringBuilder добавляете текст он не пересоздается, а String пересоздает себя.

Также если файла нет то метод выкинет Exception.

Для проверки на существование файла создадим метод, так как нам еще потребуется эта проверка в следующих методах:

    private static void exists(String fileName) throws FileNotFoundException {
        File file = new File(fileName);
        if (!file.exists()){
            throw new FileNotFoundException(file.getName());
        }
    }

Теперь проверим его:

    private static String text = "This new text \nThis new text2\nThis new text3\nThis new text4\n";
    private static String fileName = "C://a.txt";

    public static void main(String[] args) throws FileNotFoundException {
        //Попытка прочитать несуществующий файл
        FileWorker.read("no_file.txt");

        //Чтение файла
        String textFromFile = FileWorker.read(fileName);
        System.out.println(textFromFile);
    }

В первом случае когда файл не существует мы получим это:

    Exception in thread "main" java.io.FileNotFoundException: no_file.txt
    	at com.devcolibri.tools.FileWorker.read(FileWorker.java:31)

Во втором случае, мы получим содержимое файла в виде строки. (для этого закомментируйте первый случай)

3. Как обновить файл?

Как такого Update для файлов нет, но способ обновить его есть, для этого можно его перезаписать.

Давайте создадим метод update в классе FileWorker:

    public static void update(String nameFile, String newText) throws FileNotFoundException {
        exists(fileName);
        StringBuilder sb = new StringBuilder();
        String oldFile = read(nameFile);
        sb.append(oldFile);
        sb.append(newText);
        write(nameFile, sb.toString());
    }

Тут мы считываем старый файл в StringBuilder после чего добавляем к нему новый текст и записываем опять. Обратите внимание что для этого мы используем наши методы.

В результате обновления файла:

    private static String text = "This new text \nThis new text2\nThis new text3\nThis new text4\n";
    private static String fileName = "C://blog/a.txt";

    public static void main(String[] args) throws FileNotFoundException {
        //Обновление файла
        FileWorker.update(fileName, "This new text");
    }

мы получим следующее содержание файла “a.txt“:

    This new text
    This new text2
    This new text3
    This new text4
    This new text

4. Как удалить файл?

В тот же наш утилитный класс FileWorker добавим метод delete, он будет очень простым так как у объекта File уже есть метод delete():

    public static void delete(String nameFile) throws FileNotFoundException {
        exists(nameFile);
        new File(nameFile).delete();
    }

Проверяем:

    private static String fileName = "C://a.txt";

    public static void main(String[] args) throws FileNotFoundException {
        //Удаление файла
        FileWorker.delete(fileName);
    }

После чего файл будет удален.

Хотя с помощью ранее рассмотренных классов можно записывать текст в файлы, однако они предназначены прежде всего дл работы с бинарными потоками данных, и их возможностей для полноценной работы с текстовыми файлами недостаточно. И для этой цели служат совсем другие классы, которые являются наследниками абстрактных классов Reader и Writer.
