# Weaponized Exploit for Maltrail v0.53 Unauthenticated OS Command Injection (RCE)

This Python script exploits a command injection vulnerability in the Maltrail (v0.53) web service  

- The vulnerability exists in the login page and can be exploited via the `username` parameter  


## Vulnerability Explanation

[In this specific case,](https://github.com/stamparm/maltrail/blob/master/core/httpd.py#L399) the `username` parameter of the login page doesn't properly sanitize the input, allowing an attacker to inject OS commands  

The service uses the `subprocess.check_output()` function to execute a shell command that logs the username provided by the user. If an attacker provides a specially crafted username, they can inject arbitrary shell commands that will be executed on the server

In shell scripting, the semicolon `;` is used to separate multiple commands. So, when the attacker provides a username that includes a semicolon, followed by a shell command, the shell treats everything after the semicolon as a separate command. Basically, everything after `;` will run anyway.

## Exploit Overview

The exploit creates a reverse shell payload encoded in Base64 to bypass potential protections like WAF, IPS or IDS and delivers it to the target URL using a curl command  
The payload is then executed on the target system, establishing a reverse shell connection back to the attacker's specified IP and port

## Usage

The script requires three arguments: the IP address where the reverse shell should connect back to (listening IP), the port number on which the reverse shell should connect (listening port) and the URL of the target system  
> Script requires curl to be installed
```sh
python3 exploit.py [listening_IP] [listening_PORT] [target_URL]
```  
For example:
```sh
python3 exploit.py 1.2.3.4 1337 http://example.com
```


## Disclaimer
This is intended for educational purposes and legal use only.  
Always obtain proper authorization before performing penetration testing or exploiting vulnerabilities.

## Ressource
- https://huntr.dev/bounties/be3c5204-fbd9-448d-b97c-96a8d2941e87/
