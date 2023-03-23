# Desempacotamento - *args e **kwargs

Vamos entender sobre o desempacotamento, ou seja, uma forma que podemos atribuir diversos valores ao mesmo tempo.

vamos primeiro entender uma habilidade que o Python nos traz. Podemos ter múltiplas atribuições de valores ao declarar variáveis e, também, podemos retornar mais de um valor em nossas funções. Vejamos:

```python
a, b, c = 1, 2, 3

print(a, b, c, sep=" - ")
```

saída:

```
1 - 2 - 3
```
<br>

Maravilha! atribuímos 3 valores em 3 diferentes variáveis em uma só linha. E isso é a base pro no Post de hoje.

Agora vamos brincar um pouco com listas. Vejamos:

```python
a, b, c = [1, 2, 3]

print(a, b, c, sep=" - ")
```

saída:

```
1 - 2 - 3
```
<br>

Conseguimos exatamente o mesmo resultado. Ou seja, é possível utilizar os valores de uma lista para atribuições múltiplas. E como mencionamos, funções podem fazer as mesmas coisas, olha só:

```python 
def retorno_multiplo():
    return 1, 2, 3

a, b, c = retorno_multiplo()

print(a, b, c, sep=" - ")
```
saída:

```
1 - 2 - 3
```
<br>

Ótimo, agora que entendemos legal essas atribuições múltiplas, vamos ao que nos interessa. 

basicamente o desempacotamento é a idea de podemos realizar essas atribuições que fizemos de forma dinâmica. 

Eu sei, parece estranho, então vamos de exemplo:

```python 
lista = [1, 2, 3]
a, *nova_lista = lista

print(a, nova_lista)
```
saída:

```
1 [2, 3]
```
<br>

Interessante ne? Atribuímos o primeiro item da lista a variável "a" e o restante automaticamente em uma nova lista chamada "nova_lista". Pronto, esse é o tal do desempacotamento. É para isso que usamos o caractere estrela "*".

Vamos brincar um pouco com funções, afinal de contas é aqui que realmente utilizamos esse conceito no dia a dia. 

Que venha código então 😊

```python 
def recebe_nomes(*args):
    print(args)

recebe_nomes("akuma", "ryu", "ken")
```
saída:

```
('akuma', 'ryu', 'ken')
```
<br>

Olha só a mágica acontecendo. Acabamos de criar uma função que aceita vários argumentos dinamicamente. E nosso argumento "args" se tornou uma tupla com todos os argumentos que passamos da mesma forma como vimos anteriormente. 

<br>

## Lidando com argumentos/parâmetros em funções

Agora vamos fazer uma pequena pausa e entender como as funções lidam com argumentos no Python. Juro que essa pausa é importante 😜

Bem, o Python utiliza de dois tipos de argumentos

- argumentos ordenados
- argumentos nomeados

### Entendendo argumentos ordenados

Os ordenados são os que acabamos de fazer. Eles são reconhecidos primeiro nas funções. Ou seja, uma vez que você utilizar um argumento nomeados, os ordenados não terão mais efeitos, mas eu explico melhor já já. 

Os ordenados são quando passamos os valores diretamente para as funções, vejamos:

```python

def recebe_tres_nomes(nome_1, nome_2, nome_3):
    print(nome_1, nome_2, nome_3)

recebe_tres_nomes("akuma", "ryu", "ken")
```
saída:

```
akuma ryu ken
```
<br>

Veja que interessante, utilizei o mesmo exemplo que já vimos, e, também, criei a função de forma explicita para receber 3 nomes. Mas no geral a ideia é mostrar os argumentos sendo passados um a um, e vamos nos atentar que estamos passando os valores diretamente quando estamos chamando a função. Então o *args é essa lista de parâmetros que passamos de forma ordenada. 

Observe que a função "recebe_tres_nomes" não mostra uma tupla mas sim cada item isolado, isso porque definimos que seria mostrado cada item

### Entendendo argumentos nomeados

Vamos lá, agora vamos utilizar o mesmo exemplo, porém vamos passar parâmetros nomeados. Observe vou mudar a ordem dos nomes, e com isso vamos ter mesmo resultado

```python
def recebe_tres_nomes(nome_1, nome_2, nome_3):
    print(nome_1, nome_2, nome_3)

recebe_tres_nomes(nome_2="ryu", nome_3="ken", nome_1="akuma")
```
saída:

```
akuma ryu ken
```
<br>

Agora utilizei argumentos na ordem invertida e tivemos o mesmo resultado. Isso porque o Python consegue ler o nome de cada parâmetro e capturar o seu valor. 

Mas tem um detalhe aqui. Lembra que comentei que devemos respeitar a ordem de reconhecimento dos argumentos nas funções? Bem, eu disse que devemos sempre utilizar os paramentos ordenados e depois os nomeados. 

Então é possível chamar nossa função assim:

```python
def recebe_tres_nomes(nome_1, nome_2, nome_3):
    print(nome_1, nome_2, nome_3)

recebe_tres_nomes("akuma", nome_2="ryu", nome_3="ken")
```
saída:

```
akuma ryu ken
```
<br>

Viu só, eu passei o primeiro ordenado e o restante através dos nomes dos argumentos. Agora não podemos inverter isso. Olha pra mim aqui 🥸 Não funciona! Veja você mesmo. 

