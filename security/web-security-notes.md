# Web Security notes

## Topics

- Vulnerability in Cookies
- Session Hijacking & Injection
- CSRF
- CORS
- CSP
- Others: Click Jacking, post message, tab nabbing, JSON, JWT practices
  
Personal things to learn

- DDOS
- How to conduct thread modeling session

### Cookies

Cookie is way to create session by storing few data since HTTP is stateless
It actually appends the cookie not replace it as long as the key provided is different

```javascript
document.cookie = "key=value;"
```

Directly deleting cookie is not possible but we can set expiration to a past time that will delete the cookie
Scoping the cookie to a domain can also be an issue by simply narrowing down the possibilities by carrying out issues in subdomains

### CORS

Origin - tuple of protocol, domain, host. if any of them are different they are considered a different origin thus it will be considered as cross origin for requests. Same site is different from Origin

### DDOS - Distributed Denial of Service

Denial of Service attack aims to bring the system down, making it inaccessible to anyone
When multiple computers are used to attack, they are called Distributed Denial of Service

Some Exposure

- Only ping can be used to attach alone
- Only TCP SYN attack is possible as well
- HTTP flood
- Consumers machine can be used to conduct DDOS as well by using botnets

#### Defenses

 1. Monitoring + Alert System
 2. **ICMP flood can be prevented just by disabling ICMP protocol**

#### Questions

- Does Captcha helps? Does it help with botnets?

