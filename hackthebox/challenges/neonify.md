# Writeup "Neonify"

## Quick Info

<table>
   <tr><td><b>site</b></td><td>Hack the Box</td></tr>
   <tr><td><b>url</b></td><td>https://app.hackthebox.com/challenges/neonify</td></tr>
   <tr><td><b>discussion</b></td><td>https://forum.hackthebox.com/t/official-neonify-discussion</td></tr>
   <tr><td><b>type</b></td><td>challenge/web</td></tr>
   <tr><td><b>difficulty&nbsp;&nbsp;&nbsp;</b></td><td>easy</td></tr>
   <tr><td><b>startdate</b></td><td>2022-08-07</td></tr>
   <tr><td><b>enddate</b></td><td>2022-08-08</td></tr>
</table>

## Description

> It's time for a shiny new reveal for the first-ever text neonifier. Come test out our brand new website and make any text glow like a lo-fi neon tube!

## Solution

We receive an IP and port to a server and a zip file with a _Ruby_ application; this is the same application which is deployed on the server. In the browser we are presented with a website that takes a string and displays it in a neon style:

<p align="center">
   <img src="includes/neonify-01.png" />
</p>

Analyzing the source code, we find that the text provided by the user is checked using a _regular expression_. A web search for _"ruby regex insecure"_ reveals an important particuliarity in the Ruby regex syntax[^1]:

> Ruby's regular expression syntax has some minor differences when compared to other languages. In Ruby, the `^` and `$` anchors do not refer to the beginning and end of the string, rather the beginning and end of a line.
> 
> This means that if you're using a regular expression like `/^[a-z]+$/` to restrict a string to only letters, an attacker can bypass this check by passing a string containing a letter, then a newline, then any string of their choosing.
> 
> If you want to match the beginning and end of the entire string in Ruby, use the anchors `\A` and `\z`.

This is exactly how the check is done in this application:

``` Ruby
post '/' do
  if params[:neon] =~ /^[0-9a-z ]+$/i
    @neon = ERB.new(params[:neon]).result(binding)
  else
    @neon = "Malicious Input Detected"
  end
  erb :'index'
end
```

Therefore, we need a way to submit a string containing a new line character to the web app. After some research, our payload string is:

``` Ruby
a
<%= `cat flag.txt` %>
```

The first line serves only to fulfill the regex check. The second line contains the payload and is composed as follows:

 - We create a Ruby environment using `<%=` and `%>`.
 - Strings enclosed by backticks (`` ` ``) are executed by Ruby as shell commands.
 - `cat flag.txt` will output the flag.

It is not possible to submit this string directly in the application's frontend, so we use `curl`; a quick web search tells us how to send a HTTP POST request[^2]. In doing this, it is important to URL enocode the payload string; this is done using a free encoder web app[^3].

The finished command is entered as follows. `$PREFIX`, `$COMMAND`, `$IP`, and `$PORT` are temporary shell variables for easier trial-and-error:

<p align="center">
   <img src="includes/neonify-02.png" />
</p>

The flag is therefore:

```
HTB{r3pl4c3m3n7_s3cur1ty}
```

### Sources

[^1]: https://docs.ruby-lang.org/en/2.1.0/security_rdoc.html
[^2]: https://javarevisited.blogspot.com/2015/10/how-to-send-http-request-from-unix-or-linux-curl-wget-example.html
[^3]: https://www.urlencoder.org/
