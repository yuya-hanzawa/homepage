# homepage 

## Description

I create my own homepage using HTML & CSS.

In addition, I created a system that automatically updates homepage every time I execute git push with git hooks.

HomePage： https://yuya-hanzawa.com

Use Sakura's VPS for the server and a domain name obtained from Sakura's domain.

Sakura VPS：https://vps.sakura.ad.jp

</br>

## Improvements
1. It's not possible to run ansible-play-book after changing the port, so it needs to be fixed for idempotent.

2. Making homepage more stylish.

</br>

## Requirements
- Local
  - macOS Big Sur 11.4 Apple M1

- Server  
  - OS CentOS 7 x86_64(64bit)  
  - vCPU 2Core  
  - Memory 1GB  
  - SSD 50GB  

- Tools
  - ansible 2.11.6
  - nginx 1.21.4
  - git 1.8.3.1

# References
https://hsmtweb.com/tool-service/github-actions.html
https://qiita.com/suthio/items/2760e4cff0e185fe2db9
https://qiita.com/yysskk/items/974c9c55d66a26515651
