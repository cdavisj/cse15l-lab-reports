# Troubleshooting Markdown Parser
The starter code from this project was produced in [this](https://www.youtube.com/watch?v=_y9hkrN9k3w) video. This produced the following starter code.

```java
// File reading code from https://howtodoinjava.com/java/io/java-read-file-to-string-examples/
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.ArrayList;

public class MarkdownParse {
    public static ArrayList<String> getLinks(String markdown) {
        ArrayList<String> toReturn = new ArrayList<>();
        // find the next [, then find the ], then find the (, then take up to
        // the next )
        int currentIndex = 0;
        while(currentIndex < markdown.length()) {
            int nextOpenBracket = markdown.indexOf("[", currentIndex);
            int nextCloseBracket = markdown.indexOf("]", nextOpenBracket);
            int openParen = markdown.indexOf("(", nextCloseBracket);
            int closeParen = markdown.indexOf(")", openParen);
            toReturn.add(markdown.substring(openParen + 1, closeParen));
            currentIndex = closeParen + 1;
        }
        return toReturn;
    }
    public static void main(String[] args) throws IOException {
		Path fileName = Path.of(args[0]);
	    String contents = Files.readString(fileName);
        ArrayList<String> links = getLinks(contents);
        System.out.println(links);
    }
}
```

The goal is to parse a markdown file for links. This program, although works for some simple cases, has a issues with addressing any sort of edge cases. In this report, I will go over some of the fixes my lab group came up with.

## Fix 1
[comment]: <> (Show a screenshot of the code change diff from Github)
First thing that we came across was the program failes when the markdown file does not end with a link. This issue seemed to be coming from the program infinitely trying to find more links when `currentIndex` updated after a link and had more content to run the program over, but there were no more links to parse. To fix this issue, we added a simple if statement to check if there are anymore links to parse after we finish each link. This can be seen in the following diff.

![fix-1-diff](fix-1-diff.png)

[comment]: <> (Link to the test file for a failure-inducing input that prompted you to make that change)
We thought to make this change after testing the initial program with the following markdown file.

```markdown
[a link 1](https://google.com)
text after link with no link after it
```

[comment]: <> (Show the symptom of that failure-inducing input by showing the output of running the file at the command line for the version where it was failing (this should also be in the commit message history))
The output for this program was 

![fail-1-symptom](fail-1-symptom.png)

[comment]: <> (Write 2-3 sentences describing the relationship between the bug, the symptom, and the failure-inducing input.)
After encountering this symptom, we were quickly able to identify that it broke from not having more quotes, but instead other types of content, after a quote. This happened because the `indexOf` method being used could not find more instances of the string `[` when prompted. In this case the method returns `-1` which was not checked for.

## Fix 2
[comment]: <> (Show a screenshot of the code change diff from Github)

[comment]: <> (Link to the test file for a failure-inducing input that prompted you to make that change)

[comment]: <> (Show the symptom of that failure-inducing input by showing the output of running the file at the command line for the version where it was failing (this should also be in the commit message history))

[comment]: <> (Write 2-3 sentences describing the relationship between the bug, the symptom, and the failure-inducing input.)

## Fix 3
[comment]: <> (Show a screenshot of the code change diff from Github)

[comment]: <> (Link to the test file for a failure-inducing input that prompted you to make that change)

[comment]: <> (Show the symptom of that failure-inducing input by showing the output of running the file at the command line for the version where it was failing (this should also be in the commit message history))

[comment]: <> (Write 2-3 sentences describing the relationship between the bug, the symptom, and the failure-inducing input.)
