# Writeup "The Needle"

## Quick Info

<table>
   <tr><td><b>site</b></td><td>Hack the Box</td></tr>
   <tr><td><b>url</b></td><td>https://app.hackthebox.com/challenges/the-needle</td></tr>
   <tr><td><b>discussion</b></td><td>https://forum.hackthebox.com/t/official-the-needle-discussion</td></tr>
   <tr><td><b>type</b></td><td>challenge/hardware</td></tr>
   <tr><td><b>difficulty&nbsp;&nbsp;&nbsp;</b></td><td>very easy</td></tr>
   <tr><td><b>startdate</b></td><td>2022-07-29</td></tr>
   <tr><td><b>enddate</b></td><td>2022-07-29</td></tr>
</table>

## Description

> As a part of our SDLC process, we've got our firmware ready for security testing. Can you help us by performing a security assessment?

## Solution

We receive two pieces of information:

1. a zip file containing the file `firmware.bin` and
2. an IP and port to a server.

The provided IP address blocks all pings and `nmap` (using `-Pn`) discovers only the port we already know about. In the official challenge discussion, a user mentions `telnet`. Connecting via telnet is possible, but the correct credentials are needed.

Viewing the bin file using a hex editor only shows some generic program error messages. A search for the strings `"flag"` and `"htb"` results in no relevant hits. Instead, the file is extracted using `binwalk`:

```
$ binwalk --extract --directory data firmware.bin
```

The resulting directory contains a multitude of folders and files and we can see that the bin file contained an entire Linux system. Searching for files containing the strings `"flag"` and `"htb"` results in no relevant hits. Used commands:

```
$ find . -name *flag*
$ grep -rnw . -e "flag"
```

By grepping for `"login"`, we discover the file `telnetd.sh`. In line 9, we find the username used to log into the server, `Device_Admin`. In line 2, the password is read from a different file `/etc/config/sign`. We find two files named `sign` in the extracted directory which contain the same string `qS6-X/n]u>fVfAt!`.

Using these credentials, we log into the server via the specified port. Using the `eval` command, we can execute linux commands on the server and we find the flag:

```
$ eval ls
flag.txt

$ eval cat flag.txt
HTB{4_hug3_blund3r_d289a1_!!}
```

### Sources

N/A
