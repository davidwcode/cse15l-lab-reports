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