```python
def recebe_tres_nomes(nome_1, nome_2, nome_3):
    print(nome_1, nome_2, nome_3)

recebe_tres_nomes(nome_2="ryu", nome_3="ken", "akuma")
```
saída:

```
recebe_tres_nomes(nome_2="ryu", nome_3="ken", "akuma")
                                                         ^
SyntaxError: positional argument follows keyword argumentakuma ryu ken
```
<br>

O Python vai reclamar da ordem dos tipos de argumentos. 

Não tente isso em casa... Brincadeira, tente e veja você mesmo.



## Retornando ao desempacotamento

Bem, como eu havia dito, isso foi uma pequena pausa pra que possamos entender o desempacotamento nomeado

sabemos como funciona um dicionário em Python certo? (espero que sim 🤣)

temos chave seguido de valor

então se tentarmos atribuir um dicionário a várias variáveis, essas variáveis irão receber as chaves. Olha só

```python
nomes = {
    'nome_1': 'akuma',
    'nome_2': 'ryu',
    'nome_3': 'ken'
}

a, b, c = nomes
print(a, b, c)
```
saída:

```
nome_1 nome_2 nome_3
```
<br>

Agora é aqui que está o 'pulo do gato', se passamos como argumento nossa lista de nomes com 2 estrelas, iremos receber a chave e os valores na função, cada chave ira servir como um argumento nomeado para a função. Legal né? 

```python
def recebe_nomes(nome_1, nome_2, nome_3):
    print(nome_1, nome_2, nome_3)

nomes = {
    'nome_1': 'akuma',
    'nome_2': 'ryu',
    'nome_3': 'ken',
}

recebe_nomes(**nomes)
```
saída:

```
akuma ryu ken
```
<br>

Lembre-se que se caso você passar um argumento que não existe, uma exceção será levantada, porque não existe nos argumentos que foram definidos: 

```python
def recebe_nomes(nome_1, nome_2, nome_3):
    print(nome_1, nome_2, nome_3)

nomes = {
    'nome_1': 'akuma',
    'nome_2': 'ryu',
    'nome_3': 'ken',
    'nome_4': 'mega man',
}

recebe_nomes(**nomes)
```
saída:

```
Traceback (most recent call last):
  File "c:\DevNew\Myblog\MyBlog\posts\post_03\code.py", line 14, in <module>
    recebe_tres_nomes(**nomes)
TypeError: recebe_tres_nomes() got an unexpected keyword argument 'nome_4'
```
<br>

Bom, é por isso que utilizamos o **kwargs, porque aí iremos receber quantos argumentos forem precisos:

```python
def recebe_nomes(**kwargs):
    print(kwargs)

nomes = {
    'nome_1': 'akuma',
    'nome_2': 'ryu',
    'nome_3': 'ken',
    'nome_4': 'mega man'
}

recebe_nomes(**nomes)
```
saída:

```
{'nome_1': 'akuma', 'nome_2': 'ryu', 'nome_3': 'ken', 'nome_4': 'mega man'}
```
<br>

E agora o argumento "kwargs" se tornou um dicionário com todos os nossos argumentos nomeados. 

Bem, vamos pensar na ordem dos tipos de argumentos. Obrigatoriamente tem que ser os ordenados, e em seguida os nomeados. E como já brincamos com os dois. Que tal misturar as coisas e fazer com que a função aceite tanto um quanto o outro?

```python
def recebe_nomes(*args, **kwargs):
    print(args)
    print(kwargs)

recebe_nomes("akuma", "ryu", "ken", jogo="street fighter")
```
saída:

```
('akuma', 'ryu', 'ken')
{'jogo': 'street fighter'}
```
<br>

Viu só, passamos vários argumentos ordenamos, que ficou na tupla "args", e os nomeados ficou no dicionário "kwargs"

Podemos desempacotar através de listas e dicionários também:

```python
def recebe_nomes(*args, **kwargs):
    print(args)
    print(kwargs)

nomes = {
    'nome_1': 'akuma',
    'nome_2': 'ryu',
    'nome_3': 'ken',
    'nome_4': 'mega man'
}

jogos = ["street fighter", "Mega Man x4"] 

recebe_tres_nomes(*jogos, **nomes)
```
saída:

```
('street fighter', 'Mega Man x4')
{'nome_1': 'akuma', 'nome_2': 'ryu', 'nome_3': 'ken', 'nome_4': 'mega man'}
```
<br>

Bom isso foi o desempacotamento e as expressões *args e **kwargs que vemos por aí.

## Considerações

Quero te lembrar que esses nomes são uma convenção, funcionária com qualquer nome. Você não é obrigado a usar as palavras args e kwargs para funcionar. Se o planeta escreve assim, vamos escrever também, certo? Vamos manter o padrão.
 
Por curiosidade, o nome "args" vem de "arguments" (argumentos), e "kwargs" vem de "known arguments" (argumentos conhecidos)

E é isso! Obrigado por chegar até aqui 😉

## Conclusão 

Usar a técnica de desempacotamento em função é muito comum no dia a dia. É um assunto que gera um pouco de confusão quando olhamos a primeira vez, por isso tentei detalhar o máximo possível para que possam realmente entender como funciona. Espero poder ter ajudado alguém.
