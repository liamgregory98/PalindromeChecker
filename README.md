# Palindrome Checker

A piece of code that prompts the user to enter what they think
could be a Palindrome. The code converts the string to lower case and
filters out any numbers, special characters or spaces. Then it checks
to see whether the remaining letters read the same backwards as they
 do forwards.

 # Method
 Applying the single responsibility principle, separate classes were
 used to carry out each function of the code. Initially, the class Input
 was used to prompt the user to enter their palindrome attempt and store it.

 ```java
 public class Input {
    public static String palInput(){
        String palInput;
        System.out.println("Enter palindrome attempt - Any numbers, special characters will be ignored");
        Scanner userInput = new Scanner(System.in);
        String palAttempt = userInput.nextLine();
        palInput = palAttempt;
        return palInput;
    }
}
```
Afterwards, the class Converter, filtered all numbers, special characters and spaces
from the Input to return a String in the correct format for testing.
```java
public class Converter {

    public static String palConvert(String palInput){

        String palConvert;
        palInput = palInput.toLowerCase();
        palInput = palInput.replaceAll("[^a-z]", "");
        palConvert = palInput;
        return palConvert;
    }
}
```
This String (palConvert) was then ran through class Test. An empty String called reverse
was made. All of The characters in palConvert were looped through added to this String in reverse order.
A boolean set to true was combined with a for loop to then compare each character in palConvert with the reverse string
starting from the beginning. If there was a mismatch, the boolean would be set to false. The correct
result depending on the boolean would be returned.

```java
public class Test {

    public static String palTest(String palConvert){
        String palTest;
        String reverse = "";

        for (int i = palConvert.length() - 1; i >= 0; i--) {
            reverse += palConvert.charAt(i);
        }
        boolean palindrome = true;
        for (int i = 0; i < palConvert.length(); i++) {
            if (palConvert.charAt(i) != reverse.charAt(i)) {
                palindrome = false;
            }
        }
        if(palindrome) {
            palTest = "It's a palindrome";
        } else {
            palTest = "Not a palindrome";
        }
        return palTest
    }
}
```
The PalindromeChecker class prints out the result of the tests when initialised in the Main.
```java
public class PalindromeChecker {
    public static void palindromeChecker(){
        System.out.println(Test.palTest(Converter.palConvert(Input.palInput())));
    }
}
```
```java

public class Main {
    public static void main(String[] args) {
        PalindromeChecker.palindromeChecker();
    }
}

```

## Testing

JUnit tests were used to test the code. Initially a single test was ran to check
if it would recognise a palindrome and single test was ran to check if it would
recognise a input that was a palindrome.
```Java
@Test
    @DisplayName("Given a user input of racecar, the PalindromeChecker returns It's a palindrome")
    public void givenAUserInputOfRACECAR_PalindromeChecker_ReturnsItsAPalindrome() {
        String palInput = "racecar";
        String expectedOutput = "It's a palindrome";
        String output = com.sparta.Test.palTest(Converter.palConvert(palInput));
        assertEquals(expectedOutput, output);
    }// Test Passed



        @Test
        @DisplayName("Given a user input of racecars, the PalindromeChecker returns Not a palindrome")
        public void givenAUserInputOfRACECARS_PalindromeChecker_ReturnsNotAPalindrome(){
            String palInput = "racecars";
            String expectedOutput = "Not a palindrome";
            String output = com.sparta.Test.palTest(Converter.palConvert(palInput));
            assertEquals(expectedOutput, output);
            // Test Passed
```

Afterwards, parameterised tests were used to test inputs with upper and lower case letters,
spaces and special characters. All of the tests passed!

```Java
    @ParameterizedTest
    @ValueSource(strings = {"raCEcaR", "AnnA", "rADar"})
    @DisplayName("Given a user enters a Palindrome in different cases, the Palindrome Checker returns It's a palindrome")
    public void giveAUserEntersAPalindromeInDifferentCases_PalindromeChecker_ReturnsItsAPalindrome(String palInput){
        String expectedOutput = "It's a palindrome";
        String output = com.sparta.Test.palTest(Converter.palConvert(palInput));
        assertEquals(expectedOutput, output);

    } // Test Passed

    @ParameterizedTest
    @ValueSource(strings = {"***raCEcaR !$&", "An$^&nA", "rADa*&^r"})
    @DisplayName("Given a user enters a Palindrome in different cases, the Palindrome Checker returns It's a palindrome")
    public void giveAUserEntersAPalindromeWithSpacialCharacters_PalindromeChecker_ReturnsItsAPalindrome(String palInput) {
        String expectedOutput = "It's a palindrome";
        String output = com.sparta.Test.palTest(Converter.palConvert(palInput));
        assertEquals(expectedOutput, output);
    }
    @ParameterizedTest
    @ValueSource(strings = {"ra   CE ca R", " An  nA ", "rA   Dar"})
    @DisplayName("Given a user enters a Palindrome in different cases, the Palindrome Checker returns It's a palindrome")
    public void giveAUserEntersAPalindromeWithSpaces_PalindromeChecker_ReturnsItsAPalindrome(String palInput) {
        String expectedOutput = "It's a palindrome";
        String output = com.sparta.Test.palTest(Converter.palConvert(palInput));
        assertEquals(expectedOutput, output);
    } //Test Passed
```
## Retrospective

Even though this code incorporates the single responsibility principle, it still uses
public classes so it is not necessarily closed for modification. There is also no method
for validating what inputs the user enters. The code will run and accept something messy
such as "123RaCe      Ca££££R" as a palindrome when in reality it does not look like
a palindrome at all.
