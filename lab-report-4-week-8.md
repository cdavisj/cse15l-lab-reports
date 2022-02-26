# Introduction
Three different tests will be used for two different implementations of the markdown parse program. The two programs being used today are [my groups implementation](https://github.com/cdavisj/markdown-parse) and [another groups implementation](https://github.com/floatboat/markdown-parse) which we reviewed previously. Following is the file snippets in which I will be testing here, as well as a rendered preview showing us where links are actually registered based on the markdown language.

### Snippet 1
```markdown
`[a link`](url.com)

[another link](`google.com)`

[`cod[e`](google.com)

[`code]`](ucsd.edu)
```

![snippet 1 preview](images/snippet1-preview.png)

A link is registered for `` `google.com``, `google.com`, and `ucsd.edu`.

### Snippet 2
```markdown
[a [nested link](a.com)](b.com)

[a nested parenthesized url](a.com(()))

[some escaped \[ brackets \]](example.com)
```

![snippet 2 preview](images/snippet2-preview.png)

A link is registered for `a.com`, `a.com(())`, and `example.com`.

### Snippet 3
```markdown
[this title text is really long and takes up more than 
one line

and has some line breaks](
    https://www.twitter.com
)

[this title text is really long and takes up more than 
one line](
    https://ucsd-cse15l-w22.github.io/
)


[this link doesn't have a closing parenthesis](github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



)

And then there's more text
```

![snippet 3 preview](images/snippet3-preview.png)

A link is registered for `https://www.twitter.com`, `https://ucsd-cse15l-w22.github.io/`, and `https://cse.ucsd.edu/`.

# Results
### My Program
```java
@Test
public void testSnippet1() throws IOException, NoSuchFileException {
    ArrayList<String> correctOutput = new ArrayList<>();
    correctOutput
            .addAll(Arrays.asList("`google.com", "google.com", "ucsd.edu"));
    Path fileName = Path.of("test-files/snippet1.md");
    String contents = Files.readString(fileName);
    ArrayList<String> links = MarkdownParse.getLinks(contents);
    assertEquals(correctOutput, links);
}

@Test
public void testSnippet2() throws IOException, NoSuchFileException {
    ArrayList<String> correctOutput = new ArrayList<>();
    correctOutput
            .addAll(Arrays.asList("a.com", "a.com(())", "example.com"));
    Path fileName = Path.of("test-files/snippet2.md");
    String contents = Files.readString(fileName);
    ArrayList<String> links = MarkdownParse.getLinks(contents);
    assertEquals(correctOutput, links);
}

@Test
public void testSnippet3() throws IOException, NoSuchFileException {
    ArrayList<String> correctOutput = new ArrayList<>();
    correctOutput.addAll(Arrays.asList("https://www.twitter.com",
            "https://ucsd-cse15l-w22.github.io/", "https://cse.ucsd.edu/"));
    Path fileName = Path.of("test-files/snippet3.md");
    String contents = Files.readString(fileName);
    ArrayList<String> links = MarkdownParse.getLinks(contents);
    assertEquals(correctOutput, links);
}
```

```
There were 3 failures:
1) testSnippet1(MarkdownParseTest)
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet1(MarkdownParseTest.java:116)
2) testSnippet2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com((, example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet2(MarkdownParseTest.java:127)
3) testSnippet3(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://www.twitter.com, https://ucsd-cse15l-w22.github.io/, https://cse.ucsd.edu/]> but was:<[]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet3(MarkdownParseTest.java:138)

FAILURES!!!
Tests run: 12,  Failures: 3
```

### Other Program
```java
@Test
public void testSnippet1() throws IOException, NoSuchFileException {
    ArrayList<String> correctOutput = new ArrayList<>();
    correctOutput
            .addAll(Arrays.asList("`google.com", "google.com", "ucsd.edu"));
    Path fileName = Path.of("snippet1.md");
    String[] contents = Files.readString(fileName).split("\n");
    ArrayList<String> links = MarkdownParse.getLinks(contents);
    assertEquals(correctOutput, links);
}

@Test
public void testSnippet2() throws IOException, NoSuchFileException {
    ArrayList<String> correctOutput = new ArrayList<>();
    correctOutput
            .addAll(Arrays.asList("a.com", "a.com(())", "example.com"));
    Path fileName = Path.of("snippet2.md");
    String[] contents = Files.readString(fileName).split("\n");
    ArrayList<String> links = MarkdownParse.getLinks(contents);
    assertEquals(correctOutput, links);
}

@Test
public void testSnippet3() throws IOException, NoSuchFileException {
    ArrayList<String> correctOutput = new ArrayList<>();
    correctOutput.addAll(Arrays.asList("https://www.twitter.com",
            "https://ucsd-cse15l-w22.github.io/", "https://cse.ucsd.edu/"));
    Path fileName = Path.of("snippet3.md");
    String[] contents = Files.readString(fileName).split("\n");
    ArrayList<String> links = MarkdownParse.getLinks(contents);
    assertEquals(correctOutput, links);
}
```

```
There were 3 failures:
1) testSnippet1(MarkdownParseTest)
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com, ucsd.edu]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet1(MarkdownParseTest.java:42)
2) testSnippet2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com((, example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet2(MarkdownParseTest.java:53)
3) testSnippet3(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://www.twitter.com, https://ucsd-cse15l-w22.github.io/, https://cse.ucsd.edu/]> but was:<[, , ]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet3(MarkdownParseTest.java:64)

FAILURES!!!
Tests run: 7,  Failures: 3
```

# Questions and Answers
### 1) Do you think there is a small (<10 lines) code change that will make your program work for snippet 1 and all related cases that use inline code with backticks? If yes, describe the code change. If not, describe why it would be a more involved change.
### A: Yes! I just need to add an if-statement check for backtick characters and skip over the inline code blocks enclosed. Which it follows that any brackets enclosed in a code block will not count towards the parsing of the a link.

### 2) Do you think there is a small (<10 lines) code change that will make your program work for snippet 2 and all related cases that nest parentheses, brackets, and escaped brackets? If yes, describe the code change. If not, describe why it would be a more involved change.
### A: This one may be a little more difficult to manage, although it is clear what the issue is. My program does not account for multiple sets of parentheses in the link. I need to account for new open and close parentheses and take all characters up to the final closing parenthesis that actually ties to the opening one for the link. This can potentially be done with a separate stack (or the same if it doesn't confuse things, since my program already uses a stack) to track the parenthesis hierarchy.

### 3) Do you think there is a small (<10 lines) code change that will make your program work for snippet 3 and all related cases that have newlines in brackets and parentheses? If yes, describe the code change. If not, describe why it would be a more involved change.
### A: Yes! I believe this issue can be fixed with better link trimming once I believe I have encountered a link. I think I am not trimming outter whitespace and newlines at all in the current version of markdown parse.