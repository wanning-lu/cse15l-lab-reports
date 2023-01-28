# Lab 2: Servers and Bugs

## Creating and hosting your own web server 

Today, you'll be learning how to host your own web server that responds to different queries with Java! To start, here is the code for an example server I created called StringServer that takes the query from a request and concatenates it with a string. 

```java
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {

    String response = new String();

    public String handleRequest(URI url) {

        if (url.getPath().contains("/add-message")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")) {
                response += "\n" + parameters[1];
                return response;
            }
        } 
        return "404 error :(";
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

Line by line, here are what each piece of code does:
- Import statements: While the IOException handles cases where the user starts the server with something other than a port number, java.net.URI provides the API for handling URLS and parsing them.
- ```String response = new String();```: Creates the string variable that stores all of the queries.
- ```public String handleRequest(URI url) {...}```: This runs in an infinite loop, listening for new requests being made (somebody accessing the server through a URL)
- Code within ```handleRequest(URI url)```: This method specifically looks at the path and query string that follows host name. To learn more about URLs, you can go [here][2]. If the path contains the string ```"/add-message"```, the second query (the value in a name and value pair, separated by a '=') is added to the ```response``` string. Lastly, the web server prints out the current string variable for the visitor.
- ```class StringServer {...}```: Using the Server class (not included), this starts up a web server with a specified port number.

[2]: https://www.ibm.com/docs/en/cics-ts/5.1?topic=concepts-components-url

How does our web server look when it's up and running? After running
```
javac *.java
java StringServer 4000
```
you can visit your web server on [http://localhost:4000](http://localhost:4000). You'll notice that you'll get a 404 error–this is because we haven't included a path in our URL! First, I'm going to add the strings "Hey there!" and "How are you doing" to our string variable.

![String requests](images/lab2-1.png)

Even though we made multiple requests, the variable stored all of the queries we entered. ```handleRequest(URI url)``` is called for both of these requests, and the URL that we typed in the search bar is passed through the method. Since we entered strings for our queries, they are not changed as they are added on to a string variable. So what happens if we enter values that aren't strings?

![Number request](images/lab2-2.png)

When we input an integer value for our query, ```handleRequest(URI url)``` is called again with our URL. However, this time, the integer value is converted from an integer to a string and displayed on the page.

## Debugging

Debugging is an inevitable step of programming. However, in Java, there is a testing framework called JUnit which you can use to continuously test your code with less effort. In this example, I'll be testing a buggy program called ArrayExamples with my tester program, ArrayTester.

```java
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
}
```
*The function that we are testing*

To use JUnit, you must have the .jar files for Hamcrest and JUnit; afterwards, you can put them into a folder named "libs". Don't forget to configure your classpath (```Cmd + shift + p```) in VSCode so that it recognizes the JUnit statements!

Here's an example test for our program:
```java
@Test 
public void testReverseInPlace() {
    int[] input1 = {3 2 3};
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{3 2 3}, input1);
}
```

With how JUnit works, it's important to denote our tests with an ```@Test``` at the top of the test function. Next, the important piece is the function ```assertArrayEquals```. There are other versions of this function, but essentially, this allows us to manually check the outputs of our functions. Here, we're testing to ensure that ```reverseInPlace()``` actually overwrites the array with the elements in a reverse order. We can write another test to thoroughly test our outputs:
```java
@Test
public void testReverseInPlaceTwo() {
    int[] input1 = {1, 2, 3, 4};
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{4, 3, 2, 1}, input1);
}
```

If we run the test with
```
javac -cp ../libs/junit-4.13.2.jar:../libs/hamcrest-2.2.jar:. *.java 
java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ArrayTests
```
we'll get this result:

![Tests passed and symptoms](images/lab2-3.png)

We can see that while ```testReverseInPlace()``` passed, ```testReverseInPlaceTwo()``` did not. In ```testReverseInPlaceTwo()```, the outputs matched until the 2nd index; whereas we expected {4, 3, 2, 1}, the program outputted {4, 3, 3, [some number]}. When we take a closer look at ArrayExamples, we notice the bug that causes this issue:

```java
arr[i] = arr[arr.length - i - 1];
```
After the i reaches the middle of the array, it begins setting the ith position of the array with the *overwritten* elements from the beginning of the array, rather than from the original array. To fix this, we can create a new array that stores the original:
```java
int[] originalArray = new int[arr.length];
for (int i = 0; i < arr.length; i++) {
    originalArray[i] = arr[i];
}
```
Notice that we use a deep copy, rather than a shallow copy, as a shallow copy would result in the same symptoms as before. Now, we change the line such that
```java
arr[i] = originalArray[arr.length - i - 1]
```

If we save this and run our tests again, we'll notice that all of our tests pass!
![Tests passed](images/lab2-4.png)

## What I learned
The newest thing that I encountered these past weeks was coding and setting up my own web server. I already knew the basics of HTML/CSS/JS, but I didn't know that I could make and host a rudimentary server with Java. Additionally, I learned about the Server class (the one provided for lab). Since it was abstracted by the libraries that were used, the code was relatively easy to understand–for example, I understood how within the Server class, it took the output given by ```handleRequest()``` to display it on the webpage. However, the intracacies of how the server is actually started and ran is still a mystery to me, as I still don't know how HttpServer is implemented.