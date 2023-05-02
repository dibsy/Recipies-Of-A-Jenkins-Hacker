# Introduction

### Whoami
- Dibyendu SIKDAR
- CTF Player, Penetration Tester, Security Engineer
- Work in Umbrella Engineering ( CISCO France )

### Why I decided to do this talk ?
- Previously I used to think Jenkins can be compromised in two possible ways
   - Exploit CVE
   - Execute a Groovy Script in Script Console.
- I had an opinion if either or both of these conditions are *NOT* met then Jenkins is secure.
- I spent time reading about other research work and doing my own research for exploiting Jenkins.
- Hence sharing my observations how Jenkins is being used by various teams "insecurely" and studying those "internet exposed" Jenkins instances ( nearly 1000 of them ). 

### 3 things about this talk
- In this talk we are NOT going to exploit any CVE !
- We will learn about jenkins internals, features and misconfigurations.
- We will focus exploiting point 2 !

