# Weeks 2 and 3 Lab Report (10/14/22)

This lab report contains two parts:
* Displaying the functionality of week 2's search engine
* Bug debugging from week 3

___

## Part 1
To start off, here is the code from my ```SearchEngine.java``` file:

```java
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

public class SearchEngine implements URLHandler {
    ArrayList<String> database = new ArrayList<String>();

    @Override
    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            String str = "Use \"/add?s=\" to add to the database \n and use \"/search?s=\" to search the database";
            if (database.size() > 0) {
                str += "\n Current database: \n";
                for (int i = 0; i < database.size(); i ++) {
                    str += database.get(i) + "\n";
                }
            }
            return str;
        } else if (url.getPath().contains("/add")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")) {
                if (parameters.length < 2) {
                    return "Please add a string!";
                } else if (!database.contains(parameters[1])) {
                    database.add(parameters[1]);
                    return "Added \"" + parameters[1] + "\" to the database";
                }
                return "Entry already exists!";
                
            }
        } else if (url.getPath().contains("/search")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")) {
                if (parameters.length < 2) {
                    return "Please search for a string!";
                } 
                String results = "";
                for (int i = 0; i < database.size(); i ++) {
                    if (database.get(i).contains(parameters[1])) {
                        results += database.get(i) + "\n";
                    }
                }
                if (results.equals("")) {
                    return "No results found";
                }
                return results;
            }
            
        }
        return "404 Not Found!";
    }

    
    
}

class SearchMain {    
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        Server.start(Integer.parseInt(args[0]), new SearchEngine());
    }
}
```
Here's the home page when the site is first launched:
![Image](/images/lab2_3/launch.png)
This calls ```handleRequest``` and since the current argument (url path) is equivalent to ```/```, the first if-statement is triggered and the method returns a string with information about how to use the site and all the elements in the ```database``` field (```database``` is currently empty).

Here's an image of the ```/add``` path:
![Image](/images/lab2_3/firstadd.png)
When ```handleRequest``` is called, the ```/add``` directs it into the second if-statement and the query ```s=first``` is split. The "first" is then extracted and added to ```database```, and a string confirming the addition is returned.

This is the home page after several other additions:
![Image](/images/lab2_3/thirdhome.png)
Again, the return string includes the same instructions and the contents of ```database``` (which is no longer empty).

Finally, here's an image of the search function:
![Image](/images/lab2_3/searchfirst.png)
The ```/search``` path means that the third if-statement in ```handleRequest``` is triggered and the query string "f" is tested against all elements of ```database```. If an element contains the query search, it is added to the return string and displayed.

---
## Part 2
These are two bugs I encountered in the files from Lab 3.

### Bug 1

The first one was from the ```filter``` method in ```ListExamples.java```, the purpose of which was to return a new list that had all the elements of the input list for which a ```StringChecker```(interface that checks strings for certain conditions) returns true in the same order they appeared in the input list.

Here was the failure-inducing input (test code from ```ListTests.java```):
```java
 @Test
    public void filterTest() {
        List<String> input = new ArrayList<>();
        input.add("pass");
        input.add("no");
        input.add("sdf");
        input.add("pass2");
        Checker check = new Checker();
        List<String> output = new ArrayList<>();
        output.add("pass");
        output.add("pass2");

        assertEquals(output, ListExamples.filter(input, check));
    }
```
Here is the symptom I was greeted with upon testing:
![Image](/images/lab2_3/listoutput.png)
As shown, ```filter``` is returning a list with the elements in the wrong order. 

For reference, here is the code for ```filter```:
```java
static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
  }
```
The specific bug is in this line:
```java
result.add(0, s);
```
Because it specifies that ```s``` should be added at the 0 index instead of the end, it reverses the order of the elements.
Here is the amended line:
```java
result.add(s);
```
This adds ```s``` at the end of the returned list and preserves the order in which the chosen elements initially appeared.

### Bug 2

Another bug occured in the ```append``` method of ```LinkedListExample.java```. Like its name suggests, the expectation was to add an element to the end of the linked list.

Here was the failure-inducing input (test code from ```LinkedListTest.java```):
```java
@Test
    public void appendTest() {
        LinkedList list = new LinkedList();
        list.append(1);
        list.append(1);
        list.append(5);
        int[] output = {1, 1, 5};
        Node node = list.root;
        for (int i = 0; i < 3; i ++) {
            assertEquals(output[i], node.value);
            node = node.next;
        }
    }
```
Here's the symptom:

![Image](/images/lab2_3/infiniteloop.png)

Obviously it is not apparent from the picture, but the test procedure was never finishing. It appeared that the code was getting stuck.

For reference, here is the code for ```append```:
```java
public void append(int value) {
        if(this.root == null) {
            this.root = new Node(value, null);
            return;
        }
        // If it's just one element, add if after that one
        Node n = this.root;
        if(n.next == null) {
            n.next = new Node(value, null);
            return;
        }
        // Otherwise, loop until the end and add at the end with a null
        while(n.next != null) {
            n = n.next;
            n.next = new Node(value, null);
        }
    }
```
The bug can be found in the final while loop:
```java
while(n.next != null) {
    n = n.next;
    n.next = new Node(value, null);
}
```
The loop will stop when 
```java 
n.next == null
```
but because the loop is constantly adding a new node with the new value to the ```next``` field of the current node, ```n.next``` would always have an existent node, and thus the loop would never break.

Here is the fix:
```java
while(n.next != null) {
    n = n.next;          
}
n.next = new Node(value, null);
```
By moving the final line out of the loop, the new value is not added until the loop has traversed to the end of the linked list.

---

Okay, all done.