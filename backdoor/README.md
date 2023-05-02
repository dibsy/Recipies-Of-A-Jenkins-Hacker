# Backdooring

- Backdooring helps attacker to gain access over systems over long period of time ( sometimes bypassing the security checks )
- A higher level privileges might be required to do the changes in configuration



## User issued Jenkins API Tokens
- It is possible for users to generate their own API tokens.
- Can be used to bypass other authentication process ( Form based Authentication / 2FA authentication )
- Usually undected ( unless the usage count is not tracked )

```
curl http://username:token@jenkins-server/manage
```
- As an attacker we need to make sure, we need to give right priviliges to these compromised users in case Matrix Authorization Strategy is enabled.


## Using Shared Libraries


## Groovy init scripts

## Agents SSH Keys

## Malicious Agents
