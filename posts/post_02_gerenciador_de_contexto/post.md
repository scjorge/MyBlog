### Gerenciadores de Contexto

Aposto que as primeiras perguntas que surgem ao falar deste assunto são. O que é? E para que serve?

Bom, vamos pensar em funcionalides que devemos ficar atentos à todas as etapas, principalmente a final. Parece um pouco estranho mas eu explico. 

É comum precisarmos, por exemplo, manipular um arquivo, se comunicar com banco de dados ou fazer lock em threads. Mas você pode perceber o que essas atividades tem em comum? Para todas devemos ficar atentos a finalização depois do uso. 

Vejamos, é preciso lembrar de fechar um arquivo após a manipulação, fechar a conexão com o banco após o uso e dar unlock em threads.


## Bloco Try

Vamos entender melhor o bloco 'try' no Python

- try: Tenta executar um códido
- except: Executa caso haja falha do código que está no try
- finally: Esse bloco sempre será executado

Vejamos um exemplos simples:

```python
try:
    print("Faça algo aqui")
except:
    print("Faça algo caso dê algo errado no bloco 'try'")
finally:
    print("Sempre irá ser executado")

```

Retono:

```
Faça algo aqui
Sempre irá ser executado

```

Percebam que uma coisa interessante aqui, temos um bloco que tentará algo, e também um bloco que sempre será executado. É esse o princípio para os Gerenciadores de contexo. 

Vamos pegar o exemplo da manupulação de arquivos. Vamos abrir um arquivo mas ao mesmo tempo garantir que iremos fecha-lo. Agora temos o seguinte código.

```python
try:
    print("Abrindo um arquivo")
    arquivo = open('teste.txt', 'w', encoding='utf-8')
    arquivo.write("Oi")
except Exception as e:
    print("Erro ao tentar escrever no arquivo:", e)
finally:
    print("Fechando Arquivo")
    arquivo.close()

```

Retono:

```
Abrindo um arquivo
Fechando Arquivo

```

Percebam que agora podemos ficar tranquilos em relação a deixar o arquivo aberto por qualquer motivo que venha ocorrer. Mas vamos melhorar uma pouco mais essa idea. Afinal, Gerenciadores de Contexto estão aqui para isso 😉

## Classes e métodos mágicos

OBS: Para entendermos e criamos nossos próprios gerenciadores, vamos trabalhar um pouco com classes. Então vamos lá.

O Python nos fornece 2 métodos mágicos para trabalhar com os Gerenciadores de Contexto. São eles: 

- \_\_enter__ : Responsavel por retornar o 'contexto', vamos pensar no arquivo.
- \_\_exit__ : Responsavel por receber as possíveis exeções que podem surgir e por finalizar a atividade (equivalente a junção do bloco except e finally que já vimos)

Para exemplificar, vamos de código: 

```python
class MeuGerenciador:
    def __init__(self, nome_arquivo, modo, encoding) -> None:
        print("Criando arquivo a ser manipulado")
        self.arquivo = open(nome_arquivo, modo, encoding=encoding)

    def __enter__(self):
        """
        Esse bloco terá seu retorno atribuido a variável criada após a palavra 'as'
        """
        print("Devolvendo arquivo para a utilização")
        return self.arquivo

    def __exit__(self, tipo, valor, traceback):
        """
        Esse bloco sempre será executado ao saírmos do gerenciador
        Caso ocorra um erro temos o valor, tipo e traceback que recebemos como parâmetro
        """
        print("Fechando arquivo e saindo do gerenciador")
        self.arquivo.close()

```

Bom agora vamos ver como fica a utilização dessa classe.

```python
with MeuGerenciador("teste.txt", "w", encoding="utf-8") as arquivo:
    arquivo.write("oi")

```

Retono:

```
Criando arquivo a ser manipulado
Devolvendo arquivo para a utilização
Fechando arquivo e saindo do gerenciador

```

Agora ficou bem mais simples a utilização. Vamos entender o que aconteceu aqui. 

A palavra 'with' basicamente acionou o método \_\_enter__ que criamos, e, por consequencia, utilizamos a palavra 'as' para criarmos a variável(ou seja, o 'contexto') que receberá o retorno do método \_\_enter__. E quando que acionamos o método \_\_exit__ ? Quando ocorre um erro ou quando saimos do bloco 'with'. 

