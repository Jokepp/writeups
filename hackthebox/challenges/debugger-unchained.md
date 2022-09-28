# Writeup "Debugger Unchained"

## Quick Info

<table>
   <tr><td><b> site       </b></td><td> Hack the Box                                                          </td></tr>
   <tr><td><b> url        </b></td><td> https://app.hackthebox.com/challenges/debugger-unchained              </td></tr>
   <tr><td><b> discussion </b></td><td> https://forum.hackthebox.com/t/official-debugger-unchained-discussion </td></tr>
   <tr><td><b> type       </b></td><td> challenge/web                                                         </td></tr>
   <tr><td><b> difficulty </b></td><td> easy                                                                  </td></tr>
   <tr><td><b> startdate  </b></td><td> 2022-09-09                                                            </td></tr>
   <tr><td><b> enddate    </b></td><td> __                                                            </td></tr>
</table>

## Description

> Our SOC team has discovered a new strain of malware in one of the workstations. They extracted what looked like a C2 profile from the infected machine's memory and exported a network capture of the C2 traffic for further analysis. To discover the culprits, we need you to study the C2 infrastructure and check for potential weaknesses that can get us access to the server.

## Solution

We receive two files, a file called `c2.profile` containing some configuration infos and a file called `traffic.pcapng` containing network traffic. After some looking through the information, we have no good ideas where to start. After a web search, we find a writeup of the challenge[^1] which describes a very cool exploit which we'll try to replicate.

<p align="center">
   <img src="includes/debugger-unchained-01.png" />
</p>

```
HTB{__}
```

### Sources

[^1]: https://www.orangewhispers.com/posts/debugger-unchained/
[^2]: 
[^3]: 
