
### Strings:

In Python, the `str` (string) data type represents a sequence of characters enclosed within single quotes (' ') or double quotes (" "). Strings in Python are immutable, which means they cannot be changed after they are created. Here are some key points about strings in Python:

**Creation:**

- - Example: `my_string = 'Hello, World!'` or `my_string = "Hello, World!"` or `my_string = """Hello,
  `World"""` for next line texts

**Accessing Characters:**

- - You can access individual characters within a string using indexing, starting from 0.
    - Example: `print(my_string[0])` would output 'H'.

**String Concatenation:**

- - You can concatenate (join) two or more strings using the `+` operator.
    - Example: `greeting = 'Hello' + ' ' + 'World!'` would result in 'Hello World!'.

**String Length:**

- - The `len()` function can be used to determine the length (number of characters) of a string.
    - Example: `print(len(my_string))` would output the length of the string.

**String Slicing:**

- - You can extract a substring from a string using slicing, specifying the start and end indices.
    - Example: `substring = my_string[7:12]` would extract the substring 'World'.

**String Methods:**

- - Python provides various built-in methods to manipulate and transform strings. Examples include `upper()`, `lower()`, `strip()`, `split()`, `replace()`, and more.
    - Example: `print(my_string.upper())` would output 'HELLO, WORLD!'.

**String Formatting:**

- - String formatting allows you to embed values within a string. This can be done using the `%` operator or the `format()` method or the f string (string literal)
- Example:

`name = 'Alice'`
`age = 30`
`print("My name is %s and I'm %d years old." % (name, age))`

`movie = "The Hangover"` 
`print("My favorite movie is {}.".format(movie))` 
`print("My favorite movie is %s" % movie) 
`print(f"My favorite movie is {movie}")`

**String Splitting:**

- String splitting helps to split a string, using any delimiter or the default space delimiter
Example:
`print(sentence.split()) #delimeter - default is a space` 

**String Join:**

- String join helps to join the strings in the lists using a delimiter provided
Example:
`sentence_join = ' '.join(sentence_split)` 

**String quotes:**

- Using " Double quotes " or 'Single quotes' helps to ignore the usage of the quotes in the string and also using a / (escape character) helps with the same.
Example:
`quote = "He said, 'give me all your money'"` 
`quote = "He said, \"give me all your money\""` 

**String strip:**

- Using String strip helps to remove spaces in the strings
Example:
`too_much_space = "                hello    "` 
`print(too_much_space.strip()) #Output: hello`

**String find:**

- The in operator helps to find the character in the string.
Example:
`print("A" in "Apple") #returns true` 
`print("a" in "Apple") #returns false - case sensitive` 

`letter = "A" word = "Apple" print(letter.lower() in word.lower()) #improved` 


### Math:

In Python, the `math` module is a built-in module that provides various mathematical functions and constants. It allows you to perform advanced mathematical operations in your Python programs. To use the `math` module, you need to import it first using the `import` statement. Here's an overview of some commonly used math functions and operators in Python:

Math Functions in the `math` Module:

- `math.sqrt(x)`: Calculates the square root of `x`.
- `math.pow(x, y)`: Raises `x` to the power of `y`.
- `math.exp(x)`: Calculates the exponential value of `x` (e^x).
- `math.log(x)`: Calculates the natural logarithm of `x` (base e).
- `math.log10(x)`: Calculates the logarithm of `x` to base 10.
- `math.sin(x)`, `math.cos(x)`, `math.tan(x)`: Calculate the sine, cosine, and tangent of `x`, respectively (where `x` is in radians).
- `math.degrees(x)`: Converts `x` from radians to degrees.
- `math.radians(x)`: Converts `x` from degrees to radians.

Math Operators:

- Addition (`+`): Adds two numbers.
- Subtraction (`-`): Subtracts one number from another.
- Multiplication (`*`): Multiplies two numbers.
- Division (`/`): Divides one number by another.
- Integer Division (`//`): Performs division and returns the quotient as an integer (rounds down).
- Modulo (`%`): Returns the remainder of division.
- Exponentiation (`**`): Raises a number to a power.

Example usage:

`import math`
-`Using math functions` 
`print(math.sqrt(25)) # Output: 5.0` 
`print(math.pow(2, 3)) # Output: 8.0` 
`print(math.sin(math.pi/2)) # Output: 1.0` 

-`Using math operators` 
`x = 10 
`y = 3` 
`print(x + y) # Output: 13` 
`print(x / y) # Output: 3.3333333333333335` 
`print(x // y) # Output: 3` 
`print(x % y) # Output: 1` 
`print(x ** y) # Output: 1000`


### Variables and Methods:

In Python, variables and methods are fundamental concepts used in programming. Here's an explanation of each:

**Variables:**

A variable is a named storage location used to store data or values in a program. It acts as a placeholder for data that can be accessed, modified, or used in calculations throughout the program. Variables in Python are dynamically typed, meaning their data type can change during program execution. Here's an example of variable usage in Python:

**Variable assignment**
`x = 10`
`name = "John"`
`is_true = True`


**Variable usage**
`y = x + 5`
`print("Hello, " + name)`
`if is_true:`
    `print("The condition is true")`

In the example above, `x`, `name`, and `is_true` are variables assigned with different data types (integer, string, and boolean, respectively). They are used in calculations and print statements to perform operations and display values.

**Methods:**

A method is a block of reusable code that performs a specific task or action. Methods are associated with objects or classes and are called upon to perform certain operations. In Python, methods are commonly referred to as functions. Built-in functions and user-defined functions both fall under the category of methods. Here's an example:

**Built-in method example:**

`numbers = [1, 2, 3, 4, 5]`
`length = len(numbers)`
`print("Length:", length)`

**User-defined method example**:
`def greet(name):`
    `print("Hello, " + name)`
   
`greet("Alice")`

In the example above, `len()` is a built-in method that calculates the length of a list (`numbers` in this case). The user-defined method `greet()` takes a parameter `name` and prints a greeting message. It is called with the argument "Alice" to print "Hello, Alice" to the console.


### Functions:

In Python, a function is a reusable block of code that performs a specific task. Functions allow you to organize code into logical and modular units, making your code more readable, maintainable, and reusable. Here's an explanation of functions in Python:

**Function Definition:**

A function is defined using the `def` keyword, followed by the function name, parentheses, and a colon. The function may also have parameters (optional) and a return statement (optional) to send back a result. Here's an example of a simple function definition:

`def greet():`
    `print("Hello, World!")`

**Function Call:**

To execute a function, you need to call it by its name, followed by parentheses. Here's an example of calling the `greet()` function:

`greet()`

**Function Parameters:**

Functions can accept parameters, which are variables that hold values passed to the function when it is called. Parameters allow you to customize the behavior of a function based on the values you provide. Here's an example of a function with parameters:

`def greet(name):`
    `print("Hello, " + name + "!")`


`greet("Alice")`

In the example above, the `greet()` function accepts a parameter named `name`. When the function is called with an argument, such as "Alice", the value is assigned to the `name` parameter within the function body.

**Return Statement:**

Functions can also return values using the `return` statement. The returned value can be assigned to a variable or used directly in expressions. Here's an example:

`def add_numbers(a, b):`
    `return a + b`


`result = add_numbers(3, 4)`
`print(result)  # Output: 7`

In this example, the `add_numbers()` function takes two parameters (`a` and `b`) and returns their sum. The returned value is then assigned to the `result` variable and printed.


### Boolean Expressions and Relational Operators:

In Python, Boolean expressions are expressions that evaluate to either `True` or `False`. They are typically used in conditional statements and logical operations to make decisions based on the truth or falsity of certain conditions. Relational operators are used to compare values and create Boolean expressions. Here's an explanation of Boolean expressions and relational operators in Python:

**Relational Operators:**

Python provides several relational operators to compare values. Here are the commonly used relational operators:

- Equality (`==`): Checks if two values are equal.
- Inequality (`!=`): Checks if two values are not equal.
- Greater than (`>`): Checks if the left value is greater than the right value.
- Less than (`<`): Checks if the left value is less than the right value.
- Greater than or equal to (`>=`): Checks if the left value is greater than or equal to the right value.
- Less than or equal to (`<=`): Checks if the left value is less than or equal to the right value.

**Boolean Expressions:**

Boolean expressions are formed by combining relational expressions using logical operators. The logical operators in Python are:

- Logical AND (`and`): Returns `True` if both operands are `True`.
- Logical OR (`or`): Returns `True` if at least one operand is `True`.
- Logical NOT (`not`): Negates the value of the operand.

**Examples:**

`x = 5`
`y = 10`

`print(x == y)   # Output: False`
`print(x < y)    # Output: True`

`print(x < y and y > 0)    # Output: True`
`print(x < y or y < 0)     # Output: True`
`print(not (x == y))       # Output: True`

In the example above, we have two variables `x` and `y`. We use the relational operators (`==` and `<`) to compare their values and create Boolean expressions. The logical operators (`and`, `or`, and `not`) are then used to combine the relational expressions and evaluate the overall truth value.


### Conditional Statements:

In Python, conditional statements are used to perform different actions based on certain conditions. They allow you to control the flow of your program by executing specific blocks of code when certain conditions are met. Here's an explanation of conditional statements in Python:

1. if Statement:
2. The `if` statement is the most basic conditional statement. It executes a block of code if a given condition is true. Here's the general syntax:

if condition:
    # code to be executed if the condition is true

Example:

`x = 5`
`if x > 0:`
    `print("x is positive")`

In this example, the code inside the `if` block (`print("x is positive")`) will be executed if the condition `x > 0` is true.

1. if-else Statement:
2. The `if-else` statement allows you to specify two different blocks of code—one to be executed if the condition is true and another to be executed if the condition is false. Here's the syntax:

if condition:
    # code to be executed if the condition is true
else:
    # code to be executed if the condition is false

Example:

`x = 5`
`if x > 0:`
    `print("x is positive")`
`else:`
    `print("x is not positive")`

In this example, if `x` is greater than 0, the first block (`print("x is positive")`) will be executed. Otherwise, the second block (`print("x is not positive")`) will be executed.

1. if-elif-else Statement:
2. The `if-elif-else` statement allows you to check multiple conditions and execute different blocks of code based on those conditions. It provides more than two options for branching. Here's the syntax:

if condition1:
    # code to be executed if condition1 is true
elif condition2:
    # code to be executed if condition1 is false and condition2 is true
else:
    # code to be executed if all conditions are false

Example:

`x = 5`
`if x > 0:`
    `print("x is positive")`
`elif x < 0:`
    `print("x is negative")`
`else:`
    `print("x is zero")`

In this example, if `x` is greater than 0, the first block will be executed. If it is less than 0, the second block will be executed. Otherwise, if none of the conditions are true, the third block will be executed.


### Lists:

In Python, a list is a versatile and mutable data structure that can hold a collection of items. It allows you to store multiple values of different data types in a single variable. Here's an explanation of lists in Python:

List Creation:

To create a list, you enclose comma-separated values within square brackets `[ ]`. Here's an example:

`fruits = ["apple", "banana", "orange"]` 

List Access:

You can access individual elements in a list using indexing. Indexing starts from 0 for the first element and goes up to the length of the list minus one. Here are some examples:

`print(fruits[0])    # Output: "apple"`
`print(fruits[2])    # Output: "orange"`

List Modification:

Lists are mutable, which means you can modify their elements. You can assign new values to specific positions in the list or use methods to modify the list itself. Here are some examples:

`fruits[1] = "grape"     # Modifying an element`
`fruits.append("kiwi")   # Adding an element to the end`
`fruits.remove("apple")  # Removing an element`

List Operations:

Python provides various operations that can be performed on lists. Some common operations include:

- Concatenation: You can use the `+` operator to concatenate two or more lists.
- Length: The `len()` function returns the number of elements in a list.
- Slicing: You can extract a sublist from a list using slicing.
- Iteration: You can use a loop to iterate over the elements of a list.

Here are some examples:

`fruits = ["apple", "banana", "orange"]`
`fruits2 = ["grape", "kiwi"]`

`combined = fruits + fruits2`
`print(combined)         # Output: ["apple", "banana", "orange", "grape", "kiwi"]`

`print(len(fruits))      # Output: 3`

`sublist = fruits[1:3]`
`print(sublist)          # Output: ["banana", "orange"]`

`for fruit in fruits:`
    `print(fruit)        # Output: "apple", "banana", "orange"`


### Tuples:

In Python, a tuple is an ordered collection of elements, similar to a list. However, unlike lists, tuples are immutable, meaning their elements cannot be modified once they are created. Here's an explanation of tuples in Python:

Tuple Creation:

To create a tuple, you enclose comma-separated values within parentheses `( )`. Here's an example:

`fruits = ("apple", "banana", "orange")`

Tuple Access:

You can access individual elements in a tuple using indexing, similar to lists. Indexing starts from 0 for the first element. Here are some examples:

`print(fruits[0])    # Output: "apple"`
`print(fruits[2])    # Output: "orange"`

Tuple Immutability:

Tuples are immutable, meaning you cannot modify their elements. Once a tuple is created, its values cannot be changed. For example, attempting to assign a new value to an element will result in an error. Here's an example:

`fruits[1] = "grape"    # This will raise an error`

Tuple Operations:

Although tuples are immutable, you can perform certain operations on them:

- Concatenation: You can use the `+` operator to concatenate two or more tuples.
- Length: The `len()` function returns the number of elements in a tuple.
- Slicing: You can extract a subtuple from a tuple using slicing.

Here are some examples:

`fruits = ("apple", "banana", "orange")`
`fruits2 = ("grape", "kiwi")`

`combined = fruits + fruits2`
`print(combined)         # Output: ("apple", "banana", "orange", "grape", "kiwi")`

`print(len(fruits))      # Output: 3`

`subtuple = fruits[1:3]`
`print(subtuple)         # Output: ("banana", "orange")`

Tuples are useful in situations where you want to store a collection of values that should not be changed. They can be used to group related data elements and can also be used as keys in dictionaries. While tuples are immutable, they offer advantages such as faster performance and protection against unintentional modification.

### Looping:


In Python, looping allows you to repeat a block of code multiple times. It is a fundamental concept used for iterating over data structures, performing repetitive tasks, and controlling the flow of your program. There are two main types of loops in Python: `for` loops and `while` loops. Here's an explanation of looping in Python:

1. for Loop:
2. The `for` loop is used to iterate over a sequence (such as a list, tuple, string, or range) or any iterable object. It executes a block of code a fixed number of times, based on the elements or items in the sequence. Here's the general syntax:

for item in sequence:
    # code to be executed for each item in the sequence

Example:

`fruits = ["apple", "banana", "orange"]`
`for fruit in fruits:`
    `print(fruit)`

In this example, the `for` loop iterates over each item in the `fruits` list, and the block of code inside the loop (`print(fruit)`) is executed for each item. It will output:

`apple`
`banana`
`orange`

1. while Loop:
2. The `while` loop is used to repeatedly execute a block of code as long as a given condition is true. It continues looping until the condition becomes false. Here's the general syntax:

while condition:
    # code to be executed while the condition is true

Example:

`count = 0`
`while count < 5:`
    `print(count)`
    `count += 1`

In this example, the `while` loop will continue executing the code inside the loop (`print(count)`) as long as the condition `count < 5` is true. It will output:

`0`
`1`
`2`
`3`
`4`

The `break` and `continue` Statements:

Within loops, you can use the `break` statement to exit the loop prematurely and the `continue` statement to skip the current iteration and move to the next one.

Looping provides a powerful mechanism for iterating over data, performing calculations, and controlling program flow. By using loops effectively, you can automate repetitive tasks and process large amounts of data efficiently.


### Dictionaries:

