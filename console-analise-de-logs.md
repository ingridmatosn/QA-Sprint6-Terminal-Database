# Console — análise de logs Apache

Busca e filtragem de logs de servidor pelo terminal (Cygwin/Bash), com os comandos usados e os resultados obtidos.

## Tarefa 1 — Localizar as requisições de um IP específico

Comandos:

```bash
cd ~/logs/2019/12
grep -R '^233.201'
```

Resultado:

```
apache_2019-12-18.txt:233.201.188.154 - - [18/12/2019:21:46:01 +0000] "DELETE /events HTTP/1.1" 403 3971
apache_2019-12-21.txt:233.201.182.9 - - [21/12/2019:21:56:20 +0000] "PATCH /users HTTP/1.1" 400 4118
```

## Tarefa 2 — Separar os erros 400 e 500 de um dia de log

Criar a estrutura de diretórios e copiar o log do dia:

```bash
mkdir ~/bug1
mkdir ~/bug1/events
cp ~/logs/2019/12/apache_2019-12-30.txt ~/bug1/main.txt
```

Filtrar por código de status e gravar em arquivos separados:

```bash
grep " 400 " ~/bug1/main.txt > ~/bug1/events/400.txt
grep " 500 " ~/bug1/main.txt > ~/bug1/events/500.txt
```

### Resultados

| Tipo de erro | Total de ocorrências em 30/12/2019 |
|---|---|
| HTTP 400 — erro do cliente | 172 |
| HTTP 500 — erro do servidor | 156 |

Contagem obtida com `wc`:

```
172  1720 13860 /home/morty/bug1/events/400.txt
156  1560 12554 /home/morty/bug1/events/500.txt
```

Amostra de 400.txt (`head -3` e `tail -3`):

```
80.57.170.51 - - [30/12/2019:21:35:12 +0000] "DELETE /users HTTP/1.1" 400 3623
204.235.176.118 - - [30/12/2019:21:35:13 +0000] "POST /users HTTP/1.1" 400 4704
82.95.203.67 - - [30/12/2019:21:35:19 +0000] "DELETE /lists HTTP/1.1" 400 3737
...
203.106.235.105 - - [30/12/2019:22:12:38 +0000] "DELETE /events HTTP/1.1" 400 4158
18.211.28.150 - - [30/12/2019:22:12:40 +0000] "DELETE /collectors HTTP/1.1" 400 2212
229.16.123.45 - - [30/12/2019:22:12:54 +0000] "GET /auth HTTP/1.1" 400 2397
```

Amostra de 500.txt (`head -3` e `tail -3`):

```
64.250.112.189 - - [30/12/2019:21:35:13 +0000] "PUT /parsers HTTP/1.1" 500 4639
193.253.101.180 - - [30/12/2019:21:35:31 +0000] "PATCH /alerts HTTP/1.1" 500 2944
197.106.117.194 - - [30/12/2019:21:35:31 +0000] "PATCH /parsers HTTP/1.1" 500 3519
...
207.6.210.203 - - [30/12/2019:22:12:37 +0000] "PATCH /events HTTP/1.1" 500 4298
33.13.118.148 - - [30/12/2019:22:12:38 +0000] "PUT /alerts HTTP/1.1" 500 4711
107.188.33.199 - - [30/12/2019:22:12:59 +0000] "POST /parsers HTTP/1.1" 500 2833
```

### Leitura dos resultados

Em um único dia, 172 requisições foram recusadas por erro do cliente e 156 falharam no servidor. Os 500 se concentram em `PUT`, `PATCH` e `POST` sobre `/parsers`, `/alerts` e `/events`, o que aponta para os endpoints de escrita como o lugar por onde começar a investigação.

