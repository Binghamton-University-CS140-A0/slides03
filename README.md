# slides03

-----------------------------
**Chapter 2**

-----------------------------

**Every variable has to have a type**

- The types can be any of the 8 primitive types: `byte`, `short`, `int`, 
`long`, `float`, `double`, `boolean`, `char`
- The types can be any of the over 4000 *classes*, and over 1000 *interfaces* and *enum* types in the current Java library (the new API format makes it a challenge to count exactly)
- The types can also be any of the *classes*, *interfaces* and *enum* types that any Java programmer may define
- In 2.2, the textbook mentions `String` and `int` variables
- By convention, variable and method names will start with a lower-case letter. An upper-case letter is placed where each new word begins in the name:

```java
double cumulativeGradePointAverage
```

- Page ?? gives the rules about what is required and what is optional in Java identifiers

**Use of the type**

- An *identifier* is the name of a variable, constant, method, package name, class, interface or enum. The last three start with a capital letter and they can be types for variables.
- A constant is written with all upper case, with the words separated by underscore, e.g. TAX_RATE. 
- The type of variables and constants determines: 
 - What assignments are allowed
 - What operations are allowed
 - What methods may be called
 - When they can be used as an argument of a method call
 - When they can be the return value of a method
 
 **Parameters**
 
- In

```java
public static void main(String[] args)
int factorial(int n)
public void println(String x)
public GridLayout(int rows, int cols,
	int hgap, int vgap)
// etc.,
```

- `args`, `n`, `x`, `rows`, `cols`, `hgap`, and `vgap` are all *parameters*
- They are the place-holders representing the values that are to be passed to these methods 
for them to complete their action

**Arguments**

- When a method is called, an *argument* has to be passed in to be used in the places where the parameter appears:

```java
System.out.println("Hello");
factorial(10);
panel.setLayout(new GridLayout(2, 2, 5, 5));
```

- The *arguments* are the actual values used in the execution of the methods 
(they are sometimes called *actual parameters*)
- We will discuss the implicit parameter shortly

**Methods (Chapter 2 and Section 3.2)**

- The basic format of an instance method is:

```
accessSpecifier returnType methodName
	(parameterType parameterName, ...) 
{
 	method body
}
```

- *accessSpecifier* can be `public`, `protected`, `private` or nothing (nothing means
deafault access, or package private access)
- *returnType* can be any primitive or reference type or void.
- primitive means `int`, `long`, `double`, `float`, `byte`, `short`, `char`, `boolean`
- We can also put `static` for class methods (there are other modifiers: `final` and `abstract` that we will see later and `synchronized` that we do not plan to use since it was used for multithreaded programs prior to Java 5)

**Construction**

Usually we call a constructor to make objects

```java
Rectangle rect1 = new Rectangle(70,90,100,150);
```
 but not always:

```java 
LocalDate.of(2020,02,12);
JOptionPane.showMessageDialog(null, "A helpful message");
```
- The second creates a `JDialog` object

**Implicit Parameter**

- This is the key to all object oriented behaviour in Java. For example how methods operate on an object's data:

**Example: the translate method of Rectangle**

- Consider `rect1.translate(100, 80);`
- It moves the rectangle 100 units (pixels) to the right and 80 units (pixels) downward

```java
Rectangle rect1 = new Rectangle(70,90,100,150);
rect1.translate(100, 80);
```

- It moves the `rectangle` to be a rectangle with the data (170, 170, 100, 150)
- *Instance methods* are those that have to be called on an object 
(the other methods are `static` methods, also called class methods, e.g `Math.max`)

```java
public static void main(String[] args) 
{
	var rect1 = new Rectangle(70,90,100,150);
	var rect2 = new Rectangle(70,180,200,50);
	rect1.translate(100, 80);
}
```

- There is only *one copy* of the code for the method *translate*, not one copy for each object
- How is that *translate* moves `rect1` and not `rect2`?
- The code of `translate` has to receive a *reference* to the object it will read information from or modify
- Compile and run the code of `FirstGUI.java`