Então ficou fácil gerenciarmos coisas. Ou seja, podemos criar códigos que iremos trabalhar em uma etapa e criar códigos que serão executados automaticamente depois. Legal né? 😊

Bem, isso foi apenas para entermos o funcionamento do gerenciadores de contexto, porque o próprio Python já traz um gerenciador de arquivos para nós. Eu sei, eu sei "Se já tinha por que não mostrou desde o início?". Bom, a ideia era mostrar o que o Python nos fornece por baixo dos panos e também fazer com que você consiga criar seus próprios gerenciadores. 

Então vamos ver como o Python traz de forma nativa o exemplo que criamos 

```python
with open("teste.txt", "w", encoding="utf-8") as arquivo:
    arquivo.write("oi")
```

Isso mesmo, a própria função open já possui um gerenciador de contexto. Então é possível utilizarmos das duas formas.

## Entendendo as Exceções

Bom, chegamos até aqui e falta apenas entedermos como funciona as exceções dos gerenciadores.

Para que possamos capturar uma exceção no método \_\_exit__ precisamos já ter criado o contexo, ou seja, já ter atribuido valor à variavél que foi criada com o retorno do método \_\_enter__. No nosso exemplo foi a variável 'arquivo'.

O método \_\_enter__ recebe 3 parâmetros para manipulação de erros. 

- tipo: A classe do tipo de erro. Erros que vem a partir da BaseException 
- valor: A mensagem de erro. Normalmente a mensagem que nos dá a luz na hora do bug
- traceback: Um objeto do tipo traceback, nele temos os seguintes atributos:
  - tb_frame: Basicamente uma string com a mensagem de erro e seus detalhes do traceback
  - tb_lasti: índice da última instrução tentada em bytecode
  - tb_lineno: linha onde ocorreu o erro
  - tb_next: próximo trackback, casa haja mais de 1

Caso não haja nenhum erro durante a execução. Todos os parâmetros será atribuidos como None

Bem agora que entendemos melhor os erros. Vamos de exemplo. Ajustei a classe que criamos para forçamos os erros, executei o método write com 2 'e', dessa forma vamos ter um erro pois o método writee não existe. Vejamos:

```python
class MeuGerenciador:
    def __init__(self, nome_arquivo, modo, encoding) -> None:
        print("criando arquivo a ser manipulado")
        self.arquivo = open(nome_arquivo, modo, encoding=encoding)

    def __enter__(self):
        """
        Esse bloco terá seu retorno atribuido a variável criada após a palavra 'as'
        """
        print("devolvendo arquivo para a utilização")
        return self.arquivo

    def __exit__(self, tipo, valor, traceback):
        """
        Esse bloco sempre será executado ao saírmos do gerenciador
        caso ocorra um erro temos o valor, tipo e traceback que recebemos como parâmetro
        """
        print("Capturando erros")
        print("tipo -> ", tipo)
        print("valor ->", valor)
        print("traceback frame ->", traceback.tb_frame)
        print("traceback índice ->", traceback.tb_lasti)
        print("traceback linha do erro ->", traceback.tb_lineno)
        print("traceback linha do erro ->", traceback.tb_next)
        self.arquivo.close()

```

Executando a classe com método 'writee'

```python
with MeuGerenciador("teste.txt", "w", encoding="utf-8") as arquivo:
    arquivo.writee("oi")

```

Retorno: 

```
criando arquivo a ser manipulado
devolvendo arquivo para a utilização
Capturando erros
tipo ->  <class 'AttributeError'>
valor -> '_io.TextIOWrapper' object has no attribute 'writee'
traceback frame -> <frame at 0x000001FBC34AB050, file 'c:\\DevNew\\dev-to\\2-gerenciador-contexto\\code.py', line 43, code <module>>
traceback índice -> 32
traceback linha do erro -> 43
traceback linha do erro -> None
Traceback (most recent call last):
  File "c:\DevNew\dev-to\2-gerenciador-contexto\code.py", line 43, in <module>
    arquivo.writee("oi")
AttributeError: '_io.TextIOWrapper' object has no attribute 'writee'

```
<br>

Agora sim vimos os detalhes da manipulação desse recurso incrível!

## Conclusão 

Bem, Espero ter ajudado no entendimento de gerenciador de contexo. Tentei detalhar ao máximo essa jornada. Mas vamos lembrar que é muito útil a utilização desse recurso.

Se esse Post foi útil para você, curta e compartilhe. 