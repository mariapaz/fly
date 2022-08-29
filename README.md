# Validation and sanitization challenge

Web applications commonly receive user input and display it on the page. For example a website like Twitter lets people submit messages that are then displayed to other people. It's very important to make sure you are receiving the right information, and that it does not contain any malicious code.

## Validation

Your server will receive submissions that are missing information, or include information that is not valid. You cannot rely on _client-side_ validation (like the `required` attribute on input elements), as this can easily be disabled with the browser dev tools. There are also lots of other ways to submit POST requests (e.g. using `curl` in the terminal).

You must _always_ validate user submitted information on the server, otherwise your app can end up in an invalid state. E.g. if somebody signs up for an account but fails to provide a password, they could end up with an account that anyone can sign into.

Validation can be as simple as a series of `if` statements that return error responses to the user telling them what they did wrong. In a larger app you would probably user a validation library to make this easier.

### Validation challenge

## Sanitization

Displaying user-submitted information to other users can be dangerous. It's easy to end up vulnerable to an XSS attack ("Cross-Site Scripting"). This is where a malicious user puts some code inside a submitted string, hoping it will be displayed to another user, causing the browser to execute it.

For example if I submitted `hello <script>alert("I am a hacker")</script>` in a text input, and if the server naively embedded that on the page like this:

```html
<h2>Comments</h2>
<ul>
  <li>Normal comment</li>
  <li>
    hello
    <script>
      alert("I am a hacker");
    </script>
  </li>
</ul>
```

The browser would execute that script tag. This is _super_ dangerous as JavaScript running on the page can do quite a lot of damage (steal private information, mine Bitcoin etc).

You must _always_ sanitize any user input that you want to template into an HTML page. A simple way to do this is replacing any characters that will trigger the browser's HTML-parsing, so that it instead treats the value as a regular string. For example replacing the `<` characters with the `&lt;` ("less-than") HTML entity will stop the browser executing a script tag, but will display the same.

Note that the simple approach is not enough stop an attacker—in a production app you should use a well-tested library for sanitization. Templating engines like Handlebars/React/Vue etc all handle sanitization automatically. We need to do it manually here as we are just using JS template literal strings.

### Sanitization challenge
