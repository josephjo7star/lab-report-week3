# Part 1:
```ruby
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    String str = "";
    boolean check = false;
    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format("Please enter some strings %s", str);
        } else {
            System.out.println("Path: " + url.getPath());
            if (url.getPath().contains("/add")) {
                String[] parameters = url.getQuery().split("s=");
                    if (parameters[1].equals("anewstringtoadd")){
                        str = "";
                        check = true;
                        return "now you can add stuff";
                    }
                    else if (check == true){
                        str = parameters[1]+ " " + str;
                        return "now you can search stuff";
                    }
                    else
                        return "you can't do that";
            }
            if (url.getPath().contains("/search")){

                return String.format("New String is now: %s", str);

            }
            return "404 Not Found!";
        }
    }
}

class SearchEngine {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
```
![screenshot1](https://github.com/josephjo7star/lab-report-week3/blob/main/1.png)
For this screnshot, I had just enter the link to the website, so it displays the default message, which is "please enter some strings"




