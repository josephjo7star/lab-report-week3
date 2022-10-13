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
## Screenshots for part 1:
***
![screenshot1](https://github.com/josephjo7star/lab-report-week3/blob/main/1.png)

There are two methods(handleRequest, and main). For this screnshot, both methods were ran. I had just enter the link to the website, so it displays the default message, which is "please enter some strings"

***
![screenshot2](https://github.com/josephjo7star/lab-report-week3/blob/main/2.png)

For this screenshot, handleRequest method was called. I added a query and enters "anewstringtoadd" which stores "anewstringtoadd" to parameter[1], which changed the boolean "check" to "true", and that tells the program that whatever strings I entered next will be added to the string. The program then displays "now you can add stuff"

***
![screenshot3](https://github.com/josephjo7star/lab-report-week3/blob/main/3.png)

For this screenshot, handleRequest was called. I enters apple and the programs adds "apple" to its string "str", now it displays that I can search it and see what is in the string

***
![screenshot4](https://github.com/josephjo7star/lab-report-week3/blob/main/4.png)

handleRequest was still called for this screenshot. I refresh the page 3 times which adds "apple" three more times to "str", I then enters "/search" and now the program displays the string

# Part 2:
### bug 1 input
```ruby
@Test
  public void testReverse() {
    int[] a = {2, 3, 5, 7};
    assertArrayEquals(new int[]{7, 5, 3, 2}, ArrayExamples.reversed(a));
  }
```
### bug 1 symptom
```ruby
3) testReverse(ArrayTests)
arrays first differed at element [0]; expected:<7> but was:<0>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReverse(ArrayTests.java:30)
        ... 30 trimmed
Caused by: java.lang.AssertionError: expected:<7> but was:<0>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 36 more
```
### bug 1 original code
```ruby
 static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
```
### bug 1 fixed code
```ruby
 static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }
```
### bug 1 explaination
so this code is buggy because it created a new array and did not put anything in it. Therefore, all the values will be set to 0 as a default. But then the program assigns new values to the orginal array "arr" with the values of the new array, which will just all be 0s. To fixed this, I changed the position so that values was assigned to the new array from the original array, and then return the new array.

### bug 2 input
```ruby
 @Test 
	public void testReverseInPlace2() {
		ArrayList<String> result1 = new ArrayList<String>();
		ArrayList<String> result2 = new ArrayList<String>();
		result1.add("apple");
		result2.add("banana");
		result1.add("cat");
		result2.add("dog");
		
		ArrayList<String> test = new ArrayList<String>();
		test.add("apple");
		test.add("banana");
		test.add("cat");
		test.add("dog");
		assertEquals(test, ListExamples.merge(result1, result2));
	}
  }
```
### bug 2 symptom
```ruby
1) testReverseInPlace2(ArrayTests)
java.lang.OutOfMemoryError: Java heap space
        at java.base/java.util.Arrays.copyOf(Arrays.java:3512)
        at java.base/java.util.Arrays.copyOf(Arrays.java:3481)
        at java.base/java.util.ArrayList.grow(ArrayList.java:237)
        at java.base/java.util.ArrayList.grow(ArrayList.java:244)
        at java.base/java.util.ArrayList.add(ArrayList.java:454)
        at java.base/java.util.ArrayList.add(ArrayList.java:467)
        at ListExamples.merge(ListExamples.java:42)
        at ArrayTests.testReverseInPlace2(ArrayTests.java:47)
        at java.base/java.lang.invoke.LambdaForm$DMH/0x0000000801012400.invokeVirtual(LambdaForm$DMH)
        at java.base/java.lang.invoke.LambdaForm$MH/0x0000000801013000.invoke(LambdaForm$MH)
        at java.base/java.lang.invoke.Invokers$Holder.invokeExact_MT(Invokers$Holder)
```
### bug 2 original codes
```ruby
static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index1 += 1;
    }
    return result;
  }
```
### bug 2 fixed codes
```ruby
static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    List<String> result2 = new ArrayList<>();
    int index1 = 0, index2 = 0;

    while(index1 < list1.size()) {
    result.add(list1.get(index1));
    index1 += 1;
    }
    while(index2 < list2.size()) {
    result.add(list2.get(index2));
    index2 += 1;
    }
    int index3 = 0;
    String temp = result.get(0);
    int size = result.size();

    while(index3 < size){
      for(int i = 0; i <result.size(); i ++) {
        if (temp.compareTo(result.get(i)) > 0){
          temp = result.get(i);
        } 
      }
      result2.add(temp);
      result.remove(temp);
      if (result.size() != 0)
        temp = result.get(0);
      index3 ++;
    }
    return result2;
  } 
```
### bug 2 explaination
This program checks for both index1 and index2 and they must be both equal to the length of their respective list in order for the loop to stop running. However, if we finished sorting list1 but not with list2, this loop cannot stop and will run forever because there isn't any chances to increase index2 to make the conditional false. What I do to fix it is that I first load every elements to a new arraylist, then I sort the arraylist in order and put them into another new arraylist. Then I return arraylist2.
