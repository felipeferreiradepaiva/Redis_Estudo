# REDIS

### Comandos Básicos:

> SET "ultimo_usuario_que_se_logou" "guilherme silveira"

> GET "ultimo_usuario_que_se_logou"

> DEL "ultimo_usuario_que_se_logou"


### Comandos em grupo MSET e KEYS:

Para definirmos várias duplas de chave e valor em um só comando, utilizamos o MSET, passando todas as duplas sempre na ordem chave-valor:

> MSET resultado:03-05-2015:megasena "1, 3, 17, 19, 24, 26" resultado:22-04-2015:megasena "15, 18, 20, 32, 37, 41" resultado:15-04-2015:megasena "10, 15, 18, 22, 35, 43"

Ao executar o comando KEYS * vemos que as chaves foram todas definidas:

> KEYS *

> 1) "resultado:17-05-2015:megasena"
> 2) "resultado:15-04-2015:megasena"
> 3) "resultado:22-04-2015:megasena"
> 4) "resultado:10-05-2015:megasena"
> 5) "resultado:03-05-2015:megasena"

Para buscar apenas resultados do mês de abril, podemos usar a pesquisa da forma abaixo:

> KEY "resultado:*-04-2015:megasena"

Ou ainda, apenas do dia 01 até o dia 09 podemos usar o ? :

> KEY "resultado:0?-04-2015:megasena"

Outros Exemplos:
> MSET 
resultado:05-05-2015:gigasena "1, 25, 34, 67, 89, 90" 
resultado:15-05-2015:gigasena "2, 14, 28, 56, 78, 88" 
resultado:25-05-2015:gigasena "4, 17, 38, 45, 57, 68" 
resultado:29-05-2015:lotomania "02, 04, 05, 10, 13"

> KEYS resultado:??-??-2015:lotomania

> 1) "resultado:29-05-2015:lotomania"

> KEYS resultado:??-05-2015:*sena

> 1) "resultado:05-05-2015:gigasena"
> 2) "resultado:15-05-2015:gigasena"
> 3) "resultado:25-05-2015:gigasena"

Como podemos fazer para obter os sorteios da Mega-sena que ocorreram nos dias terminados em "3" ou em "7", de qualquer mês, de qualquer ano?

> KEYS "resultado:?[37]-??-????:megasena"
> 1) "resultado:03-05-2015:megasena"
> 2) "resultado:17-05-2015:megasena"

Outro Exemplo: 

> KEYS resultado:1[57]-??-????:*sena
> 1) "resultado:17-05-2015:megasena"
> 2) "resultado:15-05-2015:gigasena"
> 3) "resultado:15-04-2015:megasena"

### Comandos em HASH:

Para armazenar cada um dos conjuntos de chave e valor associados a uma chave, utilizamos o comando HSET para cada um dos conjuntos:

> HSET resultado:24-05-2015:megasena "numeros" "13, 17, 19, 25, 28, 32"

> HSET resultado:24-05-2015:megasena "ganhadores" 23

Para recuperar os valores com o comando HGET, podemos fazer:

> HGET resultado:24-05-2015:megasena "numeros"
> 1) "13, 17, 19, 25, 28, 32"

> HGET resultado:24-05-2015:megasena "ganhadores"
> 1) "23"

Para remover com o comando HDEL:

> HDEL resultado:24-05-2015:megasena "numeros"
> 1) (integer) 1

Quando tentamos recuperar "numeros" através do HGET recebemos (nil) como repsosta:

> HGET resultado:24-05-2015:megasena "numeros"
> 1) (nil)

Da mesma forma que podemos atribuir vários conjuntos de chave e valor em um único comando utilizando o MSET, podemos utilizar o comando HMSET para atribuir varios valores quando utilizamos um hash no valor.

> HMSET nome_da_chave campo1 "Ola" campo2 "Mundo"

Se quisermos recuperar todos os valores que existem no hash, podemos utilizar o comando HGETALL, passando como parâmetro o nome da chave que está associada ao hash.

> HGETALL "resultado:05-06-2015:megasena"
> 1) "numeros"
> 2) "5, 19, 23, 28, 46, 49"
> 3) "ganhadores"
> 4) "16"

### Comandos EXPIRE E TIL:

Agora vamos ver como fazer para que eles sejam apagados automaticamente. Para isso usamos o comando "EXPIRE"
> EXPIRE "sessao:usuario:1675" 1800
> 1) (integer) 1

Isso significa que daqui 1800 segundos, ou 30 minutos, os dados do usuário serão apagados, expirados. Para sabermos quanto tempo falta para que essas informações sejam apagadas usamos o comando Time Left ou "TTL":

> TTL "sessao:usuario:1675"
> 1) (integer) 1680

Passados os 30 minutos, se tentarmos pegar um dos valores da chave, o Redis retornará nulo:

> HGET "sessao:usuario:1675" "nome"
> 1) (nil)

### Comandos INCR, DECR, INCRBY, DECRBY, INCRBYFLOAT e DECRBYFLOAT

> INCR pagina:/contato:25-05-2015
> 1) (integer) 1

> INCR pagina:/contato:25-05-2015
> 1) (integer) 2

> INCR pagina:/contato:25-05-2015
> 1) (integer) 3

> DECR pagina:/contato:25-05-2015
> 1) (integer) 2

> INCRBY compras:25-05-2015:valor 15
> 1) (integer) 15

> INCRBY compras:25-05-2015:valor 15
> 1) (integer) 30

> DECRBY compras:25-05-2015:valor 5
> 1) (integer) 25

> INCRBYFLOAT compras:25-05-2015:valor 15.50
> 1) "40.50"

> DECRBYFLOAT compras:25-05-2015:valor 0.50
> 1) "40.00"




