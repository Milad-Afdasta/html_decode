# html_decode
Decode html into a multidimensional array. 

Please keep in mind, this is a very basic decoder, and does not take into account the complex variations that exists in html.

Currently the code only extracts the outer html of each element, which was the original objective. The code can be easily expanded to add more meta data, search options, or any other functionality. For example, it can easily be expanded to extract elements like img, br, hr, etc...

I've added a simple search feature that allows you to search for class or by id. This can be expanded on, to make it more functional.

Using it is very simple:

$html = html_decode(file_get_contents('example.html'));

The result will return an array with two elements:

$html['data'] // holds the decoded html
$html['search'] // holds class|id keys that reference their element(s)

By default, the code only looks for div and forms, but you can overwrite this by simply sending the tag values you want to search for, separated by the pipe ("|") character.
For example: div|td|table|form|span

By default, the code only looks for id and class attributes, you can send it your own attribute(s), separated by the pipe ("|") character.

Tags: PHP, preg_match_all, preg_match, regular expression, regex, html, parse, decode, html_decode, json_decode

<b>Input:</b>

```html
<html>
<body>
    <div class="main">
        Hello
        <div class="1">
            <div class="2">
                World!
            </div>
            <div class="2">
                <div class="3">
                Next Level
                </div>
            </div>
        </div>
    </div>
    <div class="two">
        three
    </div>
</body>
</html> 
```

<b>Output</b>

<img src="http://i.imgur.com/3H5INF1.png" />
