#### скачать и проверить хеш kubernetes

<details>

```bash
sha512sum filename  
# Потом сравнить хеши  
# Можно на глаз. Можно запихнуть в один файл и с помощью cat сравнить  
sha512sum filename >>compare.txt
cat compare.txt | uniq
```

</details>