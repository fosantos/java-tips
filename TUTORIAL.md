# Cracking the Java Coding Interview — Complete Tutorial

> **396 questions + 3 special episodes**, transcribed from the 399 short videos of the **cours-en-ligne** channel ([original playlist](https://www.youtube.com/playlist?list=PLzzeuFUy_Cngn3JZEXtu6G923y5v8y-8h)).
>
> Each video follows the same spoken pattern — **"Question number N… Short answer… Less short answer…"** — and this tutorial reproduces that structure: the question in the title, the *short answer* (often with the presenter's humor), the *less short answer* (the full explanation), and, when there is code in the video, an equivalent snippet.
>
> **How to use it:** each entry carries the link to the video and the number of the local file in the `Cracking the Java Coding Interview` folder (for instance, `local file 042` = `042 - ....mp4`). The questions are in the order **spoken** in the video ("Question number…"), which differs from the folder order.
>
> **About the numbering:** the channel itself numbered two different questions with the number **96** and two with **399** (marked here as A/B); the numbers **1, 264, 394, 396 and 397** have no video in this playlist; and three videos are special episodes with no number (the 300-episode celebration, a JavaOne promo, and an ad episode), listed at the end.
>
> *Generated in October 2026 from the automatic transcription (faster-whisper) of the video audio, reviewed and edited; the code snippets were reconstructed from the narration and the video frames. A Portuguese (pt-BR) version is available as `TUTORIAL-pt-BR.md`.*


## The 396 questions, in spoken order

### Question 2 — What is the Object class in Java?
· [video](https://youtu.be/Nf1tSgTdI1Y) · local file `398`

**Short answer:** The superclass of every class you can create.

**Less short answer:** When you create a class, you can explicitly extend another one with `extends` — you can even declare `extends Object`, which is **valid**... and dumb, because it is unnecessary. Remember: in Java a class extends only **one** class — multiple inheritance does not exist. If you declare no extension at all (the norm), the compiler makes your class extend **Object** for you. In the end, **every** Java class extends Object.

```java
class A { }                    // compiler: class A extends Object { }
class B extends Object { }     // legal... and unnecessary
// class C extends A, B { }    // ERROR: one superclass only

Object o = anything;           // universal polymorphism
```

---

### Question 3 — Can you cite methods from the Object class?
· [video](https://youtu.be/3EcM5OA_5YA) · local file `397`

**Short answer:** toString, equals and hashCode.

**Less short answer:** There are more. **`getClass`** returns the class of the object. **`wait`, `notify` and `notifyAll`** — used in concurrent programming; **never** call them unless you really know what you are doing. **`finalize`**: do not override it anymore — deprecated, and whatever you put in it is, most of the time, buggy. And the last one: **`clone`** — stay away from it too. In a nutshell: toString, equals and hashCode are the most used.

```java
Object o = "text";
o.getClass();      // class java.lang.String
o.toString();      // "text"
o.equals("text");  // true
o.hashCode();
// wait()/notify()/notifyAll(): monitors — experts only
// finalize(): deprecated; clone(): avoid
```

---

### Question 4 — What are the differences between a String and an array of chars?
· [video](https://youtu.be/cxoJ8pUL-Ao) · local file `396`

**Short answer:** A String is an object full of useful methods; an array only has the ones from Object — not very useful.

**Less short answer:** A String is **non-modifiable**: once created, it never changes. The **size** of an array is fixed — once created, you cannot add elements (only replace one with another). How do you compare them? For strings, just use their `equals`. Comparing an array with its `equals` does **not** do what you expect — use the `equals` from the **Arrays** factory class; the same goes for `toString`: use `Arrays.toString` to print an array properly. One last word: you can create a String from an array of chars with `new String(array)`.

```java
char[] cs = {'j', 'a', 'v', 'a'};
String s = new String(cs);            // char[] -> String

cs.equals(otherCs);                   // references: NOT what you expect
Arrays.equals(cs, otherCs);           // content: correct!
System.out.println(cs);               // prints the chars (special case!)
Arrays.toString(cs);                  // [j, a, v, a]
```

---

### Question 5 — How do you sum the elements of an array?
· [video](https://youtu.be/hkpNt37nauw) · local file `395`

**Short answer:** Two solutions: an old-fashioned one everyone knows — and a great one.

**Less short answer:** The first one: iterate over the array, adding the elements to a sum variable — do not forget to **initialize it to 0** — and you are done. The second one: create a **stream** from your array and just call `sum()` — the result is the same. And, by the way, on a stream you can also call `min()`, `max()` and `average()`. Yes — this is the presenter's favorite: simpler, cleaner, more readable.

```java
int[] values = {1, 2, 3, 4};

int total = 0;                       // 1) classic: don't forget the zero!
for (int v : values) total += v;

int total2 = Arrays.stream(values)   // 2) favorite: clean and readable
                  .sum();
double avg = Arrays.stream(values).average().orElse(0);
```

---

### Question 6 — What is the JVM?
· [video](https://youtu.be/lRib0qexrOc) · local file `394`

**Short answer:** Java Virtual Machine.

**Less short answer:** The piece of software that runs your Java code — and it does much more than run your application. Among many other things, it handles the **memory** of your application (with the garbage collector), it **optimizes** the running code (with the Just-In-Time compiler, the JIT), and it handles the **security**, making sure no undesired code can sneak in and be executed on your behalf. One last word: if someone talks to you about **HotSpot**, that is the name of the open-source JVM developed by the folks at the **OpenJDK**.

```java
// the JVM in one diagram:
// .class (bytecode) -> loading/verification (security)
//                   -> interpretation -> JIT (C1/C2) = native speed
//                   -> heap + GC = automatic memory
java -XX:+UseZGC MyApp    // you choose the GC; HotSpot is the JVM
```

---

### Question 7 — How can you sort an array?
· [video](https://youtu.be/TOAhbWzSk2I) · local file `393`

**Short answer:** There is a method for that: `Arrays.sort`. Pass your array to it and that's it.

**Less short answer:** When you need to sort something, forget about writing a sorting algorithm yourself. What about the best sorting algorithm? That is actually a tough question — but you do not need to worry about it, because `Arrays.sort` does the job for you. Now, if you really want to shine in an interview, you can cite three algorithms: **quicksort**, **heapsort** and **Timsort**. The one used by `Arrays.sort` (for primitives) is called **dual-pivot quicksort**. One last word: never cite **bubble sort**... unless you want to say that it is a terrible algorithm.

```java
int[] n = {5, 1, 4, 2, 3};
Arrays.sort(n);                 // [1, 2, 3, 4, 5] dual-pivot quicksort
Arrays.sort(n, 1, 4);           // sorts only the range [1, 4)

String[] s = {"b", "a"};
Arrays.sort(s, Comparator.reverseOrder()); // objects: Timsort (stable)
```

---

### Question 8 — What is the main characteristic of the String class?
· [video](https://youtu.be/9fV8NQymLfA) · local file `392`

**Short answer:** This class is non-modifiable.

**Less short answer:** There is no way to modify an instance of the String class once it has been created. So if you call `toUpperCase`, it does not change the string — it **creates a new one** holding the result. If you really want a string of characters you can modify, there is a specific class for that: **`StringBuilder`** — which is not a String; it actually wraps an array of chars/bytes whose content you can modify, and you can create a String from it by calling `toString()`. One last word: use `+` to concatenate strings. Forget the old "new StringBuilder on every line" — it is useless... most of the time.

```java
String s = "java";
s.toUpperCase();        // does NOT change s; returns a NEW string
s = s.toUpperCase();    // this works (reassignment)

StringBuilder sb = new StringBuilder("java");
sb.append("!").insert(0, "> ");   // modified in place
sb.toString();                    // "> JAVA!"
```

---

### Question 9 — What is the difference between == and equals()?
· [video](https://youtu.be/YnDcuYr2284) · local file `391`

**Short answer:** `==` compares references; `equals()` calls a method.

**Less short answer:** If you compare strings with `==`, you may get `false` even if the strings hold the same value. In a nutshell: `==` is good to compare things that are **not objects** — ints, longs and the like. You should **never** compare floats or doubles with `==`: what you should do is compute the **absolute value of the difference** and check whether it is smaller than a very small value. One last thing: everything (object-wise) is a reference in Java, so most of the time calling `==` on objects is **wrong**.

```java
String a = "x", b = new String("x");
a == b;          // false (different references)
a.equals(b);     // true

int i = 1;  i == 1;                // OK: primitive
// double d = 0.1 + 0.2;
// d == 0.3;                       // false! never == with double:
Math.abs(0.1 + 0.2 - 0.3) < 1e-9;  // true: this is the way
```

---

### Question 10 — What is the relationship between equals() and hashCode()?
· [video](https://youtu.be/RIynixWWkXk) · local file `390`

**Short answer:** The Java specification says: if two objects are **equal**, then they should have the **same hashCode**.

**Less short answer:** It means that when you override `equals`, you should **always** override `hashCode` as well. The spec says two objects that are equal must have the same hashCode — but the other way around is **not** true: you can have two different objects with the same hashCode (the collision). One last thing: always use your IDE to write these methods. First, your IDE will not forget to override both; and second, it will give you a better implementation than the one you will write... well, most of the time. *(Records generate both.)*

```java
record Point(int x, int y) { }   // consistent equals + hashCode, free

// the contract:
// a.equals(b) == true  -> a.hashCode() == b.hashCode()  (MANDATORY)
// same hashCodes       -> equal OR collision (equals decides)
// breaking the contract -> HashSet/HashMap "lose" your objects
```

---

### Question 11 — How can you duplicate an array?
· [video](https://youtu.be/Go940nyLzkA) · local file `389`

**Short answer:** Two solutions: `Arrays.copyOf` and `System.arraycopy`.

**Less short answer:** There is a `clone()` method on arrays — internally it returns an `Object` that is cast, so performance-wise it is not ideal. `System.arraycopy` needs a **destination** array to copy your source into. `Arrays.copyOf` **truncates** your source array if it is bigger than the destination length you ask for, or pads it with zeros if it is smaller; `System.arraycopy` has a richer API — it can copy a **portion** of your source array into the destination. One last word: `Arrays.copyOf` calls `System.arraycopy` internally — so use whichever you prefer.

```java
int[] a = {1, 2, 3, 4, 5};

int[] b = Arrays.copyOf(a, 3);        // [1, 2, 3] (truncates)
int[] c = Arrays.copyOf(a, 7);        // [1..5, 0, 0] (pads with zeros)
int[] d = new int[2];
System.arraycopy(a, 1, d, 0, 2);      // [2, 3] (a portion, rich API)
int[] e = a.clone();                  // works (internal cast: less ideal)
```

---

### Question 12 — How can you reverse a string?
· [video](https://youtu.be/NQ2rwWWqDzU) · local file `388`

**Short answer:** Wrap it in a StringBuilder and call `reverse`.

**Less short answer:** The String class is non-modifiable, so you cannot change what the string is — all you can do is create **another** string that is the reverse of the first one. Looping over the letters of your string to reverse it is actually what the `reverse()` method of StringBuilder does — so do not bother reinventing the wheel; it is there for you to use. StringBuilder is a modifiable class, so the letters are reversed internally. One last word: to get the reversed string, you just need to call `toString()` on your StringBuilder.

```java
String s = "java";
String reversed = new StringBuilder(s).reverse().toString(); // "avaj"

// manual equivalent (what reverse does for you):
char[] cs = s.toCharArray();
for (int i = 0, j = cs.length - 1; i < j; i++, j--) {
    char t = cs[i]; cs[i] = cs[j]; cs[j] = t;
}
```

---

### Question 13 — What is a functional interface?
· [video](https://youtu.be/ZsiFNxiqCcw) · local file `387`

**Short answer:** An interface with only one abstract method.

**Less short answer:** Functional interfaces can be implemented with **lambda expressions**. Java is a statically typed language, so all your variables have a type, and all these types are known at compile time — and the type of a lambda is **always** a functional interface, no exception. A functional interface can contain any number of **default** or **static** methods, and it can have any method from the Object class as abstract methods — but only **one** abstract method of its own: the one your lambda has to implement. One last word: add the `@FunctionalInterface` annotation and the compiler will tell you whether you got it right.

```java
@FunctionalInterface
interface Validator<T> {
    boolean valid(T t);               // the ONLY abstract method

    default Validator<T> and(Validator<T> other) {  // default: as many
        return t -> valid(t) && other.valid(t);     // as you want
    }
    static <T> Validator<T> always() { return t -> true; }
    boolean equals(Object o);         // Object doesn't count!
}
Validator<String> v = s -> !s.isBlank();
```

---

### Question 14 — What is the difference between overriding and overloading?
· [video](https://youtu.be/yUqNZcNj36Q) · local file `386`

**Short answer:** Overriding has to do with inheritance; overloading does not.

**Less short answer:** **Overloading** is the same method with different parameters. **Overriding** is the redefinition of a method from a superclass in a subclass. When your code calls a method that has several overloads, the **compiler** decides which method to call — so overloading is resolved at **compile time**: this is called early binding. Overriding is not: the compiler cannot resolve the call — it is resolved at runtime: late binding. One last word: you can prevent a method from being overridden by declaring it `final`; you cannot prevent a method from having different overloads.

```java
class A {
    void m(int x) { }        // overloads: the compiler chooses
    void m(double x) { }     // (early binding)
    void m() { }             // overridable: runtime decides (late)
}
class B extends A {
    @Override void m() { }   // overriding: same signature
    // final in the superclass would block the override; never the overload
}
```

---

### Question 15 — What is a map?
· [video](https://youtu.be/u8ujMXUM90Q) · local file `385`

**Short answer:** An object that maps keys to values.

**Less short answer:** `Map` is an interface from the Collections framework; its canonical implementation is the **HashMap**. You cannot have duplicates among the keys: a given key maps to a **single** value — there is no "multi-map" concept in the framework; if you need one, map the key to a **list** of values. Use **non-modifiable objects** for your keys — small strings are enough, or longs — prefer them over integers, even for small values. One last word: the HashMap class supports null keys and null values... but please don't do that. It will put you in trouble.

```java
Map<String, List<Order>> byCustomer = new HashMap<>();
byCustomer.computeIfAbsent("ana", k -> new ArrayList<>()).add(order);
// a hand-rolled "multimap": key -> list

Map<Long, User> byId = new HashMap<>();  // long > integer for keys
```

---

### Question 16 — What is an ArrayList?
· [video](https://youtu.be/vJUDlDq82kE) · local file `384`

**Short answer:** An implementation of the List interface backed by an array.

**Less short answer:** An ArrayList is sometimes called a *dynamic array* because when the internal array becomes full, it is transparently copied into a larger array. Be careful though: this array can only **grow** — it never shrinks. So a large ArrayList in your application can eat up a lot of memory. One last thing: ArrayList is your best choice for a list implementation — forget about LinkedList (LinkedLists are good for stacks; we will talk about that another time).

```java
List<String> l = new ArrayList<>();
l.add("a");   // internal array full? copied to a bigger one (~50% growth)
              // it NEVER shrinks on its own: l.trimToSize() if needed

// ArrayList: get O(1), add at the end amortized O(1)
// LinkedList: forget it (except as a deque)
```

---

### Question 17 — Explain finally
· [video](https://youtu.be/6OuqwJAWPkM) · local file `383`

**Short answer:** `finally` marks a block after a try block that is **always** executed when the try block exits.

**Less short answer:** Always means: even if there is a `return`, a `continue`, a `break` or an exception causing the try block to exit. The finally block is the key tool to properly **release resources** that are not AutoCloseable — for AutoCloseable resources, your best choice is the **try-with-resources** statement, which takes care of closing them for you. Non-AutoCloseable resources should be released in a finally block — that is the case for **ReentrantLocks**, for instance, or for **semaphores**. One last word: in case of a `return` in the try block, the finally block is evaluated **after** the return.

```java
lock.lock();
try {
    process();             // even with return/continue/exception...
} finally {
    lock.unlock();         // ...this ALWAYS runs
}

int test() {
    try { return 1; }
    finally { System.out.println("runs BEFORE the return!"); }
}
```

---

### Question 18 — How to make a class immutable?
· [video](https://youtu.be/GBWFd77J9EE) · local file `382`

**Short answer:** Use records if you can.

**Less short answer:** If you cannot use a record: make all the fields of the class **final** — and do not forget to make the **class** final too, because if you don't, someone could add some mutable state to it using inheritance and pass it around using polymorphism. Do not forget **defensive copies** of the modifiable objects you get as arguments in your constructor (lists, maps and the like) — and the same for your accessors: make sure they do not **leak** references to modifiable internal state; use a defensive copy wherever you need one. One last thing: String, Integer, Long and all the wrapper classes of the JDK are non-modifiable — you can check them to see how they work.

```java
public final class ClassGroup {               // final class!
    private final List<String> students;      // final fields
    public ClassGroup(List<String> students) {
        this.students = List.copyOf(students); // defensive copy (in)
    }
    public List<String> students() {
        return students;                       // already immutable: no leak
    }
}
// and the short path: record ClassGroup(List<String> students) { } + copy
```

---

### Question 19 — What is the difference between String, StringBuffer and StringBuilder?
· [video](https://youtu.be/bbjitAm5xB0) · local file `381`

**Short answer:** String is non-modifiable; StringBuilder is modifiable; StringBuffer is thread-safe.

**Less short answer:** StringBuffer is rarely used. StringBuilder is nice for certain string manipulations — like **reversing** a string or inserting a character. Should you use StringBuilder to **concatenate** strings? Most of the time, **no**: on Java 9 and later, string concatenation with `+` has been optimized; and on Java 8, in many cases concatenation with `+` was in fact compiled with StringBuilder under the hood. One last word: if you are joining string elements from a list or an array, consider using `String.join`, the StringJoiner class, or `Collectors.joining`.

```java
String s = "a" + b + c;             // Java 9+: already optimized (invokedynamic)
new StringBuilder(s).reverse();     // manipulation: builder is right here
String.join(", ", names);           // joining a collection: the right tool
// StringBuffer: forget it (unless you need per-method synchronization)
```

---

### Question 20 — How to make a singleton?
· [video](https://youtu.be/jVJboEeX5Wg) · local file `380`

**Short answer:** One pattern: use an enum with a single value.

**Less short answer:** The pattern that uses a class with a private constructor, a private static field and a factory method that tests whether the field is null... is more complex — either non-thread-safe, or buggy, or both. Making it thread-safe with **double-checked locking** is also buggy — just look for this bug on your favorite search engine. Using an enum is **super simple, thread-safe, guaranteed bug-free at the JVM level**, and recommended by the best experts. One last word: this enum-based pattern is used in many places in the JDK — you can check the source code of the `Comparator.naturalOrder()` factory method, for example.

```java
enum Configuration {                // singleton: simple and bulletproof
    INSTANCE;
    private final Map<String, String> props = load();
    public String value(String key) { return props.get(key); }
}
// usage: Configuration.INSTANCE.value("timeout")

// double-checked locking: full of historical subtleties — avoid
```

---

### Question 21 — What is a static method?
· [video](https://youtu.be/bwgjsA4T3T4) · local file `379`

**Short answer:** A method declared with the static keyword.

**Less short answer:** A method that you can invoke **without creating any object** — you invoke it with a class name; you can even invoke a static method through a **null** reference (the compiler swaps it for the class). A static method **cannot** call a non-static method from the same class. They are useful to implement the `main` method — classes like **Math**, **Arrays** or **Collections** only have static methods. One last word: static methods are sometimes called **factory methods** because they can be used to create objects in a certain way — `List.of()`, for instance; classes with only static methods are sometimes called factory classes.

```java
public class Main {
    public static void main(String[] args) { }  // main is static!
    static void util() { }                       // no this
    // void instanceMethod() { util(); }  // static does NOT call instance
}

Math.max(1, 2);        // utility "factory"
List.of("a");          // factory method: creates the special way
Integer.parseInt("7"); // classic static
```

---

### Question 22 — How can you find a character in a string?
· [video](https://youtu.be/8wHEVWcCpd8) · local file `378`

**Short answer:** There is a method for that: `indexOf`.

**Less short answer:** It has several overloads. One takes the character as an **int** (not a char — Java supports Unicode and some characters cannot be encoded on a single char); another one takes a **String**, because indexOf can also find small strings in bigger strings. Both can also take an index used to start the search from. `indexOf` returns the index of the **first** occurrence of the character or the string — so if you want to find all the occurrences of a single letter, you can loop in that way. One last word: if the character or the string is not found, `indexOf` returns **-1**.

```java
String s = "abracadabra";
s.indexOf('a');              // 0
s.indexOf('a', 1);           // 3 (search starts at index 1)
s.indexOf("cad");            // 4 (substring!)
s.indexOf('z');              // -1

int pos = 0;                                 // all the occurrences:
while ((pos = s.indexOf('a', pos)) >= 0) {
    System.out.println(pos); pos++;
}
```

---

### Question 23 — What is a method reference?
· [video](https://youtu.be/n1EAf759DwA) · local file `377`

**Short answer:** Another way of writing a lambda expression.

**Less short answer:** There are **four** kinds of method references. **Static** method references (`Type::staticMethod`); **bound** method references — the classical `System.out::println`; **unbound** method references — they may look like static calls, but they are not, so be careful (`String::length`); and **constructor** method references (`ArrayList::new`). One last word: to translate a method reference to a lambda, you need to know its **exact type**. And one very last word: method references are there to improve the **readability** of your code — so if you feel your code is actually less readable with them, then don't use them.

```java
Integer::parseInt             // static
System.out::println           // bound (known object)
String::length                // unbound (1st argument becomes 'this')
ArrayList::new                // constructor

// the translation depends on the target type:
Function<String, Integer> f = Integer::parseInt;   // s -> parseInt(s)
ToIntFunction<String> g = String::length;         // s -> s.length()
```

---

### Question 24 — What is a default method?
· [video](https://youtu.be/njx4t5faYQ0) · local file `376`

**Short answer:** It is a regular method written in an interface.

**Less short answer:** Default methods were added to the language in **Java 8**. Prior to that, all the methods in an interface had to be abstract (the `abstract` keyword is not even mandatory for them). For regular concrete methods, you need to add the `default` keyword — hence their name. Apart from that, default methods are just regular methods: a default method from an interface can be used by any object that implements that interface, and they can be overridden in any concrete class. One last word: in Java 8, default methods could only be **public**; starting with Java 9, they can also be **private**.

```java
interface Health {
    void register(int heartRate);        // abstract (abstract is optional)

    default boolean critical(int b) {    // concrete, with a body
        return b > 120;
    }
    default int doubleIt(int b) { return 2 * level(b); }
    private int level(int b) { ... }     // Java 9+: private for defaults
}
```

---

### Question 25 — What does JIT mean?
· [video](https://youtu.be/NC2ZFGBC2L0) · local file `375`

**Short answer:** JIT stands for Just In Time compiler.

**Less short answer:** What you have in a class file is the **bytecode** of your class. This bytecode is first **interpreted** by your JVM — and running your Java code that way is super slow. So a first JIT compiler, called **C1**, compiles it to machine language. C1 does a nice job, but your compiled code can be optimized even further — that is the job of a second JIT compiler, **C2**: it carefully analyzes the way your code is running and compiles it again to leverage some more optimizations. One last word: if at some point C2 sees that it can do better, it will **de-optimize** your code and optimize it again.

```java
// the life of your code:
// bytecode -> interpreted (slow)
//         -> C1 compiles (fast, with profiling)
//         -> C2 recompiles (flying, aggressive optimizations)
//         -> if the assumption breaks: DEOPT and start over
int sum(int[] data) {
    int t = 0;                       // hot loop -> C2 unrolls/vectorizes
    for (int x : data) t += x;
    return t;
}
```

---

### Question 26 — What kind of method can you override?
· [video](https://youtu.be/xaHeGjQDbUY) · local file `374`

**Short answer:** Instance methods.

**Less short answer:** **Static** methods cannot be overridden because you call them using an explicit class name — so even if a class B extends class A and defines the same static method, you can still call each of them (it is *hiding*, not overriding). Instance methods can be overridden as long as they are **non-private and non-final**, and their class can be extended — the obvious way to prevent extension is to make the class `final`, but you can also declare its no-arg constructor **private**. One last word: the safest way to override a method is to just copy its declaration in your extending class — return type, parameters and exceptions... but you can be more subtle than that. *(Another video.)*

```java
class A {
    public void m() { }            // overridable
    private void secret() { }      // private: not seen, no override
    public final void fixed() { }  // final: forbidden
    public static void util() { }  // static: hiding, not overriding
    private A() { }                // private ctor: class cannot be extended
}
class B extends A {
    @Override public void m() { }      // OK
    // @Override public void fixed() {} // ERROR
}
```

---

### Question 27 — What is the signature of a method?
· [video](https://youtu.be/mIMXXolsdOE) · local file `373`

**Short answer:** Its name and its parameters.

**Less short answer:** Neither the **return type** of a method nor its **declared exceptions** are part of its signature. Because all the methods of a given class must have different signatures, you can have several methods with the same name — as long as they have different parameters (**overloading**) — and these methods can have different return types. But it is **not** legal to have two methods with the same name, same parameters, and just a different return type: they would have the same signature. One last word: this rule holds even if one of these methods is **abstract** — it would force the implementation to break the rule.

```java
int add(int a, int b) { ... }
double add(double a, double b) { ... }   // OK: different signatures

// double add(int a, int b) { ... }  // ERROR: same signature
// (the return type is NOT part of the signature — JLS §8.4.2)
```

---

### Question 28 — What is a marker interface?
· [video](https://youtu.be/6d7Q6Df7RsI) · local file `372`

**Short answer:** An interface with no method, that is just here to mark a class.

**Less short answer:** Being a marker interface is not really a language or an API notion — it is just a way of describing the nature of some interfaces. There are several examples in the JDK: **`Serializable`**, of course (which allows the instances of a class to be serialized), among others. One thing: because there is no method in these interfaces, the way to use them is to **check** whether the object you have is an instance of the interface — with `instanceof`, or directly at the class level, with the `Class.isAssignableFrom` method.

```java
class Order implements Serializable { }  // the mark: "I can be serialized"

if (order instanceof Serializable) { writeToStream(order); }
Serializable.class.isAssignableFrom(Order.class);  // true

// other JDK markers: Cloneable, RandomAccess, EventListener
```

---

### Question 29 — What is the difference between a collection and a set?
· [video](https://youtu.be/rFqJULuKN0Y) · local file `371`

**Short answer:** There are no duplicate elements in a set.

**Less short answer:** Collection and set are interfaces from the Collections framework; Set **extends** Collection. All the instance methods defined in the Set interface override methods from Collection and are **redefined with a different semantic** — they do not work the same way (the "no duplicates" semantic changes everything). The Set interface is implemented by `HashSet`, which has another very interesting property: its implementation of `contains` is **super fast** — much faster than the one from an ArrayList. One last word: if you need both the ordered iterability of a list **and** a fast `contains`, take a look at the `LinkedHashSet` class — which, unfortunately, is not a list.

```java
Collection<Integer> c = new ArrayList<>(List.of(1, 1, 2)); // dupes OK
Set<Integer> s = new HashSet<>(c);   // [1, 2]: duplicate dropped

s.contains(2);    // O(1): hash
c.contains(2);    // O(n): scan

new LinkedHashSet<>(c); // insertion order preserved + fast contains
```

---

### Question 30 — How does finalize() work?
· [video](https://youtu.be/Z6TQ5LiPd08) · local file `370`

**Short answer:** It does not work. *(the presenter's demolition short answer)*

**Less short answer:** It was deprecated in Java 9 and deprecated for removal in Java 18 — so stop using it. Finalize is supposed to be a method called by some undefined element when the garbage collector detects that an object is not reachable anymore. Here are two of its major issues. First, the **thread** running finalize is **not specified** — odds are you can get race conditions in this method. Second, it may take a lot of time for this method to be called — the correctness of your code should **never** rely on finalize being called. One last word: there is a replacement pattern, the **Cleaner** pattern — check the `Cleaner` class for more information.

```java
@Deprecated(since = "9", forRemoval = true)  // and JEP 421
protected void finalize() { }   // unspecified thread + slow

// the replacement: Cleaner
private static final Cleaner CLEANER = Cleaner.create();
// CLEANER.register(this, () -> release()); + AutoCloseable
```

---

### Question 31 — What were the main features of Java 8?
· [video](https://youtu.be/aPhQSw8ezf8) · local file `369`

**Short answer:** Lambda expressions.

**Less short answer:** Lambda expressions were added to the language — but many other things came along. The Collections framework was **rewritten**, with many new methods; the **Stream API** is a very clean implementation of map/filter/reduce, usable in parallel; the **CompletableFuture** API brought an asynchronous programming model to the JDK; and many, many small things here and there to make your developer life better. One last word: Java 8 was released in **March 2014** — at the time of this recording, nine years ago; if you are still working on this version, you should really consider moving to a more recent one.

```java
// Java 8 in four lines:
Runnable r = () -> { };                                 // lambdas
list.stream().map(...).filter(...).collect(toList());   // Stream API
Optional<Order> p = findById(id);                       // Optional
CompletableFuture.supplyAsync(this::load);              // async
```

---

### Question 32 — Do streams and collections have common methods?
· [video](https://youtu.be/QwCVrLqfZCY) · local file `368`

**Short answer:** Yes and no. They do have **one** method that is called the same and takes the same parameters.

**Less short answer:** This method is **`forEach`**: it takes a consumer, and you have it on both the `Stream` interface and the `Iterable` interface. That being said, even if they are basically doing the same thing, these methods are **not implemented in the same way** — and most of the time you can call `collection.forEach(...)` directly instead of `collection.stream().forEach(...)`: it saves you the creation of the stream object. If you don't use it, why would you want to create it? One last word: even if collections and streams are there to process data, there are many differences between them — but that's for another time.

```java
list.stream().forEach(System.out::println);  // creates a stream for nothing
list.forEach(System.out::println);           // direct: same thing, better
```

---

### Question 33 — How can you tell that a string is an anagram of another string?
· [video](https://youtu.be/6Eld7mrWN1w) · local file `367`

**Short answer:** The easy way: sort the letters of the strings and just compare them.

**Less short answer:** There is actually a much smarter way of doing that. You can map each letter to a **prime number** — for instance, map A to 2, B to 3, C to 5, etc. — then **multiply** all these numbers for each string. If the two numbers you get are the same, the strings are anagrams — it works because the decomposition of an integer in a product of prime numbers is **unique**. The first method has a complexity of n log n; the second one is just **n**, which is faster. One last word: be careful not to **overflow** when doing the multiplication — it can happen for long strings of characters.

```java
// classic way: sort and compare — O(n log n)
Arrays.equals(
    a.chars().sorted().toArray(),
    b.chars().sorted().toArray());

// smart way: product of primes — O(n)
int[] primes = {2, 3, 5, 7, 11, 13, 17, ...};  // A, B, C, ...
long prodA = a.chars().reduce(1L, (p, c) -> p * primes[c - 'a']);
long prodB = b.chars().reduce(1L, (p, c) -> p * primes[c - 'a']);
prodA == prodB;   // anagrams! (careful: overflow on long strings)
```

---

### Question 34 — On what kind of source can you build a stream?
· [video](https://youtu.be/zFtoUwylz_U) · local file `366`

**Short answer:** There is no short answer: there are many sources.

**Less short answer:** Collections, of course — this is the most common way. But the JDK offers much more than that: you can also create a stream on the **lines of a text file**, on the **characters of a string**, on the elements of a string you split using a **regular expression**, and several others. Plus, the Stream API gives you what you need to connect a stream to **your own source** of data. One last word: be careful — the processing of a stream should not **modify its source** while it is consuming it; if you do that, the result you will get is unpredictable.

```java
collection.stream();
Arrays.stream(array);
Files.lines(path);                        // lines of a file
"abc".chars();                            // characters (IntStream)
Pattern.compile(",").splitAsStream(csv);  // lazy split
Stream.generate(supplier);                // infinite source
// your own source: StreamSupport.stream(yourSpliterator(), false)
```

---

### Question 35 — What is the groupingBy()?
· [video](https://youtu.be/rlShYvlfrdo) · local file `365`

**Short answer:** A collector you can use to build maps that are in fact **histograms** from the elements of a stream.

**Less short answer:** The pattern is `stream.collect(Collectors.groupingBy(fn))`: the function maps each element of your stream to a **key**, which is added to your map — and the corresponding element is added to the **list** bound to that key. So if several elements of your stream are mapped to the same key, you will find them **together** in that list. One last word: you can **post-process** this list with another collector, passed as a **second argument** to the groupingBy — the so-called *downstream collector*.

```java
Map<String, List<Person>> byCity = people.stream()
        .collect(Collectors.groupingBy(Person::city));

// with a downstream: instead of the list, count per city
Map<String, Long> histogram = people.stream()
        .collect(Collectors.groupingBy(Person::city,
                                       Collectors.counting()));
```

---

### Question 36 — What is the difference between FIFO and LIFO?
· [video](https://youtu.be/t9qidGAKirk) · local file `364`

**Short answer:** FIFO is a queue; LIFO is a stack.

**Less short answer:** FIFO stands for *first in, first out*; LIFO stands for *last in, first out*. There are two interfaces in the JDK to model these: **`Queue`**, which models the FIFO — extended by **`Deque`**, the second interface, which models the LIFO. The preferred implementation for both is `ArrayDeque` — which is **not thread-safe**; if you need a thread-safe implementation, use a `ConcurrentLinkedQueue` (Queue) or a `ConcurrentLinkedDeque` (Deque). One last word: there is a `Stack` class in the JDK — it extends the `Vector` class; both have been deprecated for so long that you should not use them anymore.

```java
Deque<String> stack = new ArrayDeque<>();   // LIFO
stack.push("a"); stack.push("b");
stack.pop();                                // "b" (the last one)

Queue<String> queue = new ConcurrentLinkedQueue<>(); // FIFO, thread-safe
queue.offer("a"); queue.offer("b");
queue.poll();                               // "a" (the first one)

// new Stack<>(); // 1998 called: don't
```

---

### Question 37 — What is the GoF?
· [video](https://youtu.be/NkIJrvJzWYQ) · local file `363`

**Short answer:** The nickname of the fundamental book on design patterns.

**Less short answer:** The full title of this book is *Design Patterns: Elements of Reusable Object-Oriented Software*, published in **1994**. The four authors are **Erich Gamma, Richard Helm, Ralph Johnson and John Vlissides**. It contains **23 design patterns** in three categories: **creational** (Builder, Factory, Singleton), **structural** (Adapter, Decorator, Proxy) and **behavioral** (Iterator, Strategy, Visitor). One last word: this book is a must-read — the examples are written in C++, but it is easy enough to convert them to Java or to your favorite language.

```java
// you use the GoF every day:
List.of(1, 2, 3).iterator();      // Iterator (behavioral)
new BufferedReader(r);            // Decorator (structural)
enum DB { INSTANCE; }             // Singleton (creational)
```

---

### Question 38 — How can you create a comparator?
· [video](https://youtu.be/CvsMHdYLg5c) · local file `362`

**Short answer:** Just use the factory methods of the Comparator interface.

**Less short answer:** This interface has a bunch of factory methods to create comparators that compare objects using one of their **fields**, and default methods to modify the behavior of a given comparator. For instance, you can compare people using their **last name**; in case you have people with the same last name, you can then compare them using their **first name**; and in case you want to sort in the **reverse** order, just call `reversed()` on an existing comparator. One last thing: null values are always painful to handle when sorting — the Comparator interface also has **`nullsFirst`** and **`nullsLast`** methods.

```java
Comparator<Person> byName = Comparator.comparing(Person::lastName)
        .thenComparing(Person::firstName)      // tie-breaker
        .reversed();                           // reverse order

Comparator<Person> withNulls = Comparator
        .nullsLast(Comparator.comparing(Person::name));
```

---

### Question 39 — Can you cite functional interfaces that existed before Java 8?
· [video](https://youtu.be/Mf0VaqZa5jE) · local file `361`

**Short answer:** Runnable, Comparator, Iterable.

**Less short answer:** And there are more. The definition of a functional interface is such that some **existing** interfaces became functional with Java 8 — and because you can implement a functional interface with a **lambda**, all the interfaces in your application that qualify can be implemented with lambdas **without having to recompile them**. That is why the `@FunctionalInterface` annotation is **not mandatory**. You want more? `Executor`. Even `Comparable` is a functional interface — "though I'm not sure you want to implement this one with a lambda".

```java
Runnable r = () -> System.out.println("pre-Java 8, already a SAM!");
Comparator<String> c = (a, b) -> a.length() - b.length();
// no recompilation: lambdas apply to any old SAM interface

@FunctionalInterface  // optional: just validates the rule
interface MyCallback { void onEvent(String e); }
```

---

### Question 40 — How can you open a file for writing some text?
· [video](https://youtu.be/NlwaLgvofjA) · local file `360`

**Short answer:** There is a factory method for that in the Files class.

**Less short answer:** There are actually two patterns: the old one from Java IO and the less old one from **Java NIO** — and this is the one you should prefer. The exact pattern is `Files.newBufferedWriter(path)`, which has several overloads: you can specify the **charset** used by your text file (the default being **UTF-8**), and you can specify some **open options** — whether you want to create the file if it does not exist, and if it does, whether you want to erase its content or append to it. One last word: what you get is still a regular `BufferedWriter` — you can still **decorate** it if you need.

```java
try (var w = Files.newBufferedWriter(
        Path.of("log.txt"),
        StandardCharsets.UTF_8,                    // charset
        StandardOpenOption.CREATE,                 // create if missing
        StandardOpenOption.APPEND)) {              // append vs TRUNCATE
    w.write("a log line");
}
```

---

### Question 41 — What is the difference between a sorted and an ordered collection?
· [video](https://youtu.be/Xjm1ESl_Sm0) · local file `359`

**Short answer:** *Sorted* means that you can **compare** your elements; *ordered* means that you can access them using an **index**. *(as the video puts it)*

**Less short answer:** It has to do with how you iterate over the elements of your collection. A **sorted** collection gives you the smallest element first, up to the largest one — adding an element may change the iteration order. An **ordered** collection, on the other hand, respects the order in which you **add** your elements: the first element is the first that has been added, up to the last one. One last word: in the framework, ordered collections are modeled by **lists** (your favorite implementation is ArrayList), and sorted collections are modeled by **navigable sets**, implemented by TreeSet.

```java
List<Integer> ordered = new ArrayList<>(List.of(3, 1, 2));
// iteration: 3, 1, 2 — INSERTION order (index: get(0) == 3)

NavigableSet<Integer> sorted = new TreeSet<>(ordered);
// iteration: 1, 2, 3 — COMPARISON order
```

---

### Question 42 — How can you open a file for reading binary data?
· [video](https://youtu.be/6tcGmrcIfKM) · local file `358`

**Short answer:** There is a factory method for that in the Files class.

**Less short answer:** The old patterns from Java 1.1, based on the decorator pattern, are still there — but you should use the **`Files`** factory class to create your readers, writers, input and output streams: the implementations you get are built on top of Java **NIO**, and they are going to save you some time. You can still use the **decorator** pattern if you need to build `DataInputStream`/`DataOutputStream`, gzip streams, or streams that mix text and data. One last word: the patterns from the Files factory class also give you fine control on the opening of files — on the **charsets** you can use to read your text files — and they are built to use the **Path** object instead of File objects, which is better.

```java
try (var in = Files.newInputStream(Path.of("data.bin"))) {
    // decorate as you need:
    var data = new DataInputStream(new BufferedInputStream(in));
    int n = data.readInt();
    double d = data.readDouble();
}
// built on NIO, with Path (not File)
```

---

### Question 43 — How can you print an array on the console?
· [video](https://youtu.be/xic2fLpraUE) · local file `357`

**Short answer:** Use the `toString` method from the Arrays class.

**Less short answer:** An array is an object, but with a default `toString` method that you **cannot override** — printing it directly gives you something you do not want (the hashcode). Call `Arrays.toString(array)`: it prints the **content** of your array, calling `toString` on each element. If you are not happy with the formatting, you then need to iterate over the elements of the array and create the strings of characters you need. One last word: you can use arrays in the for-each pattern — the pattern works for any object that implements the `Iterable` interface... now, Iterable declares a `forEach` method, but **you cannot call it on arrays** (arrays do not implement Iterable).

```java
int[] n = {1, 2, 3};
System.out.println(n);                    // [I@1b6d3586 (default: useless)
System.out.println(Arrays.toString(n));   // [1, 2, 3]
System.out.println(Arrays.deepToString(new int[][]{{1},{2,3}}));
                                                         // [[1], [2, 3]]
for (int x : n) { ... }                   // for-each works!
// n.forEach(...);                        // doesn't exist: not Iterable
```

---

### Question 44 — How does a SortedSet work?
· [video](https://youtu.be/7sUyT2Q8YnU) · local file `356`

**Short answer:** It is a set that keeps its elements sorted.

**Less short answer:** First point: it is a set — **no duplicates**; if you add two elements that are equal, the second one will not be added. Second point: the elements you add must be **comparable** — or, if they are not, you must provide a **comparator** when you create your sorted set. If you provide a comparator, it will be used **even if** your elements are comparable. And when you iterate over the elements, they come in **increasing** order. As of Java 6, you should favor a **`NavigableSet`** instead of a SortedSet — it gives you many more methods. One last word: the default implementation is `TreeSet`, which implements a **red-black tree**.

```java
SortedSet<Integer> s = new TreeSet<>(Set.of(5, 1, 3));
s.add(1);                  // not added: duplicate
s.first();                 // 1 — always sorted

NavigableSet<Integer> nav = new TreeSet<>(Set.of(1, 3, 5, 7));
nav.ceiling(4);            // 5 (the first >= 4)
nav.headSet(5);            // [1, 3] — a view
// TreeSet = red-black tree, everything O(log n)
```

---

### Question 45 — What pattern has been used to create the Java I/O API?
· [video](https://youtu.be/RQ5RfcYl9DU) · local file `355`

**Short answer:** The Decorator pattern.

**Less short answer:** A **structural** pattern from the GoF book. The decorator is in fact a *wrapper* on an object that **extends the class** of that object — it may add more methods or change the way the existing methods work. Wrapping an existing object has a trick in the constructor: it takes an object of the **superclass** as a parameter — so it is both an extension **and** a composition. For instance, `BufferedReader` **extends** `Reader` and adds the `readLine()` method — plus the reading now works with a buffer. One last word: you should use the Files factory class to build the base objects you need to read and write content from files... but you can still decorate them to add features to the base objects you get.

```java
// Decorator: extends Reader AND takes a Reader (extension + composition)
Reader base = Files.newBufferedReader(Path.of("a.txt"), UTF_8);
LineNumberReader withLines = new LineNumberReader(base); // +getLineNumber()
var withPushback = new PushbackReader(withLines);         // +unread()

// each layer adds behavior WITHOUT changing the interface
```

---

### Question 46 — What are the different categories of design patterns?
· [video](https://youtu.be/Zv6oYeWPcaQ) · local file `354`

**Short answer:** Three categories: creational, structural, behavioral.

**Less short answer:** It refers to the Gang of Four book — a must-read for every developer. Some **creational** patterns: Factory, Builder, Singleton. Some **structural** patterns: Decorator, Facade, Proxy. Some **behavioral** patterns: Iterator, Template Method, the well-known Visitor. The GoF gives you C++ implementations — some of them are dated, but the ideas are still excellent. One last word: there are many patterns that are so widely used that you use them **even if you don't know them** — that is the case for the iterator pattern or the factory pattern.

```java
List.of(1, 2, 3).iterator();            // Iterator (behavioral)
new BufferedReader(reader);             // Decorator (structural)
Optional.of(x);  List.of(...);          // Factory (creational)
StringBuilder b = new StringBuilder();  // Builder (creational)
```

---

### Question 47 — How can you count how many times a letter appears in a string?
· [video](https://youtu.be/bAapVHz1Jog) · local file `353`

**Short answer:** You need to iterate over the string in one way or another.

**Less short answer:** Two patterns. First, you can look for your letter in the string with `indexOf`, **until you don't find it anymore** — indexOf may take an index where it will start its search; that's a quick and efficient pattern. Second pattern: you can create a **stream** on the letters of the string, removing the letters that do not match and counting the result — efficient, more functional, more readable... "this is the one I prefer". One last word: the `count()` method returns a `long` that you can safely cast to an int — a string of characters is backed by an array, so there is no way you can have more than Integer.MAX_VALUE letters.

```java
String s = "abracadabra";

// pattern 1: indexOf in a loop
int pos = 0, total = 0;
while ((pos = s.indexOf('a', pos)) >= 0) { total++; pos++; }  // 5

// pattern 2: stream (the presenter's favorite)
long n = s.chars().filter(c -> c == 'a').count();            // 5
```

---

### Question 48 — What is a bucket in a map?
· [video](https://youtu.be/1LzriAPWvXY) · local file `352`

**Short answer:** A cell in an array.

**Less short answer:** The HashMap class from the Collections framework is backed by an array. When you add a key to a HashMap, a special hash code is evaluated for that key to **choose which cell** of this array will receive the key-value pair. If there is already a different key-value pair in this cell, this is called a **collision** — and then a linked list is created to hold the pairs; and if there are too many, this linked list is actually replaced by a **red-black tree**. To minimize collisions, when the amount of key-value pairs reaches **75%** of the size of the array, the content is copied into a new, larger array. One last word: this copying incurs a **rehashing** of all the keys present in the map, which may be costly — so be careful about that, and try to create maps with the right size.

```java
Map<String, Integer> m = new HashMap<>();
m.put("ana", 1);      // hash("ana") & (n-1) -> a cell
// cell occupied by a different key? linked list
// more than 8 in the same cell? becomes a red-black tree (O(log n))
// 75% full (load factor)? resize + REHASH of everything

HashMap.newHashMap(1000);  // right size upfront: avoids rehashing
```

---

### Question 49 — What does synchronized mean?
· [video](https://youtu.be/ZNT1IH5m3NI) · local file `351`

**Short answer:** A keyword that prevents more than one thread from executing a given block of code at the same time.

**Less short answer:** Synchronization has to do with concurrent programming. This block of code can be a delimited block inside a method, or it can be the whole method, static or not. All synchronized blocks need an object that they use as a **key** — and you can use the same key for more than one synchronized block. In the case of a synchronized method, the key is the object itself — or the class if the method is static. There are limitations with synchronized blocks that you don't have with **locks** (ReentrantLock...) — which you can use instead. One last word: **never expose** your keys or your locks — it will help you prevent deadlocks, a situation that you absolutely want to avoid.

```java
class Cache {
    private final Object key = new Object();

    public void put(String k, Integer v) {
        synchronized (key) { map.put(k, v); }  // block with its own key
    }
    public synchronized int size() { ... }     // key = this
}
// synchronized limitations: no tryLock, no timeout, not "fair"
// -> ReentrantLock (tryLock/timeout), always unlock() in the finally
```

---

### Question 50 — What is the difference between a collection and a stream?
· [video](https://youtu.be/Nq9Z1KSlVVg) · local file `350`

**Short answer:** A collection contains objects; a stream is empty.

**Less short answer:** A collection is meant to **carry** objects around, with methods to handle them. Most of the time, a stream is an **empty** object that you **connect to a source** of objects to process them — this source can be a collection, an array and many other things; you can even connect a stream to **your own** source of data. It will then process the elements one by one, lazily — and some methods like `findAny` or `allMatch` can **interrupt** the processing of these objects. One last word: there are actually two methods that need to **remember** the processed objects — `distinct` and `sorted`: if you use them, your stream will not be empty anymore.

```java
List<String> names = List.of("a", "b");  // carries the objects

names.stream()                          // stream: empty, bound to a source
     .filter(s -> !s.isEmpty())
     .findAny();                        // short-circuit: stops early

// streams "with memory": distinct and sorted store the elements
```

---

### Question 51 — What is the difference between a File and a Path?
· [video](https://youtu.be/CVAaOy5oTLw) · local file `349`

**Short answer:** A File is a class; a Path is an interface.

**Less short answer:** The `File` class is from Java 1.0; `Path` is from Java NIO, released with **Java 7**. An instance of File is the same no matter what file system you are using — Path being an **interface**, the instance you build **depends on the file system**, and you can get attributes that are specific to these file systems, like the security attributes. As of now, you should favor the use of Path objects over File objects. One last word: creating an instance of File or Path does **not** create the corresponding file or directory on your disk — you need to call `createNewFile()` on the File class, or the `Files.createFile(path)` factory method, to do that.

```java
File f = new File("data.txt");    // 1995: a single class
Path p = Path.of("data.txt");     // 2011 (Java 7): an interface

// actually create it on disk:
f.createNewFile();                // legacy
Files.createFile(p);              // preferred
Files.getAttribute(p, "size");    // attributes of the real FS (zip!)
```

---

### Question 52 — What is the difference between a checked and an unchecked exception?
· [video](https://youtu.be/8trF3lwGHPU) · local file `348`

**Short answer:** You need to write special code to handle the checked exception — not for unchecked exceptions.

**Less short answer:** All your exceptions extend the Exception class. If an exception actually extends **RuntimeException**, then it is an **unchecked** exception; if it does not, then it is a **checked** exception. You need to handle your checked exceptions, either by **catching** them or by **declaring** them in the throws clause of your methods and constructors. You do not need to do that for unchecked exceptions. One last word: all these classes extend the `Throwable` class, also extended by the **`Error`** class — errors are also unchecked exceptions, but reserved for serious problems like `OutOfMemoryError` or `StackOverflowError`.

```java
void readFile() throws IOException { ... }  // checked: declare or catch
try { readFile(); } catch (IOException e) { ... }

// unchecked (RuntimeException): no obligation
List.of().get(0);       // IndexOutOfBoundsException
Integer.parseInt("x");  // NumberFormatException
// Errors (also unchecked): OutOfMemoryError, StackOverflowError
```

---

### Question 53 — What is a stream?
· [video](https://youtu.be/_VIHuuXsMoM) · local file `347`

**Short answer:** An object that implements the **map/filter/reduce** pattern.

**Less short answer:** The stream consumes elements from a source and can run several operations. These operations are organized in a **pipeline**, where each element is passed from one step of the pipeline to the next one. **Mapping** an element consists in transforming it; **filtering** consists in deciding, based on a predicate, whether this element should be transmitted or not; **reducing** may consist in many things — summing the elements, extracting the min or max, or adding them to a list or a map. One last word: the Stream API allows you to conduct these computations **in parallel** — still, you need to be careful with that: as always when it comes to optimizations, **measure, don't guess**.

```java
double avg = orders.stream()          // source
        .filter(Order::paid)          // filter: pass or not
        .mapToDouble(Order::amount)   // map: transforms
        .average()                    // reduce: aggregates
        .orElse(0);

orders.parallelStream()...            // parallel: measure first!
```

---

### Question 54 — What is the hash code of an object?
· [video](https://youtu.be/saa_wJZB7r8) · local file `346`

**Short answer:** A mapping of an object to an integer.

**Less short answer:** The idea behind the hash code is to represent any object within an int, so that you can quickly tell if two objects are **different** by just comparing their hash codes. Indeed, the specification says that two objects that are **equal** must have the **same** hash code. But it turns out that two different objects **may also** have the same hash code. So: different hash codes ⇒ the objects cannot be equal, for sure; same hash code ⇒ you need to compare them with `equals` to decide. One last word: there are many ways of computing hash codes, some better than others — if you're not sure about what you need, relying on your IDE to generate this hashCode method is probably your safest choice.

```java
record Customer(String name, String taxId) {
    // hashCode and equals generated by the IDE (Objects.hash):
    @Override public int hashCode() { return Objects.hash(name, taxId); }
    @Override public boolean equals(Object o) { ... }
}
// equal -> SAME hash (mandatory)
// different hash -> definitely different (quick shortcut)
// same hash -> equals decides
```

---

### Question 55 — What is an ExecutorService?
· [video](https://youtu.be/dv9_Tz4x3Mg) · local file `345`

**Short answer:** An object to which you can **submit tasks** to be executed in another thread.

**Less short answer:** ExecutorService is an interface, part of the java.util.concurrent API added to the JDK 5 in 2004. It is the preferred way to run tasks in another thread — in fact, you shouldn't be creating threads by calling `new Thread` anymore. A task is created by the user, and can be either a `Runnable` or a **Callable** — both implementable with a lambda expression. Submitting a task gives you a **Future** object — you can use it to get a result, an exception if something goes wrong, or to interrupt the running of this task. One last word: an ExecutorService is sometimes called a "pool of threads" — and most of the time it is; but it can also create threads **on demand**, which is what it does for virtual threads.

```java
ExecutorService ex = Executors.newVirtualThreadPerTaskExecutor();

Future<Integer> f = ex.submit(() -> compute());  // Callable -> Future
f.get(1, TimeUnit.SECONDS);  // result OR exception
f.cancel(true);

ex.shutdown();               // the lifecycle managed by it
```

---

### Question 56 — What is an Iterator?
· [video](https://youtu.be/QsscPWfpAcE) · local file `344`

**Short answer:** An object used to iterate over the elements of a collection.

**Less short answer:** Iterator is an interface with **three** methods. `hasNext` — tells you if there are more objects to iterate over; you should call this method first. Then, if it returns true, you can call `next` to get the next object and move the iterator one step. It also has a `remove` method — which can throw an `UnsupportedOperationException`, depending on your collection being immutable. Most of the time, you use iterators for collections — but iterators can actually be created **without** them: for instance, you can create a range iterator, that can iterate over a range of integers. One last word: the iterator pattern is actually a pattern from the Gang of Four book — did I already mention that this book is a must-read?

```java
for (Iterator<String> it = list.iterator(); it.hasNext(); ) {
    String s = it.next();
    if (s.isEmpty()) it.remove();   // safe removal: through the iterator
}

// an iterator WITHOUT a collection:
Iterator<Integer> range = IntStream.range(0, 10).iterator();
```

---

### Question 57 — What are the 4 Java I/O base classes?
· [video](https://youtu.be/7UpBwY_UcKo) · local file `343`

**Short answer:** Reader, Writer, InputStream and OutputStream.

**Less short answer:** These are the 4 abstract classes that define the basic operations for reading and writing **characters** and **binary data**. They were designed in the mid-90s — and you should not use them **directly** anymore. The preferred patterns are the factory methods from the `Files` class: `newBufferedReader`/`newBufferedWriter` for the reading and writing of characters, and `newInputStream`/`newOutputStream` for the reading and writing of binary data. These factory methods give you instances built on top of Java **NIO**. One last word: you can still **decorate** the instances you get, following the general pattern of the Java I/O API.

```java
// the 4 cornerstones (mid-90s): Reader, Writer, InputStream, OutputStream

// today: Files is the entry point (on top of NIO)
try (var r = Files.newBufferedReader(Path.of("a.txt"), UTF_8);
     var w = Files.newBufferedWriter(Path.of("b.txt"), UTF_8);
     var in = Files.newInputStream(Path.of("data.bin"))) {
    // and decorate if you need:
    var data = new DataInputStream(new BufferedInputStream(in));
}
```

---

### Question 58 — How many objects can you put in a collection?
· [video](https://youtu.be/GGbf7ZLt33c) · local file `342`

**Short answer:** It depends on the implementation you have.

**Less short answer:** An `ArrayList` is backed by an array, so the limit would be `Integer.MAX_VALUE`. You may be thinking that a `LinkedList` would allow you to store more objects — but in fact, many things will fail for a LinkedList with more objects than Integer.MAX_VALUE: a LinkedList counts its elements with a field called **`size`, which is an `int`** — bad things will happen if size reaches Integer.MAX_VALUE. Its `toArray()` method will not work very well either, since arrays are also indexed by ints. One last word: the same limit also exists for maps — but don't worry: there is little chance that you can reach that limit without reaching other limitations in your application first.

```java
// ArrayList: internal array -> limit is Integer.MAX_VALUE
// LinkedList: the size field is an int -> the same ceiling
// size() returns an int: at most 2^31-1 = 2,147,483,647 elements
// (in practice: the heap runs out MUCH earlier — use a DB/off-heap beyond)
```

---

### Question 59 — What is the maximum length of a String in Java?
· [video](https://youtu.be/zZ9rWoRKZas) · local file `341`

**Short answer:** It cannot be longer than `Integer.MAX_VALUE` — because it is backed by an array.

**Less short answer:** First, that would be a very long string indeed. And second, the number of **characters** may be different from the length of this array — the reason being that many characters are actually encoded on **more than one byte**. One last word: the String class changed in 2017, with Java 9: the internal array used to be an array of **chars**, 16 bits — it is now an array of **bytes**, 8 bits. This optimization is called **Compact Strings**, and it can tremendously reduce the memory footprint of your application. One more reason to upgrade to the latest versions of the JDK.

```java
// limit: Integer.MAX_VALUE (~2.1 billion) — length() is an int
String s = "a".repeat(Integer.MAX_VALUE);  // if memory allows...

// Java 9+: internal byte[] (Compact Strings)
// "abc" -> 3 bytes (Latin-1), not 3 x 2 bytes as before
```

---

### Question 60 — How does a set know that it already contains an element?
· [video](https://youtu.be/n8FTS6G-C0I) · local file `340`

**Short answer:** It uses the **hash code** of this object; if it finds something, then it calls the `equals` method.

**Less short answer:** A HashSet stores its objects in the cells of an array. The index of the cell for a given object is computed from the **hash code** of this object. This process is very fast because it does **not depend** on the amount of elements you have in your set. Of course, there is a mechanism to handle collisions — that is, if two different objects happen to have the same hash code. One last word: because of that, a HashSet will **not work** if you override `equals` for your object without overriding `hashCode`. And one very last word: **do not mutate an object** once it has been added to a set, because you will not be able to find it again.

```java
Set<Person> set = new HashSet<>();
set.add(new Person("ana", 30));     // hash(ana) -> cell 7, say

set.contains(new Person("ana", 30)); // same hash -> equals -> true

// mutated the object (hash changed)? search goes to the NEW cell: miss!
// overrode equals without hashCode? equal objects in different cells
```

---

### Question 61 — How is Arrays.asList() working?
· [video](https://youtu.be/xBpNWchgSI0) · local file `338`

**Short answer:** Arrays.asList wraps an array and exposes it as a list.

**Less short answer:** You can call Arrays.asList with a vararg — but you can also pass an array. **Avoid** passing an array of primitive types: what you will get is a list with a single element — the array itself. If you modify the array you pass, it will modify the list too, because — as said — there is **no defensive copy** of this array. You can modify the **elements** of this list (set), but you cannot add or remove elements from it — because, you know, it is actually an array. One last word: what you get with Arrays.asList is **not** an unmodifiable list, because you can still change its elements; if what you need is an unmodifiable list, you may consider using `List.of`.

```java
String[] a = {"a", "b"};
List<String> l = Arrays.asList(a);   // wrapper: no copy!

a[0] = "X";                          // l is now [X, b] (same array)
l.set(1, "Y");                       // changes the array too
// l.add("c");                       // UnsupportedOperationException

int[] n = {1, 2};
Arrays.asList(n);                    // a List<int[]> with 1 element!
```

---

### Question 62 — Can you cite some methods from the Stream API?
· [video](https://youtu.be/ksgXO6lZ8bw) · local file `337`

**Short answer:** The three basic methods: map, filter and reduce.

**Less short answer:** Odds are that you will not use `reduce` much — you have many specialized methods to reduce the elements of your stream, like `toList`, `forEach`, `findFirst` or `findAny`. Other intermediate methods you can cite: `flatMap`, of course, `distinct`, `sorted`. Two things that you absolutely need to avoid: using `peek` unless you need to debug your stream — peek is useless in production — and second, don't do any **side effects** in any method of the Stream API; it will put you in trouble. One last word: you can also reduce your stream with the Collector API — there are a bunch of available collectors in the `Collectors` factory class, and you can even create your own.

```java
stream.map(fn).filter(p).sorted().distinct().toList();
stream.findFirst(); stream.anyMatch(p);  // specialized reductions
stream.peek(s -> log(s));                // debugging only!

// collectors: ready-made or your own
stream.collect(Collectors.groupingBy(...));
Collector.of(supplier, acc, comb, fin);  // yours!
```

---

### Question 63 — What is the var keyword in Java?
· [video](https://youtu.be/1tJWDKyKAM8) · local file `336`

**Short answer:** A keyword you can use in methods to avoid having to specify the type of a local variable.

**Less short answer:** `var` is for **local variables** only — you cannot use it for fields, for instance. You may use it for **anonymous classes**: in that case, you will have access to the methods you added in your anonymous class, that are not defined in the original class. The type of an anonymous class is called a **non-denotable type** — you can add fields and methods to it and access them if you declare your variable with the `var` keyword. One last word: you can use your IDE to see the type inferred by the compiler — and sometimes the result is unexpected.

```java
var list = new ArrayList<String>();     // local: OK

var counter = new Object() {            // anonymous + var: full type!
    int value = 0;                      // non-denotable
    void inc() { value++; }
};
counter.inc(); counter.inc();           // members of the type itself

// var field;  // ERROR: locals only
```

---

### Question 64 — How can you create an unmodifiable list?
· [video](https://youtu.be/w_SWksWA74w) · local file `335`

**Short answer:** With the `List.of` factory method.

**Less short answer:** There are actually several ways to do that, but List.of is the preferred one. Be careful with the behavior of `Collections.unmodifiableList()`: what it gives you is an unmodifiable **view** of a list — with **no defensive copy** of the list you pass as an argument; so if you modify that list, this modification will be seen through the view. On the other hand, you can pass an array to `List.of` — it will be **copied**, so no funny behavior there. One last word: be careful, because List.of does **not accept null values** *(why would you put null values in a list?)* — you will get an exception at runtime if you pass an array with a null value in it.

```java
List<String> original = new ArrayList<>(List.of("a"));
List<String> view = Collections.unmodifiableList(original);
original.add("b");             // the view now shows [a, b]!

List<String> copy = List.of("a", "b");   // a copy: truly immutable
// copy.add("x");          -> UnsupportedOperationException
// List.of("a", null);     -> NPE (nulls, never)
```

---

### Question 65 — How can you start a thread?
· [video](https://youtu.be/mm-iac2Lctk) · local file `334`

**Short answer:** There is a `start` method on the Thread class.

**Less short answer:** Two things. First, a thread is either built on a Runnable **or is itself** a Runnable — and in both cases, a common mistake is to call the `run` method instead of `start`: run will execute the runnable, but in the **current** thread, not in a new one. Second, calling start is okay for **testing, learning and exploring** how threads work. In a production environment, you should not launch your threads by hand — you should use the **ExecutorService** pattern instead. One last word: JDK 21 adds new factory methods on the Thread class to create both **platform** threads and **virtual** threads. *(Another video.)*

```java
Thread t = new Thread(() -> process());
t.start();          // a NEW thread executes run()
// t.run();         // same thread! classic mistake

// JDK 21: factories
Thread.ofPlatform().name("worker-").start(task);
Thread.ofVirtual().start(task);

// production: Executors.newVirtualThreadPerTaskExecutor()
```

---

### Question 66 — How can you create a file on a disk with Java I/O?
· [video](https://youtu.be/mVNyVjvRPjw) · local file `333`

**Short answer:** Use the `createFile` factory method from the Files class.

**Less short answer:** There is also `createNewFile` on the File class — the **legacy** way of creating files. And creating a **writer** or an **output stream** on a file can also create this file for you — once again, the preferred pattern is the `Files` class: `newBufferedWriter` / `newOutputStream` take **open options**: `READ`, `WRITE`, `APPEND`, `CREATE`, `CREATE_NEW`, among others. One last word: depending on your file system, you can also pass **file attributes** to the file creation — to handle security, for instance.

```java
Files.createFile(Path.of("new.txt"));       // fails if it already exists
Files.writeString(Path.of("log.txt"), "hi",
        StandardOpenOption.CREATE, StandardOpenOption.APPEND);

new File("old.txt").createNewFile();        // legacy (boolean)

Files.setAttribute(p, "dos:hidden", true);  // attributes (FS-dependent)
```

---

### Question 67 — What is the class named Class?
· [video](https://youtu.be/uQMhcWcRwnA) · local file `332`

**Short answer:** The class that models classes.

**Less short answer:** The JVM creates an instance of this `Class` class **for each class it loads**. The simplest way to get a reference on a Class object is to call `getClass()` on any object — but you can also use the class itself directly, like `String.class`, or call `Class.forName(...)` to get a reference from a name. The `Class` class is the **entry point of the Reflection API**, which gives you a way to manipulate objects reflectively — a technique that is used by many frameworks.

```java
Class<String> c1 = String.class;
Class<?> c2 = "hello".getClass();
Class<?> c3 = Class.forName("java.lang.String");
c1 == c2 && c2 == c3;   // true: one instance per loaded class

c1.getName();           // "java.lang.String"
c1.getMethods();        // reflection: methods...
c1.isRecord(); c1.isSealed(); c1.isEnum();
```

---

### Question 68 — How can you find duplicates in a list?
· [video](https://youtu.be/39uhouT-sqs) · local file `331`

**Short answer:** It really depends on what you need.

**Less short answer:** You can **check if** a list has duplicates by putting this list in a `Set` and comparing the size of the set with the size of the list. You could also use a stream, calling the `distinct()` method on it and comparing the counts — which also builds a set internally. If you need to **find the duplicate elements themselves**, then you need to build a counting map: create a stream on the list and group the elements **by themselves** (`groupingBy(identity())`), counting them with the `counting()` collector — the ones that go past 1 are the duplicates.

```java
List<String> l = List.of("a", "b", "a", "c", "b");

// any duplicates?
new HashSet<>(l).size() < l.size();               // true

// which ones?
Map<String, Long> counts = l.stream()
        .collect(Collectors.groupingBy(s -> s, Collectors.counting()));
counts.entrySet().stream()
     .filter(e -> e.getValue() > 1)
     .map(Map.Entry::getKey)
     .forEach(System.out::println);              // a b
```

---

### Question 69 — What is a sealed type?
· [video](https://youtu.be/w_BSlLlFKAM) · local file `330`

**Short answer:** A type that **knows all of its extensions**.

**Less short answer:** It can be any type — an interface, a class, or an abstract class. You need to declare it with the `sealed` keyword, and then declare the permitted types with the `permits` clause. Now, there are constraints on these permitted types. First, they need to be declared in the **same package** — or in the same module. Second, they need to be either `final`, `sealed`, or declared `non-sealed`. Enums and records are actually **final** types — so you cannot create an open hierarchy by accident; the `non-sealed` declaration is there for that, when you do want to reopen it on purpose. One last word: sealed types are enforced **both at compile time and at runtime**.

```java
public sealed interface Shape
        permits Circle, Square, Polygon { }

record Circle(double radius) implements Shape { }   // final by nature
non-sealed interface Polygon extends Shape { }      // reopened on purpose

// a switch on Shape: exhaustiveness checked by the compiler!
```

---

### Question 70 — How is List.of() working?
· [video](https://youtu.be/uOKgixLEgcs) · local file `329`

**Short answer:** It takes an array (or varargs) and makes a **defensive copy** to create an unmodifiable list.

**Less short answer:** You can use it with a vararg to create a pre-filled list — but you can also pass an **array**. In that case, remember: if it is an array of **primitive types**, what you get is a list with **one** element — your whole array. This list implementation does **not accept null values**: sometimes your IDE can warn you about that, but in certain cases it cannot — and you will get an exception at **runtime**. One last word: you can also create sets with `Set.of`, that works in the same way as List.of — defensive copy, null values not allowed.

```java
List<String> l = List.of("a", "b");      // defensive copy, immutable

String[] names = {"a", "b"};
List<String> l2 = List.of(names);        // array copied
names[0] = "X";                          // l2 still holds "a" (defensive!)

int[] n = {1, 2};
List.of(n);                              // a List<int[]> with 1 element!
List<Integer> wrong = List.of("a", null); // NPE at runtime
```

---

### Question 71 — What is the difference between Runnable and Thread?
· [video](https://youtu.be/Q5rcoCl6anQ) · local file `328`

**Short answer:** Runnable is an interface; Thread is a class.

**Less short answer:** `Runnable` is a model for a **task** — you can implement it with a lambda that takes nothing and returns nothing; you can execute this lambda by calling its `run` method. You can then create a **Thread** object, passing your runnable as an argument, and execute this runnable **in that thread** by calling the `start` method of the thread object. Be careful not to use the `thread.run()` method: it will execute your runnable, but in the **current** thread. One last word: this pattern is okay for playing with threads and understanding how they work. In a real application, you should use the **ExecutorService** pattern instead of this one.

```java
Runnable task = () -> System.out.println("hello from thread " +
        Thread.currentThread().getName());

Thread t = new Thread(task);
t.start();   // a new thread calls run()
// t.run();  // runs on the CURRENT thread — classic trap!

// production: Executors.newFixedThreadPool(4).submit(task);
```

---

### Question 72 — What is the difference between map and flatMap?
· [video](https://youtu.be/NrZsQ-GpNTo) · local file `327`

**Short answer:** These are two methods from the Stream API, to map objects of a given type to objects of another type.

**Less short answer:** `map` is super simple: it takes a function — like `String::length`, that maps strings of characters to their length. `flatMap` works differently: it maps **one-to-many** relationships — the mapping function should itself return a **stream** of something, and these streams are **flattened** by the flatMap method. For instance, given a stream of **countries**, you can flat-map this stream to a stream of **cities**. One last word: a `map` does **not change** the number of objects your stream processes, whereas a `flatMap` does — it can even produce **zero** elements.

```java
Stream.of("java", "kotlin")
      .map(String::length)                 // 1 -> 1 (4, 6)

Stream.of(brazil, france)
      .flatMap(c -> c.cities().stream())   // 1 -> N
      .forEach(System.out::println);

Stream.of("a", "b")
      .flatMap(s -> Stream.empty());       // 1 -> 0: an empty stream!
```

---

### Question 73 — How is synchronization working?
· [video](https://youtu.be/vUzwaU23VKI) · local file `326`

**Short answer:** It works with a key — the object with which you synchronize your block of code.

**Less short answer:** This key is called the **monitor**. When a thread reaches a synchronized block or method, it asks this object for its key. If the key is available, it takes it and runs the code; if not, it will have to wait until the key comes back. You absolutely want to avoid situations where the key **never comes back** — usually because of deadlocks, but not only. In the case of an **instance** method, the object that holds the key is simply `this`; and in the case of a **static** method, the object is the class. One last word: try to avoid synchronizing on objects that are **public** — that's a good rule to prevent deadlocks, a situation you absolutely want to avoid.

```java
class Resource {
    private final Object key = new Object();  // PRIVATE!
    void use() {
        synchronized (key) { update(); }   // takes the key, uses, returns it
    }
    synchronized void simple() { ... }     // key = this
    static synchronized void st() { ... }  // key = Resource.class
}
```

---

### Question 74 — What is a generic type?
· [video](https://youtu.be/8hrwY3gflSw) · local file `325`

**Short answer:** A type that is parameterized over types.

**Less short answer:** Once defined along with the class, you can use this parameter in the class itself to define instance fields or instance methods — and a single type can be parameterized over **several** types. Generics are massively used in the JDK: the Collections framework, the Stream API — you can check these classes to see how you can use them, and how they were created. One last word: the type T is actually **erased** (*type erasure*) — it is replaced by `Object` at runtime. So there is a number of things you cannot do in your code: you cannot try to get the class of T, because it would just return Object; you cannot call `new T()` or `new T[]` either.

```java
class Pair<A, B> {                  // several parameters
    private final A first; private final B second;
    Pair(A a, B b) { first = a; second = b; }
}

// type erasure at runtime:
new ArrayList<String>().getClass() == new ArrayList<Integer>().getClass();
// true! the T disappears (becomes Object)

// these do NOT exist: new T(), T.class, new T[10] (without tricks)
```

---

### Question 75 — How can you stop a Thread?
· [video](https://youtu.be/f4fajEBqY0g) · local file `324`

**Short answer:** You can't — and you should not.

**Less short answer:** There are two methods on the Thread class, `stop` and `suspend`, that have been **deprecated in Java 2 (1998)** — that is more than 25 years ago. They now throw an `UnsupportedOperationException`, so don't even think about using them. Why? The answer is actually not that simple. In short, they have been deprecated mainly for **security** reasons: being able to stop a thread could put your application in an **unpredictable state**, something you absolutely want to avoid. One last word: you are not supposed to start or stop your threads by hand anyway. The pattern you should be using is the **ExecutorService** pattern, that takes care of your thread lifecycle for you.

```java
Thread t = new Thread(task);
// t.stop();    // deprecated 1998; today: UnsupportedOperationException
// t.suspend(); // same

t.interrupt();   // cooperative cancellation + checks in the task

// production: the executor manages the lifecycle
ExecutorService ex = Executors.newFixedThreadPool(4);
ex.shutdown();   // graceful end
```

---

### Question 76 — How can you remove elements from a stream?
· [video](https://youtu.be/d9_cdTMFQiY) · local file `323`

**Short answer:** Use the `filter` method. *(that is the short answer — and almost all of it)*

**Less short answer:** But be careful: a stream does not contain **any object** — it consumes objects from a source. So saying "removing objects from a stream" is actually not correct: what you are really doing is telling your stream **not to process** certain elements. Filtering uses a predicate that tests certain properties of the object and decides if it should be further processed or not — in the video's example, processing only the **non-empty** strings.

```java
Stream.of("", "a", "", "b")
      .filter(s -> !s.isEmpty())        // "removing" = not processing empties
      .forEach(System.out::println);    // a b

// to really REMOVE (mutating), it's on the collection:
list.removeIf(String::isEmpty);
```

---

### Question 77 — How is Set.of() working?
· [video](https://youtu.be/0Flq8aXF6B8) · local file `322`

**Short answer:** It builds the set of its arguments — **if there are no duplicates** among them.

**Less short answer:** The arguments are added one by one to an array of the right size, using their **hash code** to detect any duplicates — if a duplicate is detected, an `IllegalArgumentException` is immediately thrown. This method only requires **one pass** over your data, and because duplicates are not allowed in your arguments, it can create an array of the right size upfront, making the memory consumption minimal. So all this is pretty optimal. One last word: as usual, be careful if you pass an **array of primitive types**, because you will get a set with a **single element**: your array. Which is usually not what you want.

```java
Set.of("a", "b", "c");              // immutable set, one pass
Set.of(1, 2, 2);                    // IllegalArgumentException!

int[] numbers = {1, 2, 3};
Set.of(numbers);                    // a Set<int[]> with 1 element (the array!)
Set.of(Arrays.stream(numbers).boxed().toArray(Integer[]::new)); // right
```

---

### Question 78 — How can you join strings with a separator?
· [video](https://youtu.be/iAPlqX34YmU) · local file `321`

**Short answer:** You have a `join` factory method for that, directly on the String class.

**Less short answer:** There are two overloads of this method. They both take a **delimiter** as a first argument; then you can pass a **vararg** (or an array) — that's the first overload — or an **Iterable**, typically a collection — that's the second one. If you need to add a **prefix and a suffix** to your string of characters, then you can check the `StringJoiner` class — which you can also **reuse** if you have many strings in the process. One last word: you can also check the `Collectors.joining` pattern, which does the same for the Stream API — that's useful if you need to create this string **lazily**, without you having to buffer everything in a collection.

```java
String.join(", ", "a", "b", "c");              // varargs
String.join("-", List.of("2026", "10", "04")); // Iterable

new StringJoiner(", ", "[", "]")               // prefix and suffix
        .add("a").add("b").toString();         // "[a, b]"

stream.collect(Collectors.joining(", "));      // lazy, in the Stream API
```

---

### Question 79 — How can you modify a field using the Reflection API?
· [video](https://youtu.be/u3byqpOlwqk) · local file `320`

**Short answer:** You can use the `set` method of the `Field` class to do that.

**Less short answer:** This method takes the instance you want to operate on, and the value you want to set. If you do not have access to this field — because it is private, for instance — you can still change it by calling `field.setAccessible(true)`. It does not make the field public, but it **suppresses the access checks** made by the language. One last word though: you can actually **mutate `final` fields** in regular classes with this pattern — which is really annoying. **Records are protected** against that: you cannot mutate them. One more reason to use records wherever you can.

```java
Field f = Config.class.getDeclaredField("timeout");
f.setAccessible(true);              // suppresses the check (JPMS: needs opens)
f.set(config, 5000);                // writes through reflection

// in a regular class: even FINAL is mutable this way (annoying!)
// in a record: the field cannot be reassigned — protected!
```

---

### Question 80 — How can you declare a generic type on a static method?
· [video](https://youtu.be/uZ8XM1dvgRA) · local file `319`

**Short answer:** You need to define the type parameter along with the method declaration.

**Less short answer:** The type parameters you define on the class are only seen by the **instance fields and methods** — not by the static members. Suppose you have a `class Box<T>` and you want to define a static method `copy`: to define the type parameter for this method, you need to add it **after the `static` keyword**. Note also that if you have several static methods, **each one** needs to declare its own type parameter. One last word: you have many examples of that in the JDK — in the `Collections` class, for instance. They also use this wildcard syntax, `? super T` and `? extends T`. *(Another video.)*

```java
class Box<T> {
    T value;                         // T: for INSTANCE members only

    static <U> void copy(Box<U> from, Box<? super U> to) {
        //  ^^^ the METHOD's type parameter (after static)
        to.value = from.value;
    }
    static <V> V identity(V v) { return v; }  // each method, its own
}
```

---

### Question 81 — What does passing by value mean?
· [video](https://youtu.be/Qeb4UQUlINI) · local file `318`

**Short answer:** It means that what you pass to a method is the **value** — not a reference to this value.

**Less short answer:** The consequence is that if your method modifies the argument it gets, the calling code **does not see** this modification. If what you pass is a **primitive type**, changing it in your method does not change the value in your calling code. If what you pass is a **reference** (to an object), changing the reference itself does not change it in the calling code either — but if you change the **content** of the referenced object, then this change **is seen** by the calling code. One last word: Java passes by value. **Always, no exception** (the reference's "value" is what gets copied).

```java
void try(int x, StringBuilder sb, Person p) {
    x = 99;                    // nothing changes outside (primitive: copy)
    sb = new StringBuilder();  // nothing changes outside (reference: copy)
    sb.append("x");            // THIS the caller sees (object content)!
    p.setName("new");          // and this too
}
int n = 1; var sb = new StringBuilder(); var p = new Person("old");
try(n, sb, p);
// n == 1; sb contains "x"; p.name == "new"
```

---

### Question 82 — What is the difference between an intermediate and a terminal operation?
· [video](https://youtu.be/BNgZsLT4DPw) · local file `317`

**Short answer:** An intermediate operation returns a stream; a terminal operation returns something else — including void.

**Less short answer:** The stream processes the data from the source only when it executes its **terminal** operation — if you do not call a terminal operation, no data is processed. And you can only call **one** intermediate or terminal operation on a given stream object — it makes storing a stream in a local variable useless. One last word: some terminal operations **do not even call** the intermediate operations of your stream — one more reason why you should **never** do side effects in an intermediate operation: there are situations where they will not be executed.

```java
var s = list.stream().map(this::transform);  // nothing happened yet!
s.filter(...).count();                       // NOW it processes
// s.anything();  // IllegalStateException: stream already consumed

Stream.of(1, 2, 3).map(n -> { save(n); return n; }).count();
// count() with no need for the map: the side effects may NOT run!
```

---

### Question 83 — What list can you pass to a method that takes a List of Number?
· [video](https://youtu.be/jyrw0eqricE) · local file `316`

**Short answer:** `List<Number>` — and certainly **not** `List<Integer>`, even though Integer extends Number.

**Less short answer:** Even if a type U extends a type T, a `List<U>` does **not** extend `List<T>`. The reason: within the method that takes a List of Number, you could add a **Float** to that list — but then, if you passed a List of Integer at runtime, you would actually end up trying to add a Float to a list of integers, which would cause an exception. One last word: you could change your method and declare that it takes a `List<? extends Number>` — and in that case, you will be able to pass lists of Integer, Double and the like (but writes become forbidden).

```java
void process(List<Number> numbers) {
    numbers.add(1.5f);       // a Float in a Number: legitimate!
}
process(new ArrayList<Number>());     // OK
// process(new ArrayList<Integer>()); // does NOT compile — and it's great:
// otherwise: a Float inside a List<Integer>!

void processAny(List<? extends Number> nums) {
    double s = nums.stream().mapToDouble(Number::doubleValue).sum();
    // nums.add(anything);  // writing is forbidden by the wildcard
}
```

---

### Question 84 — What is a deadlock?
· [video](https://youtu.be/Cs9pYB_Rvmc) · local file `315`

**Short answer:** A situation where a first thread holds lock A and needs lock B to carry on with its task — and a second thread is holding lock B and is waiting for lock A to carry on with its task.

**Less short answer:** This situation is really, really bad: most of the time, your solution will be to **reboot your application**. It can also occur with a second lock that is **outside** of your application — in your database, for instance. One last word — one word of advice to avoid that: **never expose your locks as public elements**. Always use **private fields** to store your locks. It will help you most of the time... but not always, unfortunately.

```java
class Resource {
    private final Object lockA = new Object();  // PRIVATE!
    private final Object lockB = new Object();

    // CONSISTENT acquisition order in ALL the methods:
    void one() { synchronized (lockA) { synchronized (lockB) { ... } } }
    void two() { synchronized (lockA) { synchronized (lockB) { ... } } }
    // two() with the order inverted = classic deadlock
}
```

---

### Question 85 — How can you create an object instance of a given class using the Reflection API?
· [video](https://youtu.be/4x-IB4R-98I) · local file `314`

**Short answer:** There is a pattern for that: `getDeclaredConstructor()` gives you a reference on the empty constructor of this class — call `newInstance()` on it.

**Less short answer:** The pattern that consists in calling `newInstance` directly on the class itself has been **deprecated in Java 9**, mostly because it swallows the exceptions of the constructor. If your class does not have an empty constructor, this pattern throws a `NoSuchMethodException`. One last word: you can also get a reference on the **other constructors** of your class by passing the classes of their parameters to the `getDeclaredConstructor(...)` method.

```java
Object o = Customer.class
        .getDeclaredConstructor()          // the empty constructor
        .newInstance();                    // JDK 9+: the pattern

// with arguments:
Object p = Order.class
        .getDeclaredConstructor(String.class, int.class)
        .newInstance("abc", 3);

// Customer.class.newInstance();  // deprecated since Java 9
```

---

### Question 86 — What is the difference between a collection and a list?
· [video](https://youtu.be/fOxixTsAPjk) · local file `313`

**Short answer:** A list is ordered; a collection is not.

**Less short answer:** "Ordered" means that the order in which you iterate over the elements is **stable** from one iteration to the other, and corresponds to the order in which you added the elements to your list. It is the case for **all** lists, because that's the specification. But it's not the case for a `HashSet` — and because HashSet is a collection, not all collections have this behavior: adding elements to a HashSet may change the iteration order of the other elements; it is not even stable from one iteration to the other. One last word: the consequence is that you can access the elements of your list **using an index** — which would not make sense for sets.

```java
List<String> l = List.of("a", "b", "c");
l.get(1);                   // "b" — index: forever!

Set<String> s = new HashSet<>(Set.of("a", "b", "c"));
// s.get(...)? doesn't exist; iteration order may change on insertion
```

---

### Question 87 — What is the difference between toList() and Collectors.toList()?
· [video](https://youtu.be/z5vdnTGjwRI) · local file `312`

**Short answer:** Both are terminal operations of the Stream API. `Collectors.toList()` produces an ArrayList; `toList()` produces an **unmodifiable** list.

**Less short answer:** The fact that Collectors.toList() produces an ArrayList is actually **not specified** — but it is the case. There are cases where `toList()` is actually more than a convenience — it is **more efficient**: in a case where you know how many elements your stream is going to process, toList() can create an array of the right size upfront, whereas `collect(toList())` relies on the automatic growing of your ArrayList. One last word: the `Stream.toList()` method produces an unmodifiable list that **accepts null values** — not all unmodifiable lists do: List.of doesn't, for instance.

```java
List<Integer> mutable = stream.collect(Collectors.toList());
mutable.add(42);                      // OK (an ArrayList in practice)

List<Integer> fixed = stream.toList(); // JDK 16+
// fixed.add(42);                      // UnsupportedOperationException
List<Integer> withNull = Stream.of(1, null, 3).toList(); // nulls OK!
// List.of(1, null);                   // NPE: List.of doesn't accept
```

---

### Question 88 — Can you compare integers and doubles with a comparator of numbers?
· [video](https://youtu.be/91Btf8FkNmg) · local file `311`

**Short answer:** Yes.

**Less short answer:** This is fine, because what you are defining is a method that takes two parameters of type **Number** — and it's fine to call it with any extension of Number. Actually, you could use such a comparator to define the comparison **between** integers and floating point numbers, for instance. Such a comparator is also okay to sort any list of Integers **or** Doubles, because all these types extend Number. One last word: don't make your comparator a `Comparator<? extends Number>` — because in that case you will not be able to call it with any other simple type. There is a good reason for that... but that would be for another video.

```java
Comparator<Number> byValue = Comparator
        .comparingDouble(Number::doubleValue);

byValue.compare(3, 2.5);              // int vs double: OK!
List<Integer> is = ...; is.sort(byValue);
List<Double>  ds = ...; ds.sort(byValue);

// Comparator<? extends Number> c = ...;
// c.compare(1, 2);   // doesn't compile! wildcard: arguments out
```

---

### Question 89 — How can you model a concurrent task in Java?
· [video](https://youtu.be/FiH1qzoXtmM) · local file `310`

**Short answer:** You have two interfaces for that: Runnable and Callable.

**Less short answer:** Both are functional interfaces, so you can implement them with lambdas. Neither of them takes any parameter. **Runnable** does not return anything and does not throw any checked exception; a **Callable**, on the other hand, can return a value and throw an exception. Runnable is the only task you can pass to the constructor or the builder of a **Thread**; Callable and Runnable can be passed to an **ExecutorService** or to a **StructuredTaskScope**. One last word: when you submit a task to an ExecutorService, you get a **Future** object in return — something we will talk more about in the future, precisely.

```java
Runnable t1 = () -> process();                 // void
Callable<Integer> t2 = () -> compute();        // returns, throws

Thread.ofPlatform().start(t1);                 // Runnable only here
executor.submit(t1);                           // Future<?>
Future<Integer> f = executor.submit(t2);       // Future<Integer>

try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    scope.fork(t2);                            // Callable in the scope
    scope.join();
}
```

---

### Question 90 — What is an annotation?
· [video](https://youtu.be/pPZSFsO1hqw) · local file `309`

**Short answer:** An interface with methods on which you can define default values.

**Less short answer:** You can define your own annotations easily, with methods that may return default values. Along with your annotation, you need to define two things: **where** you can add this annotation in your code, and **when** you want to access it. There are currently **12 places** (targets) where you can add an annotation — fields, classes, methods, or parameters among them. Then you can declare that you will need to use your annotation **at runtime**, only when your class is loaded, or at **compile time**. One last word: at runtime, you need to use the Reflection API to get the annotations added to an element of your code — a technique used by many frameworks: Hibernate or Spring Boot, just to name a few.

```java
@Target({ElementType.TYPE, ElementType.METHOD})   // where (12 options)
@Retention(RetentionPolicy.RUNTIME)               // when: runtime
public @interface Audited {
    String user() default "system";
}

@Audited(user = "ana")
class Service { }
Service.class.getAnnotation(Audited.class);   // reflection reads at runtime
```

---

### Question 91 — What is a question mark extends T?
· [video](https://youtu.be/d650vebo2JY) · local file `308`

**Short answer:** It is a type called a **wildcard**, that has been created to allow a parameterized class to accept extensions of its parameter.

**Less short answer:** Suppose you have two types, T and U, and U extends T: a class parameterized on U does **not** extend the same class parameterized on T. Look at this `sum` method: it won't accept a `List<Double>`. We can fix that with a `List<? extends Number>`, because `List<Double>` **extends** `List<? extends Number>`. One last word: this technique is extensively used in the Collections framework and in the lambda world — comparators, consumers, predicates, functions: they all use wildcards.

```java
static double sum(List<Number> nums) { ... }
sum(List.of(1.5, 2.5));   // ERROR! List<Double> is not List<Number>

static double sumRight(List<? extends Number> nums) {   // wildcard!
    return nums.stream().mapToDouble(Number::doubleValue).sum();
}
sumRight(List.of(1.5, 2.5));   // OK: reading from extends
sumRight(List.of(1, 2));       // OK: Integer too
```

---

### Question 92 — What is a constructor?
· [video](https://youtu.be/utoOZ6Q0jYc) · local file `307`

**Short answer:** An element of a class that is called when an instance of this class is built.

**Less short answer:** Be careful — a constructor is **not a method**: it does not have a return type, and it is not compiled in the same way. You can have several overloads of a constructor in a class; all they need is to declare different parameters. Constructors are the right place for **validation** — to prevent corrupted objects from being built. One last word: you can write **compact constructors** in records — a very nice and very simple syntax to write constructors that only do validation. One more reason to use records wherever you want.

```java
class Age {
    private final int value;
    Age(int value) {                          // validation at the source
        if (value < 0 || value > 150)
            throw new IllegalArgumentException("age: " + value);
        this.value = value;
    }
}

record Range(int min, int max) {
    Range {                                    // compact: validation only
        if (min > max) throw new IllegalArgumentException();
    }
}
```

---

### Question 93 — What is a short-circuit method?
· [video](https://youtu.be/xt1uTAOXXd8) · local file `306`

**Short answer:** Several methods from the Stream API are called short-circuit methods.

**Less short answer:** These methods may **interrupt the consumption** of the elements of your source. There are two intermediate operations — `limit` and `takeWhile` — and five terminal operations — `allMatch`, `anyMatch`, `noneMatch`, `findFirst` and `findAny`. Because elements are processed **lazily, one by one** by the Stream API, short-circuit operations can prevent some elements from being read from your source — which may be really bad if you are doing **side effects** in your intermediate operations. One last word: doing side effects in a stream intermediate or terminal operation is always a bad idea. Don't do it.

```java
Stream.of(1, 2, 3, 4, 5, 6)
      .peek(n -> saveToDatabase(n))   // SIDE EFFECT: 5 and 6 NEVER run!
      .takeWhile(n -> n < 4)
      .toList();

list.stream().anyMatch(this::critical);   // stops at the first true
```

---

### Question 94 — What is an atomic variable?
· [video](https://youtu.be/Lz6Kl2B5VqY) · local file `305`

**Short answer:** A wrapper on a primitive type or a reference, with methods to mutate it in a **thread-safe and efficient** way.

**Less short answer:** Atomic variables are actually mapping CPU operations, also called **CAS** operations — CAS means *compare-and-swap*. For instance, on an AtomicInteger, you can call the `incrementAndGet()` method: it increments the internal value and returns it — **atomically**. This operation is thread-safe and does **not** rely on synchronization, which prevents thread contention. One last word: atomic references are used to implement two thread-safe structures from the Collections framework — `ConcurrentSkipListMap`, which is a map, and `ConcurrentSkipListSet`, which is a set — all this without any synchronization.

```java
AtomicInteger counter = new AtomicInteger();
counter.incrementAndGet();          // CAS: read-compare-swap in 1 step
counter.compareAndSet(5, 10);       // swaps only if the value is 5
counter.updateAndGet(n -> n * 2);   // atomic function

// no lock, no contention — the basis of ConcurrentSkipListMap/Set
```

---

### Question 95 — What is a record?
· [video](https://youtu.be/fozD9MBIabE) · local file `304`

**Short answer:** Something you should use all the time.

**Less short answer:** It is a class that carries some **immutable state**, declared along with its declaration — this state is called the **components** of your record. A record cannot extend anything and cannot be extended, but it can implement any interface. It cannot declare an instance field (static fields are okay), and it can declare any method. The compiler gives you a constructor, accessors — that you can rewrite yourself — and toString, equals and hashCode for free. You can do some validation, and deserialization calls this constructor — so no corrupted records in your application. One last word: use records wherever and whenever you can.

```java
public record Ssn(String value) {
    Ssn {                                        // guaranteed validation
        if (!value.matches("\\d{9}"))
            throw new IllegalArgumentException("invalid SSN");
    }
    // constructor + value() + equals/hashCode + toString: free!
}
```

---

### Question 96 (A) — Why should you favor composition over inheritance?
· [video](https://youtu.be/DIZTQxkOsy4) · local file `003`

**Short answer:** Because it's written in the manual. *(the presenter's joke: "you read the f\*\*\*ing manual, didn't you?")*

**Less short answer:** The real question is the following: when you need to add a given behavior to an object, should you create a method on that object, or should you create a **delegate** that would implement this behavior? Imagine this is the year 2005, and you need to marshal your objects in XML. Solution 1: you create two XML methods on the class; solution 2, you create these two XML methods in a factory class. Now, this is 2025, 20 years later — and you do not need this XML anymore, but you need this JSON. Solution 1: you add the two JSON methods... but the real question is: what do you do with the now useless XML methods? From a functional point of view, this is **dead code**; and odds are that you will decide to keep it, because it would be too costly to refactor your application to remove all the calls to these methods. In solution 2, you just **throw the class away**, because nobody should be calling it anymore. Composition makes the decoupling of the different modules of your application much easier. And if you think this is just about organizing your application in a better way — well, you are mistaken: this is about making your code easier to **dispose of** when you do not need it anymore, thus preventing dead code from staying there and plaguing your application.

```java
// Solution 1: methods on the class itself (dead code piles up)
class Order {
    String toXML() { /* 2005: nobody calls this anymore */ }
    String toJSON() { return "..."; }
}

// Solution 2: composition — disposable delegates
class XMLMarshaller { String marshal(Order o) { ... } } // throw in the trash
class JSONMarshaller { String marshal(Order o) { ... } }
```

---

### Question 96 (B) — How can you invoke a method using the Reflection API?
· [video](https://youtu.be/WQNaBPDaknw) · local file `303`

**Short answer:** There is an `invoke` method in the `Method` class.

**Less short answer:** You can get a specific method of a class with the `getMethod` or `getDeclaredMethod` method of the class named `Class`. You need to pass the **name** of the method you want, along with the **types of its parameters**. When you have a Method object, all you need to do is call the `invoke` method on it, passing first the **object** on which you want to invoke this method (null for static methods), and then the **arguments** needed by this method. You can invoke a private method from outside the class by first calling `setAccessible(true)` — which suppresses the access checks made by the language.

```java
Method m = String.class.getMethod("substring", int.class, int.class);
String r = (String) m.invoke("hello world", 0, 5);   // "hello"

Method privateM = Mine.class.getDeclaredMethod("secret");
privateM.setAccessible(true);                         // drops the check
String s = (String) privateM.invoke(myInstance);
```

> *Note: the series has two questions numbered 96 by the channel itself — this one and Question 96 (composition over inheritance, video 003).*

---

### Question 97 — What is a ConcurrentModificationException?
· [video](https://youtu.be/IrahCREGwBo) · local file `302`

**Short answer:** An exception raised when you modify your collection **while iterating** over it.

**Less short answer:** Be careful — despite the name, it has **nothing to do** with concurrent programming. Removing elements from a collection while iterating over it is generally not a great idea. There is a `remove()` method on the **iterator** — you can use it, just make sure that it is supported by your implementation. If what you need is to remove elements using a **predicate**, for instance, you can use the **`removeIf`** method, that does exactly that. One last word: some implementations of collections are okay with removing elements while iterating over them — especially the **concurrent** ones, like a LinkedBlockingQueue or a ConcurrentLinkedQueue — so be sure to check the JavaDoc on this one.

```java
for (String s : list) {
    if (s.isEmpty()) list.remove(s);   // CME (same thread!)
}

Iterator<String> it = list.iterator();  // the right way:
while (it.hasNext())
    if (it.next().isEmpty()) it.remove();

list.removeIf(String::isEmpty);         // and the MODERN way
new ConcurrentLinkedQueue<>(list)       // concurrent: they don't even throw
```

---

### Question 98 — What is a collector?
· [video](https://youtu.be/fJRdhX_uuWQ) · local file `301`

**Short answer:** An object provided by the Stream API that you can use to **reduce** streams using the collect method.

**Less short answer:** A collector is an implementation of the `Collector` interface — which you can implement yourself. You also have a `Collectors` factory class, with plenty of pre-built collectors that should cover most of your needs. You can do many things with collectors: you can create any sort of collections or maps; you can **group** your stream elements using keys and gather them in maps; or you can **join** them in strings of characters. You even have collectors that can **map**, **filter** or **flat map** the elements of your stream. One last word: sometimes the Stream API uses *downstream collectors* — a way to **compose** collectors. *(Another video.)*

```java
List<String> list = stream.collect(Collectors.toList());
Map<String, List<P>> groups = stream.collect(Collectors.groupingBy(P::key));
String joined = stream.collect(Collectors.joining(", "));
Set<Integer> set = stream.collect(Collectors.toSet());
// and Collector.of(...) for your own
```

---

### Question 99 — What is a daemon thread?
· [video](https://youtu.be/6p1iV2WrF-g) · local file `300`

**Short answer:** It is a thread.

**Less short answer:** When you create a thread, you can decide to set its **daemon** property to true. The JVM starts its **shutdown sequence** when it does not have any more **non-daemon** threads running. The `main` thread, for instance — the one that runs your main method — is a non-daemon thread: so when you exit your main method and no other non-daemon thread is left, the JVM shuts down. Non-daemon threads **keep your JVM alive** — they can be created in ExecutorServices, for instance, so don't forget to shut them down, or your JVM will never shut down. Two examples of daemon threads: the ones that are running your **garbage collector** or your **just-in-time compiler**. One last word: you can set the daemon property of a platform thread, but a **virtual thread is always a daemon** thread.

```java
Thread cleaner = Thread.ofPlatform().daemon(true).start(() -> clean());

ExecutorService ex = Executors.newFixedThreadPool(4);
// NON-daemon threads: the JVM won't exit without...
ex.shutdown();        // ...calling this (otherwise, the app "hangs")

Thread.ofVirtual().start(t);  // virtual thread: always a daemon
```

---

### Question 100 — What is a race condition?
· [video](https://youtu.be/S-XPPT6wX54) · local file `299`

**Short answer:** A race condition occurs when two threads are **writing and reading the same variable at the same time**.

**Less short answer:** This is a situation you need to take care of, because it can put your application in an **inconsistent state**. Suppose that you are incrementing a simple counter: you first read its value, increment it, and write it back. Two threads doing that on two different cores of your CPU may be reading the **same value** — and what you will see is **one increment instead of two**. To prevent race conditions, you need to guard your variables with **synchronization** or use **atomic variables**. One last word: in Java, race conditions can only happen in the **fields** of your classes — not on local variables. It can be different in other languages.

```java
class Counter {
    private int n;                     // field: subject to races!
    void inc() { n++; }                // read + add + write: 3 steps

    // the defenses:
    private final AtomicInteger n2 = new AtomicInteger();
    void inc2() { n2.incrementAndGet(); }        // atomic (CAS)
    private synchronized void inc3() { n++; }    // mutual exclusion
}
// locals (an int x inside a method) never race: they live on the stack
```

---

### Question 101 — How can you check if a class is an extension of another?
· [video](https://youtu.be/GdDrvoQxQ7s) · local file `298`

**Short answer:** There is a method for that in the `Class` class.

**Less short answer:** This method is called **`isAssignableFrom`**. You call it on a Class instance — which could represent an interface, by the way — and pass another Class instance as an argument. `isAssignableFrom` checks if an instance of the argument class you pass can be **assigned** to a variable of the type you are calling this method on. For instance, you can check if you can assign a String object to a variable of type `Comparable` with the following code. One last word: this is a method that you should use with types that you **don't know at compile time** — if you do, then you probably don't need it.

```java
Comparable.class.isAssignableFrom(String.class);  // true
Number.class.isAssignableFrom(Integer.class);     // true
List.class.isAssignableFrom(ArrayList.class);     // true

// for types known at compile time:
if (obj instanceof String s) { ... }
// reflection: Class.isInstance(obj) is the "dynamic instanceof"
String.class.isInstance(obj);
```

---

### Question 102 — What is the type ? super T?
· [video](https://youtu.be/y5WyL-1m_9k) · local file `297`

**Short answer:** This type represents all the types that are **supertypes** of T.

**Less short answer:** This is the kind of type you need when you need to define the type of an object that you **consume**. Suppose that you have a list of `Polygon` objects: you know that all the objects of this list can be cast to the Polygon type — so processing them with a `Consumer<Shape>` would be okay too (Shape is a supertype). That is why the `forEach` method takes a consumer that can consume **any type that is a supertype** of Polygon; and the same goes for methods that take functions — these functions are declared to take `? super T`. One last word: as this type can be any supertype of T, in practice, with `? super T` you can **write** T values and only **read** them back as `Object`.

```java
List<Polygon> polygons = List.of(...);

// Consumer<? super Polygon>: accepts Consumer<Shape>, <Object>...
polygons.forEach((Consumer<Shape>) s -> draw(s));

// writing OK; reading only as Object:
List<? super Polygon> box = new ArrayList<Shape>();
box.add(new Polygon(...));    // writes a Polygon: guaranteed
Object o = box.get(0);        // reads: Object only
```

---

### Question 103 — What does static mean?
· [video](https://youtu.be/2KkGjWz_R-g) · local file `296`

**Short answer:** Static can be added on a field or a method declaration. It means that this member is attached to the **type**, not to an instance.

**Less short answer:** Static fields can be defined on classes; static methods can be added to classes **or interfaces**. Because a static member belongs to a class, you should use it **directly with the class name**. If it is a field, all your instances **share** this field — so using **mutable** static fields usually means trouble. A fun fact: if you invoke a static member with an instance, the compiler will **replace that instance with the class** — meaning that you can read a static field using a **null** instance without throwing a NullPointerException! One last word: you can also define **static blocks** in your class — but please stay away from that. This is an anti-pattern.

```java
class Cache {
    static final int SIZE = 100;   // of the class, shared
    static int counter;            // mutable static = trouble!

    static void clear() { }        // classes AND interfaces
}

Cache c = null;
c.SIZE;        // works! the compiler swaps in Cache.SIZE
// static { ... }  // static block: anti-pattern, avoid
```

---

### Question 104 — What is a downstream collector?
· [video](https://youtu.be/psNGXr-Fd98) · local file `295`

**Short answer:** A collector.

**Less short answer:** Most of the time you will create your collectors with one of the factory methods of the `Collectors` class. Some of these methods take a collector as a **last parameter**, called the *downstream collector*. This collector is used to **further process** your data. For instance, the `groupingBy` collector groups your elements using a key; the downstream collector that you use is what you choose to do with these groups — you can put them in a list, a set, you can join them, or you can even create **another map**. One last word: some factory methods **always** take a downstream collector — this is the case for the `mapping`, `filtering` or `flatMapping` methods: they actually define intermediate operations, and need this downstream collector to model the terminal operation that will process your stream.

```java
Map<String, List<Person>> byCity = people.stream()
        .collect(Collectors.groupingBy(Person::city));  // implicit toList

Map<String, Set<String>> namesByCity = people.stream()
        .collect(Collectors.groupingBy(
                Person::city,
                Collectors.mapping(Person::name,   // downstream in a chain
                        Collectors.toSet())));

Map<String, Long> counts = people.stream()
        .collect(Collectors.groupingBy(Person::city, Collectors.counting()));
```

---

### Question 105 — Why is Thread.stop() deprecated?
· [video](https://youtu.be/ZBarqW0ZIoY) · local file `294`

**Short answer:** Because it is unsafe.

**Less short answer:** Being able to interrupt the thread could lead to several problems in your application. If your thread is holding a **synchronization key** — because it is currently updating some shared in-memory structure — interrupting it would **free these keys** and expose this structure, leaving it in a state that could be **inconsistent**. And the same goes if your thread is holding a **system resource** that needs to be closed: not closing that resource properly could lead to a resource-leaking situation you want to avoid. One last word: this method has been deprecated in **Java 1.2, in 1998** — that was a long time ago. It now throws an `UnsupportedOperationException` to make sure that you don't call it.

```java
// Java 1.0: thread.stop() — killed the thread and released the monitors:
// another thread saw the structure HALF-WAY UPDATED (silent corruption)

// deprecated since 1998; today:
thread.stop();   // UnsupportedOperationException!

// cooperative cancellation:
thread.interrupt();
// and in the task: check isInterrupted() / handle InterruptedException
```

---

### Question 106 — What does Comparable mean?
· [video](https://youtu.be/eJF7nMLG-xE) · local file `293`

**Short answer:** It means that you can compare these objects **without** a comparator.

**Less short answer:** There are two mechanisms in Java to compare objects and tell if one is greater than the other. First, you can create a **Comparator** that takes two objects and returns an int. And then you can implement the **Comparable** interface, that basically does the same: you take another object and return an int. This int should be **positive** if `this` is greater than the other, **negative** if not, and **zero** if these objects are equal. With a Comparator, you can compare objects with **another semantic** than the one implemented by Comparable. One last word: remember that the comparison should be **symmetric and transitive** — even if it is not, you may bump into the famous *"Comparison method violates its general contract!"* error message.

```java
record Product(String name, double price) implements Comparable<Product> {
    public int compareTo(Product o) {           // natural order: price
        return Double.compare(price, o.price);
    }
}
// another semantic, without changing the class:
Comparator<Product> byName = Comparator.comparing(Product::name);

// contract breaks (not transitive, etc.) =>
// "Comparison method violates its general contract!" (TimSort)
```

---

### Question 107 — How can you create a pre-filled map?
· [video](https://youtu.be/swD5czProLI) · local file `292`

**Short answer:** There is a method for that: `Map.of`.

**Less short answer:** Map.of is super simple to use. Just call it and pass a key, its value, then the next key, its value, and again — **up to 10 entries**. Past 10 entries you need to change the pattern, because you cannot overload this kind of method with a vararg; so another method has been added: **`Map.ofEntries(...)`**, that takes a vararg of entries that you can create with `Map.entry(k, v)`. The map you get is **non-modifiable**, and the entry objects are also non-modifiable. You cannot pass the **same key twice** — if you do that, you will get an exception. One last word: none of these methods accept **null** — neither for the key, nor for the value, nor for the entry. Really. *(Why would you do that?)*

```java
Map<String, Integer> small = Map.of("a", 1, "b", 2);   // up to 10 pairs

Map<String, Integer> large = Map.ofEntries(             // 11+: entries
        Map.entry("k1", 1), Map.entry("k2", 2), Map.entry("k3", 3));

// small.put("c", 3);     -> UnsupportedOperationException
// Map.of("a", 1, "a", 2); -> IllegalArgumentException (repeated key)
// Map.of("a", null);      -> NPE: nulls, never
```

---

### Question 108 — What is the return type of max()?
· [video](https://youtu.be/RO4hXJ_75Aw) · local file `291`

**Short answer:** An Optional.

**Less short answer:** Whether you like your nulls or not — or think they are a good thing or not — in that particular case, this is what you need. Suppose you compute the max of a first stream, and then the max of a second stream; and then you need the max of all the elements of both streams. What you expect is that the max of both maxes gives you the max of all your elements — and it will... **unless one of the streams is empty**. If you choose to handle this case by returning **null**, then you cannot handle primitive types. And choosing a **default value** would not work either: `Integer.MIN_VALUE` will not work, for instance, because you can convert this int to a long — and then the min of a long is even smaller. The empty Optional is the correct answer left. One last word: there are several very interesting patterns to process Optionals that will make Optional look like streams. *(Another video.)*

```java
Optional<Integer> m1 = Stream.<Integer>empty().max(Integer::compare);
Optional<Integer> m2 = Stream.of(5).max(Integer::compare);

// combining maxes (an empty one doesn't spoil the count):
Optional<Integer> overall = Stream.of(m1, m2)
        .flatMap(Optional::stream)
        .max(Integer::compare);    // Optional[5]
```

---

### Question 109 — What is a Future?
· [video](https://youtu.be/-RuQ73OMh-c) · local file `290`

**Short answer:** An object to communicate with the task that is being executed in another thread.

**Less short answer:** You get a Future object when you submit a Runnable or a Callable to an **ExecutorService**. The ExecutorService immediately gives you a Future object, but it will execute your task in another thread — sometimes in the future. At this point, you can use this Future to check if your task is done or not, and to get either the result it produced **or the exception** that was thrown. There is another overload of the `get()` method that takes a **time out** — this may be safer; prefer this pattern over a regular get. One last word: starting with Java 21, you can call `state()`, that returns RUNNING, SUCCESS, FAILED or CANCELLED.

```java
ExecutorService ex = Executors.newFixedThreadPool(4);
Future<Integer> f = ex.submit(() -> compute());

f.isDone();                          // is it done?
int r = f.get(1, TimeUnit.SECONDS);  // with a timeout: safer
// f.get() returns the result OR rethrows the task's exception
f.state();                           // JDK 21+: RUNNING, SUCCESS, FAILED...
```

---

### Question 110 — What does multiple inheritance mean?
· [video](https://youtu.be/gPDu1G_xCE0) · local file `289`

**Short answer:** It means trouble. *(the presenter's blunt short answer)*

**Less short answer:** Multiple inheritance is an object-oriented notion — and there are **three types** of inheritance. Inheritance of **type**: Java supports multiple inheritance of type — a class can implement as many interfaces as you need. Inheritance of **behavior**: Java supports it also — concrete methods are inherited from a superclass, and also from **default methods** in interfaces; and you may run into issues with this one, if you implement two interfaces with the same default method. And the last one is the inheritance of **state**: that is **not** supported by Java. One last word: "favor object composition over class inheritance" — that's the introduction of the GoF. A must-read.

```java
interface A { default String m() { return "A"; } }
interface B { default String m() { return "B"; } }

class C implements A, B {          // multiple inheritance of TYPE: OK
    public String m() { return A.super.m(); }  // conflict resolved by force
}
// multiple inheritance of STATE: does not exist in Java (one superclass)
```

---

### Question 111 — What is the no-arg empty constructor?
· [video](https://youtu.be/51Vrqyf3wFY) · local file `288`

**Short answer:** A constructor that the compiler adds to a class when you don't declare any.

**Less short answer:** If you create a class and you don't provide your own constructor, the compiler will add one for you — because every class should have at least one constructor. This constructor **does not do anything and does not take any parameter** — thus its name. This may be confusing: if you create a constructor later, then this empty constructor is **not added anymore**, and that can create compiler errors somewhere else in your application. Of course, you can still add one by hand to fix that. One last word: many **frameworks that rely on reflection** are using this no-arg constructor — so there are many cases where it is wise to keep it.

```java
public class Customer {
    // nothing written: the compiler generates Customer() { super(); }
}

public class Order {
    private Order(String id) { }    // declared -> the no-arg is GONE
    // new Order()         -> does not compile anymore
    // a reflection-based framework: needs a public Order()!
}
```

---

### Question 112 — What is Iterable?
· [video](https://youtu.be/f4U8_lawPo4) · local file `287`

**Short answer:** An interface.

**Less short answer:** This interface is extended by `Collection`, so it is actually the base interface of many interfaces of the Collections framework. It has only **one abstract method**, `iterator()` — so you can implement it with a lambda expression. All Iterables can be used in the **for-each** pattern — and this is very handy: if you have an iterator in your application, you can use it to create an iterable and then go through all the values of this iterator using your iterable in this for-each pattern. One last word: Iterable also declares a `forEach` method — so you can also go through the values generated by your iterator using this pattern.

```java
class Numbers implements Iterable<Integer> {
    public Iterator<Integer> iterator() {     // the only abstract method
        return List.of(1, 2, 3).iterator();
    }
}
for (int n : new Numbers()) { ... }           // for-each, straight away!

Iterable<String> fromIterator = () -> myIterator;  // an iterator lambda
fromIterator.forEach(System.out::println);
```

---

### Question 113 — What does Thread.interrupt() do?
· [video](https://youtu.be/S2A-0jt5ESI) · local file `286`

**Short answer:** It doesn't do what you think.

**Less short answer:** Thread.interrupt tries to interrupt your thread **in two ways**. First, it can make one of your calls **throw an exception**: if your task is blocked on a call to `sleep`, `wait` or `join`, then this method immediately throws an `InterruptedException` — the same if you are waiting for a permit from a **semaphore**; there are also many cases where, if you are waiting for some data on an IO request, an exception will be thrown too. In **all the other cases**, your task should regularly call the `isInterrupted()` method — and if it returns true, then it is **your responsibility** to finish your task as soon as possible. One last word: don't try to handle your threads yourself — there are Executor Services and ForkJoin Pools especially for that.

```java
Thread worker = Thread.ofPlatform().start(() -> {
    while (!Thread.currentThread().isInterrupted()) {  // regular check
        try { processBatch(); }
        catch (InterruptedException e) { return; }      // it came from outside!
    }
});
worker.interrupt();   // "asks" to stop; it kills nobody
```

---

### Question 114 — What is the first thing a constructor does? *(version 3)*
· [video](https://youtu.be/6yMsAaI-fSg) · local file `285`

**Short answer:** It calls another constructor.

**Less short answer:** This other constructor can be a constructor from the same class, or from the superclass. If you don't write any call to any constructor, then the compiler adds a call to the **empty constructor** of your superclass — so if this empty constructor does not exist in your superclass, then you will get a compiler error. You can also call a **specific** constructor: a call to `this(...)` calls a constructor from the same class, and a call to `super(...)` calls a constructor from the superclass. Constructing an object requires that you call **all** the constructors, from `Object` down to your class. One last word: this may be annoying when you need to **validate your arguments** — and it may actually change soon (statements before super, JEP 513). *(Another video.)*

```java
class A { A() { } A(String s) { } }

class B extends A {
    B() { super(); }            // the compiler inserts it if missing
    B(String s) { super(s); }   // picks a specific constructor
}

class C {
    C() { this("default"); }    // this(...) delegates in the same class
    C(String s) { }
}   // every chain ends in Object()
```

---

### Question 115 — What are the four fundamental functional interfaces in Java?
· [video](https://youtu.be/mz8qio7f5lQ) · local file `284`

**Short answer:** Consumer, Supplier, Function and Predicate.

**Less short answer:** These are the four functional interfaces from the `java.util.function` package. A **Consumer** takes an argument and doesn't return anything; a **Supplier** doesn't take anything and returns something; a **Function** takes something and returns something; a **Predicate** is a "function" that returns a **boolean**. Then there are a number of variations: `BiConsumer`, `BiFunction`, `BiPredicate` — that take two arguments; there are also `UnaryOperator` and `BinaryOperator` — for the same type in and out — as well as specializations for primitive types, for instance `IntConsumer`. One last word: there is also a `Runnable`, that does not take any argument and does not return anything — it fits nicely in this family.

```java
Consumer<String>  consumer = System.out::println;   // T -> void
Supplier<List<String>> sup = ArrayList::new;        // () -> T
Function<String, Integer> f = String::length;       // T -> R
Predicate<String> pred = s -> s.isEmpty();          // T -> boolean

BiFunction<Integer, Integer, Integer> sum = Integer::sum;
Runnable nothing = () -> {};                        // () -> void
```

---

### Question 116 — What is an enumeration?
· [video](https://youtu.be/Hnq3MrkJDSM) · local file `283`

**Short answer:** A class with a set of instances declared **at compile time**.

**Less short answer:** Enumerations are interesting when you know that a class can only have a **limited number of instances**. Enumerations can have a state — even a mutable state ("why would you do that?") — and you can declare a constructor for them. Your enum class is not a regular class: it is **final** and extends the `Enum` class — so your enumeration cannot extend anything and cannot be extended, but it **can implement interfaces**. One last word: because you cannot create any more instances of an enumeration at runtime, using enumerations is the **preferred pattern to create singletons**. You can see examples of that in the `naturalOrder()` factory method of the Comparator interface.

```java
enum Operation implements BiFunction<Double, Double, Double> { // interface OK
    ADD { public Double apply(Double a, Double b) { return a + b; } },
    SUB { public Double apply(Double a, Double b) { return a - b; } };
    // extends Enum (final): extends nothing, cannot be extended
}
enum Singleton { INSTANCE; }   // the idiomatic singleton
```

---

### Question 117 — How is List.subList() working?
· [video](https://youtu.be/ibgtRdqBIwc) · local file `282`

**Short answer:** It gives you a **mutable view** of a given part of a list.

**Less short answer:** It's actually a nifty feature — and probably an overlooked one. First, it's a list, so you can use it as such. But it's also a **mutable view**, so you can use it to manipulate the first list. For instance, you can insert a list in another one, at a given place, by calling `subList(i, i).addAll(other)` — passing the same index for the from-index and the to-index — and calling addAll on that view. Or you can **clear a range of indexes** from a list with `subList(from, to).clear()`. One last word: if what you need is to select a range of indexes — and you don't need the rest of the list anymore — then you should **copy** the sublist; because if you don't, you will still have the original list in memory, which can be an overhead.

```java
List<String> l = new ArrayList<>(List.of("a", "d"));
l.subList(1, 1).addAll(List.of("b", "c"));  // inserts at index 1
// [a, b, c, d]
l.subList(0, 2).clear();                    // erases the range [0, 2)
// [c, d]

List<String> justTheSlice = new ArrayList<>(l.subList(0, 1)); // a copy!
// without the copy, the whole list stays retained by the "view"
```

---

### Question 118 — How can you create a map to hold 20 elements?
· [video](https://youtu.be/IyOjyoK3jG0) · local file `281`

**Short answer:** There is a factory method for that: `HashMap.newHashMap(n)`.

**Less short answer:** Creating a map with a given capacity is more complex than it seems. The HashMap carries an internal array; when the number of used cells reaches a certain level (the **load factor**), the content of the array is copied into a larger array — a costly operation. Hence the interest in creating an array **large enough upfront** — but then there is some math to do to get that size, and that math has been **written for you** in this factory method (JDK 19+): you give it the number of entries you expect, and it computes the right capacity. One last word: be careful — HashMap also has a constructor that takes an int, but that value is the capacity of the internal array, which may be too small to store all your key-value pairs.

```java
// 20 entries without rehashing:
Map<String, Integer> m1 = HashMap.newHashMap(20);   // does the right math!

Map<String, Integer> m2 = new HashMap<>(20);        // TRAP:
// 20 = array capacity; with load factor 0.75 -> only ~15 entries
new HashMap<>(27);  // (int)(20/0.75)+1 = the "old" manual computation
```

---

### Question 119 — How can you use flatMap to filter a stream?
· [video](https://youtu.be/iEWHbGuzLlo) · local file `280`

**Short answer:** Your flatMap method should return an **empty stream** when you don't want to pass the corresponding value.

**Less short answer:** Sometimes the rules for filtering a stream are **too complex or too costly** — this is the case in this example, where the condition is "parsing your string should **not** throw a `NumberFormatException`". The complete code for that is complex and really not nice; using a `flatMap` and returning an **empty stream** if the result cannot be computed is probably better. One last word: there is actually another method to do the same thing that will **save the construction of the stream object** — it's the `mapMulti` method, that was added in JDK 16.

```java
// ugly: a try/catch inside the filter
.filter(s -> { try { Integer.parseInt(s); return true; }
               catch (NumberFormatException e) { return false; } })

// better: flatMap with an empty stream = "rejected"
.flatMap(s -> parseSafely(s).stream())

Optional<Integer> parseSafely(String s) {
    try { return Optional.of(Integer.parseInt(s)); }
    catch (NumberFormatException e) { return Optional.empty(); }
}

// JDK 16+: mapMulti, without allocating streams
.mapMulti((String s, Consumer<Integer> down) -> {
    try { down.accept(Integer.parseInt(s)); }
    catch (NumberFormatException ignored) { }
})
```

---

### Question 120 — What does reentrant mean for synchronization?
· [video](https://youtu.be/jheUjeXfVPE) · local file `279`

**Short answer:** It means that, when your thread **already has** the lock of a synchronized block, it can **enter it again**.

**Less short answer:** To enter a synchronized block, the thread asks for the **key** of the object that guards this block; if the key is not there, the thread is blocked until the key becomes available again. But there are cases where the thread **already has** the key: calling a synchronized method from a first synchronized method of the same class, or from a subclass — the thread is already holding that key, so it can execute this code **without having to wait**. One last word: the synchronization classes from java.util.concurrent are **also** reentrant — this is the case for the `ReentrantLock` class, as its name may suggest.

```java
class Service {
    public synchronized void outer() { inner(); }  // already has the key...
    private synchronized void inner() { ... }      // ...enters directly!
    // without reentrancy: a DEADLOCK with itself
}
// ReentrantLock: an acquisition counter per SAME thread
```

---

### Question 121 — What does shallow copy mean?
· [video](https://youtu.be/2famM1cl_wg) · local file `278`

**Short answer:** It means duplicating an object — but **not** the objects this object has references to.

**Less short answer:** There are cases where it makes sense to shallow copy an object, and others where it can be actually dangerous. The rule of thumb that will always work is the following: if the related object is **non-modifiable**, like a String, then there is no need to duplicate it — in that case, shallow copy is OK. But if your related object is an ArrayList, for instance, it means that anyone with a reference to this ArrayList can modify it — and thus **modify the internal state of your object**... and most of the time, this is not what you want. One last word: you can protect your objects against that with the **defensive copy** pattern. *(Another video.)*

```java
class Order implements Cloneable {
    String number;             // immutable: shallow OK
    List<Item> items;          // mutable: shared!

    @Override protected Object clone() throws CloneNotSupportedException {
        Order o = (Order) super.clone();   // shallow: items UNCHANGED
        o.items = new ArrayList<>(items);  // defend yourself: copy
        return o;
    }
}
```

---

### Question 122 — What is the Vector?
· [video](https://youtu.be/Yu_HpBnUHa4) · local file `277`

**Short answer:** Something you should not be using anymore.

**Less short answer:** Vector is an old API from **before** the Collections framework — this was before Java 2, released in 1998. With Java 2, the Vector class has been **refactored** to be included in the Collections framework. Vector is thread-safe, but in a very basic way: **all its methods are synchronized**. If you do not need thread safety, you can safely use an ArrayList instead; if you do, well — there are **better implementations** depending on your use case. One last word: this Vector class has **nothing to do** with the **Vector API**, which gives you access to the **SIMD** parallel computing capacity of your CPU. *(Another video.)*

```java
Vector<String> v = new Vector<>();   // everything synchronized (and slow)
List<String> l = new ArrayList<>();  // no sync: prefer it
List<String> c = new CopyOnWriteArrayList<>(); // real concurrency

// and don't confuse: jdk.incubator.vector.VectorSpecies = SIMD (other thing)
```

---

### Question 123 — What is the difference between Runnable and Callable?
· [video](https://youtu.be/bC6sdJ-GijQ) · local file `276`

**Short answer:** Both are used to model tasks in concurrent programming.

**Less short answer:** A `Runnable` is a task model introduced in the **first versions** of Java: it doesn't take any argument and **does not return anything**. `Callable` was added in **Java 5, in 2004**, as part of the java.util.concurrent API: a Callable doesn't take any argument either, but it **does return something** — and it **can throw an exception**. Runnable is the only one that can be passed to **thread creation**, including with the factory methods added in JDK 21; both can be passed to **executor services and fork-join pools**. One last word: **structured task scopes only accept Callables**. We will talk more about that later.

```java
Runnable task = () -> System.out.println("no return");
Callable<Integer> calc = () -> { Thread.sleep(100); return 42; };

new Thread(task).start();          // Runnable only here
executor.submit(task);             // returns Future<?>
Future<Integer> f = executor.submit(calc);  // Future<Integer>

try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    scope.fork(calc);              // Callable only in the scope!
    scope.join();
}
```

---

### Question 124 — Can you create a stream from an Iterable?
· [video](https://youtu.be/enfTnGAk4-o) · local file `275`

**Short answer:** Yes.

**Less short answer:** There is actually an API to do that. Most of the time, you will create streams from Collections using the `collection.stream()` method — but sometimes all you have is an **Iterable**. In that case, creating a collection or an array and then opening a stream on it would be a **waste of memory**: you can call `StreamSupport.stream(spliterator, false)` — the spliterator comes directly from your iterable, and `false` because you don't want to mess with parallel streams. One last word: in the case your iterable produces an **unlimited amount of data**, then a stream can handle that with some **short-circuit operations** — whereas trying to build a collection would just **break** your application.

```java
Iterable<Reading> sensor = readForever();     // infinite!

StreamSupport.stream(sensor.spliterator(), false) // without materializing
        .filter(Reading::critical)
        .findFirst()                              // short-circuit saves you
        .ifPresent(alarm::trigger);
```

---

### Question 125 — How is a CopyOnWriteArrayList working?
· [video](https://youtu.be/VSEBhEx6zS0) · local file `274`

**Short answer:** Well, as the name suggests: it copies its internal array **on each write operation**.

**Less short answer:** CopyOnWriteArrayList is a **thread-safe** list that is interesting if what you have is **mostly read operations**. This class works in the following way. First, the internal array is **duplicated** and mutated within a synchronized block; then, the internal reference to this array is updated. Because this reference is **volatile**, reading does not need to be synchronized — so no contention on read operations. The major overhead is the **array duplication**. One last word: there is also a `CopyOnWriteArraySet` that actually wraps a CopyOnWriteArrayList — so same warnings on this one.

```java
List<Listener> listeners = new CopyOnWriteArrayList<>();
listeners.add(l1);          // copies the whole array (expensive write)

for (Listener l : listeners) // lock-free read, over the snapshot
    l.notify();             // safe iteration, no CME — add at will
```

---

### Question 126 — What is a switch statement?
· [video](https://youtu.be/7At_mAOyTgE) · local file `273`

**Short answer:** An old thing. That got some attention recently. *(a humorous short answer)*

**Less short answer:** Switch used to be a statement that would work on a **selector** variable that could be an integer type — not `long` — an enum, or a string of characters; and this selector could not be null. It can now be an **expression** also; it can accept **any object**, including null (for the null pattern), as the selector; and it can accept **patterns** and **guarded patterns** for the case labels. It can even check for **exhaustiveness**, and can prevent **fall through**. So in one word: **it has been fixed** — and is now much more useful. It could even handle exceptions as case labels in a future. One last word: switch expressions are now part of what is called **data oriented programming** in Java. *(Another video.)*

```java
// before: a statement, break, few types
switch (code) { case 1: ...; break; default: ...; }

// now: an expression, patterns, exhaustiveness, no fall-through
String desc = switch (obj) {
    case Integer i when i > 0 -> "positive: " + i;
    case Integer i            -> "not positive";
    case String s             -> "text: " + s;
    case null                 -> "null (allowed!)";
    default                   -> "other";
};
```

---

### Question 127 — What is the difference between an Exception and an Error?
· [video](https://youtu.be/mX22hhGONQA) · local file `272`

**Short answer:** Exceptions are for your application; Errors are for the JVM.

**Less short answer:** In many cases, you need to handle exceptions explicitly — if your exception is a runtime exception, which is an extension of the Exception class, then you don't. **Errors** are mostly used to tell you that something went wrong **in the JVM** — the JVM is loading your classes even before running them: that's the case for the `ClassFormatError`, `NoSuchMethodError`, `AbstractMethodError`, or `ClassCircularityError`. One last word: here are examples of errors that will put an end to your application: `StackOverflowError`, `OutOfMemoryError` — there is even an `UnknownError`, "but I never saw it, to be honest".

```java
// JVM errors (don't catch!):
// new byte[Integer.MAX_VALUE]     -> OutOfMemoryError
// infinite recursion              -> StackOverflowError
// invalid bytecode/broken jar     -> ClassFormatError
// a method swapped in a library   -> NoSuchMethodError

// application exceptions (handle):
try { Files.readAllLines(p); }
catch (IOException e) { ... }   // checked: handling mandatory
```

---

### Question 128 — What is a red-black tree?
· [video](https://youtu.be/642xsKA1ZdA) · local file `271`

**Short answer:** A data structure implemented by the TreeMap class.

**Less short answer:** TreeMap is actually an implementation of `NavigableMap`, that extends a `SortedMap` — and since Java 21, a `SequencedMap`. A red-black tree is a data structure that stores its elements in a **binary tree** and keeps them sorted using a nifty trick of **colored nodes** (rules that guarantee a height ≤ 2·log n). TreeMap keeps its key-value pairs **sorted by key** while you are adding or removing them — so when you iterate over them, they are already sorted. One last word: on the one hand, a red-black tree is a very efficient data structure, as most of the operations are **O(log n)**; on the other hand, it's a **reference-based** data structure, so pointer chasing will not play nice with it (cache misses).

```java
TreeMap<String, Integer> tm = new TreeMap<>();
tm.put("banana", 2); tm.put("pineapple", 1);
tm.firstKey();       // "pineapple" — always sorted
tm.subMap("a", "c"); // a view [a, c)
tm.reversed();       // JDK 21+: SequencedMap
```

---

### Question 129 — What are the visibility modifiers for members?
· [video](https://youtu.be/F9NuvwXANmw) · local file `270`

**Short answer:** Private, protected and public.

**Less short answer:** There is also a fourth one, which consists in **not putting any modifier** at all — package private. A **private** member can only be accessed from the class itself; a **public** member can be accessed from everywhere; a **protected** member can be accessed from the extending classes and from other classes within the same package; **no modifier** means that you can access it from the same package only. One last word (a very important one): **without the module system**, you can create a class in a package from any other jar that can give you access to all the protected and package-private members... so the two levels you should **really** rely on are **private and public**.

```java
public class Example {
    private int a;      // this class only (reliable)
    int b;              // package-private (fragile!)
    protected int c;    // subclasses + package (fragile!)
    public int d;       // the world (reliable)
}
// protected/package-private without modules: accessible by cheating
// a module-info with exports/opens gives the package real protection
```

---

### Question 130 — What is a sequenced collection?
· [video](https://youtu.be/nwGo4jRKbvo) · local file `269`

**Short answer:** An interface added to the Collections framework.

**Less short answer:** This interface was added in **Java 21**, to fill a gap there was between **lists** and **sorted sets**. Sequenced collections allow you to ask for the **first** or **last** element of a collection — and to **remove** it if the collection is modifiable. You can also ask for a **reversed view** on the content of the collection. One last word: you also have sequenced maps and sequenced sets — `SequencedSet` extends SequencedCollection, with a `reversed()` method that returns itself a sequenced set.

```java
List<Integer> l = new ArrayList<>(List.of(1, 2, 3));
l.getFirst();          // 1
l.getLast();           // 3
l.addFirst(0);         // [0, 1, 2, 3]
l.pollLast();          // removes and returns 3
l.reversed();          // [2, 1, 0] — a view, no copy

LinkedHashSet<Integer> s = new LinkedHashSet<>(List.of(1, 2));
s.reversed().getFirst();  // SequencedSet: 2
```

---

### Question 131 — What does encapsulation mean?
· [video](https://youtu.be/TtNVQVTXB_M) · local file `268`

**Short answer:** It means **no public instance field** in your classes.

**Less short answer:** The internal state of your class should be accessed **only through accessors** — to read it or to modify it. One of the reasons is that your code only depends on the **public method** of its collaborating objects, not on their internal implementation — meaning that you can change it **without changing the client code**. Encapsulation is usually an object concern, but you can also apply this principle to **APIs or libraries**: only depend on them through their interface. One last word: even if records are non-modifiable, you still need to access their internal state through their **accessors** — there are good reasons for that. *(Another video.)*

```java
class Account {
    private BigDecimal balance;          // encapsulated: no public field

    public BigDecimal balance() { return balance; }        // read
    public void deposit(BigDecimal v) {                    // controlled write
        if (v.signum() <= 0) throw new IllegalArgumentException();
        balance = balance.add(v);
    }
}
```

---

### Question 132 — What is a component?
· [video](https://youtu.be/L8RcHrleI5Y) · local file `267`

**Short answer:** A field of a record.

**Less short answer:** This definition is very precise: the elements you define along with your record definition are called **record components**. The word "component" became a Java platform notion when records were added to the language. In the Reflection API, for instance, you have a `getRecordComponents()` method on the `Class` class; and when you create an annotation, you can state that it can be added on a record component — and for that, the `ElementType` enum, that defines all the elements on which you can add an annotation, has a value **`RECORD_COMPONENT`**. One last word: when you declare a record component, the compiler gives you an **accessor** for it — and uses them to create an equals and a hashCode method, as well as a toString method. All that for free.

```java
public record Customer(String name, int age) { }
//            component ^        ^ component

Customer.class.getRecordComponents();  // reflection knows components

@interface NonNull { @Target(ElementType.RECORD_COMPONENT) }
```

---

### Question 133 — What are the main operations of a stack or a queue?
· [video](https://youtu.be/Nt2-PlW1iiQ) · local file `266`

**Short answer:** Peek, pop and push. *(the video plays with the pronunciation)*

**Less short answer:** These two data structures are the two simplest data structures you can imagine. **Peek** allows you to check the next available element without removing it — you cannot see the other elements. **Pop** removes it: now you have it, you can do something. **Push** adds an element. If it's a **stack**, then peek will show you the **last** element that was pushed; and if it's a **queue**, then peek shows you the **first** element that was pushed — maybe some time ago. One last word: there are two interfaces to model these: `Queue` and `Deque` — which stands for *double-ended queue* — and a number of implementations, like `ArrayDeque` for instance, or `ConcurrentLinkedQueue`, which is thread-safe.

```java
Deque<String> stack = new ArrayDeque<>();   // LIFO
stack.push("a"); stack.push("b");
stack.peek();   // "b" (the last one)
stack.pop();    // "b"

Queue<String> queue = new ConcurrentLinkedQueue<>();  // FIFO
queue.offer("a"); queue.offer("b");
queue.peek();    // "a" (the first one)
queue.poll();    // "a"
```

---

### Question 134 — What is an entry and how can you create one?
· [video](https://youtu.be/RsTfs5eFRIs) · local file `265`

**Short answer:** It is a key-value pair in a map.

**Less short answer:** This key-value pair is modeled by the `Map.Entry` interface: from it you can read — and **update** — the value, and you can read the key (not modifiable). The easiest way to create a key-value pair is to use the **`Map.entry(key, value)`** factory method from the Map class: it gives you an **unmodifiable** entry. You can create an unmodifiable map from entries with the **`Map.ofEntries(...)`** factory method. Be careful, because none of these factory methods accept **null keys or null values**. One last word: you can also get a set of the entries of a map with the `map.entrySet()` method — this set has very specific properties. *(Another video.)*

```java
Map.Entry<String, Integer> pair = Map.entry("pen", 12);
pair.getKey();    // "pen" (read-only)
pair.getValue();  // 12

Map<String, Integer> stock = Map.ofEntries(
        Map.entry("pen", 12),
        Map.entry("pencil", 30));     // immutable, no nulls

for (var e : stock.entrySet()) { /* a view on the map */ }
```

---

### Question 135 — What type of ExecutorService can you cite?
· [video](https://youtu.be/ev9i-o6h6aA) · local file `264`

**Short answer:** `newSingleThreadExecutor()`, that creates a pool with only one thread. *(...among others)*

**Less short answer:** This question refers to the factory methods of the `Executors` class — there are some that you probably need to know. **`newFixedThreadPool(n)`**, where you pass the number of threads you want. **`newCachedThreadPool()`**, that creates threads on demand and lets them **die after 60 seconds** of idleness. Some of these factory methods also take a **`ThreadFactory`** as an argument, that you can use to generate the name of the threads, for instance, or to decide whether they are daemon threads or not. One last word: with JDK 21, you even have a **`newVirtualThreadPerTaskExecutor()`** that creates virtual threads on demand and lets them die at the end.

```java
Executors.newSingleThreadExecutor();     // 1 thread (a serial queue)
Executors.newFixedThreadPool(8);         // fixed
Executors.newCachedThreadPool();         // elastic (60s lifetime)
Executors.newScheduledThreadPool(2);     // periodic tasks
Executors.newThreadPerTaskExecutor(factory);  // a platform thread per task
Executors.newVirtualThreadPerTaskExecutor();  // JDK 21+: virtual per task
```

---

### Question 136 — What is a defensive copy?
· [video](https://youtu.be/n8b5iH9CUHM) · local file `263`

**Short answer:** A pattern by which you create a copy of an object, when you **receive** it or when you **return** it.

**Less short answer:** It is useful when this object is **modifiable** — like an ArrayList, a HashMap, or a plain array, for instance. If you don't do this copy, someone with a reference to this object will be able to modify it — and thus **modify the internal state of your object**. In a class (or record) with an array as a component, you need to copy it **in the constructor** — to make sure that no one keeps a reference on it — **and in the accessor**, for the same reason. One last word: you don't need a defensive copy with a non-modifiable object — but be careful: you need to make sure that it does not have any reference to other modifiable objects.

```java
record ClassGroup(String[] students) {
    ClassGroup { students = students.clone(); }  // defense on the way in
    public String[] students() {                 // Java 16+: custom accessor
        return students.clone();                 // defense on the way out
    }
}
// List.copyOf(map.values()) — the idiomatic defensive copy for collections
```

---

### Question 137 — What is a for each loop?
· [video](https://youtu.be/2SWgLkB2IZI) · local file `262`

**Short answer:** A syntactic sugar to iterate over an Iterable.

**Less short answer:** This syntax works for **any Iterable** — that includes all the collections — and the arrays. Internally, the compiler creates an **iterator** for you and uses it to traverse your iterable. You may be thinking that this object is just another overhead — but it can quickly be removed by the JVM through its **escape analysis** optimization: no one can get a reference to this iterator, so in the end the JVM may decide to just drop it. One last word: if what you have is an Iterable, you can also use its **forEach** method and pass your consumer — this forEach method uses a for-each loop internally, so it will not make your code any faster, but it can make it easier to read.

```java
// this...
for (String s : names) { ... }

// ...becomes this (the compiler):
for (Iterator<String> it = names.iterator(); it.hasNext(); ) {
    String s = it.next();   // the iterator doesn't escape -> JIT scalarizes
}

// the readable equivalent:
names.forEach(this::process);
```

---

### Question 138 — What is the difference between an Enumeration and an Iterator?
· [video](https://youtu.be/UI6RK82N81U) · local file `261`

**Short answer:** An Enumeration is something you should not be using anymore.

**Less short answer:** Back in the days — a long time ago, before the Collections framework was added to the JDK in 1998, that was Java 2 — the `Enumeration` interface was the interface you had to use to iterate over the elements. Now that you have the Collections framework, there is no reason to use Vector anymore: you can use an ArrayList instead, or a concurrent collection from java.util.concurrent if you need thread safety. One last word: as of Java 9, the functionality of this interface is duplicated by the Iterator interface — and if you see an Enumeration, you can call its `asIterator()` method to use it as an iterator.

```java
// 1998: Enumeration was the only way
for (Enumeration<String> e = vector.elements(); e.hasMoreElements(); )
    use(e.nextElement());

// modern: Iterator (remove, for-each)...
// the bridge: Enumeration.asIterator() (Java 9+)
e.asIterator().forEachRemaining(this::use);
```

---

### Question 139 — What is the diamond problem in OOP?
· [video](https://youtu.be/F1Yi1l7E2CY) · local file `260`

**Short answer:** Something that default methods in interfaces brought to the Java platform.

**Less short answer:** This problem comes with the **multiple inheritance of behavior**. Before Java 8, Java used to support only the inheritance of **type**: a class can implement several interfaces. Starting with Java 8, you can have **default methods** — that is, behavior in interfaces. So if a given class implements **two different interfaces** that happen to have the **exact same default method** (but are doing two different things), then what you have is a diamond problem. One last word: how can you fix that? Well, the good news is that the **compiler can see** this problem — and will ask you **which default method you want to call**, at compile time.

```java
interface Flyer  { default String move() { return "flying"; } }
interface Swimmer { default String move() { return "swimming"; } }

class Duck implements Flyer, Swimmer {
    @Override public String move() {          // disambiguation MANDATORY
        return Swimmer.super.move();          // an explicit choice
    }
}
```

---

### Question 140 — What is a compact constructor?
· [video](https://youtu.be/uJGSKkCVRIM) · local file `259`

**Short answer:** It is a syntax that you can use to write the canonical constructor of records in a much simpler way.

**Less short answer:** The canonical constructor of a record is simply the constructor that takes **all its components**; you can have other constructors, but they need to call this one. You can write this constructor in a **compact form**: you do not declare its parameters, and you do not need to assign the arguments to the fields of your record — the assignment is automatic. It gives you a very clean syntax, especially when all you have to do is **validate** the arguments you get — but you can also do some **defensive copy** in a compact constructor if this is what you need. One last word: records are really great to improve the readability and expressiveness of your code — you can even create them **on the fly**, as local records; you should definitely use them everywhere you can.

```java
record Period(LocalDate start, LocalDate end) {
    Period {                                    // compact: no parameters
        if (end.isBefore(start))
            throw new IllegalArgumentException("end < start");
        // the assignment this.start = start ... is automatic
    }
}

record Password(char[] value) {
    Password { value = value.clone(); }         // defensive copy works too
}
```

---

### Question 141 — What is a Deque?
· [video](https://youtu.be/iNJ0GWgwKxo) · local file `258`

**Short answer:** A double-ended queue.

**Less short answer:** There are two interfaces that you should use when you need a queue or a stack in Java: `Queue` and **`Deque`**, that extends Queue. You can peek, pop or push **by both ends** of a deque — it makes it a model for both stacks and queues in Java. The Deque defines the two behaviors you need when the queue is **full** (push) or **empty** (pop): react immediately by **throwing an exception**, or immediately return **true or false** depending on whether the operation succeeded — there are families of methods for each style (`addFirst`/`offerFirst`, `removeFirst`/`pollFirst`...). One last word: Deque has an extension, **`BlockingDeque`**, that models concurrency — it adds a third behavior: instead of reacting immediately, it can **block** until your push or your pop can be conducted.

```java
Deque<String> deque = new ArrayDeque<>();

deque.offerFirst("a"); deque.offerLast("b");   // false if it doesn't fit
deque.pollFirst();  deque.pollLast();          // null if empty
deque.addFirst("x");                           // exception if it doesn't fit
deque.push("top"); deque.pop();                // a stack (LIFO) at the head

// BlockingDeque: put/take block until possible
```

---

### Question 142 — What is the deep copy?
· [video](https://youtu.be/VtQFcyym550) · local file `257`

**Short answer:** A mechanism to copy an object **and all its related objects**.

**Less short answer:** There are two ways to duplicate objects: **deep copy** and **shallow copy**. Shallow copy just copies the current object — if this object is referencing more objects, **they are not copied**. Deep copy visits **all the graph** of the referenced objects and copies them — which can be costly. Because it is hard to know in advance what the cost of a deep copy is, shallow copy is usually the default behavior (of `clone()`, for instance). One last word: if your objects are **non-modifiable**, then there is no need to copy them at all — you can check the source code of `List.copyOf`: it first checks if the list is non-modifiable, and in that case, just **returns it**.

```java
class Address { String city; }             // mutable
class Customer implements Cloneable {
    String name; Address address;

    @Override protected Object clone() throws CloneNotSupportedException {
        Customer c = (Customer) super.clone();   // shallow: address shared!
        c.address = new Address();               // deep: copies the graph
        c.address.city = this.address.city;
        return c;
    }
}
List.copyOf(immutableList);   // doesn't even copy: returns the same instance
```

---

### Question 143 — What is a Fork Join pool? *(version 2)*
· [video](https://youtu.be/ZE2pBylyoFA) · local file `256`

**Short answer:** It is the pool of threads used by parallel streams.

**Less short answer:** This pool of threads is called the **common ForkJoinPool**, and there is only **one** in your JVM: it is created when your JVM starts, and waits for your parallel streams to use it. The ForkJoinPool is named after the algorithm used in parallel streams. First, you **split** your data set again and again, until the chunk you have is small enough to be processed in a reasonable amount of time — that's the **fork** phase; and then you **merge** everything to compute the final result — that's the **join** phase. One last word: you should be careful with parallel streams — they may be much more costly than what you think. One piece of advice: if you're writing a service bound to run in an **application server**, you should **stay away** from them.

```java
list.parallelStream()          // runs on the commonPool (one per JVM)
    .map(this::heavy)
    .collect(toList());
// fork: splits down to minimal chunks; join: merges the results

// on an app server: the pool is SHARED with other deployments...
```

---

### Question 144 — What is an entry set?
· [video](https://youtu.be/vyrXHWT8ImQ) · local file `255`

**Short answer:** It is a set that contains the key-value pairs of a map.

**Less short answer:** You can call `map.entrySet()` to get this set: it is a `Set<Map.Entry>` — the `Map.Entry` interface models your key-value pairs. This set is actually a **view** on your map: you cannot **add** anything to it (you will get an exception if you try), but you can **remove** elements from it — either directly or indirectly, removing the corresponding key-value pair from your map. One last word: if your map is a `SequencedMap`, you can call `sequencedEntrySet()` on it to get a **SequencedSet** instead of a regular set.

```java
Map<String, Integer> m = new LinkedHashMap<>(Map.of("a", 1, "b", 2));

for (Map.Entry<String, Integer> e : m.entrySet()) {
    System.out.println(e.getKey() + " = " + e.getValue());
}
m.entrySet().removeIf(e -> e.getValue() < 2);  // removes a=1 from the map

// SequencedMap (JDK 21): sequencedEntrySet() -> SequencedSet
```

---

### Question 145 — What does fall through mean?
· [video](https://youtu.be/RbOxnkN6-ug) · local file `254`

**Short answer:** With no doubt, trouble. *(the presenter's comic short answer)*

**Less short answer:** This is something that can happen if you're using `switch` in your application... — well, **if you used** switch: as (he jokes) nobody does, there is very little chance that you can see this problem. In case you do, the problem is the following: suppose you switch on an enum, for instance, but **forgot to add a `break`** after each label — then your code will **carry on with the next one**, which is usually not what you want. One last word: maybe you will be using switch statements and **expressions** more and more in the future — because it is now part of the data oriented programming paradigm currently being added to the Java language. *(Another video.)*

```java
switch (level) {
    case HIGH:
        alert();
        // missing break! -> falls into the next case (fall through)
    case MEDIUM:
        log();      // also runs for HIGH. A classic bug.
        break;
}

// the arrow form (->) has NO fall-through:
switch (level) {
    case HIGH   -> alert();
    case MEDIUM -> log();
}
```

---

### Question 146 — What is the Collectors.toMap()? *(version 2)*
· [video](https://youtu.be/oXOdQiyUlQ0) · local file `253`

**Short answer:** A collector to create maps.

**Less short answer:** Collectors are objects you can pass to the collect method of the Stream API. `Collectors.toMap` takes **two functions** — both are applied to the elements of your streams: the first one is used to compute the **key**, and the second one to compute the **value**. In case two elements produce the **same key**, then you need to provide the **merge function** — if you don't, you will get an exception at runtime. You can also provide a **supplier** that would be used to create your empty map — useful to create TreeMaps, for instance. One last word: you also have the `groupingBy` collector, which **adds the elements that produce the same key to the same list**.

```java
Map<String, Integer> byCity = people.stream()
        .collect(Collectors.toMap(Person::city, Person::age,
                                   Integer::sum));       // merge for duplicates

Map<String, Integer> sorted = people.stream()
        .collect(Collectors.toMap(Person::name, Person::age,
                                  (a, b) -> a, TreeMap::new)); // supplier

Map<String, List<Person>> groups = people.stream()
        .collect(Collectors.groupingBy(Person::city));  // the alternative
```

---

### Question 147 — What is a Lock? *(version 2)*
· [video](https://youtu.be/iQWrfVyCj78) · local file `252`

**Short answer:** An interface.

**Less short answer:** It is used to model the object you can use to ensure that **only one thread** can execute a piece of code at a given time. There are several ways to do that in Java. The first one is the **`synchronized`** block — it has always been there. And the second one is the `Lock` interface, implemented by the `ReentrantLock` class, added in **Java 5**. The pattern to use this object is the following: create it, then call `lock()`, then use it, and release it in a **try/finally** block — to make sure that, in case something goes wrong, your lock will always be unlocked. One last word: you can do many more things with locks than with synchronized blocks — like trying to lock **with a timeout**, for instance.

```java
private final Lock lock = new ReentrantLock();

void process() {
    lock.lock();
    try {
        // only one thread at a time here
    } finally {
        lock.unlock();              // always in the finally!
    }
}

if (lock.tryLock(1, TimeUnit.SECONDS)) {   // with a timeout: synchronized can't
    try { ... } finally { lock.unlock(); }
}
```

---

### Question 148 — How can you create a collection from an Iterable?
· [video](https://youtu.be/K8dTJll0J0s) · local file `251`

**Short answer:** Why would you do that? Are you sure that this is what you need? *(the presenter questions the question itself)*

**Less short answer:** An iterable is something that can produce elements **without having to store them all in memory at once** — so you can process many elements with this pattern with a small memory footprint. For instance, you can create a **stream** on it: unless you call `distinct` or `sorted` on your stream, it will not store anything in your memory. So if you are **really, really** sure that this is what you need, then here is the pattern — the classic stream + collect (or an add loop). One last word: loading your data in memory can **dramatically** increase your memory footprint — so make sure you need it before doing it.

```java
Iterable<Record> source = hugeSource();      // lazy by nature

// if it can stay lazy: don't materialize!
StreamSupport.stream(source.spliterator(), false)
             .filter(this::valid).forEach(this::process);

// if it HAS to be a collection (does it, really?):
List<Record> all = StreamSupport
        .stream(source.spliterator(), false)
        .collect(Collectors.toList());
```

---

### Question 149 — What is the filter() method?
· [video](https://youtu.be/SNDZFdcAUPc) · local file `250`

**Short answer:** A method to prevent elements from a stream from being passed to the next stage of your pipeline.

**Less short answer:** A filter method does not really **remove** elements from a stream — because a stream is an empty object: **there is no element in a stream**. A stream consumes elements from a source, processes them one stage at a time, and then reaches a terminal operation. The `filter` operation takes a **predicate** as a parameter, that decides if a given element can be passed or not. One last word: you can also filter using two other operations — `mapMulti` and `flatMap` — which may be useful if you cannot express your validation as a simple predicate. *(Another video.)*

```java
Stream.of(1, 2, 3, 4, 5)
      .filter(n -> n % 2 == 0)     // 2 and 4 pass; the rest never gets there
      .forEach(System.out::println);

// complex validation? flatMap / mapMulti decide per element:
.flatMap(n -> valid(n) ? Stream.of(n) : Stream.empty())
```

---

### Question 150 — How is quick sort working?
· [video](https://youtu.be/otyI0Xew2mk) · local file `249`

**Short answer:** Quickly. *(the presenter's joke)*

**Less short answer:** The first step consists in choosing a **pivot** — your best choice is a value that will **split your set into equal parts**. Then you compare all the elements to the pivot: the lesser elements go to the **left**, the greater to the **right** — and then you rinse and repeat on the two halves of your set, again and again, until you have intervals of size 2. One last word: how many operations do you need? Well, each step consists in N comparisons, and you divide your set in 2, again and again — so the number of steps `p` is such that N / 2^p is still greater than 1: that's **log N** steps — **N log N** in total. You knew that already — now you *know*. *(a pun on "now you know")*

```java
void quickSort(int[] a, int lo, int hi) {
    if (lo >= hi) return;
    int p = partition(a, lo, hi);   // lesser to the left, greater to the right
    quickSort(a, lo, p - 1);        // recursion on the left half
    quickSort(a, p + 1, hi);        // recursion on the right half
}
// N comparisons per level x log N levels = N log N
// Arrays.sort(int[]): dual-pivot quicksort
```

---

### Question 151 — What is a text block?
· [video](https://youtu.be/Gu-dMff12bA) · local file `248`

**Short answer:** It is a syntax to write strings of characters.

**Less short answer:** You open a text block with a **triple double quote followed by your line terminator**. Then all the text that follows in your source code is part of the string you're creating, until you put another triple double quote that closes your string. A text block usually spans over several lines. The blank characters at the beginning of each line are **not** part of your text — they are called the **incidental whitespaces** (the common indentation is removed). One last word: you can control the start and end of each line by adding a `\s` character (and `\` to continue on the next line). Text blocks are great — you should use them wherever you can.

```java
String json = """
        {
          "name": "Ana",
          "age": 30
        }""";        // the common indentation is removed automatically

String sql = """
        SELECT * \
        FROM customer WHERE id = ?""";   // \ joins lines
```

---

### Question 152 — How can you loop backward?
· [video](https://youtu.be/JK4HJ3hfLs8) · local file `247`

**Short answer:** There is a method for that.

**Less short answer:** It only works if your collection is **ordered** — meaning that it is in fact a list; most of the time it is the case, because ArrayList is your favorite implementation. There is a `listIterator()` method on List that does not exist on Collection, and that returns a **ListIterator**. ListIterator **extends** the Iterator: so you have the classical next and hasNext, but you also have **previous and hasPrevious**, to iterate backward. One last word: you also have a `nextIndex` and a `previousIndex` method, as well as `set` and `add` — to set the current element and insert an element through this list iterator; all this is supported by the ArrayList implementation.

```java
List<String> l = List.of("a", "b", "c");

for (ListIterator<String> it = l.listIterator(l.size());
        it.hasPrevious(); ) {
    System.out.println(it.previous());   // c b a
}
// Java 21+ alternative: l.reversed() (a view) — SequencedCollection
```

---

### Question 153 — What is a semaphore? *(version 2)*
· [video](https://youtu.be/MjVNnNR7aq8) · local file `246`

**Short answer:** A lock with several permits.

**Less short answer:** A classical lock can give access to the block of code it protects to **one** thread at a time; a semaphore can give access to **several** threads. You create a semaphore by giving it a number of **permits**; then you can use it to protect a block of code: a thread needs a permit to execute the protected code — and if there is none available, then it **blocks** until there is one. Don't forget to **release your permit in a finally block**. Semaphores are useful when you need to **limit the number of threads** that can access an external resource. One last word: semaphores can also be **fair**, meaning that the first thread that was blocked is the first one to be given a permit.

```java
Semaphore slots = new Semaphore(10);   // 10 simultaneous accesses

slots.acquire();                       // blocks if no permit is available
try { callExternalApi(); }
finally { slots.release(); }           // ALWAYS in the finally

Semaphore fair = new Semaphore(1, true); // FIFO for the blocked ones
```

---

### Question 154 — What is a method handle?
· [video](https://youtu.be/GJATctQPumo) · local file `245`

**Short answer:** An object to access the members of an object — "reflection on steroids". *(our phrasing of the video's idea)*

**Less short answer:** There is a caveat in using the Reflection API with the `Field` or `Method` objects: it needs to **check access on each invocation** — and this is quite slow. A **MethodHandle** is a reference to an existing member of a class, but the checks are made **when you create** the method handle. You can invoke public constructors by getting a reference to them with the `findConstructor` method; the same for public methods with `findVirtual`, and public fields with `findGetter` and `findSetter`. One last word: method handles do **not** give you access to **private members** from outside of a class — for that, you need to use the classical `Field` or `Method` classes.

```java
MethodHandles.Lookup lookup = MethodHandles.lookup();

MethodHandle length = lookup.findVirtual(
        String.class, "length", MethodType.methodType(int.class));
int n = (int) length.invoke("java");   // the checks were made at creation

MethodHandle constructor = lookup.findConstructor(
        ArrayList.class, MethodType.methodType(void.class));
```

---

### Question 155 — Why should you prefer private fields and accessors?
· [video](https://youtu.be/YKxEI2DT4Jg) · local file `244`

**Short answer:** Because it is safer.

**Less short answer:** A private field must be accessed through an **accessor** — meaning that you can control the access, and there are two things that you can easily do. The first one is the **defensive copy**: sometimes you need to protect your internal state from outside modifications — even if your field is final, it can be a reference to a mutable container that you need to copy when you build your object. And the second one is **concurrent access**: fields can be declared `volatile`, but it only makes the **reference** thread-safe; you can then guarantee thread safety using a `synchronized` block. One last word: by default, always use private fields. Always.

```java
final class Contract {
    private final List<Clause> clauses;   // private, always

    Contract(List<Clause> cs) {
        this.clauses = List.copyOf(cs);   // defensive copy
    }
    public List<Clause> clauses() {       // controlled access
        return clauses;                   // already immutable
    }
    private volatile boolean active;      // the reference is visible...
    public synchronized void activate() { active = true; } // state in sync
}
```

---

### Question 156 — What is the difference between an interface and an abstract class?
· [video](https://youtu.be/f5hKXYeJ90s) · local file `243`

**Short answer:** Well, it's not the same thing.

**Less short answer:** There are **two major differences**. First, an abstract class **has a constructor**, which is called to construct your final object — it is not the case for an interface. And second, an abstract class can carry your **mutable state** — which, again, is not the case for an interface. This question may sound like a trick question, because you can have static and instance methods in both abstract classes and interfaces — a feature that was added in Java 8, in 2014. One last word: now you may be wondering — when should one use abstract classes or interfaces? By default, prefer interfaces. Why? Well, that will be for another time.

```java
abstract class Animal {               // constructor + STATE
    private int energy;               // an interface has none of this
    Animal() { energy = 100; }
    abstract String sound();
}

interface Speaker {                   // a pure contract
    default String speak() { return "..."; }  // default: no state
}
// several interfaces per class; only ONE abstract superclass
```

---

### Question 157 — How can you create a directory with Java I/O?
· [video](https://youtu.be/e7KcMo0KxKo) · local file `242`

**Short answer:** There is a method for that.

**Less short answer:** The method is called `Files.createDirectory(path)` — a factory method from the Files class, that takes a path as a parameter; the pattern is very simple. You can also pass **file attributes** that can set the permissions you want for this directory — here is a classical example for a public directory. One last word: there is also a `Files.createDirectories(path)` method, that takes the same parameters, but that can create **any intermediate directory** on the path to the directory you want to create.

```java
Files.createDirectory(Path.of("logs"));            // the parent must exist
Files.createDirectory(Path.of("pub"),
        PosixFilePermissions.asFileAttribute(
            PosixFilePermissions.fromString("rwxr-xr-x")));

Files.createDirectories(Path.of("a/b/c"));         // creates a, b AND c
```

---

### Question 158 — How is the Map.values() method working?
· [video](https://youtu.be/2Dq-MFrZY20) · local file `241`

**Short answer:** `Map.values()` is a **mutable view** on the values of a map.

**Less short answer:** Map.values returns the collection of the values present in your map — a view on the content of your map that reflects its modifications. This view is also mutable: it supports `remove` — from the collection itself and from an iterator — `removeAll` and `retainAll`. It does **not** support the **addition** of elements. Removing a value removes the corresponding key-value pair. One last word: if a value is present **several times** in your map, then **one of them** is removed — the first that is found when your implementation iterates over them... which may not be the first that you added to your map.

```java
Map<String, Integer> m = new LinkedHashMap<>();
m.put("a", 1); m.put("b", 1); m.put("c", 2);

Collection<Integer> values = m.values();
values.remove(1);          // removes "a"=1 (the first one found)
m.size();                  // 2
values.add(3);             // UnsupportedOperationException
```

---

### Question 159 — What is the first thing a constructor does? *(version 2)*
· [video](https://youtu.be/Oyzzx3tLktw) · local file `240`

**Short answer:** It calls another constructor.

**Less short answer:** This other constructor can be a constructor from the same class — called by `this(...)` — or from a superclass, called by `super(...)`. If you don't write any such code explicitly, then the compiler will add one for you. This series of super calls **lands in Object**. In the case of **records**, any constructor other than the canonical constructor must call another constructor — and in the end, this chain of calls must call the **canonical** constructor. One last word: you cannot add any instruction **before** this call to this or super — being able to execute code before this call is actually being discussed and should become a feature (JEP 513). We will talk more about this later.

```java
class Base { Base() { } }

class Child extends Base {
    Child() { super(); }        // the compiler inserts it if you don't
    Child(int x) { this(); }    // delegates in the same class
}   // the chain always ends in Object()

record Point(int x, int y) {
    Point() { this(0, 0); }     // record: EVERY path leads to the canonical
}
```

---

### Question 160 — How can you create a stream from an Iterator?
· [video](https://youtu.be/pgCXAEzlLIs) · local file `239`

**Short answer:** There is a code pattern for that. *(no short answer — straight to the pattern)*

**Less short answer:** First, you need to create a **spliterator** from your iterator, using the `Spliterators` factory class — the `spliteratorUnknownSize` factory method also takes the **characteristics** of the spliterator you want to create; there are several you can choose from: `DISTINCT`, `NONNULL`, `SORTED`, for instance. Then you need to pass this spliterator to the **`StreamSupport.stream(...)`** factory class, telling the API if your stream is parallel — and that's it.

```java
Iterator<String> it = legacySource();

Spliterator<String> sp = Spliterators.spliteratorUnknownSize(
        it, Spliterator.ORDERED | Spliterator.NONNULL);

Stream<String> stream = StreamSupport.stream(sp, /* parallel? */ false);
stream.map(String::trim).forEach(System.out::println);
```

---

### Question 161 — Why should you not use static initializers?
· [video](https://youtu.be/52wIam7kkL4) · local file `238`

**Short answer:** Because you can't prevent this code from running.

**Less short answer:** You can run your application in two contexts, at least — in production, and to run your tests. Odds are that your production context is not the same as your test context. Suppose you create a logger that logs to a file or a database: then your tests need the same file system, or the same database. You can play with the configuration files for that — but then what your code does is **not written in your code anymore**. Using **dependency injection**, for instance, leads to a code that is much easier to understand — and to test. One last word: there are some examples of static initializers in the JDK itself — and that's okay, because that's the same JDK, whether you're using it to test your application or to run it in a production environment.

```java
// bad: runs ALWAYS at class loading, in both contexts
static final Logger LOG = createFileLogger();  // tests: need the file!

// good: an injected dependency, a controlled context
class Service {
    private final Logger log;
    Service(Logger log) { this.log = log; }   // a test passes a fake logger
}
```

---

### Question 162 — What is a read-write lock? *(version 2)*
· [video](https://youtu.be/ubmAvn-8QTM) · local file `237`

**Short answer:** A lock that enables parallel reads and blocks on writes.

**Less short answer:** Locks are used to prevent race conditions — the race condition occurs when two threads are writing and reading the same field: will thread B see the value written by thread A? Synchronization achieves that by **blocking everything**. Read-write locks work with **two locks**: one for the read operations, and another one for the write operations. The **write lock is exclusive** — no other lock can be taken when it is active; the **read lock is not**: you can have any number of active read locks. One last word: of course, this is an optimization — it pays off only if your number of **write** operations is much smaller than the number of **read** operations.

```java
ReadWriteLock rw = new ReentrantReadWriteLock();

String read() { rw.readLock().lock();                // N readers together
    try { return data; } finally { rw.readLock().unlock(); } }

void write(String v) { rw.writeLock().lock();        // 1, exclusive
    try { data = v; } finally { rw.writeLock().unlock(); } }
```

---

### Question 163 — What is the difference between an Iterator and a Spliterator?
· [video](https://youtu.be/RNgnnRp7nxE) · local file `236`

**Short answer:** An iterator is used by collections; a spliterator is used by streams.

**Less short answer:** Spliterator comes from the contraction of *iterator* and *split* — which means that besides iterating over the data, it can **split** it. The iterator is **pulling** its data from a source, with `next`; the spliterator, on the other hand, **sends a consumer** with its `tryAdvance` method — and lets this source call this consumer; and if there is no more element, this `tryAdvance` method should return false. So: the iterator works on a **pull** model, the spliterator on a **push** model. One last word: you can actually implement your own spliterator to create operations on streams — like **shifting, sliding, zipping**, or even **folding**; it's a little tricky, but doable.

```java
// pull (collections): you ask
for (Iterator<String> it = list.iterator(); it.hasNext(); )
    use(it.next());

// push (streams): it delivers to the consumer
spliterator.tryAdvance(elem -> use(elem));    // 1 element
spliterator.forEachRemaining(this::use);      // the rest
Spliterator<String> part = spliterator.trySplit(); // split!
```

---

### Question 164 — How is the Map.keySet() method working?
· [video](https://youtu.be/Kb3YDZrBRZ0) · local file `235`

**Short answer:** `Map.keySet()` is a **mutable view** on the keys of a map.

**Less short answer:** Map.keySet is a set of the keys of your map — you can't have duplicates among your keys, so that's a set. This view is mutable — **but not all operations are supported** in it: you can **delete** keys from the set with `remove`, `removeAll`, `clear`, and `retainAll`, and you can also delete elements through its iterator; but you **cannot add** elements to it — if you try to do that, you will get an `UnsupportedOperationException`. One last word: do not use a key set if what you need is to iterate over the key-value pairs of your map, because it's inefficient — you have another set for that, the one you get with `map.entrySet()`.

```java
Map<String, Integer> m = new LinkedHashMap<>(Map.of("a", 1, "b", 2));
Set<String> keys = m.keySet();

keys.remove("a");        // removes the ENTRY from the map!
keys.add("c");           // UnsupportedOperationException
m.containsKey("a");      // false — the entry is gone

// iterating over the pairs? entrySet (not keySet + get: inefficient)
for (var e : m.entrySet()) use(e.getKey(), e.getValue());
```

---

### Question 165 — What is a garbage collector?
· [video](https://youtu.be/_CZi2fLHHH8) · local file `234`

**Short answer:** A wonderful piece of technology that does incredible things for you.

**Less short answer:** A garbage collector is an element of the Java virtual machine that works under the hood and takes care of your **memory** for you. There are actually **several garbage collectors** available, which you can choose from — you decide which one you want to use when you launch your application. It is the responsibility of your garbage collector to **give you the memory** you need to create your objects, and to **reclaim this memory** when you are not using it anymore. One last word: garbage collectors are actually critical to get good performance for your application, and there is a lot of investment in this piece of technology — the two latest ones, **ZGC** and **Generational ZGC**, are doing wonders. *(Another video.)*

```bash
java -XX:+UseG1GC App        # default: G1 (balanced)
java -XX:+UseZGC App         # sub-millisecond pauses
java -XX:+UseParallelGC App  # throughput
java -XX:+UseSerialGC App    # small applications/containers
```

---

### Question 166 — What is a final class?
· [video](https://youtu.be/TAodiZRau5Y) · local file `233`

**Short answer:** A class that you cannot extend.

**Less short answer:** `final` is a keyword that you can add on many elements of your source code. If you add it on the declaration of your class, and someone tries to extend this class, then the compiler will generate an error. There are elements that you can create in Java that are **compiled as final classes**: this is the case for **enums**, compiled as a final class that extends `java.lang.Enum` — and the case for **records** too, also compiled as a final class that extends `java.lang.Record`. One last word: instead of forbidding the extension of your class, you can also **control it** by making it `sealed` and publishing the list of classes that can extend it — it works really well, especially in conjunction with the switch on types. *(Another video.)*

```java
final class Util { }                 // nobody extends it
class Child extends Util { }         // compile-time ERROR

enum Color { RED }          // compiles as: final class Color extends Enum
record Point(int x, int y) { } // final class Point extends Record

public sealed interface Shape permits Circle, Square { } // control
```

---

### Question 167 — How can you join the elements of a collection of strings?
· [video](https://youtu.be/4--Kzpz8mbo) · local file `232`

**Short answer:** There is a method for that.

**Less short answer:** You can use the `String.join(delimiter, iterable)` method of the String class — it can also take a vararg. If you don't need a delimiter, you can still pass an empty string to this method. You should **really use this pattern**: it has been carefully optimized to make sure that it is as fast as possible — much faster than any StringBuilder-based pattern that you may be using (you can check the source code to get a hint at it). One last word: there are other patterns that you may want to check: one is based on the use of the **`StringJoiner`** class, that can take a **prefix and a suffix** as well; and there is the **`Collectors.joining`** collector, which can be useful if you need to map your elements before joining them.

```java
String.join(", ", List.of("a", "b", "c"));   // "a, b, c"
String.join("", "a", "b");                   // varargs: "ab"

new StringJoiner(", ", "[", "]").add("a").add("b").toString(); // "[a, b]"

names.stream().map(String::toUpperCase)
     .collect(Collectors.joining("|"));      // map + join
```

---

### Question 168 — What does volatile mean? *(version 2)*
· [video](https://youtu.be/qJm6RoEy9_s) · local file `231`

**Short answer:** It has to do with concurrency and race conditions.

**Less short answer:** `volatile` is a keyword you can use when declaring a field — and you should use it when there is a possibility of a race condition on this field; that is, when several threads can read **and** modify this field. Declaring a field volatile ensures that the thread reads the **last value written** to this field — even if this write occurred in **another** thread. The exact rule is the following: **a synchronized or volatile read always returns the last value written by a synchronized or volatile write**. One last word: this definition is actually the definition of the **happens-before** link between read and write operations. *(Another video.)*

```java
private volatile boolean running = true;

// thread A:             thread B:
// running = false;      while (running) { ... }
//    ^--- without volatile, B may loop FOREVER on the stale value
```

---

### Question 169 — How can you generate random numbers?
· [video](https://youtu.be/8qkq8TU64OM) · local file `230`

**Short answer:** There is an interface for that.

**Less short answer:** If what you need is just a simple series of random numbers, you can use the `Random` class. Starting with JDK 17, random generators implement the **`RandomGenerator`** interface, that models what a random generator can do: basically, you can generate series of any primitive type, evenly distributed, or following a **Gaussian** distribution; you can ask for one element at a time with the `next...` methods, or you can generate **streams** of elements. One last word: if you are working in cryptography, or if you need a specific random number generator, then you need to check the documentation of the `java.util.random` package — it gives you all the details of the algorithms available in the JDK.

```java
RandomGenerator rng = Random.from(RandomGeneratorFactory
        .getDefault().create(42));          // seedable, for tests
rng.nextInt(100); rng.nextGaussian();

rng.ints(1, 7).limit(5);                    // streams of randoms
ThreadLocalRandom.current().nextInt();      // concurrency
SecureRandom.getInstanceStrong();           // cryptography
```

---

### Question 170 — What is a Supplier?
· [video](https://youtu.be/0LnKLthcULk) · local file `229`

**Short answer:** A functional interface.

**Less short answer:** The Supplier interface models lambdas that **do not take any parameter** and **return something**. These lambdas are very useful when you need to model the **construction** of an object. Suppose that you need to log a message, or to throw an exception — but this message or this exception is **expensive to create**. If you pass it as an argument, then it will be created when **you** call the method — even if this method decides not to use it; this can be the case if your log level is not the right one, or if there is no need to throw this exception. In that case, you can pass a **supplier**: if the method needs to create this message or this exception, then it can call the `get()` method of this supplier. One last word: suppliers are very useful — you should use them wherever you can.

```java
// expensive: the string is built ALWAYS, even with debug off
log.debug("details: " + generateWholeReport());

// with a supplier: built only if the level is active
log.debug(() -> "details: " + generateWholeReport());

Supplier<List<String>> factory = ArrayList::new;   // lazy by nature
Objects.requireNonNullElseGet(x, ArrayList::new);  // a lazy default
```

---

### Question 171 — How can you join the elements of a stream of strings?
· [video](https://youtu.be/CRScHkGIQFI) · local file `228`

**Short answer:** There is a collector for that.

**Less short answer:** You can call `collect(Collectors.joining(...))` on your stream. This joining factory method has several overloads: the first one doesn't take any parameter and joins all the strings of your stream; the second one takes a **separator** — a string that is inserted between each element; and the third one takes a separator, a **prefix and a suffix**. And yes: these last two handle empty streams or single elements properly. One last word: be careful — this collector works **only on streams of strings**; if you pass it to a stream of objects, for instance, you will get a compiler error.

```java
Stream.of("a", "b", "c")
      .collect(Collectors.joining());               // "abc"
Stream.of("a", "b", "c")
      .collect(Collectors.joining(", "));           // "a, b, c"
Stream.of("a", "b", "c")
      .collect(Collectors.joining(", ", "[", "]")); // "[a, b, c]"
Stream.of().collect(Collectors.joining(", ", "[", "]")); // "[]" — ok!
```

---

### Question 172 — What does transient mean?
· [video](https://youtu.be/4cpCWAXa_BU) · local file `227`

**Short answer:** It is used to prevent a field from being serialized.

**Less short answer:** Serialization consists in saving a root object, along with all the graph of objects it points to, to a binary stream — that can be saved to a file or sent over the network; you can then recreate all this graph on another machine. Sometimes your object has references to elements that are **sensitive or system-dependent** — like files or network connections — and you don't want these elements to be serialized. Well, that's where you can use the `transient` keyword: add it on the field, and this field **will not be serialized**. One last word: serialization used to be a great idea when Java was created — but it brings many issues, including security issues. Nowadays: stay away from it.

```java
class Session implements Serializable {
    String user;                    // serialized
    transient char[] password;      // NOT serialized (sensitive!)
    transient Socket connection;    // NOT serialized (system-dependent)
}
```

---

### Question 173 — How does the forEach() method work?
· [video](https://youtu.be/ppuxTz3R1R8) · local file `226`

**Short answer:** It iterates over the elements of an Iterable and passes them to a Consumer.

**Less short answer:** There are actually **three** forEach methods: one on the `Iterable` interface and another one on the `Stream` interface — that both take a `Consumer` — and a third one on the `Map` interface, that takes a `BiConsumer`. This method iterates over the elements of the iterable, of the stream, or over the key-value pairs of your map, and passes them **one by one** to the consumer you pass as an argument — in the case of maps, the BiConsumer accepts the key and the value of each of your key-value pairs. One last word: apart from printing the content of a collection or a map, doing **side effects** in your consumer is, most of the time, a bad idea — try to use streams instead of doing side effects.

```java
list.forEach(System.out::println);           // Iterable
stream.forEach(System.out::println);         // Stream
map.forEach((key, value) ->                  // Map: a BiConsumer
        System.out.println(key + "=" + value));
```

---

### Question 174 — What is the difference between a Lock and a Semaphore?
· [video](https://youtu.be/l_IkxRvGw5E) · local file `225`

**Short answer:** Lock is an interface; Semaphore is a class. *(...but that's not the point)*

**Less short answer:** Locks and semaphores are doing the same kind of thing. Locks can hold only **one** permit, and semaphores can hold as many as you need — and it's still not the point. The **biggest** difference is that a lock may **enforce that the thread that releases the lock is the same as the thread that acquired it** — and it is the case for the ReentrantLock implementation — whereas the semaphore **explicitly states** that the thread that releases a permit can be a **different** thread than the one that acquired it. One last word: this feature is very useful — it may allow you to **unlock systems from the outside**, just by releasing permits from any other thread.

```java
Lock lock = new ReentrantLock();
lock.lock();
lock.unlock();   // rule: whoever locks, unlocks (ownership)

Semaphore sem = new Semaphore(3);
sem.acquire();           // thread A takes a permit
// ... in another thread:
sem.release();           // thread B releases! no ownership at all
```

---

### Question 175 — What is the difference between Enum and Enumeration?
· [video](https://youtu.be/E6VCxEzVSyw) · local file `224`

**Short answer:** Enumeration is an interface; Enum is a class. The only common element between both is the first four letters of their name. *(the presenter's joke)*

**Less short answer:** `Enumeration` is old stuff that you should not be using anymore — it's an interface that has been superseded by `Iterator` when the Collections framework was added to the JDK — that was Java 2, in 1998. `Enum` is a great addition of JDK 5 that allows you to **control the instances** of a given class: if you have only one instance, it gives you a **singleton** — and enums are the preferred pattern to create singletons. One last word: enums are great — they can have methods, state, and even custom constructors. Enumerations? Well, just forget about them.

```java
// Enumeration (1998): legacy
for (Enumeration<String> e = vector.elements(); e.hasMoreElements(); )
    System.out.println(e.nextElement());

// Enum (JDK 5): a closed type, with methods, fields and a free singleton
enum Planet {
    MERCURY(3.3e23), EARTH(5.97e24);   // a custom constructor
    final double mass;
    Planet(double m) { mass = m; }
    double relativeWeight() { return mass / EARTH.mass; }
}
```

---

### Question 176 — What is a preview feature?
· [video](https://youtu.be/fDVzDBNUTNo) · local file `223`

**Short answer:** A way to test upcoming features of the JDK before they become final.

**Less short answer:** It's not just about testing — it's also about **providing feedback**: so if you spot a bug, or think about an improvement, then you can go to the mailing list and talk about it — you can even get answers from the people who actually **wrote** the feature. You can find the right mailing list in the **JEP** that describes the feature. Preview features need to be **activated** to be used — so no risk of using them by accident; usually you will activate them in your IDE. One last word: be aware that a preview feature **may change** before becoming final — it can even be **dropped**. So avoid using them in your application, unless you really know what you're doing.

```bash
# activate at 2 levels (compilation and execution):
javac --enable-preview --release 25 App.java
java  --enable-preview App
# a preview can CHANGE or DISAPPEAR before becoming final!
```

---

### Question 177 — How can you reduce a stream?
· [video](https://youtu.be/T93jU7tqT04) · local file `222`

**Short answer:** There are several methods for that.

**Less short answer:** Reducing a stream means **combining all the elements** produced by this stream into one final element — that can be of a different type. There are **three** reduce methods on the Stream interface, and **two** collect methods that reduce in a mutable container. `LongStream` and `DoubleStream` have only two reduce and one collect — but they have a `sum`, a `min`/`max`, and also a nifty **`summaryStatistics()`** method, that does the max, the min, the count and the average — all in **one pass** over your data. One last word: some reduce methods return an `Optional` — and there is a very good reason for that. *(Another video.)*

```java
Optional<Integer> r1 = stream.reduce(Integer::max);
Integer r2 = stream.reduce(0, Integer::sum, Integer::sum);

DoubleSummaryStatistics st = DoubleStream.of(1.5, 2.5, 3.0)
        .summaryStatistics();   // count=3, avg=2.333..., max=3.0
```

---

### Question 178 — How is HashSet working?
· [video](https://youtu.be/fpq4SQKLPDs) · local file `221`

**Short answer:** It wraps a HashMap.

**Less short answer:** HashSet needs to store objects that are different, and refuses to add an object that it already contains — it uses an **internal HashMap** for that. It adds your object to this HashMap, putting them as the **keys** of the map, associated to a dumb value called `PRESENT` in the source code. One consequence is that, just like map keys, you should **not mutate the state of an object** once you have added it to a HashSet — if you do that (and the hashCode changes), your set will **not work properly**. One last word: if you have a lot of objects to add to a set, try to create it with the **right size upfront** — there is a factory method for that (`new HashSet<>(capacity)`).

```java
Set<String> s = new HashSet<>(1000);  // avoids rehashing while populating
s.add("a");    // internal: map.put("a", PRESENT)
s.add("a");    // put returns PRESENT != null -> add returns false

// DON'T: mutating an object after insertion (hashCode changes = it "vanishes")
```

---

### Question 179 — What is JShell?
· [video](https://youtu.be/kXWl_Tv5wiU) · local file `220`

**Short answer:** A REPL.

**Less short answer:** REPL stands for **Read, Evaluate, Print, Loop**. It's actually a command-line tool that you can use to quickly write snippets of code and test them. You can launch it with the `jshell` command — that can take a bunch of options, by the way; you can see them by typing `jshell -h`. Then you can create variables, call methods, create records, and experiment with any feature — you can even activate the **preview features** in JShell, so you can experiment with them too. One last word: you can even write your Java code on **several lines** with JShell — very handy to create complex records, for instance.

```bash
$ jshell
jshell> record Point(int x, int y) { }
jshell> var p = new Point(1, 2);
p ==> Point[x=1, y=2]
jshell> p.x() + p.y()
$3 ==> 3
jshell> /exit
```

---

### Question 180 — What is the difference between Comparator and Comparable?
· [video](https://youtu.be/LBSSz2NOeQU) · local file `219`

**Short answer:** A Comparator can compare **two** objects; a Comparable can compare **itself** to another object.

**Less short answer:** Both are interfaces. `Comparable` defines a method that receives **another object from the same class** — so a comparable object can compare itself to some other object. `Comparator` is also an interface, for the same type of objects: these objects can be both comparable, but a comparator can implement a **different comparison**. One last word: unless you really need it, don't implement your comparators yourself — check the Comparator interface first: there are plenty of factory and default methods that you can use for that.

```java
record Person(String name, int age) implements Comparable<Person> {
    public int compareTo(Person o) { return name.compareTo(o.name); }
}

Comparator<Person> byAge = Comparator.comparingInt(Person::age);
Comparator<Person> full = byAge.thenComparing(Person::name).reversed();
// compareTo = natural order; Comparator = external strategies
```

---

### Question 181 — What is the difference between the reduce() and collect() methods?
· [video](https://youtu.be/8ddP1WDxULg) · local file `218`

**Short answer:** They don't work the same way.

**Less short answer:** These methods are methods from the Stream API. **`reduce`** consists in applying a reduction operation — an **immutable** one — on the elements consumed by your stream. This operation is modeled by a `BiFunction`, that should be **associative**. **`collect`** reduces your elements in a **mutable container** — typically a collection or a map. `Stream<T>` has a collect method that takes a `Collector` as a parameter, whereas specialized streams (IntStream and the like) do not — that's for type reasons: there is no specialized collector. One last word: some reduce methods return an `Optional`, and sometimes the corresponding collector does the same — but not always: that's the case for the `summarizing` collectors, that return the statistics object. *(Another video.)*

```java
// reduce: immutable, associative
Optional<Integer> sum = stream.reduce(Integer::sum);
Integer sum0 = stream.reduce(0, Integer::sum);

// collect: a mutable reduction, in a container
List<Integer> list = stream.collect(Collectors.toList());

IntStream.of(1, 2).reduce(Integer::sum);   // OK
// IntStream has no collect(Collector): no specialized collectors
IntSummaryStatistics st = IntStream.of(1, 2).summaryStatistics();
```

---

### Question 182 — What is a vararg?
· [video](https://youtu.be/OmghkfwJaTg) · local file `217`

**Short answer:** An array.

**Less short answer:** A vararg is a feature of the language that allows you to pass **zero or more** elements to a method: you declare the parameter with the **three dots**, and then you can call it with any number of Strings. There are two constraints with varargs. First, you can only use **one** vararg per method; and second, this vararg parameter should be the **last** of your parameters — no parameter is allowed after a vararg; your compiler will tell you that. In your method, this vararg **is an array**; and by the way, you can also pass an array directly to a method that declares a vararg. One last word: you can also use varargs with constructors — and because you can pass an array, you should still use a **defensive copy** with varargs.

```java
static int sum(int... numbers) {   // it's an int[] inside
    int t = 0;
    for (int n : numbers) t += n;
    return t;
}
sum();                 // 0 elements: OK
sum(1, 2, 3);          // any count
sum(new int[]{1, 2});  // a plain array works too!
// int sum(int... a, int b)  // ERROR: the vararg must be last
```

---

### Question 183 — How should you close an IO resource?
· [video](https://youtu.be/rBNQz5hd2lo) · local file `216`

**Short answer:** You should call its close method.

**Less short answer:** The only **safe** way, that guarantees that this method is called in all circumstances, is to open your IO resources in a **try-with-resources** statement. In that way, even if an exception is interrupting your normal process, your close method is called when your program exits the try block. One last word: you can use **any object that implements `AutoCloseable`** with a try-with-resources statement — and to implement this interface, you just need to create a close method. So creating your own AutoCloseable objects is actually very simple — you should definitely use these patterns in your applications.

```java
try (var reader = Files.newBufferedReader(Path.of("a.txt"));
     var writer = Files.newBufferedWriter(Path.of("b.txt"))) {
    writer.write(reader.readLine());   // even with an exception...
}   // ...both close() run, in the REVERSE order of the declaration
```

---

### Question 184 — What is the difference between a Fork/Join pool and an ExecutorService? *(version 2)*
· [video](https://youtu.be/sSw8r2hxCtk) · local file `215`

**Short answer:** ExecutorService is an interface; the Fork/Join pool is a class. *(an implementation of the former)*

**Less short answer:** There are many implementations of ExecutorService — and ForkJoinPool, by the way, is one of them. There is one point worth mentioning: with the cached thread pools and fixed thread pools available from the `Executors` factory class, your tasks are stored in a **single waitlist** from which the threads can take them. In a ForkJoinPool, there is **one waitlist per thread**, and the pool implements the **work stealing** pattern — so a thread can steal a task from another waitlist in case its own is empty. One last word: as of now, there are **two** ForkJoinPools used by the JVM: one to run your **parallel streams**, and another one to run your **virtual threads**.

```java
ExecutorService fixed = Executors.newFixedThreadPool(8);  // 1 global queue
ForkJoinPool fjp = (ForkJoinPool) Executors.newWorkStealingPool();
// one queue per thread + work stealing from the opposite ends
```

---

### Question 185 — What is a spliterator? *(version 2)*
· [video](https://youtu.be/qcBWtKY9grk) · local file `214`

**Short answer:** An interface.

**Less short answer:** This interface models the way you can **connect a stream to a source of data**. The name comes from *iterator* and *split* — splitting is the feature used by parallel streams. Implementing this interface is not so easy: you have **four methods to implement** — `trySplit`, `estimateSize`, `characteristics` and `tryAdvance`, which is the one called to get the elements one by one — and **four more default methods** that you can redefine: `getExactSizeIfKnown`, `forEachRemaining`, `hasCharacteristics` and `getComparator`. One last word: understanding how all this is working can be very interesting for your application. *(Another video.)*

```java
Spliterator<Integer> sp = Spliterators.spliterator(
        new int[]{1, 2, 3}, Spliterator.ORDERED | Spliterator.SIZED);

sp.tryAdvance(n -> System.out.println(n));  // element by element
Spliterator<Integer> half = sp.trySplit();  // split for parallelism
```

---

### Question 186 — How can you get an instance of a Class?
· [video](https://youtu.be/r23hw0duW2c) · local file `213`

**Short answer:** There are actually four ways to do that, at least — depending on what you have.

**Less short answer:** If what you have is an **object**, then you can call `getClass()` on it — which is also working for arrays. Second, you can use the **`Type.class`** syntax, that works for object types **and** primitive types. If what you have is the **name of a class**, you can call `Class.forName(...)` — it also works with arrays. To get the class instance that represents a primitive type, you can also use the **`TYPE`** constant defined in all the wrapper types — and starting with JDK 22, you can get the class that represents a primitive type with the **`Class.forPrimitiveName("int")`** method. One last word: you can also get classes from other classes, with `getSuperclass()` or `getInterfaces()`.

```java
String s = "a";
s.getClass();                         // 1) from the object
String.class;                         // 2) the literal (object or primitive)
Class.forName("java.lang.String");    // 3) by name
Integer.TYPE;  int.class;             // 4) TYPE / forPrimitiveName (JDK 22)
Class.forName("[Ljava.lang.String;"); // arrays too!
```

---

### Question 187 — What does it mean to capture a variable? *(version 2)*
· [video](https://youtu.be/DIXeOiF52JA) · local file `212`

**Short answer:** It means that you have a local class that references a variable from its enclosing method.

**Less short answer:** Starting with JDK 8, local classes can capture **final** variables — this variable can also be **effectively final**, which means that the compiler adds the `final` keyword for you; it is only possible if you do not modify this variable in your code. Odds are that you will do that with **lambda expressions** — and in that case, this lambda is called a *capturing lambda*. One last word: there is a very good reason why lambda expressions can only capture final variables — that is, **values**, not modifiable variables — and it has to do with concurrency. *(Another video.)*

```java
Function<Integer, Integer> addWith(int base) {
    int adjust = base * 2;             // effectively final
    return x -> x + adjust;            // a capturing lambda
}
// int adjust2 = ...; adjust2++; -> a lambda over adjust2 would not compile
```

---

### Question 188 — What is serialization?
· [video](https://youtu.be/psXVAoAZ7eE) · local file `211`

**Short answer:** A way to save graphs of objects in files — or to send them over the network.

**Less short answer:** Serialization used to be one of the killer features of the Java platform. It turns out that now the situation has changed: there are **several security problems** with it. For instance, deserializing an object **does not call any of its constructors** — thus bypassing any validation rules; and so it can be used by some malicious code to inject corrupted objects in your application. One last word: there are several ways to protect your application against that. First, you can use **records** — deserialization calls the canonical constructor of records. And then you can use **deserialization filters** at the JVM level. *(Another video.)*

```java
class Order implements Serializable { ... }  // a marker interface

// protection 1: records validate on the way back
record Ssn(String value) {
    Ssn { if (!value.matches("\\d{9}")) throw new IllegalArgumentException(); }
}
// protection 2: an ObjectInputFilter (JEP 290) on deserialization
```

---

### Question 189 — What does happen-before mean?
· [video](https://youtu.be/iW8YzPJff1k) · local file `210`

**Short answer:** It has to do with concurrency.

**Less short answer:** There are several references to *happens-before* links in the JavaDoc — for instance, the `Future` interface tells you that the actions taken by the asynchronous computation **happen before** the actions following the corresponding `future.get()`. The problem may occur if you modify your variable, and this variable is read by another thread: each thread operates on its **own CPU cache**, and you need a special operation to make sure that the modification you made is **visible** from all the other CPU caches — and this special operation is triggered by the fact that the happens-before link **exists between the write operation and the read operation**. One last word: your only source of information should be **Chapter 17 of the Java Language Specification** (the Java Memory Model).

```java
// without happens-before: the write may NEVER be seen
boolean ready = false;
// thread A: data = 42; ready = true;
// thread B: while (!ready) {}; use(data);  // data may be 0 (!)

// with happens-before (volatile write -> volatile read):
volatile boolean readyV;
// thread A: data = 42; readyV = true;   // written BEFORE (hb)
// thread B: if (readyV) use(data);      // read AFTER: data=42 guaranteed
```

---

### Question 190 — When can you use finalize()?
· [video](https://youtu.be/esvDd8jTkHc) · local file `209`

**Short answer:** Never.

**Less short answer:** **Never.** Any code you put in finalize is, most of the time, buggy, useless — or both. Don't try to help the garbage collector by clearing your collections or your maps: first, the garbage collector is smart enough to do it itself; and second, you may have **race conditions** in this code, that may lead to `NullPointerExceptions`, among other nasty bugs. Don't close your resources in finalize — they may **never be closed**; prefer the **try-with-resources** pattern, which is much safer. One last word: finalize is now **deprecated for removal** — maybe it will stay there for a while, but at some point the garbage collector will not call it anymore. So stop overriding this method.

```java
class Bad {
    @Deprecated(forRemoval = true)
    @Override protected void finalize() {   // NEVER
        map.clear();     // potential race condition (GC thread!)
        connection.close(); // may never run -> leak
    }
}
// right: try-with-resources + (if needed) a Cleaner
```

---

### Question 191 — How can you skip elements in a stream?
· [video](https://youtu.be/djmcC_H8LV0) · local file `208`

**Short answer:** There is a method for that.

**Less short answer:** You can call `skip(n)` on a stream, passing the number of elements you need to skip as an argument. That being said, there are several caveats you need to be aware of: for instance, skipping elements only makes sense for **ordered** streams. If your stream lists all the lines of a text file, then calling skip(2) skips the first two lines of your text. Skipping elements on a **non-ordered** stream leads to **unpredictable results**. One last word: skipping may be **costly** to call on parallel streams — you may need inter-thread communication to drop the first elements you want to skip; there is another overhead in doing that.

```java
Files.lines(path)                  // ordered (the lines of the file)
     .skip(2)                      // the first two lines are gone
     .forEach(System.out::println);

Set.of(1, 2, 3).stream().skip(2);  // not ordered: unpredictable!
```

---

### Question 192 — What is an anonymous class?
· [video](https://youtu.be/R5M8N9vyO2Q) · local file `207`

**Short answer:** A class that does not have a name.

**Less short answer:** The syntax allows you to create **the class and its instance at the same time**. It's useful when you need to quickly implement an interface or extend a class right in your code. There are cases where an anonymous class can be written as a **lambda** — but not always: a lambda implements a **functional interface**; an anonymous class can extend **anything** — and you cannot create a constructor in an anonymous class (you can use an **instance block** if you need to do some initialization). One last word: you can use anonymous classes in conjunction with the `var` keyword — which gives you access to the associated **non-denotable type**. *(Another video.)*

```java
Runnable task = new Runnable() {      // the class + the instance at once
    @Override public void run() { System.out.println("I ran"); }
};

var box = new Object() {              // anonymous + var: access to the type
    int value = 0;                    // non-denotable (JEP 286)
    void add() { value++; }
};
box.add(); box.value++;               // members of the type itself!
```

---

### Question 193 — How can you read a GZIP compressed file?
· [video](https://youtu.be/7T-K6fPAEo0) · local file `206`

**Short answer:** There is a class for that.

**Less short answer:** This class is the `GZIPInputStream`, and it takes **another** input stream as a constructor argument — so it's actually a **decoration** on another input stream. Along with the GZIPOutputStream, this class allows you to read and write data compressed in the gzip format. Note that the gzip algorithm supports **streaming** — meaning that you can get the uncompressed data **while** you are reading the compressed stream, with the different read methods available. One last word: Java I/O also supports the **zip** file format, which allows you to compress directory trees — but that will be for another time.

```java
try (var gz = new GZIPInputStream(
             Files.newInputStream(Path.of("log.txt.gz")));
     var reader = new BufferedReader(
             new InputStreamReader(gz, StandardCharsets.UTF_8))) {
    reader.lines().forEach(System.out::println);  // decompresses while reading
}
```

---

### Question 194 — What is a LinkedHashSet?
· [video](https://youtu.be/BCwHuOyaDGw) · local file `205`

**Short answer:** A set with linked elements.

**Less short answer:** This set maintains **links between its elements** so that it maintains the **iteration order**. If you add "C, A, B, B" in a HashSet, the second B is not added — and if you iterate over the elements of this set, you will get "A, B, C", because the HashSet does not maintain the order of its elements. If you do the same with a LinkedHashSet, you will get **"C, A, B"** — the LinkedHashSet does what is needed to maintain this order. One last word: starting with JDK 21, LinkedHashSet implements **`SequencedSet`**, which brings new methods — like direct access to the first and last element of the set, and the `reversed()` method.

```java
Set<String> hs = new HashSet<>(List.of("C", "A", "B", "B"));
hs.stream().forEach(System.out::print);   // A B C (any order)

LinkedHashSet<String> lhs = new LinkedHashSet<>(List.of("C", "A", "B", "B"));
lhs.forEach(System.out::print);           // C A B (insertion order)
lhs.getFirst();   // "C"  (JDK 21+, SequencedSet)
lhs.reversed();   // a B A C view
```

---

### Question 195 — What is an intermediate collector?
· [video](https://youtu.be/2J0uNLu1ZpQ) · local file `204`

**Short answer:** A collector.

**Less short answer:** The `Collectors` factory class gives you a bunch of pre-made, ready-to-use collectors — and some of the factory methods model stream **intermediate** operations, like `mapping`, `filtering`, or `flatMapping`. These are called intermediate collectors. But then there is a problem: a collector models the **terminal** stream operation — so these factory methods also take **another collector**, to compose the mapping operation, for instance, with this other collector. You can then create complex collector compositions, ending with the terminal collector — like toList() or counting(). One last word: the Collector API is quite complex, but also very powerful — you should definitely check it out; we will talk more about it in the future.

```java
// mapping = an intermediate collector: it needs a downstream
Map<String, List<String>> byCountry = cities.stream()
        .collect(Collectors.groupingBy(
                City::country,                                 // terminal
                Collectors.mapping(City::name,                  // intermediate
                        Collectors.filtering(n -> !n.isBlank(), // intermediate
                                Collectors.toList()))));        // terminal
```

---

### Question 196 — What is a Hashtable?
· [video](https://youtu.be/TvLm8R45ziQ) · local file `203`

**Short answer:** Something you should not be using anymore.

**Less short answer:** Hashtable was created in the early days of the JDK, **before** the introduction of the Collections framework. The implementation is very similar to HashMap — apart from the handling of hash collisions, which is not as efficient, and from the fact that **all the map methods are synchronized**. So if you do not need synchronized methods, you can use the HashMap — in general, HashMap is always a better choice; and if you do need synchronization, then ConcurrentHashMap is also a better choice. One last word: be careful — Hashtable does **not support null values or null keys**, and if you try to put a null key, you will get a NullPointerException. HashMap supports null keys and null values — even if it may not be the greatest idea.

```java
// legacy (JDK 1.0): everything synchronized, no nulls
Hashtable<String, String> ht = new Hashtable<>();

// modern:
Map<String, String> hm = new HashMap<>();           // no sync, with nulls
Map<String, String> chm = new ConcurrentHashMap<>(); // real concurrency
```

---

### Question 197 — What is a Consumer?
· [video](https://youtu.be/TJeDLWUES6c) · local file `202`

**Short answer:** A functional interface.

**Less short answer:** This functional interface models a lambda that takes a **single parameter** and does not return anything. The Consumer is what you pass to the **forEach** method, that you have on the streams and the collections. You can compose consumers thanks to an `andThen` default method on the Consumer interface — so you can create one consumer from two others, just by calling `print.andThen(log)`, for instance. One last word: the java.util.function package also gives you a `BiConsumer`, that takes **two** parameters instead of one, and specialized consumers: `IntConsumer`, `LongConsumer` and `DoubleConsumer`.

```java
Consumer<String> print = s -> System.out.println(s);
Consumer<String> log   = s -> logger.info(s);

list.forEach(print.andThen(log));   // composition: prints AND logs

BiConsumer<String, Integer> put = Map::put;   // two parameters
IntConsumer printInt = System.out::println;   // primitive: no boxing
```

---

### Question 198 — How can you read chars from a binary stream?
· [video](https://youtu.be/nH3Fpwb1yFU) · local file `201`

**Short answer:** There is a class for that.

**Less short answer:** The class is `InputStreamReader`, which extends `Reader` — so it can read characters — and that takes an `InputStream` as a constructor argument: InputStreamReader **decorates** an existing input stream, reading its binary content, and exposing it as **characters**. By default, the characters you read are encoded using the **default charset** — and starting with JDK 18, this charset is **UTF-8**. One last word: you also have an `OutputStreamWriter` class, that extends `Writer`, so that it can write characters and strings on the output stream it is built on.

```java
try (var reader = new BufferedReader(new InputStreamReader(
        Files.newInputStream(Path.of("data.bin")), StandardCharsets.UTF_8))) {
    reader.lines().forEach(System.out::println);
}
// InputStreamReader decorates: bytes -> chars (UTF-8 default since JDK 18)
```

---

### Question 199 — Can you build an object without calling its constructor?
· [video](https://youtu.be/ZYEsagr1CjI) · local file `005`

**Short answer:** No. *(...actually, yes — see the less short answer.)*

**Less short answer:** The Java language is designed in such a way that calling a constructor is **the only way** to create an object. Of course, you can create factory methods that you call from your application code, so that this constructor code does not appear in this application code — but this factory method does call the constructor. There is, however, **one exception to that, which is serialization**: when you deserialize an object, the deserialization mechanism **does not call the constructor** of that object — it bypasses it. Which is actually a security issue: if you have some validation rules in your constructor, they will be bypassed by the deserialization mechanism — allowing corrupted objects to live in your application. One last word: records were created after serialization was implemented — and they are **an exception to the exception**: deserialization does call your record constructors and their validation rules; there is no way you can create a record without calling its canonical constructor. One more reason to use them wherever you can.

```java
// Deserializing a regular class: the constructor is NOT called (validation bypassed)
ObjectInputStream in = new ObjectInputStream(new FileInputStream("order.ser"));
Order o = (Order) in.readObject(); // the constructor's rules are skipped!

// Records: always go through the canonical constructor, even on deserialization
record Ssn(String value) {
    Ssn {                          // validation guaranteed on EVERY path
        if (!value.matches("\\d{9}")) throw new IllegalArgumentException();
    }
}
```

---

### Question 200 — What is the heap memory?
· [video](https://youtu.be/BcVKKXx8KT0) · local file `200`

**Short answer:** The memory managed by your garbage collector.

**Less short answer:** The memory used by the JVM is made of several parts; the heap memory is the one in which your **objects** are stored. When you call `new ArrayList()`, for instance, what you get is a reference to some piece of memory that lives in the heap memory. Then, when you do not need this ArrayList anymore, the **garbage collector** will detect it, and will reclaim it, to use it for other objects. There are several strategies, implemented by the different garbage collectors, depending on what you need for your application. One last word: there is also an **off-heap** memory, not managed by your garbage collector — the JDK gives you two APIs to access it: the ByteBuffer API and the Memory API. *(Another video.)*

```java
ArrayList<String> l = new ArrayList<>(); // the object on the HEAP...
l.add("a");                              // ...the reference on the thread stack
l = null;   // unreachable -> the GC reclaims the heap memory

// outside the GC: off-heap (a direct ByteBuffer / MemorySegment+Arena)
```

---

### Question 201 — How is the reduce() method working?
· [video](https://youtu.be/Dxij5GSUM40) · local file `199`

**Short answer:** It reduces the elements of the stream.

**Less short answer:** There are two reduce methods on the Stream interface. The first one takes only the **reduction operation**, in the form of a lambda expression: the **first** and the **second** element of the stream are passed to this lambda; then the result and the **third** element; then the result and the **fourth** — until all the elements of the stream. The second overload takes the **identity element** of your reduction operation. This identity element is then used as the first element of the reduction; and if your stream is empty and you gave an identity element, then this element is returned. If not, then the reduction is undefined — the reason why this reduction returns an **empty Optional**. One last word: that's why you need Optionals in Java — to have correct reductions.

```java
OptionalInt sum1 = IntStream.of(1, 2, 3, 4).reduce(Integer::sum);
// 1+2=3; 3+3=6; 6+4=10 -> OptionalInt[10]

int sum2 = IntStream.of(1, 2, 3, 4).reduce(0, Integer::sum);
// identity 0 -> 10 (an empty stream would return 0)

int sum3 = IntStream.empty().reduce(100, Integer::sum); // 100
IntStream.empty().reduce(Integer::sum);   // OptionalInt.empty
```

---

### Question 202 — What are the primitive types in Java?
· [video](https://youtu.be/jFBDFWAd3Cg) · local file `198`

**Short answer:** There are 8 of them.

**Less short answer:** There are 4 **integer** types — `byte`, `short`, `int` and `long`, of length 8, 16, 32 and 64 bits — then 2 **floating point** numbers, `float` and `double`, of length 32 and 64 bits; `char` for characters, and `boolean`. Primitive types are **values**, not references — so a primitive type can **never be null**. They are all associated with a wrapper type with the same name, apart from `int` → `Integer` and `char` → `Character`. One last word: operating on primitive types is **super fast** — no indirection, no pointer chasing. This is what you need to get the best performance — and this is what the **Valhalla** project aims to bring for object types. *(Another video.)*

```java
byte   b = 127;           // 8 bits
short  s = 32_767;        // 16
int    i = 2_000_000_000; // 32
long   l = 9_000_000_000L; // 64
float  f = 1.5f;          // 32
double d = 1.5;           // 64
char   c = 'J';           // 16 (UTF-16)
boolean ok = true;

Integer wi = i;        // wrapper: can be null, has a header...
int added = i + 1;     // ...primitive: a pure value, it flies
```

---

### Question 203 — What is an Optional? *(where it comes from)*
· [video](https://youtu.be/A4khh6CgsIA) · local file `197`

**Short answer:** A wrapper type that can be empty.

**Less short answer:** Optionals are there to correctly handle **reduction** in the case of **empty streams**. In that case, you would need to return the **identity element** of your reduction operation — you need that because, if you combine this result with the reduction of another non-empty stream, you still need the correct result. The problem is that **many reductions do not have** an identity element — that's the case for `max`, `min` or `average`. Returning an **empty Optional** is a way to solve this issue. One last word: even if you never produce empty streams on purpose, you may still have to deal with them because of `filter` operations or parallel streams — and both can produce empty streams **without you realizing it**.

```java
Optional<Integer> max = Stream.<Integer>of().max(Integer::compareTo);
// max() has no identity (the max of nothing does not exist) -> Optional

Stream.of(1, 2).filter(n -> n > 99)   // empty WITHOUT you realizing
      .max(Integer::compareTo);       // Optional.empty
```

---

### Question 204 — What is an expression?
· [video](https://youtu.be/7RmnsOBAXgU) · local file `196`

**Short answer:** Something that produces a value.

**Less short answer:** An expression is a combination of any operand, operators or method calls that, in the end, **produces a single value**. There are language constructs in Java that are **not** expressions — that's the case for the if/else or the while statements. The switch used to be a statement, but is now also an **expression** as of JDK 14 — and it is suddenly much more useful. You can switch on integer types (not `long`!), enumerations, strings and types. One last word: creating switch expressions on **sealed types** is part of the data oriented programming paradigm currently being implemented in Java — super interesting. *(Another video.)*

```java
int x = 1 + f(a) * 2;              // an expression: produces a value

// if is NOT an expression (it's a statement):
if (x > 0) { sign = 1; } else { sign = -1; }

// a switch expression (JDK 14+): produces a value!
String name = switch (code) {
    case 1 -> "one";
    case 2 -> "two";
    default -> "many";
};
```

---

### Question 205 — What is a ByteBuffer?
· [video](https://youtu.be/NKPNxhatAVk) · local file `195`

**Short answer:** A class from Java NIO.

**Less short answer:** NIO stands for *New Input Output* — that was new in **2002**; it's now called *Non-Blocking IO*. A ByteBuffer can be used to **allocate memory on the off-heap** and to **map files in memory** — with several caveats. First, they are indexed with **ints**; and second, they **can't be closed explicitly** — meaning that you cannot deallocate the off-heap memory. This class is now superseded by the **MemorySegment** class, indexed with `long` — memory segments are created by **Arenas**, that you can close to deallocate them. One last word: memory segments and arenas are part of **JEP 454**. *(Another video.)*

```java
// NIO (2002): int, no close
ByteBuffer buf = ByteBuffer.allocateDirect(1_000_000); // off-heap...
// ...no explicit deallocation (waits for the GC)

// FFM (JEP 454): long, with an arena
try (Arena a = Arena.ofConfined()) {
    MemorySegment s = a.allocate(1_000_000);
}   // deallocated here, deterministically
```

---

### Question 206 — What is a non-denotable type?
· [video](https://youtu.be/D6NbPd2GaFM) · local file `194`

**Short answer:** A type **inferred** by the compiler.

**Less short answer:** A non-denotable type is created by the compiler in several places — one of them is when you create an **anonymous class**; they are also used for **intersection types** (you can check JEP 286 for more details and examples). You can use the inferred type of an anonymous class with `var`: in that case, you have access to the fields and methods of the **anonymous type** — you can use this feature to create efficient **mutable boxes**, for instance, much more efficient than using an atomic variable. One last word: atomic variables are costly performance-wise, and they are there to handle concurrency — you should not use them outside of this context.

```java
// a mutable box with an anonymous class + var (non-denotable type)
var counter = new Object() { int value = 0; };
counter.value++;         // a field of the anonymous type, accessible via var!
counter.value++;

// AtomicInteger outside of concurrency = cost with no need
```

---

### Question 207 — What is the difference between a Zip and a Gzip stream?
· [video](https://youtu.be/MerwUgbYUXc) · local file `193`

**Short answer:** Gzip can compress a single file; Zip can compress a full directory tree.

**Less short answer:** Both are built on the same compression algorithm, called **DEFLATE**, and both are modeled by their own input and output stream classes. But Zip is a little hairy to use: you first need to create Zip entries for directories and files; then, after adding a file entry, you need to add the **content** of that file. Reading this content mirrors this procedure: you first need to **discover the entries**, then read the files. One last word: you can actually create a **file system** for that — and then you can just create directories and copy files to this file system, normally. Much simpler.

```java
// Gzip: a single stream
try (var gz = new GZIPInputStream(Files.newInputStream(Path.of("log.gz")))) {
    gz.readAllBytes();
}

// Zip: a file system, and done (much simpler than manual ZipEntry)
try (FileSystem zip = FileSystems.newFileSystem(Path.of("data.zip"))) {
    Files.copy(Path.of("photo.png"), zip.getPath("/photo.png"),
               StandardCopyOption.REPLACE_EXISTING);
}
```

---

### Question 208 — What is a SequencedMap?
· [video](https://youtu.be/-rNpk1f1VY4) · local file `192`

**Short answer:** An interface.

**Less short answer:** It is a nice addition to the Collections framework in **JDK 21**: it models maps that have a **defined encounter order**. Because this order exists, you can get the **first entry** (`firstEntry`) or the first key, the **last** entry (`lastEntry`)... and it gives you other methods, like `reversed()`, that you can use to get a **reversed view** on the map — no copy is made. Implementation-wise, **TreeMap and LinkedHashMap are both sequenced maps**. One last word: `SequencedCollection` and `SequencedSet` have also been added along with the SequencedMap interface — that's **JEP 431**, that you can check for all the details, design decisions and examples.

```java
SequencedMap<String, Integer> m = new LinkedHashMap<>();
m.putFirst("a", 1);      // a new JDK 21 API
m.putLast("z", 26);
m.firstEntry();          // a=1
m.lastEntry();           // z=26
m.reversed().firstKey(); // "z" — a view, no copy
// TreeMap (by comparison) and LinkedHashMap (by insertion) implement it
```

---

### Question 209 — What is the Void type?
· [video](https://youtu.be/Uph8NepNSIw) · local file `191`

**Short answer:** *(no short answer in the video)*

**Less short answer:** This class is used in the **Reflection API**, which gives you access to the return type of methods — including the ones that can be `void`. Because `getReturnType()` returns a `Class`, this **Void** type was added to model this return. You could call that a dirty hack. You cannot **create an instance** of this type, and you cannot get anything of this type — so apart from this use in the Reflection API (and in the MethodHandle API, by the way), there is not much you can do with it. One last word: the compiler lets you use `void.class` in an instanceof or in a switch on types — but that's pretty useless, because the result will **always be false**. Don't do it.

```java
// in reflection: models the void return
Method m = Service.class.getMethod("start");
m.getReturnType() == void.class;     // true (Class<Void>)

// in generics: "a result that does not exist"
Callable<Void> task = () -> { run(); return null; };
CompletableFuture<Void> cf = CompletableFuture.runAsync(...);

// Void.TYPE == void.class; an instance of Void: impossible
```

---

### Question 210 — Why are Vector and Hashtable not deprecated?
· [video](https://youtu.be/FzHXE-cihC0) · local file `190`

**Short answer:** *(no short answer — straight to the point)* They stay for **backward compatibility**.

**Less short answer:** Removing them would break some old code. Deprecating something creates **warnings at compile time**, telling you "hey, this isn't safe to use" — and removing them could then be harmful. So they stay as they are: frozen. One last word: maybe in the future there will be a way to **advertise that there is a better pattern** (in your IDE, for instance) without generating compiler warnings. That would be great.

```java
// frozen legacy — it works, but don't use it in new code:
Vector<String> v = new Vector<>();       // synchronized method by method
Hashtable<String, String> h = new Hashtable<>();

// modern:
List<String> l = new ArrayList<>();      // or truly concurrent:
Map<String, String> m = new ConcurrentHashMap<>();
```

---

### Question 211 — What does late binding mean?
· [video](https://youtu.be/7O_WrIpY9NU) · local file `189`

**Short answer:** It has to do with knowing what method should be called **only at runtime**.

**Less short answer:** When you call an interface method in your code — or any method that can be overridden — the compiler **cannot tell** which method would be called at runtime, since it depends on the concrete type you have. Hence the name **late binding**: it means that your method call is **bound to an implementation only at runtime**. **Early binding** means that this binding can be resolved at **compile time** — that's the case when you call a particular **overload** of a method. One last word: late binding occurs when you call a method that can be overridden; it has a cost, but can be optimized in some cases.

```java
List<String> l = new ArrayList<>(...);
l.size();   // late: ArrayList.size? LinkedList.size? runtime decides

Math.max(1, 2);        // early: the overload resolved at compile time
Math.max(1L, 2L);      // (another overload, already decided)
```

---

### Question 212 — How is Stream.distinct() working?
· [video](https://youtu.be/qaRycnqa1s4) · local file `188`

**Short answer:** It removes the duplicates from the stream.

**Less short answer:** Internally, distinct adds the elements of the stream in a **hash set**. When you add an element to a hash set, the `add` method returns a boolean: if this boolean is **false**, it means that the element was **not added** because it was already in the hash set — and in that case, distinct does **not push** this element to the downstream. If it is **true** (it was not there), this element is immediately pushed downstream. So internally, distinct **materializes** all the different elements of your stream. One last word: if you call distinct on an **ordered** stream, then a **LinkedHashSet** is used instead of a regular hash set — to preserve the order of your stream.

```java
Stream.of("a", "b", "a", "c", "b")
      .distinct()                     // a LinkedHashSet internally
      .forEach(System.out::print);    // abc

// under the hood: seen.add(x) ? push it downstream : drop it
```

---

### Question 213 — What does capturing a variable mean?
· [video](https://youtu.be/_Zh4cBXy2AM) · local file `187`

**Short answer:** "Capturing" is when the lambda refers to a field or to a local variable.

**Less short answer:** A lambda expression can refer to the **fields** of a class it is declared in, and to the **local variables** of the method it is declared in. In case your local variable is not declared `final`, the compiler will try to make it final for you — and if it can, the variable becomes **effectively final**, and you can capture it. If it can't, then you get a compiler error. One last word: returning or capturing lambdas can **expose the captured state to race conditions** — local variables are never subject to race conditions in Java, and that is exactly why you **cannot capture them** if they are not final.

```java
class Service {
    private int calls;                    // a field: captured through this

    Runnable counter() {
        int local = 42;                   // effectively final: OK
        return () -> {
            use(local);
            // local++ is illegal: only final values can be captured
            calls++;                      // a field: always "sees" changes
        };
    }
}
```

---

### Question 214 — How can you create temporary files?
· [video](https://youtu.be/DROyk0_wkqI) · local file `186`

**Short answer:** There is a flag for that.

**Less short answer:** The factory methods from the `Files` factory class can take **`StandardOpenOption`s** as parameters — one of them is called **`DELETE_ON_CLOSE`**. If you pass it, the JVM will try to **delete the corresponding file when you close** this writer or this output stream; and if you don't close it — which is a mistake, you should close it — then it will try to delete it when you **exit your application**. One last word: you should use this option with caution — you can't be sure that some other outside process did not open your file; so deleting it may be either impossible, or dangerous, or bug-inducing.

```java
Path tmp = Files.createTempFile("processing", ".tmp");

try (var out = Files.newOutputStream(tmp,
        StandardOpenOption.DELETE_ON_CLOSE)) {   // deletes on close
    out.write(data);
}   // closed = the temporary file is removed
// without closing: the removal is only attempted when the JVM exits
```

---

### Question 215 — How is Collection.retainAll() working?
· [video](https://youtu.be/TL1g6CedBAo) · local file `185`

**Short answer:** It computes the **intersection** of two collections.

**Less short answer:** retainAll only keeps the elements of this collection that are **also in** the collection you pass as an argument — so, to be kept, an element needs to be present in both collections. retainAll returns a **boolean**: true if this collection was modified. Internally, retainAll calls `contains` on the collection you pass **for each element** of this collection — so if you want this call to be efficient, you should pass **HashSets** to retainAll. One last word: there are three methods to deal with sets in the Collections framework: `addAll` for the **union**, `removeAll` for the **set difference**, and `retainAll` for the **intersection**.

```java
Collection<String> letters = new ArrayList<>(List.of("a", "B", "c"));
Set<String> vowels = Set.of("a", "e", "c");   // a HashSet: contains O(1)

boolean changed = letters.retainAll(vowels);
// [a, c]; changed == true
```

---

### Question 216 — What does tearing mean?
· [video](https://youtu.be/CJGZBdOdBiw) · local file `184`

**Short answer:** It has to do with concurrency, and 32-bits versus 64-bits machines.

**Less short answer:** As you know, some types are stored on 64 bits. On a 64-bits machine, writing a `long` or a `double` in memory is done in **one operation**; but on a **32-bits** machine, you need **two write operations**. If you're not cautious in a multi-thread environment, the thread during the write operations can be interrupted — it means that some other thread could see an **intermediate state**, where only the first 32 bits have been updated, but not the last. This is what **tearing** is. One last word: you can prevent that using **synchronization** — but it has a cost.

```java
long timestamp;   // 64 bits

// a 32-bits JVM: two 32-bits writes
// thread A in the middle of the write || thread B reads:
// -> half old + half new = a torn value (torn read)

// protection: volatile (the JMM guarantees atomicity for volatile long/double)
private volatile long safe;
```

---

### Question 217 — What is a stream builder?
· [video](https://youtu.be/GCvqipGBdy0) · local file `183`

**Short answer:** A builder for streams. *(literally)*

**Less short answer:** You can build a stream by using `Stream.builder()`. The pattern is the following: first, you create this builder with the `builder()` factory method; second, you can add objects to it with chained `add()` calls (or `accept()`); once you're done adding elements, you can call `build()` — you can only call build **once** — and then you can use this stream as any other. Note that this stream is **ordered** and can accept any value, **including nulls**. One last word: this is useful when you can only generate your objects **one by one** — you do not need to put them in an array or a collection; you can rely on the internal spine buffer of this builder.

```java
Stream<String> s = Stream.<String>builder()
        .add("a")
        .add("b")
        .add(null)          // nulls are accepted!
        .build();           // build() only once

// useful when the elements are born one by one:
// while (hasNext()) builder.add(next());
```

---

### Question 218 — How can you stream the content of a text file?
· [video](https://youtu.be/coOY1WygWhg) · local file `182`

**Short answer:** *(straight to the two patterns)*

**Less short answer:** There are two patterns, depending on what you have. If what you have is a **Path**, then you can call the `lines()` method of the `Files` factory class, passing this path as an argument — simple. And if what you have is a **BufferedReader**, then you can call `lines()` directly on it (note that this `lines` method does **not** exist on regular readers). What you get is a stream of the lines of your text file. The nice thing is that the `Stream` interface is itself **AutoCloseable** — so you can create this stream in a try-with-resources statement: it will close the stream, which in turn calls the `close()` method of the underlying reader. One last word: you also have a `lines()` method on the String class, that does the same thing.

```java
try (Stream<String> lines = Files.lines(Path.of("app.log"))) {
    lines.filter(l -> l.contains("ERROR"))
         .findFirst();
}   // closes the stream AND the file

new BufferedReader(new FileReader(f)).lines();  // the variant
text.lines();                                   // String.lines()
```

---

### Question 219 — What is a Read-Write Lock?
· [video](https://youtu.be/oqset-iBpXs) · local file `181`

**Short answer:** An interface.

**Less short answer:** This has to do with concurrent programming. The Read-Write Lock is useful in situations where you want to allow **concurrent reads**, but you need to have **exclusive write** operations. It works better when you have many reads and very few writes. The pattern is the following: you create an instance with the `ReentrantReadWriteLock` implementation; then acquire the read or the write lock, use it, and **release it in a `finally` block** — releasing it there is important, because it will be executed even if you have an exception along the way. One last word: you can impose **fairness** on this implementation, giving the priority to the longest-waiting thread — but it has a cost.

```java
ReadWriteLock lock = new ReentrantReadWriteLock();

String read() {
    lock.readLock().lock();              // many readers together
    try { return data; }
    finally { lock.readLock().unlock(); }
}
void write(String next) {
    lock.writeLock().lock();             // 1, exclusive with everyone
    try { data = next; }
    finally { lock.writeLock().unlock(); }
}
```

---

### Question 220 — How is Collection.removeIf() working?
· [video](https://youtu.be/n__AzYh8r3E) · local file `180`

**Short answer:** It removes the elements of a collection that match the predicate passed as an argument.

**Less short answer:** The implementation of removeIf varies depending on the collection you have. The default behavior is implemented in the `Collection` interface: it iterates over the elements of the collection, and removes them if they match the predicate — it works for a HashSet or a LinkedList, for instance. Then this method is **overridden in ArrayList**: an ArrayList scans and **marks** all the elements to be removed, then removes them in a second pass. One last word: be careful — your predicate should **not read the collection** it works on, nor throw any exception, because then the behavior could be different on an ArrayList and on other implementations.

```java
List<Integer> l = new ArrayList<>(List.of(1, 2, 3, 4, 5));
l.removeIf(n -> n % 2 == 0);   // [1, 3, 5]

// ArrayList: marks in one pass, compacts in the other (O(n))
// a predicate reading 'l' = unspecified behavior!
```

---

### Question 221 — What is a weak reference?
· [video](https://youtu.be/VvTEZ1dCU64) · local file `179`

**Short answer:** A reference that is managed by the garbage collector in a certain way.

**Less short answer:** You can create an instance of `WeakReference` by passing a reference to any object — what you're doing is actually telling the garbage collector: "hey, if you need some space, you can **reclaim this object**". If some other object has a **regular (strong) reference** to your object, then calling `get()` will always return your object; but if at some point no regular reference exists anymore, then the garbage collector is **allowed** to set the object your weak reference is pointing to, to null, and reclaim this object — calling `get()` will then return **null**. One last word: weak references are used in a `WeakHashMap` — a map that can be really useful to design **caches**. *(Another video.)*

```java
Config big = new Config();                  // a strong reference
WeakReference<Config> weak = new WeakReference<>(big);

weak.get();     // != null as long as 'big' exists
big = null;     // nobody holds it anymore...
System.gc();
weak.get();     // probably null: the GC reclaimed it
```

---

### Question 222 — What is an IdentityHashMap?
· [video](https://youtu.be/ptKwNH0BmMA) · local file `178`

**Short answer:** A map.

**Less short answer:** When you look for a key in a map — whether it is to put a new key-value pair, or because you are looking for a key — your first compute a **hash code** for that key; and because two keys that are different can still have the same hash code, you compare them. In a regular HashMap, you compare them using their **`equals`** method; in an **IdentityHashMap**, you use **`==`** — that is, reference equality. So if you run this code, you will end up with **one** key-value pair in your map... and if you run this other one, you end up with **two**. One last word: IdentityHashMap can work as long as your keys can be **referenced** — so don't use value objects in them, because it will fail when Valhalla is there.

```java
record Key(String id) { }
Map<Key, Integer> m = new IdentityHashMap<>();
m.put(new Key("a"), 1);
m.put(new Key("a"), 2);   // different == -> TWO entries!
m.size();                 // 2 (with a HashMap it would be 1)
```

---

### Question 223 — How can you limit a stream?
· [video](https://youtu.be/qLB5Vx-sYuU) · local file `177`

**Short answer:** There is a method for that.

**Less short answer:** Technically speaking, you are not really "limiting" a stream — what you're saying is that, once this stream has processed the given number of elements, it should **stop**. And you need to be careful, because at each stage, what you get is a **new** stream object. If what you need is to **skip the first two and the last two** elements, then writing the "obvious" `skip(2).limit(...)` does not give you what you expect — the stream on which you call limit has consumed only 4 elements, not 6; the correct code takes that into account. One last word: skipping/limiting only makes sense on **ordered** streams — and both manage an internal mutable counter, so **don't use them in parallel streams**.

```java
Stream<Integer> s = Stream.of(1, 2, 3, 4, 5, 6);

// skip the first 2 and the last 2:
s.skip(2).limit(6 - 2 - 2)   // limit = total - skipStart - end (not limit(4)!)
 .forEach(System.out::print); // 3 4
```

---

### Question 224 — What is a latch?
· [video](https://youtu.be/6XqDEaIez1U) · local file `176`

**Short answer:** A closed door that will open after you knocked on it a certain amount of times.

**Less short answer:** A `CountDownLatch` is a class used in concurrent programming. You create an instance of it by giving it a **counter**; some threads will call `latch.await()`, and some other threads can also call the `latch.countDown()` method, which will **decrease** the internal count. Once this counter reaches **zero**, then the door opens, and all the threads blocked on the await calls are **free to run**. Once a latch opens, it **cannot be closed again** — you can use a latch, for instance, to start an application where you need to initialize resources concurrently. One last word: you can check the `CyclicBarrier` class if what you need is something that you can **close again**.

```java
CountDownLatch ready = new CountDownLatch(3);

for (int i = 0; i < 3; i++) {
    new Thread(() -> { initResource(i); ready.countDown(); }).start();
}
ready.await();            // waits for the 3 to knock on the door
System.out.println("all ready");   // the door is open forever
```

---

### Question 225 — What is a try with resources statement?
· [video](https://youtu.be/bFS_vxuhHMo) · local file `175`

**Short answer:** A great language feature that can close your resources for you.

**Less short answer:** The try-with-resources statement works with any class that implements **`AutoCloseable`** — which defines only one method, `close()`. So you can use it with all your classes that have a `close()` method that returns void. You create your instances as the argument of the try; and then, when your program exits the following block of code — whether it is with an exception or not — all the `close()` methods are called **in the right order** (the reverse one). One last word: it's important to **open all your resources within the try** — because if you don't, you may have some buffer that will not be properly flushed before the close is called, leading to some loss of data.

```java
try (var input = Files.newBufferedReader(in);
     var output = new BufferedWriter(Files.newWriter(out))) {  // order!
    // output closes first, then input (reverse of the declaration)
    output.write(input.readLine());
}   // close() always runs, even with an exception (close exceptions: suppressed)
```

---

### Question 226 — What is a Gatherer?
· [video](https://youtu.be/enCKyj4tgB8) · local file `174`

**Short answer:** An interface.

**Less short answer:** You can see a gatherer as the **equivalent of a collector, but for intermediate operations**. The Gatherer API gives you three elements: the `Gatherer` interface; a `gather` method on the Stream interface; and a `Gatherers` factory class with some ready-to-use gatherers. A gatherer can model intermediate operations that **do not exist** on the Stream API. Suppose that you want to remove the duplicates from a stream of strings of characters, but **ignoring case differences** — you can't do it with the Stream API, but you can design a gatherer to do that. One last word: gatherers can do many things — they can push as many elements as you need to the stream, they can manage an internal mutable state, and they can decide to **interrupt** the stream (or not).

```java
// the collector equivalent, for INTERMEDIATE operations (JDK 22+)
Stream<String> folded = words.gather(Gatherers.fold(...));

// the cited example: distinct ignoring case — the API doesn't have it;
// a gatherer does:
Gatherer<String, ?, String> distinctIgnoreCase = /* your gatherer */;
Stream.of("Java", "JAVA", "java").gather(distinctIgnoreCase);
```

---

### Question 227 — What is type inference?
· [video](https://youtu.be/biKNoNMKnNs) · local file `173`

**Short answer:** A way to deduce the type of a variable or an argument.

**Less short answer:** There are many places where you do not declare the type of the variable you use, and where the compiler needs to make sure that this type is correct. For instance, when you pass a **lambda** to a method, the compiler checks if this lambda matches the type of the parameter. In **JDK 10**, a `var` keyword was added to the language to extend the scope of type inference. One last word: this var keyword can be used **only for local variables** — not fields, nor method parameters — and these variables need to have an **initializer**.

```java
var map = new HashMap<String, List<Integer>>();  // JDK 10+: local with init

// does NOT compile:
// var field;            (a field)
// void m(var x) { }     (a parameter)
// var empty;            (no initializer)

Function<String, Integer> f = s -> s.length();    // a lambda: inferred type
```

---

### Question 228 — What is putIfAbsent()?
· [video](https://youtu.be/y0rwoUmjnb4) · local file `172`

**Short answer:** A method from the Map interface.

**Less short answer:** putIfAbsent takes a key and a value, and binds this value to this key under a condition that may be **slightly different from what you expect**. If you check the source code of putIfAbsent, you can see that it does **not** call `containsKey` — it calls `get`: if get returns null (the key is not there, **or** it is bound to null — why would you do that?), then it remaps it with the new value. So it is a good reason to use map implementations that do **not accept** null values or keys. One last word: putIfAbsent **returns the value that was previously bound** to this key — which may be null; be careful with that.

```java
Map<String, Integer> m = new ConcurrentHashMap<>();
m.putIfAbsent("count", 1);       // returns null (there was none)
m.putIfAbsent("count", 99);      // returns 1 — and does NOT overwrite
m.get("count");                  // 1

Map<String, Integer> hm = new HashMap<>();
hm.put("x", null);
hm.putIfAbsent("x", 5);          // get returned null -> WRITES 5!
```

---

### Question 229 — What is a PrintWriter?
· [video](https://youtu.be/P_ky1ui_bW4) · local file `171`

**Short answer:** *(the presenter skips it — straight to the explanation)*

**Less short answer:** This type is important because it is the type of **`System.out`**, the one on which you call `println` to pass "hello world" to it. It is an extension of the `Writer` class, and it's the class that carries the `println` and `printf` methods — this one takes that nifty, **horrible** format as a parameter. This format is coming from C and C++, and it allows you to format your strings of characters to properly display them on text terminals (yes, that was some time ago). By the way, this format is documented in the `java.util.Formatter` class, in case you need it. One last word: there are several formatting classes in the JDK — another one is based on the `Format` abstract class, extended by `MessageFormat` or `SimpleDateFormat`. *(Another video.)*

```java
PrintWriter out = new PrintWriter(Files.newBufferedWriter(
        Path.of("log.txt"), StandardCharsets.UTF_8));
out.println("start of processing");
out.printf("used: %,d (%.1f%%)%n", 32_000, 12.5);
out.flush();   // PrintWriter swallows exceptions: check checkError()!
```

---

### Question 230 — What is a ThreadLocal variable? *(version 2)*
· [video](https://youtu.be/8aAF2dvhOoY) · local file `170`

**Short answer:** Old stuff from Java 2.

**Less short answer:** ThreadLocal variables were added to allow **passing arguments from one servlet to another** without having to rely on method parameters — that was very useful in the early days of J2EE, because a servlet was actually your callback. Now, some of the design decisions made at that time do not fit very easily with modern frameworks — and especially with **virtual threads**. The reason why the **Loom Project** — that delivered virtual threads in JDK 21 — is developing **ScopedValues**: scoped values do the same thing as ThreadLocals, but **without the drawbacks**. One last word: ThreadLocals can be the source of **security vulnerabilities** in your application — so do not forget to call `remove()` on them once you're done with them.

```java
// the classic use: a request context without parameters
private static final ThreadLocal<Context> CTX = new ThreadLocal<>();
CTX.set(ctx);
try { chainOfServices(); }     // any servlet/service reads CTX.get()
finally { CTX.remove(); }      // in a thread POOL: MANDATORY
```

---

### Question 231 — What is a final field?
· [video](https://youtu.be/g0QSsRvC2sI) · local file `168`

**Short answer:** A field with the `final` modifier on its declaration.

**Less short answer:** A final field can be static or not. If it is a **static** field, then it must have an initializer — and once it has been initialized, it cannot be modified anymore; `Math.PI` is a `public static final` field defined that way in the `Math` class. If your field is an **instance** field, then it needs to be initialized **before your object is fully constructed** — and once you give a value to it, you cannot change it, not even from within another constructor. One last word: `final` also means that the value of your fields is **visible from other threads** — so you never need to declare a final field `volatile`.

```java
class Converter {
    static final double FACTOR = 1.609;    // static: initializer required
    private final BigDecimal rate;         // instance: set in the constructor

    Converter(BigDecimal rate) {           // the only chance to write it
        this.rate = rate;
    }
    // this.rate = otherValue;  // ERROR anywhere else
}   // final: visible to other threads without volatile
```

---

### Question 232 — How can you sort a collection?
· [video](https://youtu.be/_t0exqxJjck) · local file `167`

**Short answer:** Well... you can't.

**Less short answer:** It does not make sense to sort a Collection, because a collection doesn't have any **order** — the elements you will see if you iterate over them could change from one iteration to the other. You may be thinking that sorting only makes sense for **lists**, since lists are ordered — and indeed, there is a `sort` method. But you can also sort **some sets** — sorted sets, or navigable sets — and this is captured by the **sequenced collections** added in JDK 21: `SequencedSet` extends SequencedCollection, and there is also a `SequencedMap` for maps. One last word: **ordering and sorting** are two different notions — you need ordering to sort, but you can have order without sorting.

```java
List<Integer> l = new ArrayList<>(List.of(3, 1, 2));
l.sort(Comparator.naturalOrder());          // [1, 2, 3]

Set<Integer> sortedSet = new TreeSet<>(Set.of(3, 1, 2));
// TreeSet: always sorted (it's not a "sort", it's order maintenance)

// LinkedHashSet: ORDERED (insertion) but not SORTED
```

---

### Question 233 — How can you create an Iterable from a stream?
· [video](https://youtu.be/3GLBpNVtDtQ) · local file `166`

**Short answer:** *(straight to the pattern)*

**Less short answer:** The idea is to **avoid putting all the elements produced by the stream** in a collection or an array — to save on memory. So what you can do is get the **iterator** of the stream and use a factory method of the `Spliterators` class... *(the idea in short: an Iterable is basically a **supplier of iterators**; you pass the construction of this iterator to build your Iterable)*. This iterable produces the elements as they are **pulled** by the underlying stream, without buffering them. One last word: be careful — a stream can produce an **unbounded** number of elements, so you probably need to protect your application against that.

```java
Stream<String> stream = lines();           // can be HUGE

Iterable<String> asIterable = stream::iterator;  // a supplier of iterators

for (String s : asIterable) {             // pulled on demand, no buffering
    if (s.startsWith("END")) break;       // protect yourself from infinity!
}
```

---

### Question 234 — What is a VarHandle?
· [video](https://youtu.be/f7SDXVQFVYs) · local file `165`

**Short answer:** An object to access fields.

**Less short answer:** You may be thinking that VarHandle is the equivalent of MethodHandles, but to access fields — it's actually not the case: you can access fields with MethodHandles too. The VarHandle was added in **JDK 9** — whereas the MethodHandle was added in JDK 7; so during these six years, MethodHandles were used to access fields. What the VarHandle gives you is access to fields, but **with the concurrent semantics** — like volatile or atomic. Accessing a field using this mode mimics the fact that the field was defined as a `volatile` field, for instance. One last word: be careful — accessing a field value using a VarHandle **ignores the possible `volatile` declaration** that this field may have.

```java
class Counter {
    private int value;                       // a plain field
    private static final VarHandle VALUE;
    static {
        try { VALUE = MethodHandles.lookup()
                    .findVarHandle(Counter.class, "value", int.class); }
        catch (Exception e) { throw new RuntimeException(e); }
    }
    void increment() {
        VALUE.getAndAdd(this, 1);   // atomic, without volatile in the declaration
    }
}
```

---

### Question 235 — What is a MessageFormat?
· [video](https://youtu.be/4B45mnCWHD4) · local file `164`

**Short answer:** A class.

**Less short answer:** MessageFormat works with a **pattern** — and this pattern can be used to **format** messages from a given object, or to **parse** strings of characters to extract the information they contain. There is one major trap in the MessageFormat class: you may be tempted to **reuse** your MessageFormats and cache them in `public static` variables... well, **don't do that** — MessageFormat is a **mutable** object, and it is **not thread-safe**: sharing such an instance between threads will produce race conditions. One last word: date formatting has been superseded by the Date and Time API in JDK 8 — so you may want to check the `DateTimeFormatter` class. *(Another video.)*

```java
String msg = MessageFormat.format(
        "Hi {0}, you have {1,choice,0#no messages|1#one message|1<{1,number,integer} messages}",
        "Ana", 3);

// do NOT cache: mutable and not thread-safe
// public static final MessageFormat FMT = ...  <- race condition!
```

---

### Question 236 — What is a collector built on?
· [video](https://youtu.be/j2ZKW7JTxeA) · local file `163`

**Short answer:** Three elements plus one.

**Less short answer:** A collector collects the elements of the stream in a **mutable container**. For that, it needs two elements: one used to **build** the mutable container — in the form of a `Supplier` — and another one to **add** an element to this container — a `BiConsumer`: that's the *accumulator*. Then, as you can use the collector **in parallel** — meaning that several instances of the container may be built in different threads — at some point you will need to combine them: that's the **combiner**, a `BinaryOperator`. And the last one is there to make your container, for instance, non-modifiable — that's the **finisher**, a function. One last word: a collector also has **characteristics**. *(Another video.)*

```java
Collector<Integer, ?, List<Integer>> toList = Collector.of(
        ArrayList::new,               // 1. supplier: creates the container
        List::add,                    // 2. accumulator: adds an element
        (l1, l2) -> { l1.addAll(l2);  // 3. combiner: merges (parallel)
                      return l1; },
        Collections::unmodifiableList // 4. finisher: transforms the result
);
```

---

### Question 237 — What does escape analysis mean?
· [video](https://youtu.be/7QXV77nGiXo) · local file `162`

**Short answer:** It means performance.

**Less short answer:** There are cases in your code where you declare variables that are objects, and you already know that they will be **destroyed very soon**. Objects are a little costly to create — and then they need to be garbage-collected. And sometimes they are just not referenced by your code: an iterator in the for-each pattern, for example; or they do not **escape** the method you're in. In these cases, the JVM may decide **not to create these objects**: it uses their **content** where it's needed, but does not create them. One last word: on a **stack**, this is called *scalarization*; and on the heap, *flattening* — something Valhalla already does very well.

```java
int totalLength(List<String> l) {
    // iterator wrappers, counters... the JIT removes the allocations
    int total = 0;
    for (String s : l) {          // the iterators do not escape
        var box = s.length();     // the boxing does not escape
        total += box;             // -> scalarized code, no objects
    }
    return total;
}
```

---

### Question 238 — What is getOrDefault()?
· [video](https://youtu.be/QBoBybiqkYQ) · local file `161`

**Short answer:** A method from the Map interface.

**Less short answer:** getOrDefault takes two parameters: a key, and a **default value**. You may be thinking that, if the key is not in the map, then it returns the default value — well, the detail is that getOrDefault does **not** check if the key is in the map with `containsKey`: it checks if `get(key)` returns **null**. In a map that does not accept null values — like a ConcurrentHashMap — then yes: an absent key means getOrDefault returns the default value you provide. But if your map **accepts null values** (that's the case for HashMap), then it also returns the default value in the case of a **null value** bound to that key. One last word: getOrDefault is aligned with what `putIfAbsent` does — we already talked about that.

```java
Map<String, Integer> stock = new HashMap<>();
stock.getOrDefault("pen", 0);        // 0 (absent)

stock.put("pencil", null);          // HashMap accepts null!
stock.getOrDefault("pencil", 0);    // 0 (!) — get returned null

Map<String, Integer> conc = new ConcurrentHashMap<>();
conc.getOrDefault("pencil", 0);     // 0 (there is never a real null)
```

---

### Question 239 — What is this?
· [video](https://youtu.be/o1U-8Sj3KY0) · local file `160`

**Short answer:** Come on. *(the presenter's ironic short answer)*

**Less short answer:** `this` is a keyword of the language — a **reference on the object you are in**. Because it is a reference on your instance, you cannot use it in **static** members of your class, for obvious reasons. One fun fact: you can write your methods in such a way, with **`this` as a first parameter** — this is actually the trick the compiler is using to give you a this reference in your method... and that's a very good way of writing unreadable code, and of tricking people. One last word: `this(...)` is also a way to call another constructor from the constructor — the old restrictions on the calling of constructors within constructors have been **lifted** (for Valhalla to have immutable objects). *(Another video.)*

```java
class Point {
    private final int x, y;
    Point(int x, int y) { this.x = x; this.y = y; }  // this.x = the field
    Point() { this(0, 0); }   // this(...) calls another constructor

    // the unreadable-code trick (this "as a parameter" — the compiler's)
    // static int x(Point this, int delta) { return this.x + delta; }
}
```

---

### Question 240 — How expensive is a thread?
· [video](https://youtu.be/nRazntf53FQ) · local file `159`

**Short answer:** A thread is quite expensive.

**Less short answer:** It actually depends on your operating system, because an instance of the `Thread` class is a wrapper on a thread **provided by your OS**. A kernel thread holds **several megabytes** of memory, and may take **several milliseconds** to create. So yes — it is an expensive resource; the reason why applications create them when they start, try to keep them alive, and **reuse them** (pools). This led to the callback-based asynchronous programming model, that can work with very few threads. One last word: all this is valid for **kernel threads**; **virtual threads**, on the other hand, are like regular objects — you can create them on demand, and have as many as you need in your application. *(Another video.)*

```java
// kernel thread: ~1 MB of stack, creation in ms -> a POOL
ExecutorService pool = Executors.newFixedThreadPool(20);

// virtual thread: KBs, creation in µs -> one per task, by the thousands
Thread.ofVirtual().name("worker-", 0).start(task);
```

---

### Question 241 — What is pattern matching?
· [video](https://youtu.be/OEkeVEVmzqg) · local file `158`

**Short answer:** An old feature from the 70s that is making its way to the Java language.

**Less short answer:** Pattern matching was defined by the functional programming people about 50 years ago. So far there are two patterns in Java: the **type pattern** and the **record pattern**. When you write `o instanceof String s`, what you're really asking is: "could this variable `o` be declared as `String s`?" — and if it is the case, then `s`, called the **pattern variable**, is created, and you can use it where it makes sense. With the **record pattern**, you can deconstruct records and create pattern variables for their components. One last word: there are more patterns to come — primitive type patterns and named patterns. *(Another video.)*

```java
// type pattern: the question + the variable creation in one step
if (o instanceof String s && !s.isEmpty()) {
    System.out.println(s.length());   // no cast!
}

// record pattern: deconstruction
if (o instanceof Point(int x, int y)) {
    System.out.println(x + "," + y);  // the components, directly
}
```

---

### Question 242 — Why can't you map filter reduce a collection?
· [video](https://youtu.be/QrAjiFtuNPQ) · local file `157`

**Short answer:** Because you **don't want** to map filter reduce a collection.

**Less short answer:** There are cases where the reduce operation does not need to process **all** the elements you have to produce a result. If you're looking for the **first** element, having to map and then filter all the rest of your elements would be a terrible overhead. So mapping everything, then filtering everything before reducing would be a terrible way to implement this algorithm. What you want to do is to take **every element through your entire processing pipeline, one step at a time** — and this is what Streams are doing. One last word: Eclipse Collections implemented the concept of **lazy collections** — so if this is what you need, please use it.

```java
// wrong (eager on a collection): maps EVERYTHING, filters EVERYTHING, then seeks
huge.map(...).filter(...).findFirst();

// Streams: each element flows through the whole pipeline, one at a time
huge.stream().map(...).filter(...).findFirst();  // short-circuit!
```

---

### Question 243 — Can you explain the parameter types of a collector?
· [video](https://youtu.be/wLtPSEl6mJY) · local file `156`

**Short answer:** There are three of them.

**Less short answer:** But you only need to understand the **first** one and the **last** one. The first one is the type of elements this collector can **consume**; the third type is the type of the element that this collector can **produce**. You can leave the second type anonymous — it represents the **internal container** your collector implementation may use. One last word: actually, collectors do not depend on the Stream API — you can use their supplier, accumulator and finisher independently. *(Another video.)*

```java
Collector<String, ?, Long> counting = Collectors.counting();

Collector<String, StringBuilder, String> joining =
        Collector.of(StringBuilder::new,      // A: the container (the middle)
                     StringBuilder::append,   // T: consumes Strings
                     (a, b) -> a.append(b),
                     StringBuilder::toString);// R: produces a String
// you write Collector<String, ?, String> — the middle stays anonymous
```

---

### Question 244 — What does volatile mean?
· [video](https://youtu.be/yUatdetXNtU) · local file `155`

**Short answer:** It has to do with the **visibility** of fields in a concurrent environment.

**Less short answer:** `volatile` is a keyword you can put on a field. It means that the read and write operations on this field are **volatile reads and volatile writes** — and this has to do with the existence of **happens-before** links between your read operation and your write operation. The specification says that a happens-before link exists between a synchronized/volatile **write** and the corresponding synchronized/volatile **read**. One last word: a **volatile reference** on a **mutable container** does not protect the **content** of this container against race conditions — so be careful with that.

```java
private volatile boolean running = true;   // visible write/read

// writer: running = false  ->  reader: while (running) ... SEES the change

private volatile List<Integer> list;      // the reference is visible... the content is NOT!
list.add(1);   // race condition: volatile does not protect the inside
```

---

### Question 245 — How can you compute the median value of an array?
· [video](https://youtu.be/-c0n_gFQ9oI) · local file `154`

**Short answer:** By using the right algorithm.

**Less short answer:** The lazy way of doing it would be to sort the array, then read the middle index — but it's inefficient. The first step of the quicksort algorithm is to choose a pivot and move all the smaller elements to the left — meaning that if, after doing this, you have split your array in two equal parts, it's done: you don't need to go any further. The problem is that you may not get the middle index in one guess — so you need to repeat, but **only on the largest part** that you have. On average, you will not have to sort everything, so you will go faster. One last word: you can use better strategies if what you need is just a value that is **close enough** — that can be useful, especially if you have a lot of data.

```java
// QuickSelect: the quicksort partition repeated only on the side that matters
int quickSelect(int[] a, int k) {
    int lo = 0, hi = a.length - 1;
    while (true) {
        int p = partition(a, lo, hi);      // the pivot in its right place
        if (p == k) return a[p];
        else if (p < k) lo = p + 1;        // only the right side!
        else hi = p - 1;                   // only the left side!
    }
}
int median = quickSelect(vals, vals.length / 2);  // ~O(n) on average
```

---

### Question 246 — How is the toArray() method working?
· [video](https://youtu.be/Z6M7X6kR_Ew) · local file `153`

**Short answer:** There are several toArray methods — on Collection, on streams, and on specialized streams.

**Less short answer:** On **specialized streams** (IntStream...), toArray builds an array with your primitives — plain and simple. On a **`Stream<T>`**, toArray builds an array of objects, but you can also pass a **supplier** of the given type to produce an array of values of that type. On **Collection**, toArray does the same — but it can also take an **array** of the given type. One last word: for collections, passing an **empty array** may actually be a better idea than passing an array of the right size — it has to do with the fact that an array must be filled with zeros; but as usual, when it comes to performance: **measure, don't guess**.

```java
int[] p = IntStream.of(1, 2, 3).toArray();       // primitives: direct

String[] a1 = stream.toArray(String[]::new);     // a typed Stream<T>
Object[] a2 = stream.toArray();                  // Object[]

String[] a3 = list.toArray(new String[0]);       // collection: an empty array
String[] a4 = list.toArray(new String[list.size()]); // the exact size
```

---

### Question 247 — What is an unnamed variable?
· [video](https://youtu.be/4URTbHw2m2Y) · local file `152`

**Short answer:** A variable that has no name.

**Less short answer:** What could be the use of such a variable? Well, with **record patterns**, you create pattern variables when you deconstruct a record — and creating such a variable **calls the accessor** of your record, which may be costly. So, if you don't need them, you can declare them as **unnamed** (with `_`), and they will not be created. You can also use these unnamed variables in **lambda expressions**, or when you **catch exceptions**. One last word: for record deconstruction, using unnamed variables may also **hide a change in your model** — as does the use of the `var` keyword, by the way. So be aware of that before using this feature.

```java
// an unnamed type pattern: only the type matters, nothing is created
if (obj instanceof Circle _) { processAsCircle(); }

// a record pattern with an ignored component (the accessor is not even called):
if (obj instanceof Circle(double radius, Point _)) {
    System.out.println("radius=" + radius);   // 'center' never becomes a variable
}

for (var _ : list) counter++;                                     // ignores the element
try { parse(v); } catch (NumberFormatException _) { return 0; }   // unnamed
```

---

### Question 248 — What does the reversed() method do?
· [video](https://youtu.be/XMsNZq-BmG8) · local file `151`

**Short answer:** It reverses a list.

**Less short answer:** It is actually one of the methods brought to you by the **Sequenced Collections** API (Java 21): this method returns you a **view** on your sequenced collection. Since it is a view, if the underlying sequenced collection is modified, you will see this modification — and note that for an ArrayList, this reversed view is also **modifiable**: modifying it modifies the underlying list. If what you need is a **copy** and not a view, you can pass it to `List.copyOf()`, that will happily copy it for you. One last word: be careful though, because List.copyOf does **not accept null values**. *(Why would you put null values in your collections, anyway?)*

```java
List<Integer> l = new ArrayList<>(List.of(1, 2, 3));
List<Integer> view = l.reversed();   // a view: [3, 2, 1]
view.set(0, 99);                     // modifies l: [1, 2, 99]
List.copyOf(l.reversed());           // an independent copy (no nulls!)
```

---

### Question 249 — Why does Java not allow variable capturing?
· [video](https://youtu.be/2yiW5ix-uIA) · local file `150`

**Short answer:** Because it would have changed the concurrent programming model.

**Less short answer:** A lambda can only capture a **final** variable — that is, a **value**. It cannot capture a **modifiable local variable**. If it had been allowed — since this lambda can escape the method it is declared in — **different threads** would have been exposed to these local variables, and then to **race conditions**; something that does not exist in Java (for locals, in the Java model). So the decision was: you can only capture **values**, never modifiable variables. One last word: there are JVM-based languages that allow the capturing of local variables in lambdas — but when it comes to race conditions, you're on your own.

```java
int counter = 0;                    // effectively final: OK
Runnable r = () -> use(counter);    // captures the VALUE (a copy)

// counter++; above would make the capture illegal — by design:
// a Runnable in another thread mutating a local = a free race condition

// if you need shared mutable state:
AtomicInteger n = new AtomicInteger();
Runnable r2 = n::incrementAndGet;   // explicit and safe
```

---

### Question 250 — What is a blocking call?
· [video](https://youtu.be/62RSK2vObXQ) · local file `149`

**Short answer:** Something you want to avoid.

**Less short answer:** How much time does it take to process a network request-response cycle? Creating a request and analyzing a response takes in the order of **microseconds**; waiting for a network response takes in the order of **milliseconds** — a thousand times slower. "Blocking" means that during this time, the thread that is running your request is **waiting, doing nothing**: it could handle about a thousand request constructions or response analyses, but it does not. One last word: the **existing** solution to overcome this problem is to use **reactive programming**, avoiding the writing of blocking code; the **future** is to write blocking code — but in virtual threads. *(Another video.)*

```java
// blocking: an OS thread stopped for milliseconds...
Response r = client.send(request);

// with virtual threads, blocking is cheap — the carrier is released:
Thread.ofVirtual().start(() -> client.send(request));
```

---

### Question 251 — What is a Predicate?
· [video](https://youtu.be/VeK2UECqCnk) · local file `148`

**Short answer:** A functional interface.

**Less short answer:** This interface defines a `test` method, that takes an object and returns a **boolean**. It is used in several places in the Stream API and the Collections framework — for instance, in the `filter` method, that can filter a stream, or in the `Collection.removeIf` method, that will remove elements from a collection. This interface defines a default method to **combine** a predicate with `and` / `or`; you can also **negate** a predicate with the `negate` method, or with the `not` factory method, that takes another predicate. One last word: it is one of the **four fundamental functional interfaces** that you need to know: Consumer, Supplier, Function and Predicate.

```java
Predicate<String> empty = String::isEmpty;
Predicate<String> short = s -> s.length() < 5;

empty.or(short).test("abc");            // true
empty.and(short).negate().test("xy");   // false
Predicate.not(empty).test("java");      // true (a static factory)
```

---

### Question 252 — What is a WeakHashMap?
· [video](https://youtu.be/bZoUTUUzGu4) · local file `147`

**Short answer:** A map.

**Less short answer:** The keys of a WeakHashMap are **weak references**, that can be garbage-collected if they are not used by anything else in your application — meaning that the size of this map **may decrease over time**, without your application ever calling a remove. This is very useful to create **caches**, for instance: put key-value pairs in a WeakHashMap — and if at some point your application is running low on memory, the garbage collector can just remove the ones that are not used. One last word (an important one): the **values** in a WeakHashMap are stored using **strong references** — so if your value has a strong reference on its key, your WeakHashMap will just behave as a regular HashMap. Don't do that.

```java
Map<Config, CachedData> cache = new WeakHashMap<>();
cache.put(config, data);     // config is weak: without external refs, the entry goes

// TRAP: a value referencing the key = an immortal entry
// class CachedData { final Config key; }  <- DON'T!
```

---

### Question 253 — How can you use a collector outside of the Stream API?
· [video](https://youtu.be/9v3dddH7xyI) · local file `146`

**Short answer:** There is a pattern for that.

**Less short answer:** A collector is built on four elements: a **supplier**, an **accumulator**, a **combiner** and a **finisher**. You can apply these elements on a list, for instance: first, you need to create the mutable container with the supplier; second, you can accumulate the elements of this list in it, using the accumulator; and once this is done, you need to call the **finisher** on the result, if you have one — and you're done. One last word: collectors **do not depend on the Stream API** — you can use them independently, and it's a very powerful API that can model complex processing. The same is true for **gatherers**. *(Another video.)*

```java
Collector<String, ?, Long> counting = Collectors.counting();

// driving the collector manually (without any stream):
var box = counting.supplier().get();              // creates the container
counting.accumulator().accept(box, "a");          // accumulates...
counting.accumulator().accept(box, "b");
Long total = counting.finisher().apply(box);      // finishes: 2
```

---

### Question 254 — What is a StampedLock?
· [video](https://youtu.be/X4jBaHuynxw) · local file `145`

**Short answer:** A lock with an **optimistic read** mode, among other things.

**Less short answer:** A StampedLock has three modes: exclusive **write** and non-exclusive **read** — the two modes of read-write locks — plus a third one: the **optimistic read**. The *stamp* you get when you call one of the lock methods must be provided to unlock this StampedLock. Optimistic reads are interesting, but also fragile: you need to acquire the stamp, **check that it is non-zero**, read what you need, and then **validate** what you just read, to make sure no write was done from another thread. One last word: locks obtained from a StampedLock can also be **converted** — from a read lock to a write lock, for instance. *(Another video.)*

```java
StampedLock lock = new StampedLock();

double read() {
    long stamp = lock.tryOptimisticRead();   // without blocking!
    double x = data;
    if (!lock.validate(stamp)) {             // a write happened meanwhile?
        stamp = lock.readLock();             // fallback: really locks
        try { x = data; } finally { lock.unlockRead(stamp); }
    }
    return x;
}
```

---

### Question 255 — How can you create multi-dimensional arrays?
· [video](https://youtu.be/VHw0qK856Tc) · local file `144`

**Short answer:** There is a pattern for that.

**Less short answer:** There is a syntax to create arrays with multiple dimensions — but under the hood, what you get is an **array of references to other arrays**, that can be stored anywhere in the memory of your application. It is actually working in the following way: first, you create an array of references; then, for each of them, you create another array of the right size — and by the way, this size **can vary** from one array to the other (jagged arrays). One last word: there is no way you can automatically map a **large contiguous region of memory** to a multi-dimensional array, as you can do in C — if this is what you need, you will have to write some code (a linearized index).

```java
int[][] grid = new int[3][4];        // an array of 3 references...
grid[0] = new int[10];               // ...each row can have its own size

int[][] jagged = { {1}, {1, 2}, {1, 2, 3} };  // jagged: legal!

// truly contiguous? Do it yourself:
var flat = new int[3 * 4];
flat[row * 4 + column] = 1;
```

---

### Question 256 — How can you stream the content of a directory? *(with DirectoryStream)*
· [video](https://youtu.be/SC39k-wdTqs) · local file `143`

**Short answer:** There is a method for that.

**Less short answer:** The method is `Files.newDirectoryStream(dir)`, declared on the Files factory class. It returns a `DirectoryStream` object — and this object is **not** a stream from the Stream API: it is an **Iterable**, from which you can get an iterator. This iterator gives you the guarantee that a call to `next()` that follows a call to `hasNext()` that returned `true` **will not fail** — which, when you think about the **concurrent** nature of file systems, is not obvious. Note that the directory you stream is **not frozen** while you iterate on its content; and what you get is the content of this directory — **sub-directories are not explored**. One last word: you can also pass a **filter** (a glob) to the construction of this object, to list only the files of a certain type, for instance.

```java
try (DirectoryStream<Path> ds = Files.newDirectoryStream(
        Path.of("."), "*.{java,txt}")) {          // a glob filter
    for (Path p : ds) {                           // it's an Iterable
        System.out.println(p);                    // one level only
    }
}
```

---

### Question 257 — What is a stack trace?
· [video](https://youtu.be/o2i7GaF8UZE) · local file `142`

**Short answer:** An array.

**Less short answer:** All the exception classes extend `Throwable` — and when you create an instance of such an object, an **array** is created, that contains all the methods that called the line of code on which this exception was created. Note that this array is filled **when the exception is created** — the reason why, if you create an exception to report an error and throw it, you'd better call `throw new ...`, so that the stack trace you get takes you to the right place in your application. One last word: sometimes the stack trace does **not contain all the stack frames** — in extreme cases, you may even have no stack frame at all; that may happen if the VM has no stack trace information.

```java
try { process(); }
catch (IOException e) {
    // the trace points HERE (where the exception was created):
    throw new IntegrationException("integration failure", e);
}
e.getStackTrace();   // StackTraceElement[] — frames, newest first
```

---

### Question 258 — How can you read a ZIP file with the FileSystem API?
· [video](https://youtu.be/NnaMY-leO0M) · local file `141`

**Short answer:** There is a pattern for that.

**Less short answer:** Java NIO gives you the **FileSystem** abstract class to model file systems — and one of them can access data compressed in a ZIP/JAR, used in Java applications. You can create such a file system on a zip file, and then use it to create paths — you can do the same to read them: it's exactly as if you were writing or reading from a regular disk file system. One last word: as you know, `Path` objects are actually bound to a specific file system — so when using this pattern, you need to use path objects (or Files) **created from the zip file system**, not paths from the default file system.

```java
try (FileSystem zip = FileSystems.newFileSystem(Path.of("data.zip"))) {
    Path inside = zip.getPath("reports/q3.csv");
    Files.readString(inside);                       // reads as if it were a disk!
    try (Stream<Path> s = Files.walk(zip.getPath("/"))) {
        s.forEach(p -> System.out.println(p));      // browses the whole zip
    }
}
```

---

### Question 259 — Is the String class an immutable class?
· [video](https://youtu.be/AcpU5cL2s0s) · local file `140`

**Short answer:** That's actually a trick question — and the correct answer is **no**.

**Less short answer:** You probably think that the String class is non-modifiable, which is **partially** true. What is true: once you fix the value of a string of characters, you cannot change it; all the methods of the String class — like `toUpperCase` or `substring` — actually return **another** String object, wrapping a different array. **But** there is a **mutable field** in the String class: the one that holds the **hash code** of that String object — computed on the first call, and **cached** in this field. One last word: this mutable field is what makes it impossible for String to become a **value object** in Valhalla, unless the class is changed. *(Another video.)*

```java
public final class String {
    private final byte[] value;  // immutable (final, defensively copied)
    private int hash;            // MUTABLE: computed and cached later!
    // -> that's why String is not 100% immutable, nor value-based
}
```

---

### Question 260 — What is a virtual thread?
· [video](https://youtu.be/y8pjM0lbtpY) · local file `139`

**Short answer:** A class. That is not public. *(a joke — it is much more than that)*

**Less short answer:** Virtual threads are an amazing addition of **JDK 21**. A virtual thread is a **wrapper on a task** that you want to submit to a regular thread — this task can be modeled by a `Runnable` or a `Callable`. Formally, virtual threads can be **mounted and unmounted** on regular threads, so that they can execute the task that they wrap; and when your task **blocks** — like when it is waiting for data from the network — the NIO API **unmounts** the virtual thread, so that the regular thread is not blocked. Being able to unmount a task from a thread **was not possible** in Java before virtual threads — and that's a game changer. One last word: virtual threads are made to wrap **blocking tasks**. Forget about using them for anything else.

```java
// 1,000,000 blocking tasks? No problem:
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    for (var url : urls) {
        executor.submit(() -> download(url));  // blocks? unmounts. Cheap.
    }
}
```

---

### Question 261 — How can you represent time?
· [video](https://youtu.be/ThiuM4v0YyM) · local file `138`

**Short answer:** There is a class for that. *(the problem is that there are two of them)*

**Less short answer:** The legacy `java.sql.Time` (that extends `java.util.Date`) and **`LocalTime`** — which is the one you should favor. A LocalTime models an hour, a minute and a second, with **nanosecond** precision, and **no time zone information** — and it's an immutable class. You have factory methods to create a LocalTime, accessors to read the time it carries, methods to create another LocalTime from the one you have, and even `with*` methods. Neat. One last word: LocalTime is now a **value-based class** — the Valhalla notion; among other restrictions, you should **not synchronize on it**. *(Another video.)*

```java
LocalTime now = LocalTime.of(14, 30, 15);
now.getHour();        // 14
now.plusMinutes(90);  // 16:00:15 (a new object)
now.withHour(9);      // 09:30:15
now.isBefore(LocalTime.NOON);
// legacy: java.sql.Time — avoid
```

---

### Question 262 — What is mapMulti()?
· [video](https://youtu.be/e13IF5juf4o) · local file `137`

**Short answer:** A method from the Stream interface.

**Less short answer:** mapMulti allows you to **flatMap without having to create intermediate Stream objects** — which can be another overhead in your data processing pipeline. You need to call mapMulti with a lambda that takes **two arguments**: the element of the stream you need to process, and the "downstream" object. When you want to pass an object to the downstream, you need to call `downstream.accept()` — you can call accept any number of times, or not call it at all. One last word: you need to **specify the type** of the object you are pushing to the downstream when you call mapMulti — if you don't, this method will return a `Stream<Object>`. Usually, it's not what you want.

```java
record Token(String text) { }

List<Token> tokens = Stream.of("a b", "c")
        .<Token>mapMulti((phrase, downstream) -> {   // the explicit type!
            for (String p : phrase.split(" "))
                downstream.accept(new Token(p));     // 0..N per element
        })
        .toList();
```

---

### Question 263 — What is a ConcurrentMap?
· [video](https://youtu.be/KJ7I3RKQkLw) · local file `136`

**Short answer:** An interface.

**Less short answer:** The ConcurrentMap interface has a double role: defining **thread safety** and the **atomicity** of some operations on maps. It has two implementations in the JDK: `ConcurrentHashMap` and `ConcurrentSkipListMap`. Each method defined in this interface can be specified to be **atomic** — that's the case for `putIfAbsent`, for instance, which cannot be interrupted between the checking of the absence of the key and the binding of this key to the value you provide. Atomicity may also apply to some methods of the collections you can get from this map — `values()`, `keySet()` or `entrySet()`. One last word: be careful, because some ConcurrentMaps do **not accept null keys**. *(Why would you put null keys in, anyway?)*

```java
ConcurrentMap<String, Integer> map = new ConcurrentHashMap<>();

// putIfAbsent is ATOMIC: check + put are indivisible
map.putIfAbsent("count", 0);
map.compute("count", (k, v) -> v + 1);   // also atomic
// map.put(null, 1);  // NullPointerException (no nulls!)
```

---

### Question 265 — How can you access the off-heap memory?
· [video](https://youtu.be/9-OgkDZ7erc) · local file `135`

**Short answer:** There is a whole API for that. *(two, in fact)*

**Less short answer:** The first API is really old — from Java 4, in 2002: the `ByteBuffer` class. The **Panama** Project brought new patterns to access the off-heap memory, based on the use of an **Arena** object: from an arena, you can get **memory segments**, and then read and write data from them — directly with an offset, or with an index. You can also **map them to files**, just like you could with byte buffers. One last word: byte buffers are indexed with **integers**, and memory segments with **longs**; and you can also define **layouts** on them, to access their content in a structured way. *(Another video — "see you in the next one".)*

```java
try (Arena arena = Arena.ofConfined()) {
    MemorySegment seg = arena.allocate(1_000);
    seg.set(ValueLayout.JAVA_DOUBLE, 0, Math.PI);  // by offset
    seg.getAtIndex(ValueLayout.JAVA_LONG, 3);       // by index (the 3rd long)
}
// ByteBuffer (legacy, Java 4): int indexes, no arena
ByteBuffer b = ByteBuffer.allocateDirect(1_000);
```

---

### Question 266 — What is a deserialization filter?
· [video](https://youtu.be/89Wg9nEMSFk) · local file `134`

**Short answer:** It is a filter that you can declare to prevent the deserialization of specific classes.

**Less short answer:** Deserializing an unknown stream of data is an **inherently dangerous** activity. A deserialization filter can prevent instances of certain classes, and graphs of objects that are **too deep or too complex**, from being deserialized. You need to implement the `ObjectInputFilter.checkInput()` method, with the information you need on the data stream; you can then return a status that can be **ALLOWED**, **REJECTED**, or even **UNDECIDED**. One last word: serialization is the source of **many** security issues — using records can prevent some of them, so use them wherever you can.

```java
ObjectInputFilter filter = info ->
        info.serialClass() == Order.class ? ObjectInputFilter.Status.ALLOWED
        : info.serialClass() != null      ? ObjectInputFilter.Status.REJECTED
        : info.depth() > 5                ? ObjectInputFilter.Status.REJECTED
                                          : ObjectInputFilter.Status.UNDECIDED;

var in = new ObjectInputStream(inputStream);
in.setObjectInputFilter(filter);   // JEP 290: protection against gadgets
```

---

### Question 267 — How can you get the class of a primitive type?
· [video](https://youtu.be/BKrw4ejbKLw) · local file `133`

**Short answer:** *(straight to the method)*

**Less short answer:** Primitive types are associated with classes, that you need when you want to get a method from a class using the Reflection API. You can use the **`int.class`** syntax, that gives you a Class instance — but JDK 22 added a new method on the `Class` class: **`Class.forPrimitiveName("int")`**, that takes the primitive type as a string of characters, and that returns you the same object. One last word: so far, primitive types are different from object types; this may change in the future, once the value types from Valhalla make it to the JDK — you will even be able to define your own primitive types, like a 16-bits float. *(Another video.)*

```java
Class<Integer> c1 = int.class;                  // the literal
Class<?> c2 = Integer.TYPE;                     // the wrapper constant
Class<?> c3 = Class.forPrimitiveName("int");    // JDK 22+
c1 == c2 && c2 == c3;                           // true: the same object

// a typical use: reflection of methods with primitive parameters
String.class.getMethod("charAt", int.class);
```

---

### Question 268 — Why do you need Optional?
· [video](https://youtu.be/_AdrYSsFtS8) · local file `132`

**Short answer:** To tell that a method **cannot return anything**.

**Less short answer:** There are cases where you need that — and returning `void` or `null` does not tell that you cannot return anything: it's a way of returning nothing, which is different. Where does it come from? Because a stream can be computed in parallel, the reduction operation has to be **associative** — otherwise, the result of your operation would depend on the order in which it has been computed. But then, a stream can be empty: what could be the result of the reduction of an empty stream? It would have to be the **identity element** of your reduction operation... but what can you do for `min` or `max`, or any reduction that does **not have** an identity element? The choice was to return an `Optional` — that can be empty. One last word: using Optionals in **any other context** is usually a bad idea. Don't do that.

```java
Optional<Integer> max = Stream.<Integer>empty().max(Integer::compare);
max.isEmpty();   // true — a reduction without identity returns an Optional

// Optional was born in the Stream API: use it there (and for possibly-absent returns)
// Optional as a field/parameter: a terrible idea
```

---

### Question 269 — What is a Function?
· [video](https://youtu.be/EDSkidTxn4g) · local file `131`

**Short answer:** A functional interface.

**Less short answer:** A Function maps an object of a given type to an object of another type — it is used by the `map` method of the Stream interface. The Function interface also defines methods to **chain and compose** functions (`andThen`, `compose`), and a factory method that returns the **identity function** (`Function.identity()`). There are also specialized functions to avoid the **boxing** of primitive types when you want to process numbers (`IntFunction`, `ToIntFunction`...), and the `UnaryOperator` — a function that **does not change the type**. One last word: Function is part of the four fundamental functional interfaces of the JDK — Consumer, Supplier, Predicate and Function; and yes, it's okay to add Runnable to the lot, even if Runnable was there long before Java 8.

```java
Function<String, Integer> length = String::length;
Function<Integer, Integer> twice = n -> n * 2;

length.andThen(twice).apply("java");  // 8
twice.compose(length).apply("java");  // 8 (the reverse order)
Function.identity();                  // x -> x
UnaryOperator<Integer> triple = n -> 3 * n; // the same type in and out
```

---

### Question 270 — What is a Spliterator?
· [video](https://youtu.be/ijZ8odZ5IPM) · local file `130`

**Short answer:** An interface.

**Less short answer:** A spliterator is the object **on which a stream is built**. It has a number of methods to **traverse** the elements of the source, to **partition** them, and to get information on the source. Writing your own spliterator is **not an easy task** — you need to be careful about the behavior of each method. Fortunately, there are many examples in the JDK itself to help you: you can check, for instance, the `ArrayListSpliterator`, or the `HashMap.KeySpliterator` — which is the spliterator of the HashSet. One last word: a spliterator can produce an **unbounded** number of elements, and it can produce them **lazily** — you can check the `Random.ints()` spliterator, or the `MatcherIterator`, used in the `Pattern.splitAsStream()` method.

```java
// iterate -> an unbounded, lazy spliterator
Spliterator<Integer> sp = Stream.iterate(1, n -> n * 2)
        .spliterator();   // good for building your own pipeline
Stream<Integer> s = StreamSupport.stream(
        List.of(1, 2, 3).spliterator(), /* parallel */ false);
```

---

### Question 271 — What is a value-based class?
· [video](https://youtu.be/Z0XDfpbg7Mw) · local file `129`

**Short answer:** A class that wraps a value.

**Less short answer:** There are several examples of value-based classes in the JDK: `Integer`, `Double`, and all the classes that wrap primitive types, are all value-based classes. All the classes of the Date and Time API are also value-based classes, as well as others — like `Optional`. A value-based class can only carry some **non-modifiable information** — thus its name. And since its instances should behave as **values**, you should not use anything related to their **identity** — that is, their address in memory: things like `==`, `System.identityHashCode`, or synchronization, should not be used — and **will fail at some point**. One last word: String is **not** a value-based class — first, it is not fully unmodifiable (it carries this `hash` field that is not final); and second, it may wrap a **very large array of bytes**, that does not fit well with this notion of value.

```java
Optional<Integer> o = Optional.of(1);  // value-based
LocalDate d = LocalDate.now();         // value-based

if (o == another) { }          // forbidden: identity
synchronized (d) { }           // forbidden: synchronizing on a value
System.identityHashCode(o);    // avoid
```

---

### Question 272 — What is a non-null stream?
· [video](https://youtu.be/usiJ9Qdh4Z0) · local file `128`

**Short answer:** The stream with the *non-null* characteristic.

**Less short answer:** **NONNULL** is a characteristic that a stream may have, if you have the guarantee that this stream is built on a source that **cannot produce any null element**. You can check this characteristic through the spliterator your stream is built on, by calling its `hasCharacteristics()` method, for instance. You can rely on this characteristic when you need to collect your elements in a container that **does not accept null values** — which is the case for most concurrent collections, or for the ConcurrentHashMap. One last word: avoiding null values in your data processing pipelines is something you should aim for — it will make your life so much easier.

```java
Spliterator<String> sp = List.of("a", "b").spliterator();
sp.hasCharacteristics(Spliterator.NONNULL);   // true for List.of

// useful: collecting into a container that does not accept nulls
ConcurrentHashMap.newKeySet().addAll(List.of("a")); // a null would blow up
```

---

### Question 273 — What is a fail-safe iterator?
· [video](https://youtu.be/KvEsuJ5Tp0g) · local file `127`

**Short answer:** Something that **does not exist** in Java. *(the presenter's trick question)*

**Less short answer:** "Fail-safe" would describe a specific behavior when you iterate on a collection that is modified while you iterate over it. In Java, modifying the collection **outside of the iterator itself** results in a **`ConcurrentModificationException`** being thrown — this is called the **fail-fast** behavior. Concurrent collections usually have a different behavior, called **weakly consistent**. And the `CopyOnWriteArrayList` has yet another behavior, called **snapshot**: each modification creates a copy, so you can keep iterating on your own version of this copy, without any problem. One last word: be careful — the ConcurrentModificationException you get has, in fact, **nothing to do with concurrency**: everything usually happens in the **same thread**.

```java
List<String> l = new ArrayList<>(List.of("a", "b"));
for (String s : l) {
    if (s.equals("a")) l.remove(s);   // CME — same thread, no concurrency!
}

List<String> cow = new CopyOnWriteArrayList<>(List.of("a", "b"));
for (String s : cow) {
    if (s.equals("a")) cow.remove(s); // OK: iterating over the snapshot
}
```

---

### Question 274 — How can you create a sealed type without permits?
· [video](https://youtu.be/MBf7c42fX30) · local file `126`

**Short answer:** There is a pattern for that.

**Less short answer:** A sealed type needs to know its permitted extensions **at compile time**, and all these extensions need to live in the same compilation unit — that is, in the same package, or in the same module if you're using the Java module system. Instead of declaring your extending types in separate class files, you can declare them as **member types** — and in that way, the `permits` clause becomes **optional**: the compiler assumes that all the member types that extend your sealed type are permitted. A very convenient way of organizing your codebase, especially if you need to conduct a refactoring at some point. One last word: sealed interfaces implemented by records are a very powerful feature, used in data oriented programming. *(Another video.)*

```java
public sealed interface Shape {          // no permits!
    record Circle(double radius) implements Shape {}
    record Square(double side) implements Shape {}
    // members in the SAME file = permitted automatically
}
```

---

### Question 275 — What is heap pollution?
· [video](https://youtu.be/gRFUBUAdnzk) · local file `125`

**Short answer:** A problem you can easily avoid.

**Less short answer:** Heap pollution has to do with **generics and raw types**. If you declare a list with a **raw type** — that is, you omit to give a value to the type parameter — you may get an exception, usually a `ClassCastException`; so don't do that. And even if you declare a `List<String>`, you can still add Integers to it by using raw types — this is, of course, silly, and the compiler gives you hints about that by issuing warnings. But, because of backward compatibility, raw types are **legal**, so you can still use them. One last word: sometimes it may look easier to use raw types because generics are complex — that's always a mistake. Don't do that.

```java
List<String> names = new ArrayList();   // raw: an unchecked warning
List unsafe = names;                    // a raw type, again
unsafe.add(42);                         // "works"... for now
String s = names.get(0);                // ClassCastException (!)
```

---

### Question 276 — What is an Arena?
· [video](https://youtu.be/sHu3sv5dj3k) · local file `124`

**Short answer:** An interface.

**Less short answer:** An arena is the object you need to use to create **memory segments** in the memory part of the Foreign Function & Memory API. An arena is responsible for **allocating** these memory segments, and controls how your application is using them: it is `AutoCloseable`, and when closed, it **deallocates all the memory segments it created**; it also controls whether your read and write operations are within the boundaries of your segments, and which thread is accessing them. One last word: there are **four different arenas** in the JDK, all with different behaviors — your preferred choice should be `Arena.ofConfined()`, or `Arena.ofShared()` in a multi-threaded environment; and you can even create **your own arena**, in case you need it.

```java
try (Arena arena = Arena.ofConfined()) {
    MemorySegment seg = arena.allocate(128);   // a controlled allocation
    seg.set(ValueLayout.JAVA_INT, 0, 7);
}  // close: deallocates EVERYTHING, checks bounds and thread on each access
```

---

### Question 277 — What is thread pinning?
· [video](https://youtu.be/MrrGtcHrqxg) · local file `123`

**Short answer:** Something you want to avoid.

**Less short answer:** Thread pinning is an issue that came with virtual threads. Virtual threads are executed by regular threads, called **carrier threads** in this context; and when a virtual thread executes a piece of blocking code, it is **unmounted from its carrier thread** — and when the data it is waiting for is available, this virtual thread is **mounted again**, possibly on a **different carrier thread**. Now here is the trick: C or C++ code has access to **addresses on the stack**, so a virtual thread **cannot be unmounted** if it is running native code — those addresses would just be corrupted. **This is what pinning is**: the thing that prevents the unmounting. One last word: in the first version, from JDK 21, virtual threads were also pinned if they were executing a `synchronized` block — this one has been fixed in **JDK 24**.

```java
Thread.ofVirtual().start(() -> {
    callNativeLibrary();  // pinning: it does not unmount during JNI/FFM
});

// JDK 21: synchronized also pinned; JDK 24 (JEP 491): not anymore
synchronized (lock) { io(); } // OK on JDK 24+
// detect it with: -Djdk.tracePinnedThreads=full
```

---

### Question 278 — What is a skip list?
· [video](https://youtu.be/49NYZqqa8g8) · local file `122`

**Short answer:** A nifty structure to create lists.

**Less short answer:** One of the bad things of linked lists is that accessing an element by its index has a big O(n) complexity. A skip list adds an internal structure that reduces this complexity to big **O(log n)** — which is much faster: the log of 1000 is roughly 10, in base 2. This structure relies on the fact that the elements you store are **comparable**, and it creates **fast lanes** to quickly move to the element you need. When you add or remove elements from a skip list, this structure is updated, so that it is always balanced — well, as much as possible. In the JDK, the skip list has been used to implement the **`ConcurrentSkipListSet`** — a set, and even a navigable set — and the **`ConcurrentSkipListMap`**, a map, and even a navigable map. One last word: both structures are **concurrent** and built on the use of **atomic references** — you can go and check the source code; it's interesting.

```java
// a skip list search: starts on the upper "fast lane" and goes down
// expected O(log n), without balancing rotations
ConcurrentSkipListMap<String, Integer> map = new ConcurrentSkipListMap<>();
map.put("ana", 1); map.put("bob", 2);
map.firstKey();  // "ana" — navigable and thread-safe
```

---

### Question 279 — What is scalarization?
· [video](https://youtu.be/DHwaUchRTTE) · local file `121`

**Short answer:** An optimization that consists in **not creating** the object declared in your code.

**Less short answer:** Take the example of joining strings of characters: running this code in a naive way would lead to the creation of a bunch of intermediate strings that are actually not used by your code. Well, the same could happen with your own objects: you end up creating a bunch of `Population` objects that you don't need. Since these objects are **not escaping the method** they are created in, the simple optimization is to **avoid creating them**, and just use the values they carry — this is what is called **scalarization**: you replace your object with the numbers you need to conduct your computations. One last word: believe it or not, this is what **Valhalla** is already doing — just make this Population record a value type, and you will get scalarization for free.

```java
int total(Country a, Country b) {
    // the JIT (escape analysis) can remove the allocation:
    Population sum = new Population(a.pop() + b.pop());
    return sum.value();
    // scalarized -> becomes: return a.pop() + b.pop();
}
```

---

### Question 280 — How is the Collectors.joining() working?
· [video](https://youtu.be/XdUlWFT3nA0) · local file `120`

**Short answer:** *(the presenter skips the short answer)*

**Less short answer:** First, it creates the internal mutable container — a `StringBuilder`. Second, it accumulates in it the elements produced by your stream; if you passed a **separator, a prefix or a suffix**, then they are added to this StringBuilder too. And third, once all the elements of the stream have been consumed, it calls `toString()` on its internal StringBuilder, and returns this result. If you want to write your own collectors, taking a look at how this one is working is interesting: it exhibits the three elements every collector has — an *initializer*, an *accumulator*, and a *finisher* (it also has a combiner, "but I'm not sure you want to use this joining collector in a parallel stream"). And its set of characteristics is **empty** (`CH_NOID` — none). One last word: joining works **only on a stream of strings** — your compiler will tell you if you try to collect a stream of the wrong type.

```java
String r = Stream.of("java", "kotlin", "scala")
        .collect(Collectors.joining(", ", "[", "]"));
// "[java, kotlin, scala]"

// the internal design: supplier=StringBuilder::new,
// accumulator=sb.append, finisher=StringBuilder::toString, characteristics={}
```

---

### Question 281 — How can you split a string of characters?
· [video](https://youtu.be/djyHFEL9hvY) · local file `119`

**Short answer:** There are several patterns for that.

**Less short answer:** Choosing the right pattern depends on how you want to split your string — and on whether you think you're going to use **all** the elements, or if you think you will be needing just the **first ones**. In case you need all the elements: the good old `String.split()` pattern creates an array with what you need in it. And in case you're interested only in the first ones — or you're running in an environment where memory is tight — then you could prefer a pattern that splits your string **lazily**, like `Pattern.splitAsStream()`. One last word: depending on what you want to do, you may find analyzing your string with **regular expressions and named groups** useful; it's a little tricky to set up — regular expressions are not easy — but super powerful. *(Another video.)*

```java
String csv = "id;name;age";

String[] all = csv.split(";");            // the full array

try (var parts = Pattern.compile(";")
        .splitAsStream(csv)               // lazy: splits only what is read
        .limit(2)) {
    // uses only the first 2, without materializing the rest
}
```

---

### Question 282 — What is a parallel stream?
· [video](https://youtu.be/Lu8Lc20nGuQ) · local file `118`

**Short answer:** A stream that is processed in parallel.

**Less short answer:** Creating a parallel stream is super simple: you just need to call `.parallel()` on your stream, and you're done. The internal algorithm will then **split your data in two chunks**, and process each of them in its own thread; then each chunk may be split again — and again and again — until the algorithm decides that the chunk it has is small enough to be processed entirely. All these mechanics are managed by the Stream API: you do not need to worry about anything — neither race conditions, nor visibility issues, and the like — as long as you're **not doing any side effects** in your stream pipeline. (You know that side effects in a stream pipeline are a bad idea, don't you?) One last word: there are good use cases for parallel streams, and bad use cases — if your processing is not a good use case, then you may **hurt** your performance instead of improving it. As always, when it comes to performance: **measure, don't guess**.

```java
long sum = LongStream.range(0, 100_000_000)
        .parallel()                     // automatic divide and conquer
        .filter(n -> n % 3 == 0)
        .sum();                         // no side effects = safe
```

---

### Question 283 — What set operations are supported by the collection framework?
· [video](https://youtu.be/1HqV_ERIGxI) · local file `117`

**Short answer:** Actually, all of them: union, intersection and set difference.

**Less short answer:** These operations are defined in the `Collection` interface: `addAll` is the **union**, `removeAll` is the **set difference**, and `retainAll` is the **intersection**. These methods take another collection as a parameter, **mutate** the collection you are invoking the method on, and return a boolean that is true if the collection was mutated. If you plan to use these operations, you need to choose your implementation wisely, as they do not perform the same: removeAll removes all the elements of your other collection, which is **not efficient on an ArrayList** — it is much better on a HashSet; and the same goes for retainAll, which calls `contains` instead of `remove` — again, ArrayList is not very good at that, HashSet is much faster. One last word: for this kind of operations, HashSet is most of the time your best choice — as long as you're okay with its distinct nature.

```java
Set<Integer> a = new HashSet<>(Set.of(1, 2, 3, 4));
Set<Integer> b = new HashSet<>(Set.of(3, 4, 5));

a.addAll(b);      // union:        [1..5]  (mutates a!)
a.removeAll(b);   // set difference: [1, 2]
a.retainAll(b);   // intersection:   [3, 4]
// boolean return: true if anything changed
```

---

### Question 284 — Why is finalization being deprecated?
· [video](https://youtu.be/-HBnmiB-Ba8) · local file `116`

**Short answer:** Because it was buggy from the beginning.

**Less short answer:** Finalization consists in calling the `finalize()` method on an object that is about to be garbage — this method is defined on the Object class, and you can override it. There are **two main problems** with this mechanism. First, the **thread** that is calling the finalize method **was never specified** — and it was never a thread from your application: it is a thread from the **garbage collector**. So if you call `list.clear()` or `map.clear()` in there, for instance, on an ArrayList or a HashMap, then you have a nice **race condition** in clean code — which, if you're unlucky, can lead to a nice NullPointerException. Second, because you are in the object about to be garbage, you can actually **revive it** — by storing `this` in a static field, or a static container, for instance — which prevents some optimizations at the garbage collector level. One last word: finalization has been **deprecated** and will be removed at some point; you can use the **Cleaner** API instead, as a replacement — which is safer and fixes the two issues we just mentioned. *(Another video.)*

```java
@Deprecated(since = "9", forRemoval = true)
class Bad {
    @Override protected void finalize() {   // the GC thread calls this!
        list.clear();                       // a potential race condition
        STATIC_REGISTRY.add(this);          // revives the object (!)
    }
}
```

---

### Question 285 — Should you favor streaming or iterating?
· [video](https://youtu.be/ncE0dVAwLRc) · local file `115`

**Short answer:** It really depends on your use case.

**Less short answer:** What is important is that you understand the differences between both approaches. You can stream or iterate over your data in a **lazy** way — as long as you write your own iterator. Depending on the processing you need to conduct, streaming may be **more readable**: you pass the data, then you pass your operations, and then they are applied to your data by the API; if you iterate yourself, then you need to write all this code explicitly. Streaming also allows you to **go parallel easily**, if this is what you need. But there is a caveat: **creating a stream is not free** — it has a cost, memory-wise and performance-wise. One last word: you should favor the **readability** of your code in all cases — writing readable code makes the long-term maintenance of your application less expensive; favoring performance is a short-term view, and in many cases it is just longer.

```java
// streaming: declarative
var active = clients.stream()
        .filter(Client::active).map(Client::email).toList();

// iterating: explicit, no overhead
var active2 = new ArrayList<String>();
for (Client c : clients)
    if (c.active()) active2.add(c.email());
// choose whichever is more READABLE in the context
```

---

### Question 286 — What is a MemorySegment?
· [video](https://youtu.be/u3PTqVdijvg) · local file `114`

**Short answer:** An interface.

**Less short answer:** MemorySegment models the **contiguous zone of memory that lives outside of the heap** memory of your application. So this MemorySegment object lives on the heap, as any Java object, and has an **off-heap reference** to this piece of memory. A memory segment is created by an **Arena** object, that controls several things — among them, whether you are reading and writing **within the boundaries** of your segment, and **from the right thread**; arenas are also responsible for closing your memory segments. A memory segment is indexed by a **long**, so it can be really large — as of now, probably larger than what your machine can give you. One last word: you can write **structured data** in a memory segment, that you can then stream — which is amazing; all this using the Memory Layout API. We will talk more about it later.

```java
try (Arena arena = Arena.ofConfined()) {
    MemorySegment seg = arena.allocate(1L << 30);   // 1 GB, long-indexed
    seg.set(ValueLayout.JAVA_LONG, 0, 42L);          // bounds + thread check
}  // closed by the arena — no use-after-free
```

---

### Question 287 — What thread does a CompletableFuture use?
· [video](https://youtu.be/C5GJL_Vr91g) · local file `113`

**Short answer:** The default behavior is to use a thread from the *common ForkJoin pool*.

**Less short answer:** The CompletableFuture API allows you to define **asynchronous pipelines** of operations that are triggered by the API itself: you define this pipeline — passing your operations, modeled by lambda expressions — and the API processes the data when it is available. It has been designed to be non-blocking, and applies the callback-based reactive approach. The thread that is executing your lambda expression is taken from the **common ForkJoin pool** — but the API accepts Executor Services for specific operations; in that case, **your threads** will be used. One last word: this pool of threads is the **same** as the one used to run your parallel streams — so if you're using — hmm, using both APIs in your application, you will have a **competition** on the use of these threads, which may slow down both your streams and your CompletableFutures. You may want to avoid this situation.

```java
CompletableFuture.supplyAsync(this::load)      // common pool...
        .thenApply(this::transform)
        .thenAccept(this::save);

CompletableFuture.supplyAsync(this::load, myExecutor)  // YOUR threads
        .thenApplyAsync(this::transform, myExecutor);
```

---

### Question 288 — When is a stack trace created?
· [video](https://youtu.be/DMRHcRjhpJA) · local file `112`

**Short answer:** When the **exception** is created.

**Less short answer:** There is no exception to this rule — as all exceptions extend `Throwable`, directly or indirectly, and the stack trace is created **when the Throwable is created**. This is why, as a rule of thumb, you should always create your exception **where you throw it**, with the `throw new MyApplicationException(...)` pattern; in that way, the stack trace takes you directly where the error was noticed in your application. This is one of the drawbacks of the callback-based reactive frameworks: since the element that calls your lambda is the framework itself, the stack trace takes you into this framework — and not into your application — making the bug much harder to find. When you re-throw an existing API or language exception wrapped in one of your application exceptions, don't forget to **pass the first exception as the cause** of your application exception: in that case, you will have **both stack traces**, making the debug much easier.

```java
try {
    process();
} catch (IOException e) {
    // the cause preserved = 2 chained stack traces
    throw new ProcessingException("failure in batch 7", e);
}
// throw new ProcessingException(msg);  // WITHOUT the cause: harder debug
```

---

### Question 289 — What is wrong with ThreadLocal? *(version 2)*
· [video](https://youtu.be/7HBI6VwM7wk) · local file `111`

**Short answer:** Everything.

**Less short answer:** In a nutshell: ThreadLocal is an API that was added to the JDK in Java 2, 1998 — that is, **six years before** the ExecutorService pattern was introduced in Java 5, 2004 — and the interaction between both patterns does not work so great. A ThreadLocal variable is actually stored in a **hash map attached to a thread** — so it shares the life cycle of this thread: if you do not remove your ThreadLocal variables **by hand**, then they will stay **forever**, and can be read by any code running in this thread — any code; that could lead to **serious security breaches**. Don't do that. A ThreadLocal variable is **mutable**, so it prevents the JVM from optimizing it, making them quite costly in your application. And last but not least: the binding is local to a thread — so if you move from one thread to the other, well, you need to tell that you want to **inherit** your ThreadLocal variables. But the fact is: **you need thread locals** — and if you think you don't, your favorite application framework does, and uses them heavily. One last word: starting with JDK 25, you can use **ScopedValues** instead of ThreadLocal variables — that fixes all these issues. We will talk more about that in the future.

```java
// the thread's hash map holds the value; without remove, it outlives the task:
static final ThreadLocal<Context> CTX = new ThreadLocal<>();
CTX.set(requestContext());
try { serve(); } finally { CTX.remove(); }  // ALWAYS (seriously!)
```

---

### Question 290 — What is a stateful operation?
· [video](https://youtu.be/K5XcctGAlHk) · local file `110`

**Short answer:** A special type of intermediate operation on the Stream API.

**Less short answer:** Stateful means that there is a **state** that needs to be carried over from the processing of one element to the other. Being stateful means that the result of this operation depends on the element being processed, **and** on some **mutable state** — the state that lives inside or outside of your stream. For instance, `distinct` is a stateful operation, as it stores all the elements it sees in an internal hash set — so passing an element depends on this element **and** on the content of this hash set; and the same goes for `limit`, that needs to count the number of elements it has passed so far. One last word: this mutable state needs to be **shared among the different threads** processing your stream, in the case of a parallel stream — in other words, stateful operations **don't play well with parallel streams**. You've been warned.

```java
stream.distinct();  // stateful: an internal HashSet
stream.limit(10);   // stateful: a shared counter
stream.map(fn);     // stateless: element by element, free
stream.filter(p);   // stateless
```

---

### Question 291 — How can you format a floating point number?
· [video](https://youtu.be/1LsdWU2rAgU) · local file `109`

**Short answer:** There is a pattern for that.

**Less short answer:** Java actually supports the same kind of **ugly formatting thingy** that you can use in C/C++ — you know this format with sharp characters, and plus, and dot, and other things and numbers, that you never know how it works; yes, that's the one. To access this formatting thingy, you need to call `printf` instead of `println`: this printf method works the same as the printf from C — where the F means *format*. It takes two parameters: the format, and a variety of arguments to print — which can be empty, by the way. The format itself is implemented in a class called `Formatter`, where you have all the documentation and the syntax you can use — this is the class you need to check for the documentation. You can use a Formatter directly to format a string, or you can use the `String.format()` factory method, which creates and calls a Formatter for you. One last word: you can also format elements with one of the extensions of the `Format` abstract class from java.text — extended by MessageFormat, NumberFormat, DateFormat, and some others. *(Another video.)*

```java
System.out.printf("PI ~ %.2f%n", Math.PI);  // PI ~ 3.14
String s = String.format("%,.3f", 12345.6789);
NumberFormat currency = NumberFormat.getCurrencyInstance(Locale.US);
currency.format(3199.99);                   // $3,199.99
```

---

### Question 292 — What is a parallel friendly stream source?
· [video](https://youtu.be/eojE7M2sj3Q) · local file `108`

**Short answer:** A source of data that is easily **splittable**.

**Less short answer:** A parallel stream splits its source in two chunks, again and again, until each chunk reaches a certain size where the algorithm decides to process it. For this way of splitting your source to work properly, your source needs to have **two properties**. First, the parallel stream needs to **know how many elements** this source will produce — if it's not known, then dividing this source in two parts of the same size is not going to work very well. Then, the **size of each chunk** also needs to be known, for the algorithm to carry on. You may be thinking that, if the size of the source is known, then it's also the case when it is split — well, it's **not always** the case: for instance, if you split the internal array of a hash set, you cannot know in advance how many elements you will have in each subarray. One last word: in a nutshell, the best source is an **array**; other in-memory collections — like hash sets — are not so good; and **IO sources are all bad**. `Pattern.splitAsStream`, which is a non-collection, in-memory structure, is also very bad.

```java
int[] data = ...;
Arrays.stream(data).parallel().map(this::heavy);  // great: SIZED + SUBSIZED

hashSet.stream().parallel()...   // bad: unbalanced split
Files.lines(path).parallel()...  // terrible: an IO source
```

---

### Question 293 — What is a Cleaner?
· [video](https://youtu.be/daE5thDlfEM) · local file `107`

**Short answer:** An API.

**Less short answer:** The Cleaner API is some kind of **replacement of finalization** that fixes the problems this mechanism has. It allows you to **execute a task when an object is about to be garbage** — this task is usually a lambda; in its simple form, it's just a Runnable. Of course, if this lambda holds a reference to the object you want to clean, this reference will **prevent your object from ever being garbage** — and your cleaner will never be called. Be careful if you implement a cleaner with an **anonymous class**, because anonymous classes have a **sneaky reference on `this`**, which will prevent your object from being garbage. Your cleaners can be called by different ways, so you need to be careful with race conditions and the like. One last word: the nice thing is that a cleaner **cannot revive an object**, as finalization could. Great.

```java
private static final Cleaner CLEANER = Cleaner.create();

class Resource implements AutoCloseable {
    private final Cleaner.Cleanable cleanable;
    Resource() {
        cleanable = CLEANER.register(this, () -> releaseNative(id()));
        // the lambda must NOT capture 'this'!
    }
    @Override public void close() { cleanable.clean(); } // safety net
}
```

---

### Question 294 — What is operator overloading?
· [video](https://youtu.be/6HVeItSN1iU) · local file `106`

**Short answer:** Something Java has — but not you.

**Less short answer:** Operator overloading means that a given operator, like plus, can operate on **different types of operands**. For instance, in Java, you can apply plus on numbers, and on strings: plus can add two numbers, and can concatenate strings — this is an example of overloading. Technically, adding ints and floats is **also** overloading: the bytecode instruction that "plus" compiles to is not the same, and the addition is not the same either — adding ints is associative; adding floats or doubles is **not**. What you **cannot** do in Java is define **your own** overloading: for instance, you could define a `Population` record, and could define the addition of Population objects — that could make sense — but Java does not allow this; you cannot define your own overloading of operators. One last word: once Valhalla brings value types to the Java platform, your Population record will become a value record — and maybe adding operator overloading for value types could make sense. But that will be for later.

```java
1 + 2;          // iadd
1f + 2f;        // fadd (a different instruction, a different semantics!)
"a" + "b";      // StringBuilder.append (an overload for String)

record Population(int value) { }
// Population p3 = p1 + p2;   // ERROR: no user-defined overloading
Population p3 = new Population(p1.value() + p2.value()); // a method, as usual
```

---

### Question 295 — How can you stream the content of a directory?
· [video](https://youtu.be/0boCP_fZOA8) · local file `105`

**Short answer:** There is a method for that.

**Less short answer:** The method is defined on the `Files` factory class, and is called `walk`. It needs a **path** where the walking starts; and you can give it a **depth**, to avoid exploring super deep hierarchies; and you can even pass an argument to tell if you want to follow **symbolic links** or not. This algorithm is said to be **depth-first**, meaning that it will explore directories before moving to the next element in a given directory. The result you get is a `Stream<Path>` that is **lazily evaluated** — so if you're looking for a specific element, you can use a `findFirst` or `findAny` terminal operation, and the stream will interrupt the exploration once it is found. One last word: don't even think about going **parallel** on these streams — you can only produce them one at a time, so it will not work very well.

```java
try (Stream<Path> s = Files.walk(Path.of("./src"), 3)) {
    Optional<Path> readme = s.filter(p -> p.endsWith("README.md"))
                             .findFirst();     // lazy: stops when found
}
```

---

### Question 296 — How can you map a file to memory?
· [video](https://youtu.be/V1kvNFUda_Y) · local file `104`

**Short answer:** There is a pattern for that — actually, two patterns.

**Less short answer:** In both cases, you start by creating a **FileChannel**, by providing a path and some open options. Then you have two very similar patterns. The first one consists in calling `map()`, passing another option — the mode — a position in the file you want to map, and the size of the region to be mapped: this is the old-fashioned code, that returns a **ByteBuffer**. The second pattern consists in passing **another argument, which is an Arena** — then what you get is a **MemorySegment**, and this pattern is much better than the first one for many reasons: closing your arena — usually in a try-with-resources statement — **invalidates your memory segment**, and **flushes your modifications to the disk** if needed. One last word: be careful, because **closing your FileChannel does not invalidate your ByteBuffer** — and if you continue writing data to it, this data may still be written to the disk; probably not what you will expect.

```java
// the new pattern (FFM): the arena invalidates and synchronizes
try (Arena arena = Arena.ofMap()) {
    try (FileChannel ch = FileChannel.open(Path.of("data.bin"),
            StandardOpenOption.READ, StandardOpenOption.WRITE)) {
        MemorySegment seg = ch.map(FileChannel.MapMode.READ_WRITE, 0, ch.size(), arena);
        seg.set(ValueLayout.JAVA_INT, 0, 42);   // write-through
    }
}   // closing the arena: safely invalidates the segment

// legacy: closing the channel does NOT invalidate the buffer!
MappedByteBuffer buf = ch.map(FileChannel.MapMode.READ_WRITE, 0, size);
ch.close();  // buf is still alive and writable... a trap
```

---

### Question 297 — How can you check if a character is a letter?
· [video](https://youtu.be/qIGC7j9WEuI) · local file `103`

**Short answer:** There is a method for that.

**Less short answer:** For all these kinds of things, you need to check the factory methods of the `Character` class — there are about **50** of them, to check many things about your character. But you still need to be careful, because a character — the one that you write — may actually be encoded in **two Java chars**: that is, 32 bits, and not 16. So if your application needs to deal with non-trivial characters, the answer to this question may be **much more complex** than what it seems. One last word: it also has an impact on the value returned by the `length` method of the String class — length returns the number of **Unicode code units**, so it may actually be larger than what is printed on your screen.

```java
Character.isLetter('a');      // true
Character.isLetter('á');      // true (Unicode, not just ASCII!)
Character.isLetterOrDigit('7');
Character.isLetter('😀');     // false — and it takes 2 chars (a surrogate pair)
"😀".length();                // 2 — more than the "1 character" you see
```

---

### Question 298 — Are static members inherited?
· [video](https://youtu.be/cUMf_Mblcxs) · local file `102`

**Short answer:** No. But yes. *(a trick question)*

**Less short answer:** This one is actually a trick question. Inheritance is defined on **instances**, and not for static members — so the answer is no. But there is a trick in Java, which is the following: if you have a B class that extends the A class, and A defines a **public static field or method**, then you can get a reference on these **through B** — you can write `B.method()`, and the compiler will be happy. But if the class B defines **its own** public static method with the same name, then this method will **hide** the method defined on the class A. One last word: this is confusing, but it is **not inheritance, nor overriding** — inheritance is for instances only, not for static members. Calling an instance method that can be overridden is a **virtual call**, that is resolved at **runtime**; static code is resolved at **compile time**.

```java
class A { public static String who() { return "A"; } }
class B extends A { }

B.who();   // "A" — accessible through B, but... inheritance? NO: just access
// if B declares its own who(), it HIDES A's — static resolution
```

---

### Question 299 — What is a cache miss?
· [video](https://youtu.be/x3eqcdmxEzw) · local file `101`

**Short answer:** Something you want to avoid.

**Less short answer:** A cache miss has to do with how our CPUs are working. All the data you are processing lives in the memory of your machine — accessing this memory is **slow**, so your data is first moved to the **different levels of caches** you have on your CPU, so that it can be accessed quickly. This transfer is made by **chunks of memories**: more than one piece of the data you need is transferred in one go — think about an array, for instance: a single chunk may hold like **eight elements** of it, that are transferred in one go; for your application, that's a win. If at some point your CPU checks its cache and sees that the data it needs is **not there**, then it needs to go back to the **main memory** to get it — this is what a **cache miss** is. As this round trip is costly, your CPU may decide to **switch to another computation**, and remove yours from the CPU until the data is there — and that may be a **significant performance hit**. One last word: this is why you should always favor **array-based, in-memory structures or collections**, over reference-based structures — because cache misses can really kill your performance.

```java
int[] data = new int[1_000_000];       // contiguous: 1 cache line ~ 8+ ints
for (int x : data) sum += x;           // very few cache misses

List<Integer> list = new ArrayList<>(); // references scattered on the heap
for (int x : list) sum += x;            // miss after miss (and boxing!)
```

---

### Question 300 — What is a distinct stream?
· [video](https://youtu.be/SLCp6PPSbUs) · local file `099`

**Short answer:** A stream where all the elements are distinct.

**Less short answer:** `DISTINCT` is actually one of the **characteristics** that are defined in the spliterator of your stream — and that you can check. The stream created on a `Set`, for instance, has this characteristic set: it means that if you call `distinct()` on it, **nothing is done** — all the elements are passed directly. If the stream is created on an ArrayList, then calling distinct will **store them in a hash set** before pushing them — or not — to the downstream. Since calling distinct returns a distinct stream, calling distinct again **will not trigger any computation** — which is what you want. One last word: unfortunately, you cannot check these characteristics **from the lambdas** you pass to the intermediate or terminal methods of your stream — and yes, that includes forEach and collect.

```java
Set<String> set = Set.of("a", "b");
set.stream().distinct();   // a no-op: the spliterator is already DISTINCT

List<String> l = List.of("a", "a", "b");
l.stream().distinct();     // goes through an internal HashSet
// .distinct().distinct()  // the 2nd is a no-op by construction
```

---

### Question 301 — What is dependency injection?
· [video](https://youtu.be/anPv9mqGztY) · local file `098`

**Short answer:** A mechanism to inject dependencies into an object.

**Less short answer:** Especially in large applications, the initialization of some objects can be really complex, with **complex dependency graphs** — like: object A needs object B to work, that itself needs B1 and B2, etc. — and this initialization requires to be made in a **precise order** to work properly. To organize this initialization, your best bet is: for each object to **declare what it needs** to work, and let some **external element** initialize everything. It could be some third-party framework, or some internal tool that you've developed yourself — in simple cases, it could even be a simple `main` method. The easiest and safest way to declare that an object A needs objects A1, A2 and A3 to work, is to create a **constructor in A** that takes instances of A1, A2 and A3. By using Reflection, a third-party framework can discover that, and do the **wiring** of all your objects for you — this is called **constructor injection**, and this is the way you should favor. One last word: the type of your dependencies should always be **interfaces** — it allows you to easily create dummy implementations for testing, and you can even use frameworks for that.

```java
class OrderService {                          // constructor injection
    private final Repository repo;            // a dependency by INTERFACE
    private final Notifier notifier;
    OrderService(Repository repo, Notifier notifier) {
        this.repo = repo; this.notifier = notifier;
    }
}
// the "external element" (framework/main) does the wiring
new OrderService(new PostgresRepo(), new EmailNotifier());
```

---

### Question 302 — What is a monitor?
· [video](https://youtu.be/Dqv8f_dbG2c) · local file `097`

**Short answer:** Something that has to do with synchronization.

**Less short answer:** Objects have been designed in Java so that **each of them can act as a monitor** for synchronization. You can use any object — even just a simple `new Object()` — to synchronize a block of code; this is due to the fact that **every object carries a monitor**, which is stored in the **header** of any instance. To be able to enter a synchronized block of code, a thread needs to **hold this monitor**: it enters the block, takes this monitor, and gives it back when it exits. Then, if another thread tries to enter the same block at the same time, it will discover that the monitor is absent, so it will have to wait until it is available again. One last word: having a monitor for each object has two consequences: first, it **consumes memory** — not much, but it is still an overhead; and second, it **prevents some optimizations**, like inlining or flattening objects — the reason why value types **will not have any monitor**. *(Another video.)*

```java
private final Object lock = new Object();  // any object will do

synchronized (lock) {        // the thread holds the header's monitor
    counter++;              // mutual exclusion guaranteed
}                           // the monitor is released here
```

---

### Question 303 — What is the epoch?
· [video](https://youtu.be/eXAhGp_axHA) · local file `096`

**Short answer:** A special date in operating systems and APIs.

**Less short answer:** Measuring time is actually harder than it seems — especially when all you have are 16-bits integers. So at some point, when the first operating systems were created, people decided that the time would be measured in **seconds, starting on January 1st, 1970**; it could be different from one OS to the other, so this definition may vary. Of course, this used to have limitations, and could generate bugs — especially if you were working with a language whose definition of the epoch was different from the one in your operating system. But now things have been sorted out... well, mostly. One last word: if you need it in your application, you have **two constants** in the Java API to model this epoch: `LocalDate.EPOCH` and `Instant.EPOCH`.

```java
Instant.EPOCH;                  // 1970-01-01T00:00:00Z
LocalDate.EPOCH;                // 1970-01-01
Instant.now().getEpochSecond(); // seconds since the epoch
```

---

### Question 304 — How is switch on String working?
· [video](https://youtu.be/fZjDaw6mpxE) · local file `095`

**Short answer:** Switching on a string is actually switching on an integer.

**Less short answer:** The possibility to switch on strings was added in **JDK 7, in 2011** — and it uses a trick: it actually computes the **hash code** of your selector variable, along with the hash codes of your case labels, and switches on **integers**. The hashCode method that is used is the one of the String class — which has an unfortunate consequence: it comes **carved in marble**. Changing it would require to **recompile all the existing Java code**, since the output of this method is **hard-coded in the bytecode** of your switch statements on strings. One last word: you can now switch on **types** — this is one of the cornerstones of data oriented programming — and switching on primitive types may also come in the future.

```java
// compiles to: switch (s.hashCode()) { case 104827: if (s.equals("run"))... }
switch (command) {
    case "start" -> start();
    case "stop"  -> stop();
    default      -> help();
}
// this is why String.hashCode() can NEVER change
```

---

### Question 305 — What is a design pattern?
· [video](https://youtu.be/_k9G-cFOVvs) · local file `094`

**Short answer:** A recipe to write clean code.

**Less short answer:** It is actually a term that refers to a book published in 1994, called *Design Patterns: Elements of Reusable Object-Oriented Software* — also called the Gang of Four, due to its four authors. Its influence on programming is still immense, even if some of the proposed implementations are a little dated — the book is written in **C++**, because it was published before Java was released, in 1995. There are **23 design patterns** in it, each one to solve a particular problem in programming. Some are still very well known — like Iterator, Factory, Singleton, or Strategy; some less used, like Memento. The GoF also gives you **two principles** in programming: favor object composition over class inheritance, and program to an interface, not to an implementation. One last word: the authors created more design patterns in other books — but I don't think any of them could reach the popularity and influence of the Gang of Four.

```java
// two GoF principles in action:
List<String> list = new ArrayList<>();  // to the INTERFACE (List)
class Car { private final Engine engine; // COMPOSITION, not inheritance
    Car(Engine e) { this.engine = e; } }
```

---

### Question 306 — What are bounded type parameters?
· [video](https://youtu.be/zVDCSUkZA0A) · local file `093`

**Short answer:** Some ugly stuff you never know how it is working.

**Less short answer:** You know this bizarre question-mark-super-T syntax, or question mark extends R? Yes, this is what we're talking about. This syntax was added with Generics, to manage type inheritance. On the one hand, we need it; and on the other hand, we never know what to put, and end up trying stuff until the compiler is happy. So, in a nutshell: **`? super T`** represents all the types that are **supertypes** of T. For instance, the method `Collection.forEach` takes a `Consumer<? super T>`, so that when you write `strings.forEach(...)`, you can pass `System.out::println` — that takes instances of Object, which is indeed a supertype of the String type. On the other hand, `Stream.map` takes a function that returns `? extends R`, and produces a `Stream<R>` — meaning that this function can return **any subtype of R**, so that it can be cast into R. One piece of advice: if you're struggling with all this, you can check the signatures of the methods of the Stream API — there are plenty of examples that will help you understand all that. One last word: this is probably the most tricky part of Generics — and if you check the methods of Comparator, you may even come across **multiple bounded type parameters**. *(Another video.)*

```java
// ? super T (a consumer): accepts supertypes
List<String> names = List.of("a");
names.forEach(System.out::println);   // println accepts Object (super String)

// ? extends R (a producer): returns subtypes
Stream<Number> nums = Stream.of(1, 2L).map(n -> n);  // Integer -> Number

// multiple bounds:
static <T extends Object & Comparable<T>> T max(List<T> l) { ... }
```

---

### Question 307 — What is a statement?
· [video](https://youtu.be/9yHnGqQ0DFM) · local file `092`

**Short answer:** Almost anything is a statement.

**Less short answer:** A statement is a construct that represents a **complete unit of execution**. An expression can be made a statement if you add the semicolon after it: assignments, method invocations, incrementations or decrementations, object creations — these are called the **expression statements**. There are also **control flow statements**: if-then-else, switch, for, while, even return — all these are control flow statements. One last word: statements are grouped into **blocks**, that are delimited by curly braces; such blocks can be used wherever a single statement can be used — and sometimes making a block, even for a single statement, can make your code **more readable**.

```java
int x = 0;                 // assignment (an expression statement)
method();                  // invocation
x++;                       // increment
if (x > 0) {               // control flow + a block
    log("positive");       // a readable block, even with 1 statement
}
```

---

### Question 308 — What is a thread scheduler?
· [video](https://youtu.be/VCXDZKcVNR4) · local file `091`

**Short answer:** An object that schedules threads.

**Less short answer:** A thread is a piece of code that is executed independently of other threads — and while doing so, it uses some CPU resource. At some point, if there are many threads running at the same time, you need to be able to **suspend** a thread so that the others get a chance to run. The suspension should happen if the thread is **blocked** — for instance, waiting for some data coming from the disk or the network, or waiting for some monitor that is preventing it from executing a synchronized piece of code. That is the role of the **thread scheduler**: making sure that the CPU resource is **evenly shared** among threads, and that the active threads are indeed **using the CPU** resource — and not waiting on the CPU doing nothing. This capacity of suspending a thread is called **preemptiveness**: the thread scheduler can be preemptive, or non-preemptive. One last word: virtual threads are using a specific virtual thread scheduler, that is **not preemptive** — the reason why you should **never** conduct long-running, in-memory computations in your virtual threads; if you do, you could block your entire virtual thread system.

```java
// OK: blocking on IO -> the virtual thread unmounts, the carrier is released
Thread.ofVirtual().start(() -> downloadFile(url));

// BAD: a long computation monopolizes the carrier (non-preemptive scheduler)
Thread.ofVirtual().start(() -> computePi(10_000_000)); // use a platform thread!
```

---

### Question 309 — What is a final method?
· [video](https://youtu.be/AnobDwQF7wo) · local file `090`

**Short answer:** A method with the `final` keyword on its declaration.

**Less short answer:** The use of the final keyword there **prevents any kind of overriding or hiding** from the extending classes. For **static** methods: you cannot create a static method with the same signature in any extending class — that would hide this static method, and it is forbidden; so a final static method cannot be hidden in any extending class. For **instance** methods, it's the same: it means that this method cannot be overridden in any extending class. In both cases, it is possible to have a method with the **same name and different parameters**, in the same class or in any extending class. One last word (no pun intended): if you have a final static method in a class A, it also prevents a **non-static** method with the same name in any extending class — and the reverse is also true: a final instance method in A prevents any **static** method with the same name in its extending classes.

```java
class A {
    final void process() { }        // cannot be overridden
    final static void helper() { }  // cannot be hidden
}
class B extends A {
    // void process() { }           // ERROR: overriding a final method
    // static void helper() { }     // ERROR: hiding a final method
    void process(String s) { }      // OK: overloading (other params)
}
```

---

### Question 310 — What is a LocalDateTime?
· [video](https://youtu.be/MJPzohzmYl4) · local file `089`

**Short answer:** A class from the Date and Time API.

**Less short answer:** The Date and Time API has several classes to represent dates and times — all final — and LocalDateTime is one of them. **Local means that the date it represents is not bound to any time zone**; LocalDateTime carries both a date and a time. There are several factory methods to create instances — that take a year, month and day of the month, plus an hour, minute, second, and even nanoseconds. There are two methods to bind this date to a time zone — `atOffset` and `atZone` — and plenty of others to add temporal amounts, like days or hours. LocalDateTime objects are **non-modifiable**: all these methods return new objects. One last word: all these objects from the Date and Time API are **good candidates to become value types** once Valhalla makes it as a final feature — as such, they are already annotated with `@ValueBased`. *(Another video.)*

```java
LocalDateTime dt = LocalDateTime.of(2026, 10, 4, 15, 30);
dt.plusDays(1);                              // a new object (immutable)
dt.atZone(ZoneId.of("America/Sao_Paulo"));   // binds to a zone
dt.toLocalDate();                            // the date only
dt.toLocalTime();                            // the time only
```

---

### Question 311 — When does a switch check for exhaustiveness?
· [video](https://youtu.be/wm6iTz8rm0o) · local file `088`

**Short answer:** Not always. *(the presenter's short answer for this one, which is trickier than it seems)*

**Less short answer:** The switch changed over time. What does it mean for a switch to check for exhaustiveness? It means that if, for some values of your selector variable, you can traverse your switch block **without matching anything**, you get an error. In its initial version, the switch was a **statement** — and statements do **not** check for exhaustiveness, and they still don't, for backward compatibility reasons. But the **switch expression** — the one that returns a value — **requires exhaustiveness**: either you cover all the cases, or you provide a `default` — without that, a compilation error. With **sealed types and pattern matching**, the compiler knows the closed set of subtypes and can prove the coverage — that is where the check shines.

```java
int days(Weekday d) {           // a statement: no requirement
    switch (d) {
        case MON -> return 1;   // missing the other cases: it compiles!
        default -> return 0;
    }
}

int code(Weekday d) {           // an expression: EXHAUSTIVE or it doesn't compile
    return switch (d) {
        case MON -> 1;
        case TUE -> 2;
        // ... covering all of them (or a default) is mandatory
    };
}
```

---

### Question 312 — What is wrong with ThreadLocal variables?
· [video](https://youtu.be/YRxdbhF29KQ) · local file `087`

**Short answer:** Almost everything.

**Less short answer:** ThreadLocal is a class from Java 2, 1998 — a time when there was no ExecutorService, which is a feature of Java 5, 2004 — so that's six years later. If you carefully check the API, you will see that in Java 5, a `remove()` method was added to the ThreadLocal class. Because before the ExecutorService pattern, you would create a thread on demand and let it die — and its ThreadLocal variables would go long with it. The ExecutorService pattern **put an end to this golden era**: now you **reuse your platform threads** — and you need to take care of the **removal of your ThreadLocal variables**. If you don't, everything you put in them can be reused by **any other code running in the thread you are reusing** — probably not what you want. One last word: ThreadLocal variables are fully supported by virtual threads, but starting with **Java 25**, you can use **ScopedValues** instead — they work much better, they can be optimized by the JVM, and you don't need to remove them.

```java
private static final ThreadLocal<User> CTX = new ThreadLocal<>();

CTX.set(user);            // in a thread pool...
process();                // ...another task on the SAME thread
CTX.remove();             // ...without this, it sees your user!

// Java 25+: no remove, bound to the scope
private static final ScopedValue<User> CTX2 = ScopedValue.newInstance();
ScopedValue.where(CTX2, user).run(this::process);
```

---

### Question 313 — What is a guarded pattern?
· [video](https://youtu.be/05bcRDdEnVg) · local file `086`

**Short answer:** A pattern with a guard.

**Less short answer:** A guarded pattern is something you can use in a **case label**. Such a label starts with a **type pattern** or a **record pattern**, and is followed by your check — the `when` clause — on the type variables that you created. For instance, you can create a case where you only select the circles that are **blue**, another one for the circles that are **green**, and a last one for all the other circles. Two elements you need to be aware of. First: a case label with a guarded pattern **never counts** when it comes to checking if your switch is exhaustive or not. And second: a guarded pattern can **never dominate** any other pattern — so most of the time, you will need a case where you check the type or the record **with no guard**, for your switch to compile. One last word: there is an exception to all this, for the guarded pattern `when true` — but please avoid doing that in your code.

```java
switch (shape) {
    case Circle c when c.color() == BLUE  -> paintSea(c);
    case Circle c when c.color() == GREEN -> paintField(c);
    case Circle c                         -> paintNeutral(c); // no guard!
    default                               -> ignore();
}
```

---

### Question 314 — What does work stealing mean?
· [video](https://youtu.be/TNDdBab0PTo) · local file `085`

**Short answer:** Probably not what you think.

**Less short answer:** It is a pattern that is used in a **ForkJoinPool**, where you have several **waitlists** that can accept tasks; each waitlist is bound to a specific thread, that picks tasks from this list. Now imagine a time where this list — and the end of this list — is empty, while the other lists in the pool are not: in that case, the **starving thread can steal a task from another list** — and this is called **work stealing**. This pattern is used in **parallel streams** — where the main thread creates the first tasks, which are stolen by other threads from the common pool — and it's also used in **virtual threads**, where a virtual thread that is coming back to the carrier it was unmounted from can also be taken by another kernel thread, in case the first one is busy. One last word: all this is **transparent** for you, as an application developer — which is a good thing, because it comes with issues when it comes to race conditions and visibility, that are all taken care of by the API.

```java
// ForkJoinPool: a deque per thread + stealing from the opposite end
var pool = ForkJoinPool.commonPool();
list.parallelStream().map(this::heavy)  // tasks are divided;
    .collect(toList());                 // idle threads steal work
```

---

### Question 315 — How can you get the substring from a string?
· [video](https://youtu.be/WaLyP6SNIsU) · local file `084`

**Short answer:** There is a method for that.

**Less short answer:** There are actually **two overloads** of this method. The first one takes only the **starting index**, and gives you all the rest of the string; and the second one gives you the string between a starting index and an **end index — excluded**. You will get a `StringIndexOutOfBoundsException` if you provide bad indexes. One last word: there was a time, back in the days, where the string that was returned would **share the same char array** as the original string — and this was seen as a memory optimization. First, strings are now built on **byte arrays**, not char arrays anymore; and second, this was **removed a long time ago** — just because if you select only two characters from a very large string that you don't need anymore, keeping this original array alive was not really a memory optimization.

```java
String s = "Cracking the Java Coding Interview";
s.substring(13);          // "Java Coding Interview"
s.substring(13, 17);      // "Java" (the end is exclusive)
// s.substring(-1);       // StringIndexOutOfBoundsException
```

---

### Question 316 — How is the Collectors.toMap() working?
· [video](https://youtu.be/T4nwVNzI6ak) · local file `083`

**Short answer:** This collector is used to build maps from keys and values.

**Less short answer:** It takes two parameters — both are functions that take the element of the stream: the first one is used to **build a key**, and the second one to **build the value**. If you are sure that **no duplicate** will be generated from your keys, then you can stop there — each element will generate a key-value pair in the final map. If you think that you may have duplicates — or you're sure you will — then you can provide the **third argument, which is a merging function**: it is called by the implementation with the **two values that are bound to the same key**, and you should return a merged value. If you don't, you will have an exception. One last word: if merging means **adding the elements to a list**, then you may consider using `Collectors.groupingBy` instead of toMap — which can create this list for you.

```java
Map<String, Integer> byName = people.stream()
        .collect(Collectors.toMap(Person::name, Person::age));

Map<String, Integer> withMerge = people.stream()
        .collect(Collectors.toMap(Person::city, Person::age, Integer::sum));

// merging into lists? groupingBy is better:
Map<String, List<Person>> byCity = people.stream()
        .collect(Collectors.groupingBy(Person::city));
```

---

### Question 317 — What is an object header?
· [video](https://youtu.be/ADWzf-lgKzc) · local file `082`

**Short answer:** A header that the JVM needs to manage your objects in memory.

**Less short answer:** Each object in Java has a header that carries information that the JVM needs — the size of this header can be as big as **16 bytes**: something significant for small objects, like an instance of Integer. Several things are stored in this header: the **monitor** used for locking, that takes 2 bits; 4 bits are used by the **garbage collector**; 31 bits for the **identity hash code**; and 32 bits for a **pointer to the class** of the object — and some of the bits are not used. One last word: this is a lot of memory, and not all of it is actually used — some objects never get an identity hash code, and most of them are never used for locking. That's the goal of the **Lilliput** project: to compress things and **reduce the size of this header**. *(Another video.)*

```text
Header (64 bits, compressed klass pointer):
| monitor 2b | GC 4b | identity hash 31b | unused | klass ptr 32b |
```

---

### Question 318 — What is data alignment?
· [video](https://youtu.be/4lhyo1Ce-GQ) · local file `081`

**Short answer:** Something that has to do with storing elements at specific addresses in memory, to read them efficiently.

**Less short answer:** Most CPUs require your data to be **aligned** in a specific way in memory. Your shorts — two bytes — should be aligned on addresses that are **multiples of two**; your ints — four bytes — on addresses that are multiples of four; and so on. Any short, int or long that does not comply is said to be **not properly aligned, or misaligned**. It does not mean that you cannot read it anymore, of course — it only means that you may need **two CPU cycles instead of one** to read it. One last word: it depends on where your CPU communicates with your memory; a safe approach would be to make sure — to always align your data properly. But dealing with **misaligned data is always possible** — this can be configured if you use the Memory API to read this data. *(Another video.)*

```text
an aligned int    @ address 100 (a multiple of 4): 1 read cycle
a misaligned int  @ address 102: spans 2 words -> 2 cycles
```

---

### Question 319 — What is a reference?
· [video](https://youtu.be/PrKuyfPIXzo) · local file `080`

**Short answer:** That one is actually a trick question.

**Less short answer:** Every variable in your Java code that is not of a primitive type is a **reference variable**: a reference to an object. It is implemented by a **pointer** that contains an address — but it is **not itself an address**. If we had a `sizeof` method in Java, the size of any reference variable would be... the memory needed to store the **pointer** (4 or 8 bytes depending on your machine) — not the size of the object this reference is pointing to. We do not have pointers in Java — only references — and, of course, no possibility to **add anything to a pointer**, as is the case in C or C++. One last word: as you know, if you compare two reference variables with `==`, what you're comparing are the **values of these references**, not the objects that these references are pointing to — so the pointers are still there, just under the surface. This may change with value types and Valhalla. *(Another video.)*

```java
String a = new String("x");
String b = new String("x");
a == b;        // false: compares the REFERENCES (2 different addresses)
a.equals(b);   // true: compares the objects
// does not exist in Java: a + 1 (pointer arithmetic)
```

---

### Question 320 — What is a wrapper class?
· [video](https://youtu.be/GwxL37n7hfg) · local file `079`

**Short answer:** A class that wraps a primitive type.

**Less short answer:** Six of them are extensions of the `Number` abstract class — Byte, Short, Integer, Long, and then Float and Double; the two others are not — Character and Boolean. They are all very convenient, as they carry **utility methods** — for instance, to parse strings of characters, or to check for some properties of the wrapped value — and they also define **constants**: MAX_VALUE and MIN_VALUE for the integer types; TRUE and FALSE for Boolean; and the floating point numbers also have `NaN` — which stands for *not a number* — POSITIVE_INFINITY, NEGATIVE_INFINITY, and some others. They also have a `TYPE` constant, that returns the `Class` of the primitive type — used in the Reflection API. One last word: there is no plan to add any more primitive types to the Java language — but with Valhalla and value types, there will be more wrappable types, like a `Float16`, that is already there as a preview feature.

```java
Integer.parseInt("42");         // parsing utilities
Integer.MAX_VALUE;              // constants
Double.isNaN(x);                // properties
Character.isLetter('a');
Integer.TYPE == int.class;      // for reflection
Float16.valueOf(1.5f);          // preview (Valhalla)
```

---

### Question 321 — How can you insert a list into another list?
· [video](https://youtu.be/8aHJg0mqa4k) · local file `078`

**Short answer:** You can use `subList()` for that.

**Less short answer:** subList is a method from the List interface, which gives you a sublist from a list — it takes two arguments: a beginning index and an ending index. Another point: this sublist is a **view** on the first list, and this view is **modifiable**. Calling `addAll` on this sublist will **insert** the collection you pass into the main list, right at the position of the end of the view. If you're using an ArrayList — as you should — you should also favor a **single addAll** over several add calls: each of them triggers a `System.arraycopy` on the internal array. One last word: this pattern also works with `clear()`, that can delete a **range** of elements in a list.

```java
List<String> l = new ArrayList<>(List.of("a", "d"));
l.subList(1, 1).addAll(List.of("b", "c")); // inserts at index 1
System.out.println(l);                     // [a, b, c, d]

l.subList(0, 2).clear();                   // erases the range [0, 2)
System.out.println(l);                     // [c, d]
```

---

### Question 322 — What is polymorphism?
· [video](https://youtu.be/Yu4BivG4dV4) · local file `077`

**Short answer:** One of the principles of object-oriented programming.

**Less short answer:** Inheritance makes it so that, when you call an instance method on an object, the **real** method that your program is going to use can only be known at **runtime**, and not at compile time. Why? Because at runtime, you can pass an **extension** of the declared class — with new code that **overrides** the method you are calling, and that is not known by the compiler. This overriding method is the one that should be called; so the method that is executed when you run your application can only be resolved at runtime. This is obvious when you think about **interfaces**, because there you may not have an implementation — but it's also the case for regular classes. One last word: this is for instance methods; a static method can also **hide** another static method, but it is a different mechanism, that is resolved at **compile time**.

```java
Shape s = new Circle(2);   // declared Shape, really a Circle
s.area();                  // which area()? only at RUNTIME -> Circle.area()

static class A { static String m() { return "A"; } }
static class B extends A { static String m() { return "B"; } }
A x = new B();
x.m();                     // "A"! static: resolved at COMPILE time
```

---

### Question 323 — What is a module?
· [video](https://youtu.be/Qr9dRAXIvoo) · local file `076`

**Short answer:** A notion that is very useful — and that you probably don't use.

**Less short answer:** Modules were added in Java 9, in 2016. A module is a set of classes, and other things, that can live in a jar once compiled, and that has a **descriptor** — in the form of a `module-info.java` file. This descriptor defines, among other things, the modules this module **depends on**, and **which classes it exposes** to the outside world. This point is very interesting, because it adds a **level of protection** for your classes: you can choose the ones you need to make public, and the ones you prefer to keep private. So if you're a framework developer, you can change the internals **without breaking your users' application** — they just can't see these classes; super convenient. One last word: the nice thing with this system is that it's **completely optional** to use: you can stick to the classical class path, and continue writing your application as if it was not there. Backward compatibility is preserved, as it should be.

```java
module com.acme.payments {
    requires java.net.http;            // explicit dependencies
    exports com.acme.payments.api;    // only this is visible outside
    // com.acme.payments.internal -> invisible to the others!
}
```

---

### Question 324 — What is a ServiceLoader?
· [video](https://youtu.be/ZmewAZiQ1W0) · local file `075`

**Short answer:** A service that can tell you what implementation was loaded for any given interface. *(in practice: the implementation-discovery mechanism)*

**Less short answer:** If you build a jar with an implementation of an interface — like your JDBC driver, for instance — you can add a **declaration** in this jar, telling that you're providing this implementation. This declaration is a simple **text file**, that sits in the `META-INF/services` directory of your jar file: its **name** is the full name of the interface you implement, and its **content** is the name of the class that implements that interface (note that you can put **several class names**, if you have more than one). Then you can use the `ServiceLoader.load(MyInterface.class)` method, passing it the interface you want: it returns a ServiceLoader object, that can give you an **iterator** or a **stream**, to find the implementations you're looking for. One last word: this mechanism was added in **Java 6** — that was a long time ago! You can check the jars as examples, to see how this ServiceLoader is working.

```text
META-INF/services/com.acme.Repository
-------------------------------------
com.acme.PostgresRepository
com.acme.MongoRepository
```

```java
ServiceLoader<Repository> loaders = ServiceLoader.load(Repository.class);
loaders.stream().forEach(p -> System.out.println(p.type()));
for (Repository r : loaders) { ... }   // the iterator, too
```

---

### Question 325 — What are the different kinds of arenas?
· [video](https://youtu.be/K3UCtCeHyEI) · local file `074`

**Short answer:** There are four of them.

**Less short answer:** An arena is the object from which you can create memory segments in the Memory API — and you can construct **four types of arenas**, with the four factory methods on the Arena interface: the **global**, the **auto**, the **confined**, and the **shared** arenas. The global arena **shares the life cycle of your application**; the auto arena can be garbage-collected — but as for the global arena, calling `close()` on it **throws an exception**. The confined and the shared arenas **can be closed** by calling close() on them, which closes the memory segments you created with them. And of all these four arenas, the **confined arena is the only one that cannot be shared** among different threads — meaning that the memory segments you create from it can only be used in the thread that created this arena. One last word: if you're just exploring the API, writing some code you will throw away after that, you can safely use the auto or the global arena; but for your application, you should favor the **confined** arena — which is the easiest one to close — or the **shared** arena, if you need the multi-thread access; but in that case, closing it will take longer.

```java
Arena.ofGlobal();   // lives with the JVM; close() throws
Arena.ofAuto();     // released by the GC; close() throws

try (Arena a = Arena.ofConfined()) {  // deterministic close; 1 thread
    MemorySegment s1 = a.allocate(1024);
}
try (Arena a = Arena.ofShared()) {    // multi-thread; slower close
    MemorySegment s2 = a.allocate(1024);
}
```

---

### Question 326 — How can you format a date?
· [video](https://youtu.be/lA7ZodMPXcA) · local file `073`

**Short answer:** There are patterns for that.

**Less short answer:** There is the `SimpleDateFormat` in java.text, that extends Format — which is the **legacy** way of formatting dates. This is the class that carries the JavaDoc for these **date and time patterns**, based on the use of y, M, d, and other letters that you never know how they work. **Be careful**, because this class carries a **mutable state** — so trying to put an instance of it as a constant in a class, so that you can reuse it, is a mistake that will give you **race conditions**. The Date and Time API from Java 8 has its own class to format and parse dates and times, called `DateTimeFormatter`: it is **non-modifiable**, so it can be safely shared between threads. One last word: it also uses these "horrible" y-M-d letter-based formats — but it also provides a bunch of **standard formats** that follow the ISO recommendations.

```java
// legacy: mutable — do NOT share between threads
var sdf = new SimpleDateFormat("dd/MM/yyyy");
sdf.format(new Date());

// modern: immutable and thread-safe
DateTimeFormatter fmt = DateTimeFormatter.ofPattern("dd/MM/yyyy");
LocalDate.now().format(fmt);                    // 04/10/2026
LocalDate.parse("04/10/2026", fmt);
DateTimeFormatter.ISO_LOCAL_DATE;               // 2026-10-04 (ISO standard)
```

---

### Question 327 — How is the Collectors.mapping() working?
· [video](https://youtu.be/o9z54Z8w_Oo) · local file `072`

**Short answer:** This is the factory method from the `Collectors` factory class, that creates a collector that can do a **mapping** — and that needs to push the result to **another collector**.

**Less short answer:** Mapping is an intermediate operation of the Stream API — and a collector is used in a **terminal** operation; so this factory method needs a **downstream collector**, which will receive the result of the mapping of the elements pushed by the upstream. Of course, this downstream collector can itself be a **composition** of more collectors — but you need to be careful there, because composing too many collectors may lead to some code that is **hard to read** and harder to understand. One last word: collector composition is a **very powerful** feature of the Stream API — you can use it when you build maps with groupingBy, for instance, to create the exact **histogram** you need to analyze your data.

```java
// group cities by country, mapping to the name (a composed downstream)
Map<String, List<String>> byCountry = cities.stream()
        .collect(Collectors.groupingBy(
                City::country,
                Collectors.mapping(City::name,
                        Collectors.toList())));
```

---

### Question 328 — How can you get the content of a directory?
· [video](https://youtu.be/zFhDqY80ljw) · local file `071`

**Short answer:** There are several patterns for that.

**Less short answer:** There is a **legacy** pattern available on the `File` class: it consists of several methods — `list()` or `listFiles()` — that return arrays of strings (the file names), or arrays of Files. You can also give a **FilenameFilter** object — which can be a lambda, by the way — that will filter the elements of the file directory before adding them to the result. But if you need to visit **sub-directories**, you need to do that by hand. The preferred patterns are on the **`Files` factory class**: the **`walkFileTree(dir, visitor)`** method takes a **file visitor** as a parameter, that is called for every element visited — and that can visit sub-directories; and the **`walk(dir)`** method returns a **stream of paths**, that contains all the elements of these directories — and which can also be used to visit sub-directories. One last word: you should always prefer the patterns from the Files class over the File class — they are based on the use of the `Path` interface, that supports different file systems, among other things; the File class is mostly a wrapper on a string of characters, with fewer possibilities than the Path interface.

```java
// legacy
File dir = new File(".");
dir.listFiles((d, name) -> name.endsWith(".txt"));

// preferred: a stream of paths, including sub-directories
try (Stream<Path> s = Files.walk(Path.of("."))) {
    s.filter(p -> p.toString().endsWith(".txt"))
     .forEach(System.out::println);
}
```

---

### Question 329 — What is the difference between a value and an object?
· [video](https://youtu.be/lT1qwWPsswg) · local file `070`

**Short answer:** A value is a value; an object is an object. *(the presenter's deliberate tautology)*

**Less short answer:** A **value** is something that is inherently **unmodifiable** — and, as a consequence, can be defined by a name that fully qualifies it. For instance, **one is a value**: its name qualifies it — and by the way, this name is not related to the object. If you assign this value to a variable, then you can change the value of this variable to **two** — it changes the value **of your variable**, but neither one nor two change. A value can safely be **flattened and inlined** where you use it. Whereas an **object** is something that can carry a **state** — this state can be defined by a set of possible values, and can be modifiable; in that case, if your code needs to check this state, it needs to **read a specific piece of memory**: no inlining, no flattening, no scalarization. An object can also have a **behavior**, in the form of methods — as well as a value, by the way: in Java, primitives do not have behavior, but wrappers do. One last word: if an object **cannot be modified**, then it may define a value that can be inlined, scalarized or flattened — and this is exactly what **Valhalla** is doing.

```java
int one = 1;              // a value: inlinable, no identity
one = 2;                  // the variable changes; the values 1 and 2, never

var list = new ArrayList<>();  // an object: mutable state, identity,
list.add("x");                 // an address in memory, no inlining
```

---

### Question 330 — How is flatMap() working?
· [video](https://youtu.be/j4n3I792X_k) · local file `069`

**Short answer:** Very well. But you can do better. *(the presenter's joke)*

**Less short answer:** flatMap is an intermediate method from the Stream interface. It works like mapping, except that the **mapping function needs to return a stream**, instead of any object: so each element of the upstream is mapped to its **own stream**, by the flat-mapping function — and then all these streams are **flattened** in the resulting stream. flatMap can be used to **concatenate streams**, and is actually more efficient than `Stream.concat()`, that you should use with caution. One last word: you can also implement the flat-mapping feature with the **`mapMulti()`** method — in that case, you do not create any **intermediate streams** (the ones that are flattened); and in some cases, it can be more efficient to use this method.

```java
Stream.of("java rocks", "streams top")
      .flatMap(s -> Arrays.stream(s.split(" ")))
      .forEach(System.out::println);   // java rocks streams top

// the alternative without intermediate streams:
      .mapMulti((String s, Consumer<String> down) -> {
          for (String p : s.split(" ")) down.accept(p);
      })
```

---

### Question 331 — What is a memory layout?
· [video](https://youtu.be/m6t01pHhviE) · local file `068`

**Short answer:** An API from the Memory API.

**Less short answer:** A memory layout is an API that allows you to **describe how your data is organized** — in a file, for instance, or in the off-heap memory of your application. Think about the **structs** you have in C, for instance, where you state that a color is made of opacity, cyan, magenta, yellow, and black — and that a pixel is made of two coordinates and a color. Well, the Memory Layout API gives you the elements you need, so that you can describe this kind of thing in Java. You can create these memory layouts **by hand**, or use a tool for that, called **jextract**: jextract can analyze a `.h` file that contains a C struct, and **generate the corresponding memory layout objects** for you — plus, it will generate the **var handles** that you need to read and write these struct-like elements in your memory. One last word: once you have these memory layout objects, you can use them to create var handles — precisely to access this data in files, or in the off-heap memory of your application. *(Another video.)*

```java
// C struct: struct color { unsigned char opac, cyan, magenta, yellow, black; }
StructLayout COLOR = MemoryLayout.structLayout(
        ValueLayout.JAVA_BYTE.withName("opac"),
        ValueLayout.JAVA_BYTE.withName("cyan"),
        ValueLayout.JAVA_BYTE.withName("magenta"),
        ValueLayout.JAVA_BYTE.withName("yellow"),
        ValueLayout.JAVA_BYTE.withName("black"));

VarHandle opac = COLOR.varHandle(MemoryLayout.PathElement.groupElement("opac"));
opac.set(segment, (byte) 255);
```

---

### Question 332 — How is the string hash code computed?
· [video](https://youtu.be/UEkmf8cTyYU) · local file `067`

**Short answer:** It is a polynomial: the coefficients are the characters of the string, the degree is the length of the string, and it is computed for the value 31.

**Less short answer:** Why 31? Well, because it's a **prime number** — and because 31 − 2 = 29 is also a prime number ("I'm not kidding", says the presenter). But don't worry: there is an **iterative algorithm** to compute that — `h = 31h + c` — and if the string is too long, there is even an implementation that leverages the **SIMD** of your CPU. In a nutshell, this hash code is quite **expensive to compute** — fortunately, a string is non-modifiable, so this hash code is computed **once**, when you first need it, and then **cached** for later use. One last word: unfortunately, this caching makes the String class "modifiable" — because it has two fields: the hash code, and a `hashIsZero` flag — and for that, and probably other things, the String class **cannot be a value class** as it is now.

```java
// s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1], computed as:
int h = 0;
for (int i = 0; i < s.length(); i++) {
    h = 31 * h + s.charAt(i);   // the JIT optimizes to (h << 5) - h
}
// the value is cached in the private hash field (String is immutable)
```

---

### Question 333 — How is the Collectors.partitioningBy() working?
· [video](https://youtu.be/BnpSAVpLkC8) · local file `066`

**Short answer:** It partitions your data on a predicate, and puts the result in a map.

**Less short answer:** Collectors.partitioningBy is a factory method of the `Collectors` factory class, that builds a map with keys that are **booleans**. It is built on a **predicate**, that is evaluated for all the elements that your stream has: the element is put in a map where the keys are the two booleans, true and false. So, once your data is processed, you can call `map.get(true)` or `map.get(false)`, to get the two partitions. This is useful when you need to **filter your data, but still need to work with the elements that were rejected** by your predicate. One last word: there is **no overload** of this method that would take a **map supplier** to create the instance of the map — as you have on other factories from the Collectors factory class, like groupingBy or toMap; for partitioningBy, what you get is an implementation specific to `Map<Boolean, List<T>>`.

```java
Map<Boolean, List<Integer>> parts = Stream.of(1, 2, 3, 4, 5, 6)
        .collect(Collectors.partitioningBy(n -> n % 2 == 0));
parts.get(true);   // [2, 4, 6]
parts.get(false);  // [1, 3, 5]  <- the rejected ones stay reachable
```

---

### Question 334 — What is the identity hash code?
· [video](https://youtu.be/6Vqy8xUx6zY) · local file `065`

**Short answer:** An int, that is the identity of this object.

**Less short answer:** The `System.identityHashCode()` static method is a **native** method — meaning that the implementation is not written in Java. It guarantees that, for a given object, it will **always return the same value** — meaning that this value can be used in the same method; it cannot be created from the fields of this object, since they can be mutated. The value of this identity hash code has to do with the **address where this object is stored** in the heap memory of your application — which is a little more complicated than it seems, because the garbage collector would probably like to **move it around** while doing its job. One last word: with Valhalla, instances of **value classes do not have an identity** — so this method will throw an exception in that case.

```java
Object o1 = new Object();
Object o2 = new Object();
System.identityHashCode(o1) == System.identityHashCode(o1); // true (stable)
System.identityHashCode(o1) == System.identityHashCode(o2); // almost never
// the basis of IdentityHashMap (compares by identity, not equals)
```

---

### Question 335 — How can you use Stream.peek()?
· [video](https://youtu.be/_leDdcjP7S0) · local file `064`

**Short answer:** With moderation. *(the video jokes about it)*

**Less short answer:** Unless you use it to **debug your stream processing, stay away from this method** — it's even written in the JavaDoc: this method mainly exists to support debugging. It takes a Consumer, and returns the object processed by the stream. If you are doing **side effects** in this method, and you are on a parallel stream, you are **on your own** when it comes to synchronizing your data — and if you do that, you will probably lose all the benefits of going parallel. One last word: stick to the documentation — if you want to understand the objects that are flowing through your stream, this method is useful; but don't use it in a production environment.

```java
Stream.of("a", "b", "c")
      .peek(s -> log.debug("before the filter: {}", s))  // debug only!
      .filter(s -> !s.equals("b"))
      .forEach(System.out::println);
```

---

### Question 336 — What is the singleton?
· [video](https://youtu.be/J556rmDhh50) · local file `063`

**Short answer:** A pattern from the GoF.

**Less short answer:** In a nutshell, the singleton is implemented by classes that can have **only one instance** — and implementing a singleton correctly is actually not that simple. There are **two constraints** on a singleton. First, you want to **initialize them lazily** — only if they are needed, and when they are needed; why? Because in large applications, you may have many singletons — and if you decide to initialize all of them before your application is available for doing anything, then you may have to wait for some time. And second, you want this initialization to be **thread-safe** — and if it's not handled correctly, you may end up with **several instances instead of one**, which could be a problem. So people came up with — same with many — many solutions to implement singletons correctly; but in the end, you need **one class per singleton**, which is not that great. One last word: implementing a singleton with an **enumeration** is probably, as of now, your best bet — avoid this horrible double-checked locking pattern; and keep an eye on the **lazy constants** that are coming to the JDK — that could be the solution you need. *(Another video.)*

```java
enum ConnectionPool {             // the idiomatic singleton (Effective Java)
    INSTANCE;
    private final Pool pool = Pool.create();
    Pool get() { return pool; }
}
// usage: ConnectionPool.INSTANCE.get()
```

---

### Question 337 — What is double-checked locking?
· [video](https://youtu.be/3oXTQA-On1w) · local file `062`

**Short answer:** Something that looks like a good idea — but that is not that great.

**Less short answer:** It is a complex pattern, error-prone — and odds are that **you don't need it**. This pattern was first created to implement the **Singleton** pattern: it can initialize your singleton **lazily and in a safe way** — but the code you need to write for that... it's just horrible; and performance-wise, it's not so great: your field needs to be `volatile`, meaning that every time you read it, there is a **check to make sure that it has not been modified by some other thread** — which, by the way, is not supposed to happen. One last word: using an **enumeration** was the solution proposed by the excellent book *Effective Java*, written by Joshua Bloch — and it is still the simplest way to create singletons in your application; it is used in the JDK itself — you can check the `Comparator.naturalOrder()` factory method, for instance.

```java
// double-checked locking: it works, but... (a volatile field is mandatory)
class Config {
    private static volatile Config instance;
    static Config get() {
        if (instance == null) {
            synchronized (Config.class) {
                if (instance == null) instance = new Config();
            }
        }
        return instance;
    }
}

// the Effective Java way: an enum — simple and bulletproof
enum ConfigHolder { INSTANCE;
    static final Config CONFIG = new Config(); // lazy at first class-init
}
```

---

### Question 338 — What is the string pool?
· [video](https://youtu.be/Cb4tr78qdZc) · local file `061`

**Short answer:** A pool of strings. *(the short answer of someone who doesn't want to spoil it)*

**Less short answer:** Strings of characters may take a lot of space in your memory — and since you **cannot change the value** of a string of characters during its lifetime, it may be an optimization to try to **cache them**. You can do so by calling the `intern()` method on the string: it returns a string — calling this method will **check if an instance already in the pool has the same value** as your string; if it's not the case, then it will add it — and in both cases, it returns the instance that is in the pool. In the end, you may **save memory** by doing that: no duplication is possible in a pool. Putting **all your strings** in a pool may sound like something you should be doing — actually, you need to be a little more cautious than that: being a string in a pool is interesting **only if you have many references to it**; if it is a specific string that could be garbage soon — don't put it in a pool. One last word: you need to be **very cautious** when it comes to optimization or possible performance gain: whatever you do, you need to carefully **observe the effects on your application in a production environment**.

```java
String a = "hello";                  // a literal: already goes to the pool
String b = new String("hello");      // a new instance, outside the pool
a == b;                              // false
a == b.intern();                     // true (intern returns the pool instance)
```

---

### Question 339 — What is the off-heap memory?
· [video](https://youtu.be/sJlXbQvgU-4) · local file `060`

**Short answer:** A portion of memory that is **not** in the heap memory.

**Less short answer:** The heap memory is the realm of the **garbage collector**: the GC can give you pieces of this memory, and then, when you don't need them anymore, it will take them back — in a nutshell, that's what a garbage collector is doing. The **off-heap memory is not managed** by the garbage collector; to get pieces of it, you need to use the old **ByteBuffer** API — or better, the **Memory API**. You can get **very large segments** in it, that will **not be moved around** by the garbage collector activity — so it's great to **map files in memory**, for instance. One last word: the ByteBuffer API is an API from Java 4, that was in 2002, and it can give you access to the off-heap memory — it was **superseded in Java 22 by the Memory API**, which is safer to use, and can index the memory segments with **long** instead of int for the ByteBuffer: much better for 64-bits systems.

```java
// off-heap via FFM (Java 22+): a large segment, outside the GC
try (Arena arena = Arena.ofShared()) {
    MemorySegment file = mapInMemory(arena, Path.of("data.bin"));
    long size = file.byteSize();   // long indexes (ByteBuffer: int)
}
```

---

### Question 340 — Why don't stateful operations play well with parallel streams?
· [video](https://youtu.be/p_NSLrJM--Y) · local file `059`

**Short answer:** Because you need to **share the state** between the multiple threads.

**Less short answer:** Some stream operations are said to be **stateful** — that's the case for `limit` or `skip`, for instance. Limit needs to **interrupt your stream** after a given amount of elements — and if your stream is ordered, you need to be able to get the **N first elements** of the upstream, not N elements randomly chosen. Even if your stream is not ordered, it still needs an **internal counter that is shared** among the different threads that are computing your stream in parallel — and this **added synchronization** can only slow down your computation, something you probably want to avoid. One last word: you need to be careful when you use parallel streams in general — a parallel stream will use **all the cores of your CPU** to conduct the operations, and may slow down your other processes. When it comes to performance, as usual: **measure, don't guess**.

```java
IntStream.range(0, 10_000_000).parallel()
         .limit(100)            // stateful: a counter shared between threads
         .toArray();            // synchronization that only slows things down
```

---

### Question 341 — How does dropWhile() work?
· [video](https://youtu.be/-pS-LUWMU1Y) · local file `058`

**Short answer:** It acts like a door that is **closed at first**, and that opens on a predicate.

**Less short answer:** dropWhile is an intermediate method of the Stream API; it takes a **predicate** as a parameter, that is evaluated for the elements of the stream. While the predicate results in **false**, no element is pushed to the downstream; when the predicate results in **true**, then the door opens — meaning that **all the remaining elements** are pushed to the downstream, and the predicate is **not evaluated anymore**. If your stream is ordered, then dropWhile drops the **first elements** of your stream; but if your stream is not ordered, then this notion of "first element" is not defined — so the results may vary: the implementation can actually **drop any subset** of your source. One last word: dropWhile is a **stateful operation** — meaning that it does not play well with parallel streams, especially if your stream is ordered; actually, using dropWhile on an ordered parallel stream will **probably slow down** your operation — so it's probably not a good idea.

```java
Stream.of(1, 3, 5, 6, 7, 2, 4)     // ordered
      .dropWhile(n -> n % 2 == 1)   // drops 1, 3, 5 and STOPS testing at 6
      .forEach(System.out::print);  // 6 7 2 4  (the "oddness" of 7 isn't even tested)
```

---

### Question 342 — What is String deduplication?
· [video](https://youtu.be/iJbcVL8NCv0) · local file `057`

**Short answer:** A feature that may save some memory in your application.

**Less short answer:** String deduplication consists in **detecting if two strings of characters have the same value** in your application — and then creating **only one instance of their internal arrays**. We can do that because strings are immutable: once you have created a string, you cannot change its value. This deduplication feature was added to the **G1** garbage collector back in the days of **Java 8** — and deduplication actually operates on the **byte array** of the string, not on the String object itself. And because there is some CPU time involved, deduplication is **attempted only on strings that have reached a certain age**. One last word: **ZGC** also supports string deduplication, since Java 18 — you need to add the option `-XX:+UseStringDeduplication` to activate it.

```bash
java -XX:+UseStringDeduplication -XX:+UseG1GC MyApp
# equal strings now share the same internal byte[]
# (typical savings: heaps with many repeated strings)
```

---

### Question 343 — What is the difference between a wrapper class and a value-based class?
· [video](https://youtu.be/VLRzyHEJSQs) · local file `056`

**Short answer:** They are not the same thing.

**Less short answer:** A wrapper class **is** a value-based class — but not the contrary. A wrapper class is a class that wraps a primitive type (`Integer` for `int`...), and they have been in the JDK since the beginning — and that's all there is to it. A **value-based class** is a notion that is coming from **Valhalla**, and that appeared in the JDK. Technically speaking, it is an **annotation** added on some classes — wrapper classes have it, but also some classes from the Date and Time API, for instance, or `Optional`, and even some non-modifiable collections are annotated with value-based. This annotation triggers some **warnings at compile time**, if you do things with these classes that will fail once Valhalla is there — like using their instances **for synchronization**, for instance. One last word: this annotation is **internal to the JDK** — you can see it if you check the source code, but you cannot use it for your own classes.

```java
Integer n = 42;                        // a wrapper AND value-based
Optional<String> o = Optional.empty(); // value-based
LocalDate d = LocalDate.now();         // value-based

synchronized (n) { ... }   // warning: will break with Valhalla (value types)
if (n == anotherN) { ... } // warning: identity is not reliable
```

---

### Question 344 — How does the Collectors.teeing() work?
· [video](https://youtu.be/bdA4l8YQs3I) · local file `055`

**Short answer:** The teeing collector is a rather complex collector, added in JDK 12.

**Less short answer:** It consists in **collecting your stream with two different collectors**, that will produce two different results — and then **merging these results** with a merging function. These two collectors are the first two parameters of `teeing()`, and you need to know your Collector API pretty well to write them. That being said, this collector is very useful when you need to **apply two different algorithms on the same stream without going through it twice**. One last word: avoiding the streaming of your data twice is especially useful when your streaming can only be done **once** — like your streaming an IO, for instance — and you need to avoid buffering everything in memory; in that case, the teeing collector may be extremely useful.

```java
// min and max in a single pass (without traversing twice)
var minMax = Stream.of(3, 1, 4, 1, 5, 9, 2, 6)
        .collect(Collectors.teeing(
                Collectors.minBy(Integer::compareTo),
                Collectors.maxBy(Integer::compareTo),
                Map::entry));               // merges the two results
minMax.getKey();   // Optional[1]
minMax.getValue(); // Optional[9]
```

---

### Question 345 — How many objects are there in a stream?
· [video](https://youtu.be/Ux_RvBNau3w) · local file `054`

**Short answer:** *(straight to the point)* None.

**Less short answer:** The stream is a **pipeline** that pulls objects from the source, and pushes them to the downstream — or terminal operation. So the rule is: **there is no object in a stream**. That being said, some streams are managing an **internal mutable state**, that may be storing the elements it consumes for processing — that's the case of the `distinct` operation, that stores the elements of the stream in a set, and the `sorted` operation, that needs to store them before sorting them. But mapping, filtering, flat mapping — all these regular operations **do not store any object**. One last word: this feature makes the Stream API very powerful: the stream can consume a **reasonable amount of memory**, while staying able to process **very large amounts of data** — much larger than what you could store in the memory of your application.

```java
Files.lines(path)                    // GBs of data...
     .filter(l -> l.contains("ERROR"))
     .map(String::trim)
     .limit(10)
     .forEach(log::error);           // ...with a tiny memory footprint
```

---

### Question 346 — How can you create a pre-filled list?
· [video](https://youtu.be/tnJ_0cnhg5Q) · local file `053`

**Short answer:** There is a method for that.

**Less short answer:** You can just call `List.of(...)` and pass the elements you need in that list. Just be aware that there are **restrictions** on the implementation you get. First, you **cannot have null values** in this list — why would you put null values in this list? And then, the list you get is **non-modifiable**. The implementations you get are specific to these factory methods, and optimized in several ways, so that the operations on these lists are fast — and by the way, the implementations you get are also **serializable**. One last word: this List.of factory method was added in **JDK 9**, as part of the *convenience factory methods for collections* feature — so there is an equivalent method for `Set`, and — even if it's not mentioned in the name of the feature — also for `Map`.

```java
List<String> colors = List.of("blue", "green", "red");
Set<Integer>  evens = Set.of(2, 4, 6);
Map<String, Integer> stock = Map.of("pen", 12, "pencil", 30);
// colors.add("yellow");  -> UnsupportedOperationException
// nulls are rejected at creation time
```

---

### Question 347 — What is hashcode collision?
· [video](https://youtu.be/20gOHclBO9g) · local file `052`

**Short answer:** Something that can happen when two **different** objects have the same hashcode.

**Less short answer:** From a technical point of view, a hashcode is an `int` encoded in **32 bits** — and you can have way more different objects than 2³², so there **has to be collisions**. By the way, that's the reason why the specification says that **two objects that are equal need to have the same hashcode** — but the opposite is not true; in fact, it cannot be true. Having hash collisions can have unexpected effects on your application, including bad ones: for instance, if you store your objects in a set, they are actually added to an **internal map**, where the keys are the hashcodes of these objects; in case of **many collisions**, calling `contains`, for instance, may not be an O(1) operation anymore — but an **O(log n)** (after the tree transformation). Not that bad, but still not as fast. One last word: one more reason to avoid parallel streams on sets — if you have too many collisions, your set may **not be split properly**, and going parallel may **hurt your performance** instead of improving it.

```java
record Point(int x, int y) { }         // a 32-bits hashcode
// a collision: two different points, same hash -> the same bucket
new Point(1, 2).hashCode() == new Point(2, 1).hashCode(); // possible
// in a HashSet: the bucket becomes a list -> a tree if they pile up (>8)
```

---

### Question 348 — What is dependency inversion?
· [video](https://youtu.be/Q06kGqyY9B8) · local file `051`

**Short answer:** "Inversion" means that the dependency between two modules, for instance, **at compile time, is the opposite of the dependency at runtime**.

**Less short answer:** You may be wondering: how is this even possible? Well, it's actually very simple — and very common too. If you have a module A that uses some classes of a module B, then A depends on B at compile time, and also at runtime. If you want to make **B depend on A at compile time**, then what you need to do is: create **an interface in A**, with what A needs from B — this interface can use **records** to transport the data from B to A — and then B needs to **implement this interface**. Now, B depends on A. This is exactly how the **JDBC** API works, for instance: most of what it contains are **interfaces**, implemented by drivers that are specific to the database you're using. One last word: the nice thing with this approach is that, when you need to **update B** for whatever reason, **there is no need to recompile A** — why? Because A does not even know B. One less pain point in your application.

```java
// Module A: defines the interface (and the transport records)
interface OrderRepository { void save(OrderRecord o); }

// Module B: implements A's interface (the inverted arrow)
class PostgresOrderRepository implements OrderRepository { ... }
// swapping/recompiling B does not affect A
```

---

### Question 349 — What is the first thing a constructor does?
· [video](https://youtu.be/6oEA9XRaOnk) · local file `050`

**Short answer:** It calls another constructor.

**Less short answer:** You can issue this call manually — which is handy if you need to call a specific constructor — either from the same class with `this(...)`, or from the superclass with `super(...)`. And if you do not write this code yourself, then a call to `super()` — the no-argument super constructor — is **added for you by the compiler**; the reason why, if your superclass does not have a no-arg constructor, you need to call one by hand, or suffer a compiler error. And if you don't write any constructor at all, then the compiler adds one for you, that does not take any argument, and that just calls super. One last word: you **cannot prevent the execution of the superclass constructors** — and it can be an overhead if, in the end, you decide not to create this object, because the arguments you got were not valid... well, until now: this changes in **JDK 25 — you can now call super whenever you want** (JEP 513), validating your arguments before. *(Another video.)*

```java
class Base { Base() { ... } Base(String cfg) { ... } }

class Child extends Base {
    Child() {
        this("default");      // calls a constructor of the SAME class
    }
    Child(String cfg) {
        super(cfg);           // calls a constructor of the SUPERCLASS
    }                         // (unwritten, the compiler inserts super())
}
```

---

### Question 350 — What is the JavaDoc?
· [video](https://youtu.be/4nbqPLV8Sh0) · local file `049`

**Short answer:** A very handy tool to write and render the documentation of your code — because you always write some documentation for your code, don't you?

**Less short answer:** From the beginning, Java thought about documenting all the elements you can write — classes, interfaces, and the like. You can write your JavaDoc in special comments (`/** ... */`), and use special tags to document what a method is doing, what the parameters it takes are and what their uses are, or what exceptions it may throw. You can add some JavaDoc on classes, methods, fields — but also on **packages**, using a special file called `package-info.java` — very handy. All the JDK JavaDoc is written in that way — you can check it out for examples. One last word: back in the days, it was written in **HTML** — and it was great, because you could read it from a browser; now you can also write it in **Markdown** — that's a feature from JDK 23. *(Another video.)*

```java
/**
 * Converts currencies.
 * @param amount  the amount to convert
 * @param currency  the target currency
 * @return the converted amount
 * @throws IllegalArgumentException if the currency is not supported
 */
public BigDecimal convert(BigDecimal amount, String currency) { ... }
```

---

### Question 351 — How can you run a parallel stream in a specific pool of threads?
· [video](https://youtu.be/_zqbPNmkxqs) · local file `048`

**Short answer:** There is a pattern for that — a little hidden.

**Less short answer:** As you know, a parallel stream runs in a **common ForkJoin pool** by default — there is only one such pool in your application, and the number of threads is set to the number of cores you have on your CPU. There may be situations where you need to **decrease this number of threads**, because you want to keep some CPU cycles for other things than running your parallel streams — you can do that with the JVM option `java.util.concurrent.ForkJoinPool.common.parallelism`: if you set this property to 3, it will create 3 threads in the common ForkJoin pool of your application. But you can do better: **if you run a stream in a thread that is part of a ForkJoin pool, and this stream is a parallel stream, then it will run in this ForkJoin pool** — and not the common ForkJoin pool. So the trick is to create a **Callable** that will carry your parallel stream computation, and **submit this callable to a ForkJoin pool that you created yourself** — and that's it. One last word: is it a good idea? Well, you need to think about it very carefully — because having a gazillion ForkJoin pools in your application, hammering your CPU like crazy, is most certainly a very, very bad idea. When it comes to performance: **measure, don't guess**.

```java
// a global tuning of the common pool
System.setProperty("java.util.concurrent.ForkJoinPool.common.parallelism", "3");

// the pattern: run the parallel stream inside your own pool
ForkJoinPool pool = new ForkJoinPool(2);
var result = pool.submit(() ->
        list.parallelStream()
            .map(this::transform)
            .collect(Collectors.toList())
).get();
```

---

### Question 352 — What is the difference between a ByteBuffer and a MemorySegment?
· [video](https://youtu.be/SSBGkD7YPeM) · local file `047`

**Short answer:** Oh, not the same thing.

**Less short answer:** ByteBuffer is a class from **Java 4**, designed in 2002, that gives you access to the off-heap memory of your application; MemorySegment is an interface from **Java 22**, from 2024 — that kind of does the same thing. ByteBuffer has **32-bits indexes**; a MemorySegment has **64-bits indexes**. You **cannot free a ByteBuffer yourself** — which may lead to `OutOfMemoryExceptions`, without you being able to do anything about it; a MemorySegment is obtained from an **Arena** — that is auto-closable — so you can free them **on demand**. With MemorySegment, you can **control how different threads access your memory segments** — you can even prevent any other thread than yours to access them (a confined arena); with ByteBuffer, when it comes to concurrency, **you're on your own**. One last word — in case it has not been explicit enough: using ByteBuffers in any place other than **legacy code** is really not a good idea; learn how to use **arenas, memory segments, and memory layouts** — it will be much better for your application.

```java
// Legacy (Java 4): no explicit release, int indexes
ByteBuffer buf = ByteBuffer.allocateDirect(1_000_000);

// Modern (Java 22): a closable arena, long indexes, controlled access
try (Arena arena = Arena.ofConfined()) {
    MemorySegment seg = arena.allocate(1_000_000);
    seg.set(ValueLayout.JAVA_LONG, 0, 42L); // deterministic release at close
}
```

---

### Question 353 — What is a ScopedValue?
· [video](https://youtu.be/ppLigfPXdnA) · local file `046`

**Short answer:** A value that you can pass around in your application, without relying on method arguments.

**Less short answer:** Passing elements from one method to another method, without using method parameters, is something you need when you write **callbacks**. ThreadLocal variables were created exactly for that — to pass elements around (and you can also relate to the other one, that was in Java 2, in 1998: the InheritableThreadLocal). ThreadLocal variables have several issues: they are **mutable**; they are **bound to a thread**; you need to call `remove()` on them when you're done with them — and if you don't, **the next task this thread will execute will have access to it** — something you want to avoid. **ScopedValues** are a replacement, that is **bound to a task** — namely, a Runnable or a Callable — and **not bound to any thread**; you don't need to remove them, because they are not transmitted by the thread — and they cannot escape your method call, unless they were created by a `StructuredTaskScope`. One last word: the API does not give you thread safety — so if you share a **mutable** scoped value among different threads, then you need to manage the synchronization yourself; sticking to **non-modifiable** scoped values is probably your best choice.

```java
private static final ScopedValue<User> USER = ScopedValue.newInstance();

ScopedValue.where(USER, authenticatedUser)
           .run(() -> processRequest());

// inside any callback in the scope:
User u = USER.get();   // no ThreadLocal, no remove()
```

---

### Question 354 — How can you explore a tree of directories?
· [video](https://youtu.be/E5Orh8GI1Z8) · local file `045`

**Short answer:** There is a pattern for that. *(actually, two)*

**Less short answer:** You can use the `Files.list(dir)` method to give you access to all the entries of a directory — you can then filter them with a regular expression, and when you find a directory, **recursively explore this directory**; but using recursion is always a little dangerous — you can get `StackOverflowExceptions` while doing that, something you usually want to avoid. So there is another method, **`Files.walkFileTree(dir, visitor)`**, that takes a **file visitor** as a parameter — it implements the real Visitor pattern: you can give a **callback for every entry that is found**; it visits the content of a directory, and when it finds a subdirectory, **you can tell this method to explore this subdirectory or not**. One last word: you need to keep in mind that a **file system can be modified while you explore** the content of a directory — so you can come across weird errors if it takes too long, competing with other processes accessing your file system; that's another level of concurrency issues.

```java
// 1) manual recursion (careful with StackOverflow)
try (Stream<Path> s = Files.list(dir)) {
    s.filter(Files::isDirectory).forEach(this::explore);
}

// 2) a visitor: fine control, without your own recursion
Files.walkFileTree(Path.of("."), new SimpleFileVisitor<>() {
    @Override public FileVisitResult visitFile(Path f, BasicFileAttributes a) {
        System.out.println(f);
        return FileVisitResult.CONTINUE;
    }
});
```

---

### Question 355 — What is an EnumMap?
· [video](https://youtu.be/nfhkV2MrGbs) · local file `044`

**Short answer:** A map.

**Less short answer:** An EnumMap is a map where the **keys are the values of a single enum** — that allows for a very efficient implementation, both memory-wise and CPU-wise. Since the enumerated values are **known before the map is created**, and these values have an **index** — the ordinal — you can simply store the values in an **array, which size is known upfront** (you will never need to resize this array once it has been created), and using this index makes it so that you **do not need to compute any hash code**. One last word: this map does **not support null keys** — why would you put a null key in an array? But it **does support null values**: trying to add a null key will throw a NullPointerException, but you can still check for the presence, or try to remove a null key, and get false as a result.

```java
enum Season { SUMMER, FALL, WINTER, SPRING }

Map<Season, Double> temp = new EnumMap<>(Season.class);
temp.put(Season.SUMMER, 32.5);
temp.get(Season.SUMMER);       // direct array access, no hashing
temp.containsKey(null);        // false (no exception)
// temp.put(null, 10.0);       // NullPointerException
```

---

### Question 356 — How can you use markdown in your JavaDoc?
· [video](https://youtu.be/Y5RSvfH_RAQ) · local file `043`

**Short answer:** There is a pattern for that.

**Less short answer:** Writing JavaDoc may not be your thing — but it is still a very interesting feature of the Java platform. In 1995, people chose to use **HTML** for the JavaDoc — and it was probably an excellent choice at the time; and somehow, that's still the case: you can easily read your JavaDoc in the browser. But 1995 — that was a long time ago; and nowadays, writing **Markdown** is probably easier to do, and as efficient. To write some Markdown code in your JavaDoc, all you need to do is add this **triple slash `///`** at the beginning of each line. Several Markdown features are supported, including titling and links — which is great. One last word: you can also include **snippets of code** in your JavaDoc, so that the examples you show are **tested during the building process** of your application — another great addition to this old feature. *(Another video.)*

```java
/// Converts a value to a currency.
///
/// ## Example
/// ```java
/// Money m = Money.of("12,34");
/// ```
///
/// [See more](https://docs.example.com)
public class Money { ... }
```

---

### Question 357 — How should you check if a value is null?
· [video](https://youtu.be/enUeUmY4TdQ) · local file `042`

**Short answer:** Use `Objects.requireNonNull()`.

**Less short answer:** We can always write a test with double equal — that will work, of course. But a better pattern is to call **`Objects.requireNonNull()`**: it has three advantages. First, this simple factory method **throws the NullPointerException for you**, explicitly. Second, it can **return the object you pass** — so you can inline this null check with the use of the variable you want to check (assigning it directly to a field, for instance). And third, it can take a **custom error message**, if you want to give context or information to debug the problem. One last word: there is also an **overload** of this method that can return a **default value** instead of throwing the NullPointerException — you can pass this value as an argument, or — if it's expensive to create — pass a `Supplier` to build it (`requireNonNullElseGet`).

```java
this.name = Objects.requireNonNull(name, "name cannot be null");
// throws an NPE with a clear message, and assigns

Config cfg = Objects.requireNonNullElse(currentCfg, Config.DEFAULT);
Config costly = Objects.requireNonNullElseGet(currentCfg, () -> loadFromDisk());
```

---

### Question 358 — Is Collections.unmodifiableList() non-modifiable?
· [video](https://youtu.be/Z2yOZKjGUjs) · local file `041`

**Short answer:** *(the video answers it in practice)* It should be an obvious yes — and in a way, it is.

**Less short answer:** `Collections.unmodifiableList()` returns a list that **you cannot modify** — calling add, remove, clear, and all these kinds of things. But this non-modifiable list is actually a **wrapper on the list you pass as an argument**: **no defensive copy is made**. So if you keep a reference to this list, and modify it, you will see these modifications in the unmodifiable list. One last word: whatever you do, if you need an unmodifiable list built from a regular list, you need to make a **defensive copy** — unmodifiableList is a very good first choice, but you can also use one of the `List.of(...)` / `List.copyOf(...)` patterns.

```java
List<String> original = new ArrayList<>(List.of("a", "b"));
List<String> readOnly = Collections.unmodifiableList(original);
original.add("c");            // the "unmodifiable" list sees the change!
List.copyOf(original);        // a defensive copy: truly immutable
```

---

### Question 359 — How can you use regular expressions?
· [video](https://youtu.be/TQkXONpJ3Rg) · local file `040`

**Short answer:** There are several classes for that.

**Less short answer:** All these classes are in the `java.util.regex` package. First, you need to create a `Pattern` with the Pattern class, using the `compile()` factory method — immutable and reusable. You can use this pattern in two ways. First, you can **split a text lazily** with the `splitAsStream()` method, passing the text: what you get is a stream, that you can analyze with the traditional stream patterns. Second, you can call `matcher()`, and pass a text to match against this pattern: what you get is a `Matcher` object; you can then call `Matcher.find()` and then `Matcher.group()`, to jump from one matching subsequence to the other; calling `Matcher.start()` and `Matcher.end()` gives you the position of this subsequence in the text; and you can also call `replaceAll()` on this Matcher, to replace the matching subsequences with what you need. One last word: the regular expression engine also supports **groups and named groups**, with methods to export them directly — but that will be for another video.

```java
Pattern p = Pattern.compile("\\w+@(\\w+)\\.com");

// 1) a lazy split into a stream
p.splitAsStream("a,b,,c").forEach(System.out::println);

// 2) the classic matcher
Matcher m = p.matcher("contact: john@acme.com, ana@zupper.com");
while (m.find()) {
    System.out.println(m.group() + " domain=" + m.group(1)
            + " pos=" + m.start() + "-" + m.end());
}
```

---

### Question 360 — How can you gather statistics on a stream?
· [video](https://youtu.be/nNmsahCPE9k) · local file `039`

**Short answer:** There is a collector for that.

**Less short answer:** There are actually **three**, depending on the type of statistics you need — you can get them with the **summarizing** factory methods from the Collectors class: one for integers, one for longs, and one for doubles. They return a **summary statistics object** — one for each type — that computes the **count, the sum, the min, the max and the average, in one pass** over your data. One last word: these summary statistics objects are in fact **consumers** — all the statistics are computed in the `accept` method of these consumers; so if you need other statistics than these, copying this pattern is very easy — you just need to create your own summary statistics object, and copy the pattern from one of the factory methods; and it even supports parallelism for free.

```java
IntSummaryStatistics st = Stream.of(2, 7, 1, 9, 4)
        .collect(Collectors.summarizingInt(Integer::intValue));
st.getCount();   // 5
st.getSum();     // 23
st.getMin();     // 1
st.getMax();     // 9
st.getAverage(); // 4.6
```

---

### Question 361 — Should you comment your code?
· [video](https://youtu.be/q5EuaH5n65g) · local file `038`

**Short answer:** No.

**Less short answer:** If you need comments to explain your code, it means that it is **not readable enough** — so instead of writing comments, you should **work on the readability of your code**; that should be your first reflex — and it's much harder to do. **Comments are not documentation**: documentation is written using the JavaDoc — and JavaDoc is of paramount importance: it should be precise, accurate; it should give you examples of how to use your code, and explain what happens if something goes wrong; your JavaDoc should be **proofread during your code reviews**, to make sure it has the right level of quality. Comments, on the other hand, **live their own life** — and your code also lives its own life; but both are not always related: sometimes you fix bugs in your code, or change what it's doing, **leaving the comments as they are** — and at some point, what the comments tell you about your code is **no longer related to what your code is actually doing**; so no one reads them anymore, and they just stay there, **rotting**, polluting your code base. One last word: there are exceptions to this rule — for instance, when you need to give some detail on a **complex algorithm** your code implements; you can check the `ConcurrentHashMap` or the `ConcurrentSkipListMap` classes.

```java
// bad: a comment explaining what the code already says
i = i + 1; // increments i

// good: a readable name; a comment only for the non-obvious WHY
int attemptsLeft = limit - attempts;
// exponential backoff: a requirement of the partner's SLA (contract §4.2)
```

---

### Question 362 — How can you know what thread is running your code?
· [video](https://youtu.be/ERq3DMDaT2Y) · local file `037`

**Short answer:** There is a method for that.

**Less short answer:** The method you need to call is a factory method from the Thread class, called **`Thread.currentThread()`**: it gives you a reference on the thread that is running your code. There are several cases where you need to call this method, apart from debugging or analyzing purposes: if you execute a long-running task, and you need to **manage the interruption of this task yourself**, then you need to monitor the interrupted status of the thread that is executing your task — you can do that by calling `Thread.currentThread().isInterrupted()`; if this method returns true, then you need to **stop your task**. One last word: currentThread returns the platform thread, **or the virtual thread** that is running your task — and in that case, you don't have access to the underlying platform thread.

```java
while (hasWork()) {
    if (Thread.currentThread().isInterrupted()) {  // cooperative cancellation
        cleanup(); return;
    }
    processNext();
}
Thread.currentThread().isVirtual(); // true in a virtual thread
```

---

### Question 363 — How can you generate the JavaDoc?
· [video](https://youtu.be/zLk6_xjaLrI) · local file `036`

**Short answer:** The JavaDoc tool is a tool that you have in the standard JDK distribution — and most of the time, your IDE uses it for you.

**Less short answer:** JavaDoc can analyze all your class files, and will take the so-called JavaDoc comments (`/** ... */`) to generate a set of **HTML pages** that you can distribute along with your application. And, as all the tools available in your JDK, JavaDoc is also available **as a service**, that you can use programmatically — you need to check the `ToolProvider` interface.

```bash
javadoc -d docs -sourcepath src/main/java -subpackages com.acme
```

```java
// calling javadoc programmatically
DocumentationTool tool = ToolProvider.getSystemDocumentationTool();
```

---

### Question 364 — What is the Template Method?
· [video](https://youtu.be/9e3aEapI4B0) · local file `035`

**Short answer:** A pattern from the Gang of Four.

**Less short answer:** The template method pattern is very simple: it is about creating an **abstract class** with an **abstract method** — this abstract method is the template method itself; then you need to **extend your abstract class**, and provide a **specific implementation for the template** — that is used at runtime by all the other methods of your abstract class. Both ArrayList and LinkedList apply this pattern: they extend AbstractList and AbstractCollection. **AbstractCollection has two abstract methods — `size()` and `iterator()`** — and one method that throws an UnsupportedOperationException: `add(element)`. So implementing your own collection is very simple: you just need to extend AbstractCollection, implement size and iterator — and override add if what you want to make is a mutable implementation. AbstractList extends AbstractCollection, and adds **one abstract method — `get(index)`** — and three methods that throw an UnsupportedOperationException: add at index, remove at index, and set at index. Same thing: implementing a list is super simple — just extend AbstractList: you have one method to implement, and up to four to override if you need to make it a mutable implementation. One last word: the same goes for maps — there is one class to extend, AbstractMap, with **one abstract method (`entrySet`)**, and `put(key, value)` to override if you need a mutable implementation.

```java
class MyList extends AbstractList<String> {
    private final String[] data = {"a", "b", "c"};
    @Override public String get(int i) { return data[i]; }
    @Override public int size() { return data.length; }
    // the inherited add/set/remove throw UnsupportedOperationException
}
```

---

### Question 365 — How does the calling of super() work?
· [video](https://youtu.be/HrAhjRfVP5Q) · local file `034`

**Short answer:** It calls the super constructor of your class.

**Less short answer:** Calling another constructor — this is the **first thing any constructor does**; it can be a constructor from the same class, or a constructor from a superclass. This pattern is actually problematic: as you can **access fields from the subclass** in a constructor from a superclass — and these fields may **not be initialized**, even if they are final. So it means that during the construction process of an object, you can observe the transition of a **final field** from its initial null value, to its final value — which kind of defeats the point of being final. **JEP 513** — "statements before super" — changed that: you can now **execute some code before the call to the other constructor** — meaning that you can, and you should, conduct any initialization of your object **before calling the super constructor**. There is a restriction on what you can do: you cannot call any **instance method** before calling this super constructor — eliminating the risk of executing subcode from a subclass. One last word: instead of letting the compiler call the super constructor from the constructor file for you, the good practice is: not to call super explicitly without having initialized your object first — one less place for bugs to hide.

```java
class Base { Base(String config) { ... } }

class Child extends Base {
    private final Config cfg;
    Child(String raw) {
        // JDK 25 (JEP 513): initializing BEFORE super() is possible
        this.cfg = Config.parse(raw);          // was a compile error before
        super(cfg.validatedName());            // super gets ready data
    }
}
```

---

### Question 366 — What is the difference between an ExecutorService and a Fork/Join pool?
· [video](https://youtu.be/czrQeV5ovSQ) · local file `033`

**Short answer:** A Fork/Join pool **is an** ExecutorService.

**Less short answer:** ExecutorService is an interface. The first interface is the `Executor` interface: it defines the **submission of a Runnable**; and the ExecutorService is an interface that **extends Executor**. It supports **Callables** — returning a Future on a submission of a Callable — and defines **two shutdown strategies**. Usually, the implementation of the ExecutorService has **one waitlist**, in which they store the tasks you submit, and a **pool of threads**: each thread takes a task from the waitlist, and executes it. A **ForkJoinPool** has **one waitlist per thread**; and each task is expected to **split itself, spawning at least two other tasks** that are stored in the same waitlist; then it implements the **work-stealing pattern** — meaning that if a thread has an empty waitlist, it can steal a task from another waitlist. This has a cost — when it comes to visibility issues and cache misses — but it's necessary to **keep all your threads busy**. One last word: there are **two ForkJoinPools running in your JVM**: one common pool, that runs your parallel streams — and another one, that runs your **virtual threads**; and they don't work exactly in the same way. *(Another video.)*

```java
ExecutorService ex = Executors.newFixedThreadPool(4); // 1 shared queue
ForkJoinPool fj = (ForkJoinPool) Executors.newWorkStealingPool(4);
// a queue/deque per thread + work stealing -> divide and conquer
```

---

### Question 367 — How does takeWhile() work?
· [video](https://youtu.be/USoj821B-3M) · local file `032`

**Short answer:** It acts like a door that closes on a predicate.

**Less short answer:** takeWhile is an **intermediate** method of the Stream interface, that takes a **predicate** as a parameter. If your stream is not parallel, then it will evaluate the predicate for all the elements, **one by one**: as long as the predicate evaluates to true, the elements are pushed to the downstream — but when it becomes **false**, then **the stream is interrupted**. If your stream is **not ordered**, and possibly parallel, then takeWhile may push **any subset of elements** that match the predicate downstream, and interrupt the stream when it finds an element that does not. If your stream is ordered, then this subset is **the longest sequence of matching elements, starting at the front** — the first element of your stream. One last word: in the case of an **ordered parallel** stream, takeWhile has to manage an **internal counter, shared among the different threads** running your stream — and it will hurt your performance; one more reason to think twice before using parallel streams.

```java
Stream.of(2, 4, 6, 7, 8, 10)          // ordered
      .takeWhile(n -> n % 2 == 0)     // stops at 7
      .forEach(System.out::print);    // 2 4 6   (8 and 10 are never evaluated)
```

---

### Question 368 — What is padding?
· [video](https://youtu.be/EAE3lPYdoq8) · local file `031`

**Short answer:** Padding is about adding unused bytes in memory.

**Less short answer:** It has to do with **memory alignment**. Some CPUs are connected to their memory in a way that makes **accessing some words** — like 32-bits words or 64-bits words — **faster if they are stored at addresses divisible by 4, or by 8**. The JVM knows that — and to optimize the read and write performance, it **adds bytes when needed**, to make sure that all your data is **properly aligned** in memory. It can also optimize the **layout of your objects in memory**, to make sure that padding is kept minimal. One last word: when you need to read a **file that is not using any padding** — compact binary formats — you may come across **alignment issues**; the reason why you can specifically read **unaligned data** with the Memory API, from JDK 22. *(Another video.)*

```text
Object with fields long x; int y; byte z;  (8-byte alignment)
| header (8) | x (8) | y (4) | z (1) | padding (3) |
                                   ^--- 3 bytes of padding to close the 8
```

---

### Question 369 — How can you compare objects for equality?
· [video](https://youtu.be/sXs2wqpoFDE) · local file `030`

**Short answer:** Your best choice is to use the `Objects.equals()` utility method.

**Less short answer:** Of course, the most obvious choice would be to call `a.equals(b)` — but it can **fail if a is null**. So if you want your code to be robust, you first need to check if a is null — and then, if b is also null, you get an option: well, then you may want to return true... And suddenly, something that was supposed to be super simple becomes **a mess in your application**. JDK 7 added this small utility class, `Objects`, that has an `equals` method that does all this for you; it's not a revolution — it's just **so much simpler**. One last word: Objects also has a `hashCode(Object)` method, that takes an object — and if this object is null, it returns 0. Simple and useful.

```java
a.equals(b);                  // NPE if a == null
Objects.equals(a, b);         // null-safe: true only if both null, or equals
Objects.hashCode(null);       // 0, no NPE
Objects.hash(name, age);      // a hashCode from several fields
```

---

### Question 370 — What are the SOLID principles?
· [video](https://youtu.be/WkZF3uOA9hA) · local file `029`

**Short answer:** A set of principles for object-oriented programming, that you should know and follow.

**Less short answer:** The video attributes the acronym to Kent Beck (author of several books on TDD and extreme programming) — historically, it was coined by Michael Feathers, and Robert Martin popularized it. **S** is for the **Single Responsibility** principle: one reason to change your class or your method — one stakeholder; two is already too much. **O** is the **Open/Closed** principle: open for extension, and closed for modification — you can implement it with composition, which makes it even more powerful. The **L** principle is the **Liskov Substitution** principle, named after the scientist Barbara Liskov: it defines what inheritance is — if an instance of B behaves the same as an instance of A, to a point that you cannot tell which one is which, then B extends A. **I** is for **Interface Segregation**: in a nutshell, an interface should not have methods that you don't use. And **D** is for **Dependency Inversion**: if a module A depends on a module B at runtime, then B should depend on A at compile time — the two dependency arrows are opposite; this is what "inversion" means. One last word: all this may sound like old stuff — and it is: these principles have been around for **more than 25 years** — but applying them will help you make your legacy code much easier to manage.

```java
// D: OrderService depends on the abstraction; the implementation is injected
class OrderService {
    private final OrderRepository repo;    // the abstraction
    OrderService(OrderRepository repo) { this.repo = repo; } // the inverted arrow
}
```

---

### Question 371 — What is a megamorphic call?
· [video](https://youtu.be/-g7oY06n5C8) · local file `028`

**Short answer:** A method call that the JVM has a hard time optimizing.

**Less short answer:** You write your code against interfaces, right? Like when you need a list, you make it a List, and not an ArrayList. So it means that at runtime, the JVM needs to **find the implementation you're calling** — because it's not in the bytecode. This is called a **virtual call**, and it happens every time you call a method that can be overridden somewhere. But then, if the JVM realizes that, at runtime, a particular virtual call **always calls the same implementation**, then it can optimize things, and make it like a non-virtual call — a regular call, which will be much faster. A **megamorphic call** is a call that **cannot be optimized** in that way, because you have **too many overriding versions** of the method you're calling — and the JVM needs to check which one you are calling. And the bad news is that "too many" starts at **three**: a megamorphic call is a virtual call with **at least three possible implementations**. One last word: with **sealed types and pattern matching**, you can eliminate all the virtual calls of your application. Will it be better performance-wise? Well, it is something worth checking — when it comes to performance: **measure, don't guess**.

```java
List<String> list = ...;
list.size(); // a virtual call: ArrayList? LinkedList? ...
// monomorphic (1 impl) -> the JIT inlines
// bimorphic  (2 impls) -> the JIT still inlines
// megamorphic (3+ impls) -> virtual dispatch, no inlining

// sealed + switch: static dispatch, no virtual call
sealed interface Shape permits Circle, Square {}
double area(Shape s) {
    return switch (s) {
        case Circle c  -> Math.PI * c.radius() * c.radius();
        case Square q -> q.side() * q.side();
    };
}
```

---

### Question 372 — How can you read and write unaligned data?
· [video](https://youtu.be/zgTxuSnVn4g) · local file `027`

**Short answer:** There is a button for that. *(the presenter jokes: the ready-made option exists.)*

**Less short answer:** You need to use the **Memory API** for that, from the **Panama** Project. First you create an **Arena** — and then, create a **memory segment** from this arena; this memory segment can represent a portion of memory from the off-heap memory of the application, or an array that lives on the heap. You can use the **`set` method to write some data** to this memory segment: it takes a **memory layout**, an **offset** — and, of course, a value. The memory layout and the offset you pass need to be **compatible**: if you need to write some **unaligned** data, then you need to pass an **unaligned memory layout** (like `JAVA_INT_UNALIGNED`); if you don't, and you try to write at an offset that is not aligned, then you will get an `IllegalArgumentException`. One last word: you need to keep in mind that there is **another overhead** when you read and write unaligned data — so you should use this feature **only if you absolutely need it**.

```java
try (Arena arena = Arena.ofConfined()) {
    MemorySegment seg = arena.allocate(100);
    // a compact file header: an int field at offset 3 (unaligned!)
    seg.set(ValueLayout.JAVA_INT_UNALIGNED, 3, 42);
    int value = seg.get(ValueLayout.JAVA_INT_UNALIGNED, 3);
    // with JAVA_INT (aligned) at offset 3 -> IllegalArgumentException
}
```

---

### Question 373 — How much memory does an object consume?
· [video](https://youtu.be/i53fUDFy19U) · local file `026`

**Short answer:** It depends.

**Less short answer:** It really depends — it is even dependent on the machine that is running your application. Each object has a **header** that is made of one 32-bits word and one 64-bits word on 32-bits machines — or two 64-bits words on 64-bits machines, with your application living in a heap of up to 32 GB (thanks to the *compressed oops*; above that, more). Then there is a second overhead, which is due to **alignment**: for instance, an Integer in an application that has a 64-GB heap on a 64-bits machine takes **two 64-bits words** — the header — then the integer itself, 32 bits, and then **32 more bits of padding** to preserve the alignment. That's **6 32-bits words, to carry 1 32-bits word of information**. One last word: there are two ways of optimizing that: the first one is the **Lilliput project**, which consists in drastically **reducing the size of the header** — and the second one is the **Valhalla project**, that removes the header completely for the value objects.

```text
An Integer on a 64-bits JVM (heap > 32 GB):
+---------------------------+-----------+-----------+
| header (2 x 64 bits)      | int 32 b  | padding   |
|      128 bits             |  32 bits  |  32 bits  |
+---------------------------+-----------+-----------+
= 24 bytes to carry 4 bytes of data
```

---

### Question 374 — How can you add a snippet of code in your JavaDoc?
· [video](https://youtu.be/ZNe_-Z1qxp8) · local file `025`

**Short answer:** There are actually several — and one of them is much better.

**Less short answer:** The first one consists in adding this snippet **directly in your JavaDoc comment** with `{@snippet ...}`: it's nice, it works well — but you **cannot validate this snippet at runtime**, like it was a unit test. So there is a better way, which consists in **including a region of a class**: you reference that class with the `class` tag, and then the region you want to include in your JavaDoc. This class needs to live in the **`snippet-files` directory** of the package where the class that references the snippet lives. On the one hand, it's convenient — because your IDE can read this class, and render your JavaDoc on the fly; but on the other hand, **Maven will have a hard time running all these classes as unit tests** — which you can solve with the `--snippet-path` option, when you run the JavaDoc tool. One last word: all this also works if you write your JavaDoc in **Markdown** (`///` comments).

```java
/**
 * Usage example:
 * {@snippet class="MoneyExamples" region="rounding"}
 */
public class Money { ... }
// in src/main/java/.../snippet-files/MoneyExamples.java
public class MoneyExamples {
    // @start region="rounding"
    Money m = value.setScale(2, RoundingMode.HALF_UP);
    // @end region
}
```

---

### Question 375 — What is an EnumSet?
· [video](https://youtu.be/1BzJQf2fP3U) · local file `024`

**Short answer:** *(the video goes straight to the explanation)*

**Less short answer:** An enum set is a set that can hold some values of a **single enum**. The implementation stores them in a **bit vector** — which is very efficient, both memory-wise and CPU-wise. There are **several factory methods** that you can use to create an enum set: `allOf(Weekday.class)` puts all the enum values in a set; `noneOf()` gives you an **empty enum set for the right type** — then you can choose the values you want to add, one by one; and then you have this `complementOf(otherSet)` method, that takes another set, and that gives you the **complement** of this other set. One last word: an enum set is modifiable, but you **cannot add null elements** to it — which is expected, since it can only contain values from enums, and enums cannot have null values. *(And why would you add null elements to a set, anyway?)*

```java
enum Day { MON, TUE, WED, THU, FRI, SAT, SUN }

Set<Day> weekdays = EnumSet.range(Day.MON, Day.FRI);
Set<Day> all = EnumSet.allOf(Day.class);
Set<Day> weekend = EnumSet.complementOf(weekdays); // [SAT, SUN]
```

---

### Question 376 — What is context switching?
· [video](https://youtu.be/m7HvmcRAvac) · local file `023`

**Short answer:** Something you want to avoid.

**Less short answer:** It has to do with concurrency — doing **too many context switches** in an application is something that will hurt your performance badly; the reason why you need to avoid that. When a thread is running on one of the cores of your CPU, it is there with **all the information it needs** to run the code — the data, and some other stuff; all this is stored in a **cache and in the registers** of this core: this is what the **context** of this thread is. At some point, the operating system may decide to **pause this thread, and give the hand to another one** — so all the data this thread uses is **removed from the core**, to be brought back at a later time. Doing this takes in the order of **100 microseconds** — which is quite a lot. It will happen, for instance, if this thread is executing some **network call**, and is waiting for the data to be there, doing nothing — the reason why you should **never do that with kernel threads**. One last word: using **virtual threads** helps you preventing context switching from happening, since **blocking a virtual thread does not block the kernel thread** that is running it — one less bug to take care of in your application.

```java
// a kernel thread blocked on IO = an expensive context switch (~100 µs)
var response = client.send(request); // the OS thread just sits there

// with virtual threads, blocking is cheap — the carrier is released
Thread.ofVirtual().start(() -> process(client.send(request)));
```

---

### Question 377 — What is the subsized stream?
· [video](https://youtu.be/WLsaE5eC9k8) · local file `022`

**Short answer:** A good candidate to be executed in parallel.

**Less short answer:** **Sized** means that you know how many elements this stream will process — for instance, a stream opened on any collection is sized, because you just need to call `size()` on the collection to get this information. **Subsized** means that if you **split this stream in two sub-streams** — well, if you split the stream — you still know how many elements each sub-stream will process. Not all collections give you subsized streams: **lists do** — if you split a list in two, you know how many elements you have in each sub-list; but **sets do not** — splitting a set consists in splitting its internal array, and you do not know how many elements you have in the two sub-arrays you get: if you are unlucky, you may have **zero elements in the first one, and all the elements in the second one**. One last word: splitting a source is what **parallel streams** do — so do not use parallel streams with a source of data that is not easily splittable; and even if it is — think twice before using parallel streams: **it may hurt your application** instead of improving it.

```java
// SIZED + SUBSIZED: splitting a list preserves the notion of size
List<Integer> list = List.of(1, 2, 3, 4, 5, 6);
// HashSet: splitting the internal array guarantees no balance
Spliterator<Integer> it = new HashSet<>(list).spliterator(); // not SUBSIZED
```

---

### Question 378 — How can you get the number of characters in a string?
· [video](https://youtu.be/7al_ZQn99CY) · local file `021`

**Short answer:** You probably think that's an easy answer — well, it's harder than what you think.

**Less short answer:** It depends on what you call a **character**. You have a `length` method on the String class — and if you check the JavaDoc carefully, you will see that it returns **the number of Unicode code units** in this string class: a code unit is used for 16-bits chars. But some Unicode characters may be **encoded on two code units** — and even more than that. So if your application is playing with characters encoded on two chars, then length does **not** return the number of characters of your string — and you may end up with a string that **displays only one character on your screen, but with a length of two, or more**. One last word: there are two notions: the **code unit** — which is a char — and the **code point**, which is an int; and not all values of this int are valid. That will be for another time.

```java
String plane = "✈";                  // a single visible character
plane.length();                      // 2  (a surrogate pair = 2 code units)
plane.codePointCount(0, plane.length()); // 1  (the real code points)
```

---

### Question 379 — How can you limit the number of concurrent accesses to a resource?
· [video](https://youtu.be/q-coQ6MBjeE) · local file `020`

**Short answer:** That's the job of the semaphore.

**Less short answer:** There is indeed a `Semaphore` class in the JDK that you can use for that — and this is the nice way of doing it, because your code is then **saying what it does**: "I want to limit the number of requests on this server". A **wrong** way of doing it would be to use a specific Executor Service for that, and to **fix the number of threads to your limit** — it will also work, but it's a hidden way to achieve the same result, and it's dangerous for your application. But there is yet a **third way**, which consists in using a *gatherer*: streaming your requests, calling **`Gatherers.mapConcurrent(maxConcurrency, mapper)`** — each mapping is executed in a **virtual thread** (perfect for IO requests), and the number of active mappings at a given time is limited by this max-concurrency integer: a "free semaphore" doing the job for you. One last word: **do not use this pattern in a parallel stream** — it will be a complete disaster; stick to a normal stream — your code will be simpler, and will work as intended.

```java
Semaphore semaphore = new Semaphore(10); // at most 10 concurrent
semaphore.acquire();
try { serveRequest(); } finally { semaphore.release(); }

// the modern alternative (JDK 22+): a gatherer with bounded concurrency
var results = requests.stream()
        .gather(Gatherers.mapConcurrent(10, this::serve))
        .toList();
```

---

### Question 380 — How can you search for an element in a list?
· [video](https://youtu.be/hrLJTsM9c4M) · local file `019`

**Short answer:** There is a very good pattern for that.

**Less short answer:** Of course, you can call `list.contains()` — which will tell you if the object you pass is present or not — or `indexOf()`, which will give you the **first index** of this object. These methods are **slow**, because they **scan all the elements** of your list, one after the other — so if you have many elements in your list... If what you have is a **list that was previously sorted**, then you can call **`Collections.binarySearch()`**: it works if your objects are Comparable, or it can take a **comparator** as an argument — the implementation uses a **binary search algorithm, that has a log-n complexity**. Two caveats: first, if your list is **not sorted**, it will not throw any exception, and will return something — but it may take some time; and second, if the object you are looking for is **present several times** in the list, then you will get **any one of the valid indexes**. If the object is not in the list, then you will get **minus insertion minus one** — where "insertion" is the index at which this object would have been inserted, without breaking the sorted order; this number is negative, to denote that the object was not found. One last word: only use this pattern **on a list** — because this log-n complexity is achieved only if you can **access any element in constant time**.

```java
List<Integer> sorted = List.of(2, 5, 8, 12, 16, 23, 38);
int idx = Collections.binarySearch(sorted, 12);     // 3
int notFound = Collections.binarySearch(sorted, 7); // -3 (it would be inserted at index 2)
```

---

### Question 381 — How can you compare arrays for equality?
· [video](https://youtu.be/4f2MDQg15J8) · local file `018`

**Short answer:** *(no short answer in the video — straight to the method.)*

**Less short answer:** When you think about it, it can be a little more complex than what you may think at first. As you know, an array itself is an object in Java — so it has an `equals` method; but this equals method **compares the two references** of the array: in that sense, an array can only be equal to itself. Most of the time, what you need is to **compare the contents** of your arrays — saying two arrays are equal if the elements they contain are equal, and in the same order; so what you need in that case is one of the **`Arrays.equals()`** methods: they take two arrays, and can also take **indexes**, to compare only a portion of the first array and of the second array. And if you think you're done — well, you're not: because an array can itself contain **sub-arrays**; and if you want to compare them with the same Arrays.equals method, then you need to call another method, which is **`Arrays.deepEquals()`**. So for simple arrays, you call Arrays.equals — and for **multi-dimensional arrays**, you call Arrays.deepEquals. One last word: all these methods **support null values** in your array — two null values are considered equal — and will not fail in case of nulls... but seriously: **don't put null values in your arrays**.

```java
int[] a = {1, 2, 3}, b = {1, 2, 3};
a.equals(b);               // false! (compares references)
Arrays.equals(a, b);       // true  (content)

int[][] m1 = {{1, 2}, {3}}, m2 = {{1, 2}, {3}};
Arrays.equals(m1, m2);     // false (sub-arrays are objects)
Arrays.deepEquals(m1, m2); // true
```

---

### Question 382 — How can you get the max of a collection?
· [video](https://youtu.be/1XLa9QEMMyI) · local file `017`

**Short answer:** There are patterns for that (more than one).

**Less short answer:** There are actually several patterns, with different pros and cons. If what you need is just to extract the max **without transforming the elements** of your collection, or without filtering them, then `Collections.max()` is a good choice — it's a factory method: you need to pass your collection, and pass a comparator if you need to. Note that if your collection has **null elements, you will get a NullPointerException** — and if your collection is empty, you will get a NoSuchElementException. The second pattern is of course based on **streams**: just call stream, then `max`, and pass your comparator — you will also get an NPE if there is a null value in your stream; but if your stream is empty, you will get an **empty Optional** — which is probably better when it comes to error handling. One last word: be careful with the stream pattern, because you still need to **pay the price of the creation of the stream** — another overhead you may want to avoid; so unless there are specific stream features that you need, using the good old Collections.max is probably your best choice.

```java
var ints = List.of(1, null, 3, 4);
var max = Collections.max(ints);   // NullPointerException (a null in the list)

var empty = List.<Integer>of();
Collections.max(empty);                        // NoSuchElementException
empty.stream().max(Comparator.naturalOrder()); // Optional.empty()
```

---

### Question 383 — What is JLink?
· [video](https://youtu.be/bJ3GDdTmRJc) · local file `016`

**Short answer:** It is a standard tool of the JDK.

**Less short answer:** The JDK gives you **two tools** that work together, to package and distribute your application as an executable, or as an installer. The first tool is called **jlink** — it is part of the JDK distribution, and it does exactly what it says: if your application is a **modular** application, it lists **all the modules your application depends on** — and it does that even for the modules of the JDK itself; so you get **only the modules you need**, stripping the ones you do not depend on. It also means that you need to be careful with the modules you depend on: depending on a several-megabytes module, when all you need is a single class or two, may not be the best idea. One last word: once you have that, you can use **jpackage** to create an installer, to distribute your application — it works on Windows, macOS, and various flavors of Linux; but that will be for another time.

```bash
jdeps --print-module-deps app.jar        # discovers the needed modules
jlink --add-modules java.base,java.net.http \
      --output my-runtime                 # a lean, customized runtime
```

---

### Question 384 — How can you use the preview features?
· [video](https://youtu.be/2gmWx0-zqkk) · local file `015`

**Short answer:** There are two options for that: one for the compiler, and another one for the JVM.

**Less short answer:** You need to activate preview features **at two levels**. The first one is the **compiler level**: you add `--enable-preview` — and then you need to specify either the version of the source you're using, with `--source`, followed by the version — which could be between 8 and the version of the JDK you're using — or the version of the bytecode you want to generate, with `--target`, followed by the version. And then you need to specify that you wish to **run** this code with the preview feature enabled, by adding the same option, `--enable-preview`, to the **java** command. So in a nutshell: you cannot use preview features by just using the Java code — you need to tell the compiler that you want to activate them, and at runtime you need to tell the JVM that you want to run them. One last word: this preview feature mechanism is useful, because you can **activate them on demand**, without having to download some separate elements of the JDK; so it's easy to check for them, to play with them — and to **provide feedback** on the OpenJDK mailing lists; if you feel something is missing or wrong, this is something you should definitely do.

```bash
javac --enable-preview --release 25 Main.java
java  --enable-preview Main
```

---

### Question 385 — What is a group in a regular expression?
· [video](https://youtu.be/iTRIbbiZBVs) · local file `014`

**Short answer:** Something very useful to analyze strings of characters.

**Less short answer:** Groups are actually a feature of regular expressions, specified **outside of the JDK**. A group is just a **portion of your regular expression between parentheses**, like in this example. With such a regular expression, you can analyze a string of characters, match it to get a match — and if you have a match, then **get the different elements you need from it**. The nice thing is that you can **give names to your groups**, to make your code more expressive: you can specify the name of this group in that way, in your regular expression — and then, if you have a match, you can get the values of the different groups **by their names**, which makes your code much more readable. One last word: remember that you have a pattern to analyze a **long text** with a regular expression in a **lazy** way — if what you're looking for is the first occurrence of something, then this pattern is the one you want to use.

```java
Pattern p = Pattern.compile("(?<day>\\d{2})/(?<month>\\d{2})/(?<year>\\d{4})");
Matcher m = p.matcher("Reference: 12/25/2025");
if (m.find()) {
    System.out.println(m.group("day"));    // 12
    System.out.println(m.group("month"));  // 25
    System.out.println(m.group("year"));   // 2025
}
```

---

### Question 386 — What is a canonical constructor?
· [video](https://youtu.be/V_dVhb8QZuA) · local file `013`

**Short answer:** It is the constructor that takes all the components of a record.

**Less short answer:** A canonical constructor has to do with **records**. When you declare a record, you do not need to write its constructor: you just declare the **components** of this record, and the **compiler generates the constructor** that takes these components for you — this constructor that takes all the components is called the **canonical constructor**. Records have been designed in such a way that **you cannot create a record instance without calling its canonical constructor**: even if you create more constructors for your record, then you need to call this canonical constructor — and even **deserialization** follows this specification. So if you have **validation rules** in a canonical constructor, you have the guarantee that they will be executed **for all the instances of this record, without any exception**. One last word: when you think about it, a record is the **only kind of class** that offers this guarantee — for regular classes, deserialization does not call any constructor, so it's not possible to validate your instances in that case.

```java
record Range(int min, int max) {
    Range {                          // the compact form of the canonical ctor
        if (min > max) throw new IllegalArgumentException("min > max");
    }                                // the min/max assignment is automatic
}
// Range r = new Range(10, 5);  -> always throws, no escape hatch
```

---

### Question 387 — What is the stack of a thread?
· [video](https://youtu.be/pcyxXj-YH4s) · local file `012`

**Short answer:** A portion of memory.

**Less short answer:** Fortunately, this is something you do not need to take care of in Java, as it is managed for you by the API. In a nutshell, the **stack of a thread is the portion of memory used by a thread to store everything it needs**. It stores all the **local variables** created by the code it executes — and these variables can have references to the heap. The references themselves live on the stack, and the referenced memory lives on the heap. What lives on the stack **is not shared**: each thread has its own — which is not the case for the heap, of course, where **all the threads of your application can read and write data** — and where race conditions can happen; no race condition for what you have on the stack. One last word: threads are a **system resource**, and the size of this stack is fixed at your system level — it may vary from one operating system to the other, but it's typically **several megabytes** of memory.

```java
void process() {
    int local = 42;              // on the current thread's stack
    var order = new Order();     // the reference on the stack, the object on the heap
    // only the current thread sees 'local' and 'order'
}
```

---

### Question 388 — How can you format a string of characters?
· [video](https://youtu.be/DUX5bEvepbo) · local file `011`

**Short answer:** There is a method for that.

**Less short answer:** There are several solutions given to you by the JDK; the simplest one is probably the `String.format()` factory method: it takes the **format** as a first argument, and then the objects you want to pass this format to, to render them as a string. This format is inspired by the **"horrible" format from the C printf function**, that everybody knows; it is described in the JavaDoc of the `Formatter` class — and there are some differences between the C printf and the Java format: for instance, the **errors are not handled in the same way**, and some customization has been made. One last word: **thread safety** may be an issue when it comes to formatting strings: you may think that sharing your Formatters is a good idea, and will save you some resources — but you need to be careful, because **they carry some mutable state**; something you need to be aware of.

```java
String s = String.format("Hi %s, you have %,d messages", "Ana", 32000);

// Formatters carry state: create one per use/thread
var fmt = new SimpleDateFormat("MM/dd/yyyy"); // do NOT share between threads
```

---

### Question 389 — How can you replace all the elements of a list?
· [video](https://youtu.be/eRRKMgEBsHQ) · local file `010`

**Short answer:** There is a method for that.

**Less short answer:** The method is `Collections.replaceAll(list, oldValue, newValue)`: it takes the list, the value to replace, and the new value — and that's basically it. It **supports the replacement of null values**, which is great — because you can use this pattern to replace the null values you may have in your list with a default value, for instance. If you check the implementation of the method, you will see that it's also kind of **optimized**: depending on the size and the nature of your list, it may use **indexes** to access your elements, or a **list iterator**. One last word: this method returns **true if the element to be replaced was found** in your list — which does not mean that your list was modified, because you could have asked to replace it **with the same element**. Why would you do that? I don't know — but the API designers thought about it.

```java
List<String> names = new ArrayList<>(List.of("ana", null, "bob"));
Collections.replaceAll(names, null, "guest");       // [ana, guest, bob]
boolean found = Collections.replaceAll(names, "ana", "ANA"); // true
```

---

### Question 390 — Why should you favor immutability?
· [video](https://youtu.be/p9jdVv0BOzI) · local file `009`

**Short answer:** For many good reasons.

**Less short answer:** Let me go through some of them. When your objects are **non-modifiable**, you don't need to bother about the state they carry — it is the state you created them with. So **debugging is much simpler**, because you do not need to track when the state was modified. And it is simpler also in a **concurrent environment**: non-modifiable comes with **built-in thread safety** — race conditions can only occur when you have a write operation, and here, the only write operation you have is the **creation of your objects**. So if you are using records, or classes with `final` instance fields, then the creation of your objects is **protected against race conditions**; non-modifiable makes your job much easier when it comes to bug hunting in your applications. One last word (with the presenter's joke): immutability will even bring **better performance** when Valhalla delivers the value classes — "I'll stop making fun of Valhalla not coming anytime soon, because it is coming sooner than you expect".

```java
// A record: immutable by definition — no setters, final fields
record Money(BigDecimal amount, Currency currency) { }

// A classic immutable class: final fields, no setters, defensive copies
final class Period {
    private final LocalDate start, end;
    Period(LocalDate start, LocalDate end) {
        this.start = LocalDate.from(start);   // a defensive copy
        this.end = LocalDate.from(end);
    }
}
```

---

### Question 391 — How can you reverse the elements of a list?
· [video](https://youtu.be/zlEC4rVx6uA) · local file `008`

**Short answer:** It's a little more tricky than what it seems.

**Less short answer:** There is a factory method in the `Collections` factory class, simply called `reverse()`, that does exactly that — but note that there are **several traps** in this method. First, this method takes a **`List`** — and not a Collection — just because a collection is **not ordered**: so it does not make sense to try to reverse a collection. Second, this method **modifies your list** — so your list needs to be modifiable, and it also needs to have a list iterator that supports the `set()` operation; it's the case for an ArrayList, a LinkedList, and even the list you get when you call Arrays.asList() — but this is still something you need to keep in mind, if your application uses other implementations. But you also have a `reversed()` method, added as part of the **SequencedCollection interface** (Java 21), that List extends: this reversed method returns a **view on your original list, without modifying it** — so it also works on non-modifiable lists; and note that this view is **modifiable if the original list is itself modifiable** — and in that case, modifying the view modifies the list, and modifying the list modifies the view. One last word: using Collections.reverse may be **costly**, as it needs to process all your list to reverse it — creating a view is a **lightweight** process, because what you get is a wrapper that operates **lazily** on your list.

```java
List<Integer> nums = new ArrayList<>(List.of(1, 2, 3));
Collections.reverse(nums);      // modifies in place: [3, 2, 1]

List<Integer> view = nums.reversed(); // Java 21+: a view, no copy
view.set(0, 99);                      // modifies the original list too
```

---

### Question 392 — What are the characteristics of a collector?
· [video](https://youtu.be/zsSblXfes88) · local file `007`

**Short answer:** They are defined in `Collector.Characteristics`.

**Less short answer:** And are used by the Stream API implementation, to use your collector properly. The first one is **`CONCURRENT`**: it says that these collectors support concurrency — for the Stream API, it means that a parallel stream can use your collector normally. The second one is **`UNORDERED`**: it means that you are collecting your data in a container that **does not care about the order** in which it receives elements — that is the case for the `Collectors.toSet()` collector, for instance; this property can be used to relax some constraints for the computation of parallel streams. And the last one is **`IDENTITY_FINISH`**: it means that the finisher of your collector is the identity — so the implementation does not need to call it; that's the case for Collectors.toList() or toSet() — but it is **not** the case for `Collectors.joining()`, for instance. One last word, and an important one: unfortunately, setting the CONCURRENT characteristic to true **does not magically make your collector concurrent** — providing a thread-safe implementation is **your responsibility**.

```java
Collector<Characteristics, ?, ?> c = Collectors.toSet();
// toSet() declares:  Characteristics.UNORDERED, IDENTITY_FINISH
// toList() declares: Characteristics.IDENTITY_FINISH
```

---

### Question 393 — How does ScopedValue inheritance work?
· [video](https://youtu.be/FMZfhjprSE8) · local file `006`

**Short answer:** The presenter does not see a short answer to this one. *(laughs)*

**Less short answer:** This "inheritance" term comes from ThreadLocal variables — but scoped values work in a different way. First, you **bind** a scoped value to a value, and then you call a method that sees this binding. Now the question is: if this method **creates new threads**, can these threads see the scoped value bindings that you defined? And the default answer is **no** — but there is one important exception: if this method creates a **StructuredTaskScope**, then the bindings **are seen** by the virtual threads created by this structured scope. The reason is that none of these virtual threads can **escape the scope** of the original method call — so the bindings cannot escape this scope either. One last word: scoped values are a **great replacement for ThreadLocal variables** — you should use them now; and because you can control their lifecycle when you bind them, they are much safer.

```java
private static final ScopedValue<User> USER = ScopedValue.newInstance();

ScopedValue.where(USER, user).run(() -> process());
// inside the scope — and in StructuredTaskScopes created in it:
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    // the child virtual threads INHERIT the USER binding
    scope.fork(() -> loadProfile(USER.get()));
    scope.join();
}
```

---

### Question 395 — What are the differences between collections and streams?
· [video](https://youtu.be/TEZNPeH1Hwo) · local file `004`

**Short answer:** There are different interfaces.

**Less short answer:** The two concepts are in fact **fundamentally different**: a **collection carries elements** — a **stream does not**. The Collection interface defines how you can **add, remove, and iterate** elements; the Stream interface, on the other hand, defines how you can **process them** — typically map, filter, and reduce them. So the two concerns — managing elements, and processing them — are very cleanly **separated in two different interfaces**. This separation is very powerful, because it allows streams to **connect to any source of data**: collections, of course — but also strings of characters, regular expressions, files, file systems, network sockets... the limit is your imagination. One last word: connecting a stream to a **custom source of data** is not that hard — you need to implement the `Spliterator` interface (not easy, but perfectly possible), and pass this spliterator to the **StreamSupport** stream factory class.

```java
Collection<String> names = List.of("ana", "bob"); // carries elements
Stream<String> processed = names.stream()          // processes elements
        .filter(n -> n.length() == 3)
        .map(String::toUpperCase);
```

---

### Question 398 — How can you generate repeatable random numbers?
· [video](https://youtu.be/mhd_T3bQEeQ) · local file `002`

**Short answer:** A series of random numbers is always repeatable — just provide the same seed.

**Less short answer:** When you create an instance of the `Random` class, you **initialize the series of random numbers** that you can get one at a time, with the various methods of the class — nextInt, nextDouble, and the like. If you give a **seed when you create this instance**, then for a given seed, you will always get the **same series** — this may be very useful, for instance, to write **deterministic tests**. If you do not provide any seed, then the seed is generated from `System.nanoTime()`. One last word: **predictability is a different topic** — having non-predictable random series is harder than it seems; for cryptographic and security applications, use `SecureRandom` instead of Random — the series it generates still depends on the seed, but it is much harder to predict.

```java
Random random = new Random(42);          // a fixed seed: a repeatable series
System.out.println(random.nextInt(100)); // always the same value for seed 42
System.out.println(random.nextInt(100));

SecureRandom secure = new SecureRandom(); // for security/cryptography
```

---

### Question 399 (A) — What are the components of a collector?
· [video](https://youtu.be/vEqhlRmjUDQ) · local file `001`

**Short answer:** An initializer (supplier), an accumulator, a combiner — and an optional finisher.

**Less short answer:** That makes **three mandatory components, and one that is optional**. In a nutshell, a collector collects elements in a **mutable container**, and can optionally transform it before returning it. So you need an operation to **create** this mutable container: that's the initializer — and it's modeled by a `Supplier`. Then you need a way to **add the elements** that are pushed to this collector, to this container: that's the accumulator — and it's a `BiConsumer`; it modifies the container. In case this accumulation is performed **in parallel** — you may have one instance of your container per worker — and at the end of the day, you need to **combine them**: that's the job of the **combiner**, which is a `BinaryOperator`; it may or may not modify the containers it gets. And the last element **maps the final modifiable container to something else** — and that's a job for a `Function`; as well, this function is the identity function, it can be omitted. One last thing worth noting: the Collector interface **does not depend on the Stream API** — it is completely independent, so you can use a collector to collect your data as you see fit; and by the way, that's also the case for the `Gatherer` interface — that will be for another time.

```java
Collector<String, ?, String> joining = Collector.of(
        StringBuilder::new,            // initializer (supplier)
        StringBuilder::append,         // accumulator
        (sb1, sb2) -> sb1.append(sb2), // combiner
        StringBuilder::toString);      // finisher (optional)
```

---

### Question 399 (B) — What is Java?
· [video](https://youtu.be/mBT4qNFfRu8) · local file `399`

**Short answer:** A programming language.

**Less short answer:** An **object-oriented** programming language — with some zests of functional programming in it, actually more and more. Object-oriented programming means four things: **abstraction and encapsulation, inheritance, and polymorphism** — Java supports all that, with one restriction: it does not support **multiple inheritance** of classes, meaning that one class can only extend one other class. And one last one for you: Java is a **statically typed** language — meaning that the type of any variable you create or receive is **known at compile time**... even if it is the compiler that guesses the type of this variable for you (`var`).

```java
// Java in one line: static OO with functional touches
record Customer(String name) { }               // OO + immutable
var active = customers.stream()                // functional (Java 8+)
        .filter(Customer::active).map(Customer::name).toList();
```

> *Note: this is the series opener ("First question") — the channel numbered it 399, the same number as the collector-components question (Question 399 A).*

---

## Special episodes (no question number)

---


## Special episodes (no question number)

### 🎬 Special Episode 300 (no question)
· [video](https://youtu.be/gGumOIMCPsg) · local file `100`

**This video does not follow the question/answer format**: it is a celebratory episode — the series reached **300 episodes**, totaling about **5 hours of shorts**. In it, the presenter pays tribute to the series and to the viewers, with no new technical content.

---

### 🎬 Institutional episode (no number) — What is JavaOne?
· [video](https://youtu.be/cWd1D3VLlG0) · local file `169`

**No question number** — a promotional episode. JavaOne is described as "a conference you don't want to miss": the most important Java event, held in San Francisco in March (17th to 20th), with the people working on the platform speaking there — plus classic talks, hands-on labs, and open spaces to hang around and chat. Whether you're a student or an expert, JavaOne has something for you; and of course, social activities after hours. It is not a free event — but even if you cannot attend, stay tuned for what's happening there, and check the talk replays.

---

### 🎬 Ad episode (no number) — The 60-question milestone
· [video](https://youtu.be/95trS6sn8bU) · local file `339`

**No question number** — a promotional episode. The presenter realizes he has completed **60 questions (1 hour of content)**, and comments that one hour is already longer than most Java coding interviews (at least the technical part), taking the opportunity to promote his training courses. No technical content.

---

## Final notes

- **Source:** *Cracking the Java Coding Interview* playlist (cours-en-ligne channel), 399 videos downloaded in October 2026 to the local folder `Cracking the Java Coding Interview`.
- **Method:** the audios were transcribed locally with the Whisper `base.en` model (with a context vocabulary), and the entries were written from **what is said** in each video — including the presenter's running jokes ("measure, don't guess", "why would you put null?").
- **Code:** reconstructed from the narration and spot-checked against the frames shown in the videos; some variable names may differ from the original.
- **Transcription doubts:** a few question numbers came out garbled by the transcription and were corrected by sequence analysis and visual verification of the video frames; the corrections are recorded in `correcoes_qnum.json`.
- The full transcripts (in English) are in the `transcripts/` folder, in case you want to check the original text of any video.
- The Portuguese (pt-BR) version of this tutorial is kept as `TUTORIAL-pt-BR.md`.

---