**The reference is passed as if it were a parameter**

- The activation record of a method contains various parts starting with the space designated for the 
parameters, where the arguments are stored
- For instance methods, the reference to the object is also stored in the activation record as if it were a parameter. 
- It is called the *implicit parameter* (Section 3.7)
- The `rect1.translate(dx, dy)` call causes the values of 

```
rect1
dx
dy
```
 to be put into the activation record in the call stack.
- The implicit parameter carries the name "this" if the program has to reference it.
- The code executed is then

```java
rect1.x = rect1.x + dx;
rect1.y = rect1.y + dy;
```

- The code of the translate method can be written as

```java
x = x + dx;
y = y + dy;
// or
this.x = this.x + dx;
this.y = this.y + dy;
```
- They are synonymous for the compiler. Hence if the current value of "this" is the reference `rect1` then the code executed is indeed

```java
rect1.x = rect1.x + dx;
rect1.y = rect1.y + dy;
```

![](https://www.cs.binghamton.edu/~lander/cs140/Implicit1.JPG)

**Instance vs Class methods**

- If the method needs to read or modify the instance fields (i.e. fields that are not declared as static), 
then the code needs access to the reference "this" which we call the implicit parameter
- If the method is more stand-alone as was the case with the methods we saw for arrays, 
then they can be declared as *static*. These are also called *class* methods and they *do not* have 
the implicit parameter in their activation records

**Accessor and Mutator methods (2.5)**

- Another distinction between methods is the following:
- If a method call changes the data in the object it is called on, the method is said to be a *mutator* method
- If the method call never changes the data in the object it is called on, the method is said to be an *accessor* method
- `rect1.getWidth()` only reports the width of the rectangle, so it is an *accessor*
- String is *immutable* and *all its methods are accessors*

**Constructors use "this" for the object being built**

![](https://www.cs.binghamton.edu/~lander/cs140/CallStack1.jpg)

- Remember this is just the beginning of the significance of "this"

**Do not be misled by the diagrams on pp.42-43**

- Figures 5 through 9 could be a bit misleading
- There is no method code stored in an object
- An object only contains the data (instance variables) of the object
- The code of the methods is only stored once (we can assume it is in a code area of memory along with code of methods of other classes)
- The diagrams hint that each object has its own copy of the code but that is misleading
- By the way there is also one *class object* for every Class that is used in a program and that is where static fields are stored

**Chapter 4 discusses common number times**

- Mostly we will use `int`, `long` and `double`
- If Java sees a number like 67263829, it assumes it is an `int`
- If Java sees a number like 6726382901, it gives an error, e.g.

```java
System.out.println(6726382901);
// is an error, we would need:
System.out.println(6726382901L);
```
- Even for long, we get an error from:

```java
System.out.println(67263829012345678901L);
```

- See the table on p.100
- If Java sees a decimal point or an exponent notation, it assumes `double` unless you terminate with 
'F' or 'f' to indicate `float`:

```
17.56			17e-5    (i.e. 0.00017)
1.8625E36, i.e. 
1862500000000000000000000000000000000.0
```

- Note the inaccuracies of doubles (and float):

```
System.out.println(18625000000000010000e17);
System.out.println(18625000000000001000e17);

// print out

1.862500000000001E36
1.8625E36
```

**Ranges and precision**

- The primitive types `int` and `long` are accurate but limited
- `float` and `double` scan store larger values but much less accurately
- The fact that computers use binary for storage means that anything things like 1.0/5 cannot be stored precisely

[http://en.wikipedia.org/wiki/IEEE_754-1985](http://en.wikipedia.org/wiki/IEEE_754-1985)

- The above is a good reference for the 32- and 64-bit representations of floats and doubles, respectively. The following
describes the newer 128-bit format for longer doubles (quad)--we still await news of Java implementations

[http://en.wikipedia.org/wiki/IEEE_754-2008](http://en.wikipedia.org/wiki/IEEE_754-2008)

- Input a double: 12.5
- Binary representation:

```
0100000000101001000000000000000000000000000000000000000000000000
```

- Binary representation with some spacing:

```
0 10000000010 1001000000000000000000000000000000000000000000000000
```

- **Consider the program NumberStringConversion.java**

```java
package notes;
import java.util.Scanner;
/**
 * A program to show the double and float representations
 * as they are stored in the computer memory
 * 
 * @author CS 140
 */
public class NumberStringConversion 
{
	/**
	 * The main method showing that allows the user to input
	 * doubles and floats and shows the binary representation
	 * @param args command line not used
	 */
	public static void main(String[] args) 
	{
		var inputD = 1.0;
		try(var keyboard = new Scanner(System.in)) 
		{
			while (inputD != 0.0) 
			{
				System.out.println("Input a double:");
				inputD = keyboard.nextDouble();
				var longRepD = Double.doubleToRawLongBits(inputD);
				var strRepD =  Long.toBinaryString(longRepD);
				strRepD = "00000000000000000000000000000000" +
						"00000000000000000000000000000000" + strRepD;

				int len = strRepD.length();
				strRepD = strRepD.substring(len-64);
				System.out.println("Binary representation: \n" + strRepD);
				System.out.println("Binary representation with some spacing:\n" + 
						strRepD.charAt(0) + " " +
						strRepD.substring(1,12) + " " +
						strRepD.substring(12));

				System.out.println("Input a float:");
				var inputF = keyboard.nextFloat();
				int intRepF = Float.floatToRawIntBits(inputF);
				var strRepF =  Integer.toBinaryString(intRepF);
				strRepF = "00000000000000000000000000000000" + strRepF;
				len = strRepF.length();
				strRepF = strRepF.substring(len-32);
				System.out.println("Binary representation: \n" + strRepF);
				System.out.println("Binary representation with some spacing:\n" + 
						strRepF.charAt(0) + " " +
						strRepF.substring(1,9) + " " +
						strRepF.substring(9));
			}
		}
	}
}
```

- Input a float: 12.5F
- Binary representation: 

```
01000001010010000000000000000000
```

- Binary representation with some spacing:

```
0 10000010 10010000000000000000000
```

**The textbook discusses 4.35**

- The representation of 4.35 as a `float` (or a `double`) is an infinite binary expansion that is truncated

```
01000000100010110011001100110011 (float)
0100000000010001011001100110011001100110011001100110011001100110 (double)
```

4.3499999999999993 through 4.35 all give the same `double` (use NumberStringConversion in the Code repository

- Weird effects of approximation:
- 4.35F*100 prints as 435.0
- 4.35*100 prints as 434.99999999999994
- 4.05F*100 prints as 405.00003

**Explaining the format: look at 12.5**

- First 12.5 is 8 + 4 + ½ = 1x2<sup>3</sup>+1x2<sup>2</sup>+0x2<sup>1</sup>+0x2<sup>0</sup>+1x2<sup>-1</sup> = 1100.1 = 1.1001x2<sup>3</sup>
- The "1." is dropped because it is always there except for 0.0
- The exponent 3 is biased to (2<sup>10</sup>-1)+3 = 1026 = 1x2<sup>10</sup>+0x2<sup>9</sup>+0x2<sup>8</sup>+...+0x2<sup>2</sup>+1x2<sup>1</sup>+0x2<sup>0</sup> = 10000000010
- So we get 0 (positive) then 10000000010 (exponent), then 1001 (the fractional part of 1.1001) then 48 0s to fill 64 bits


**Pay attention to truncation and rounding**

- System.out.println(4.35*100);
- System.out.println((int)4.35*100);
- System.out.println((int)(4.35*100));
- System.out.println(Math.round(4.35*100));

```
434.99999999999994
400
434
435  // note this is of type long 
```

See <a href="https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/Math.html#round(float)" target="blank">Math.round(float)</a> , <a href="https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/Math.html#round(double))" target="blank">Math.round(double)</a>

**Constants: Review 4.1.2**

- When working with numbers in programs it is of huge benefit to give names to constants
- By introducing named constants, code becomes more transparent to readers and if a change is needed, the change is only made in one place.
- Defined by Horstmann: 

```java
final double QUARTER_VALUE = 0.25;
```

- similarly `DIME_VALUE`, `NICKEL_VALUE`, `PENNY_VALUE`
- They offer a degree of self documentation
- From Chapter 4 of Horstmann:

```java
package ch04.sec01;
/**
 * A cash register totals up sales and computes change due.
 */
public class CashRegister 
{
	public static final double QUARTER_VALUE = 0.25;
	public static final double DIME_VALUE = 0.1;
	public static final double NICKEL_VALUE = 0.05;
	public static final double PENNY_VALUE = 0.01;

	private double purchase;
	private double payment;

	/**
	 * Records the purchase price of an item.
	 * @param amount the price of the purchased item
	 */
	public void recordPurchase(double amount) 
	{
		purchase = purchase + amount;
	}

	/**
	 * Processes the payment received from the customer.
	 * @param dollars the number of dollars in the payment
	 * @param quarters the number of quarters in the payment
	 * @param dimes the number of dimes in the payment
	 * @param nickels the number of nickels in the payment
	 * @param pennies the number of pennies in the payment
	 */
	public void receivePayment(int dollars, int quarters, 
			int dimes, int nickels, int pennies) 
	{
		payment = dollars + quarters * QUARTER_VALUE + dimes * DIME_VALUE
				+ nickels * NICKEL_VALUE + pennies * PENNY_VALUE;
	}

	/**
	 * Computes the change due and resets the machine for the next customer.
	 * @return the change due to the customer
	 */
	public double giveChange() 
	{
		double change = payment - purchase;
		purchase = 0;
		payment = 0;
		return change;
	}
}
```

- Tester:

```java
package ch04.sec01;
/**
 * This class tests the CashRegister class.
*/
public class CashRegisterTester 
{
   public static void main(String[] args) 
   {
      CashRegister register = new CashRegister();

      register.recordPurchase(0.75);
      register.recordPurchase(1.50);
      register.receivePayment(2, 0, 5, 0, 0);
      System.out.print("Change: ");
      System.out.println(register.giveChange());
      System.out.println("Expected: 0.25");

      register.recordPurchase(2.25);
      register.recordPurchase(19.25);
      register.receivePayment(23, 2, 0, 0, 0);
      System.out.print("Change: ");
      System.out.println(register.giveChange());
      System.out.println("Expected: 2.0");
   }
}
```

- From the Tax return code in Chapter 5 (Note I would have made the
the RATEs public since it would give us a SINGLE PLACE in all the classes of the 
program to make any changes to the values
)

```java
package ch05.sec04;
/**
 * A tax return of a taxpayer in 2008.
 */
public class TaxReturn 
{  
	public static final int SINGLE = 1;
	public static final int MARRIED = 2;

	private static final double RATE1 = 0.10;
	private static final double RATE2 = 0.25;
	private static final double RATE1_SINGLE_LIMIT = 32000;
	private static final double RATE1_MARRIED_LIMIT = 64000;

	private double income;
	private int status;

	/**
	 * Constructs a TaxReturn object for a given income and 
	 * marital status.
	 * @param anIncome the taxpayer income
	 * @param aStatus either SINGLE or MARRIED
	 */   
	public TaxReturn(double anIncome, int aStatus) 
	{  
		income = anIncome;
		status = aStatus;
	}

	public double getTax() 
	{  
		double tax1 = 0;
		double tax2 = 0;

		if (status == SINGLE) 
		{  
			if (income <= RATE1_SINGLE_LIMIT) 
			{
				tax1 = RATE1 * income;
			} 
			else 
			{
				tax1 = RATE1 * RATE1_SINGLE_LIMIT;
				tax2 = RATE2 * (income - RATE1_SINGLE_LIMIT);
			}
		} 
		else 
		{  
			if (income <= RATE1_MARRIED_LIMIT) 
			{
				tax1 = RATE1 * income;
			} 
			else 
			{
				tax1 = RATE1 * RATE1_MARRIED_LIMIT;
				tax2 = RATE2 * (income - RATE1_MARRIED_LIMIT);
			}
		}
		return tax1 + tax2;
	}
}
```

**Constants in the Library (e.g. java.lang.Math, java.awt.Color)**

- In Math: two approximate values

```java
public static final double E = 2.7182818284590452354;
public static final double PI = 3.14159265358979323846;

/**
 * The color yellow.  In the default sRGB space.
 */
public final static Color yellow = new Color(255,255,0);
/**
 * The color yellow.  In the default sRGB space.
 * @since 1.4
 */
public final static Color YELLOW = yellow;
```

- More arithmetic
- Whole number division: 
- Division of int and long discards remainders: 23 / 4 is 5, and 23 % 4 is 3. For long:
```java
System.out.println(4000000000L / 1234567); // prints 3240
System.out.println(3240 * 1234567L); // prints 3999997080
System.out.println(4000000000L % 1234567);  // prints 2920
```

**Interruption!--Interesting links**

- Current relevance of Java:
<a href="http://www.tiobe.com/tiobe-index" target="blank">TIOBE Index</a>
- <a href="https://finance.yahoo.com/news/risky-bet-saved-facebook-hundreds-184738287.html" target="blank">Why compilers can save money, not just time</a>
- (Drew Parowski did the BS and MS in Binghamton--also went to world finals of Top Coder while here at Binghamton
- The details and <a href="https://www.youtube.com/watch?v=XqK8Yuoq4ig" target="blank">C++ is great for systems software</a> (using the fancy features):

**A few String methods**

```java
int z = "CS 140".length( ); //gives z the value 6
String str = "CS 140";
String s1 = str.substring(0, 2);  // s1 is “CS”
char ch = str.charAt(3); // gives ch the value '1‘
char[] array = str.toCharArray();
```

- The indexing of the characters in a String object starts at position 0, i.e., the first char is at index 0, the second is at index 1, etc.
- [String substring(int beginIndex)](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#substring(int))
- [String substring(int beginIndex, int endIndex)](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#substring(int,int))
- [String replace(char oldChar, char newChar)](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#replace(char,char))
- [String replace(CharSequence target, CharSequence replacement)](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#replace(java.lang.CharSequence,java.lang.CharSequence)), Note a CharSequence can be a String
- [public String replaceAll​(String regex, String replacement)](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#replaceAll(java.lang.String,java.lang.String)). Needs an understanding of regular expressions
- [String toLowerCase()](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#toLowerCase())
- [String toUpperCase()](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#toUpperCase())
- [boolean equals(Object anObject)](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#equals(java.lang.Object))
- [boolean equalsIgnoreCase(String anotherString)](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#equalsIgnoreCase(java.lang.String))
- [int indexOf(int ch)](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#indexOf(int))
- [int indexOf(int ch, int fromIndex)](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#indexOf(int,int))
- [int indexOf(String str)](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#indexOf(java.lang.String))
- [int indexOf(String str, int fromIndex)](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#indexOf(java.lang.String,int))
- [trim()](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#trim()) and [split()](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/String.html#split(java.lang.String))

```java
String str = "This is a simple String, with some punctuation!"
String[] parts = str.split("[\\s,!]+");
System.out.println(Arrays.toString(parts));
// [This, is, a, simple, String, with, some, punctuation]
str = "     This is space       in a String       "
System.out.println(str.trim());
// "This is space       in a String"
```

**String is immutable**
- It is important to keep in mind that String is one of a small group of classes, which only have immutable objects:
- `str.substring(0,2);` does NOT change str. The method just returns a part of the String as its value
- This is a rare feature among all the classes in Java, most objects can be changed by some method calls


