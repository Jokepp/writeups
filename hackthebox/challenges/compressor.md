<link rel="stylesheet" type="text/css" href="includes/style.css">

<h1>Writeup "Compressor"</h1>

<h2>Quick Info</h2>

<table>
	<tr><td><b>site</b></td><td>Hack the box</td></tr>
	<tr><td><b>url</b></td><td><a href="https://app.hackthebox.com/challenges/compressor">https://app.hackthebox.com/challenges/compressor</a></td></tr>
	<tr><td><b>discussion</b></td><td><a href="https://forum.hackthebox.com/t/official-compressor-discussion">https://forum.hackthebox.com/t/official-compressor-discussion</a></td></tr>
	<tr><td><b>type</b></td><td>challenge/misc</td></tr>
	<tr><td><b>difficulty&nbsp;&nbsp;&nbsp;</b></td><td>very easy</td></tr>
	<tr><td><b>startdate</b></td><td>2022-08-03</td></tr>
	<tr><td><b>enddate</b></td><td>2022-08-04</td></tr>
</table>

<h2>Description</h2>

<blockquote>Ramona's obsession with modifications and the addition of artifacts to her body has slowed her down and made her fail and almost get killed in many missions. For this reason, she decided to hack a tiny robot under Golden Fang's ownership called "Compressor", which can reduce and increase the volume of any object to minimize/maximize it according to the needs of the mission. With this item, she will be able to carry any spare part she needs without adding extra weight to her back, making her fast. Can you help her take it and hack it?</blockquote>

<h2>Solution</h2>

<p>We receive an IP and port to a server. When we access the server using a web browser, we see the error output of a program:</p>

<img src="includes/compressor-01.png" />

<p>The problem seems to be that the program asks for a user input and receives an HTTP GET request. A web search reveals that the used PHP <code>input()</code> function has vulnerabilities [1], but they seem not to be exploitable in this case.</p>

<p>Next, we try to telnet to the given IP and port and receive a working interactive version of the program. The program allows us to select a body part and then displays a menu with different manipulation options:</p>

<img src="includes/compressor-02.png" />

<p>Given in brackets are the shell commands that are executed for each option. Option 4 seems usable for searching though the file system, but it actually only returns to the first screen. Option 3 however gives us access to the <code>zip</code> command and allows us to specify options. This looks promising and we start studying the man page for <code>zip</code>. The first helpful option we find is <code>-sf</code>:</p>

<blockquote><b>-sf</b><br />
<b>--show-files</b> Show the files that would be operated on, then exit. For instance, if creating a new rchive, this will list the files that would be added.</blockquote>

<p>Using this option and <code>-r</code> for traveling the directory structure, we can look through the file system:</p>

<img src="includes/compressor-03.png" />

<p>We discovered the location of <code>flag.txt</code>! Now how to read its content?</p>

<p>In the <code>zip</code> man page, we find another useful command line option:</p>

<blockquote><b>-T</b><br />
<b>--test</b> Test the integrity of the new zip file. If the check fails, the old zip file is unchanged and (with the -m option) no input files are removed. <br />
<b>-TT</b><br />
<b>--unzip-command</b> Use command cmd instead of 'unzip -tqq' to test an archive when the -T option is used. On Unix, to use a copy of unzip in the current directory instead of the standard system unzip, could use: zip archive file1 file2 -T -TT "./unzip -tqq"<br />
<br />
In cmd, {} is replaced by the name of the temporary archive, otherwise the name of the archive is appended to the end of the command. The return code is checked for success (0 on Unix). </blockquote>

<p>Using this knowledge, we can zip the flag.txt file and then call the <code>cat</code> command to print out the zip file after creation. If we choose this way, we also have to include the <code>-0</code> option so that the zip file is not compressed:</p>

<img src="includes/compressor-04.png" />

<p>This prints out the flag!</p>

<pre>
HTB{z1pp1ti_z0pp1t1_GTFO_0f_my_pr0p3rty}
</pre>

<h2>Sources</h2>

<ol id="sources">
	<li><a href="https://www.geeksforgeeks.org/vulnerability-input-function-python-2-x/">https://www.geeksforgeeks.org/vulnerability-input-function-python-2-x/</a></li>
	<li><a href="https://linux.die.net/man/1/zip">https://linux.die.net/man/1/zip</a></li>
</ol>
