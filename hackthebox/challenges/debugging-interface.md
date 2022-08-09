# Writeup "Debugging Interface"

## Quick Info

<table>
   <tr><td><b> site       </b></td><td> Hack the Box                                                           </td></tr>
   <tr><td><b> url        </b></td><td> https://app.hackthebox.com/challenges/debugging-interface              </td></tr>
   <tr><td><b> discussion </b></td><td> https://forum.hackthebox.com/t/official-debugging-interface-discussion </td></tr>
   <tr><td><b> type       </b></td><td> challenge/hardware                                                     </td></tr>
   <tr><td><b> difficulty </b></td><td> very easy                                                              </td></tr>
   <tr><td><b> startdate  </b></td><td> 2022-05-31                                                             </td></tr>
   <tr><td><b> enddate    </b></td><td> 2022-05-31                                                             </td></tr>
</table>

## Description

> We accessed the embedded device's asynchronous serial debugging interface while it was operational and captured some messages that were being transmitted over it. Can you decode them?

## Solution

[^1][^2][^3]

<p align="center">
   <img src="includes/debugging-interface-01.png" />
</p>

```
HTB{d38u991n9_1n732f4c35_c4n_83_f0und_1n_41m057_3v32y_3m83dd3d_d3v1c3!!52}
```

### Sources

[^1]: https://www.saleae.com/downloads/
[^2]: https://support.saleae.com/protocol-analyzers/analyzer-user-guides/using-async-serial
[^3]: https://support.saleae.com/tutorials/learning-portal/learning-resources/learn-asynchronous-serial#whats-a-framing-error
