# S.O.L.I.D com ChatGPT

Decidi perguntar ao ChatGPT como seria um código para exibir uma mensagem de "Parabéns" utilizando S.O.L.I.D.

A perguntar que fiz foi:

"escreva um código em Python pra dar parabéns utilizando S.O.L.I.D"

Pois bem, nossa resposta foi: 

"Claro! Aqui está um exemplo de como utilizar os princípios S.O.L.I.D para escrever um código em Python para dar parabéns:"

```python
class MensagemParabens:
    def __init__(self, mensagem):
        self.mensagem = mensagem

    def enviar_mensagem(self):
        print(self.mensagem)


class ServicoParabens:
    def __init__(self, mensagem):
        self.mensagem = mensagem

    def enviar_parabens(self):
        mensagem = MensagemParabens(self.mensagem)
        mensagem.enviar_mensagem()


if __name__ == '__main__':
    mensagem = "Parabéns!"
    servico_parabens = ServicoParabens(mensagem)
    servico_parabens.enviar_parabens()
```

Em seguida completou a explicação com a seguida mensagem:

<br>

"Neste código, utilizamos os seguintes princípios S.O.L.I.D:

Single Responsibility Principle (Princípio da Responsabilidade Única): as classes MensagemParabens e ServicoParabens têm apenas uma responsabilidade cada, tornando-as mais fáceis de entender, modificar e testar.

Open/Closed Principle (Princípio Aberto/Fechado): o código é aberto para extensão (p. ex., podemos adicionar outras formas de envio de parabéns) e fechado para modificação (p. ex., não precisamos modificar as classes existentes quando adicionamos uma nova forma de envio).

Liskov Substitution Principle (Princípio da Substituição de Liskov): as classes MensagemParabens e ServicoParabens são intercambiáveis com suas subclasses (se houvesse alguma), sem afetar o comportamento esperado do programa.

Interface Segregation Principle (Princípio da Segregação de Interfaces): não há interfaces desnecessárias nas classes.

Dependency Inversion Principle (Princípio da Inversão de Dependência): as classes dependem de abstrações (no caso, a classe MensagemParabens) em vez de depender de implementações concretas.

Espero que isso ajude!"

<br>

Quando executamos o código temos o seguinte retorno:

```bash
Parabéns!
```

<br>

Vamos debater cada item e entender como o ChatGPT tratou os tópicos de S.O.L.I.D

<br>

## Single Responsibility Principle (Princípio da Responsabilidade Única)

Uma classe deve fazer apenas uma coisa e, portanto, deve ter apenas uma razão para ser modificada.

O ChatGPT respondeu que "as classes MensagemParabens e ServicoParabens têm apenas uma responsabilidade cada", bem, discordo um pouquinho. 

A classe 'MensagemParabens' ficou responsável por tratar e também por enviar a mensagem. Acredito que podemos melhorar isso. 

Criei a classe 'Mensagem' para tratarmos apenas o texto da nossa mensagem. Dessa forma, podemos tratar quaisquer mudanças e teremos uma classe própria para a Mensagem.

Em seguida criei a classe 'Mensageiro' para ficar responsável apenas por enviar a mensagem. 

Nosso novo código ficou assim:

```python
class Mensagem:
    def __init__(self, mensagem: str):
        self.mensagem = mensagem

    @property
    def texto(self):
        return self.mensagem


class Mensageiro:
    def __init__(self, mensagem: str):
        self.mensagem = mensagem

    def enviar_mensagem(self):
        print(self.mensagem)


class ServicoParabens:
    def __init__(self, mensageiro: Mensageiro):
        self.mensageiro = mensageiro

    def enviar_parabens(self):
        self.mensageiro.enviar_mensagem()


if __name__ == "__main__":
    mensagem = Mensagem("Parabéns!")
    mensageiro = Mensageiro(mensagem.texto)
    servico_parabens = ServicoParabens(mensageiro)
    servico_parabens.enviar_parabens()
```


<br>

Quando executamos o código temos o mesmo retorno:

```bash
Parabéns!
```

<br>

## Open/Closed Principle (Princípio Aberto/Fechado)

As classes devem estar abertas para extensão, mas fechadas para modificação.

Vamos pensar um pouco nos meios que iremos enviar a mensagem. Vamos imaginar que queremos enviar por meio de Whastapp ou quem sabe pela DM no Instagram. 

É nessa linha de pensamento que vamos deixar nosso código aberto para novas formas de envio.

Modifiquei a classe 'Mensageiro' para se tornar abstrata, então podemos adicionar novas formas de envio sem modificar nenhuma outra parte do nosso código.

Nosso novo código ficou assim:

```python
from abc import ABC, abstractmethod


class Mensagem:
    def __init__(self, mensagem: str):
        self.mensagem = mensagem

    @property
    def texto(self):
        return self.mensagem


class Mensageiro(ABC):
    @abstractmethod
    def __init__(self, mensagem: str):
        ...

    @abstractmethod
    def enviar_mensagem(self):
        ...


class EnviaWhatsApp(Mensageiro):
    def __init__(self, mensagem: str):
        self.mensagem = mensagem

    def enviar_mensagem(self):
        print(self.mensagem, "Enviado para WhatsApp")


class EnviaInstagram(Mensageiro):
    def __init__(self, mensagem: str):
        self.mensagem = mensagem

    def enviar_mensagem(self):
        print(self.mensagem, "Enviado para Instagram")


class ServicoParabens:
    def __init__(self, mensageiro: Mensageiro):
        self.mensageiro = mensageiro

    def enviar_parabens(self):
        self.mensageiro.enviar_mensagem()


if __name__ == "__main__":
    mensagem = Mensagem("Parabéns!")
    mensageiro_wpp = EnviaWhatsApp(mensagem.texto)
    mensageiro_insta = EnviaInstagram(mensagem.texto)
    servico_wpp = ServicoParabens(mensageiro_wpp)
    servico_insta = ServicoParabens(mensageiro_insta)
    servico_wpp.enviar_parabens()
    servico_insta.enviar_parabens()
```

<br>

Quando executamos o código temos um novo retorno. Agora podemos ver a diferença de cada tipo de mensageiro:

```bash
Parabéns! Enviado para WhatsApp
Parabéns! Enviado para Instagram
```

<br>


## Liskov Substitution Principle (Princípio da Substituição de Liskov)

Nesse tópico devemos garantir que nossas classes filhas consigam executar os mesmos métodos com os mesmos comportamentos. 

Em nosso exemplo o método 'enviar_mensagem' já está fazendo bem esse papel herdaddo da classe 'Mensageiro'. Acredito que não há muito o que mexer em nosso código nesse ponto.


## Interface Segregation Principle (Princípio da Segregação de Interfaces)

Nesse tópico devemos pensar que nenhuma classe deve ser forçada a depender de métodos que não será utilizado.

Isso quer dizer não devemos criar classes que herdarão de classes que trazem métodos que podem quebrar nossa aplicação.

Vejamos, imagina que queremos que nossa aplicação poderá apagar a mensagem que foi enviada. Para isso devemos lembrar que nem toda forma de envio irá permitir. WhatsApp, por exemplo, permite. Mas vamos imaginar que o Instagram não permita que a mensagem enviada seja excluída. Nesse caso não podemos criar uma única classe abstrata para ambos. Devemos separar em diferentes abstrações, pois se caso tentarmos executar um método para deletar a mensagem no Instagram, teremos um problema em nossa aplicação.

Bem, criei uma nova classe abstrata 'MensageiroEditor' que herda de nossa abstração base 'Mensageiro', nela incluímos no método 'apaga_mensagem'. E como agora vamos trabalhar diretamente com a classes dos mensageiros, então retiramos a classe 'ServicoParabens' e vamos trabalhar diretamente com os nossos métodos. Dessa forma, vamos garantir a execução correta.

Nosso novo código modificado ficou assim:

```python
from abc import ABC, abstractmethod


class Mensagem:
    def __init__(self, mensagem: str):
        self.mensagem = mensagem

    @property
    def texto(self):
        return self.mensagem


class Mensageiro(ABC):
    @abstractmethod
    def __init__(self, mensagem: str):
        ...

    @abstractmethod
    def enviar_mensagem(self):
        ...


class MensageiroEditor(Mensageiro):
    @abstractmethod
    def apaga_mensagem(self):
        ...


class EnviaWhatsApp(MensageiroEditor):
    def __init__(self, mensagem: str):
        self.mensagem = mensagem

    def enviar_mensagem(self):
        print(self.mensagem, "Enviado para WhatsApp")

    def apaga_mensagem(self):
        print("Deletando mensagem enviada", "mensagem:", self.mensagem)


class EnviaInstagram(Mensageiro):
    def __init__(self, mensagem: str):
        self.mensagem = mensagem

    def enviar_mensagem(self):
        print(self.mensagem, "Enviado para Instagram")


if __name__ == "__main__":
    mensagem = Mensagem("Parabéns!")
    mensageiro_wpp = EnviaWhatsApp(mensagem.texto)
    mensageiro_insta = EnviaInstagram(mensagem.texto)
    mensageiro_wpp.enviar_mensagem()
    mensageiro_insta.enviar_mensagem()
    mensageiro_wpp.apaga_mensagem()
```
<br>

Quando executamos o código temos um novo retorno. E, novamente, podemos ver a diferença de cada tipo de mensageiro:

```bash
Parabéns! Enviado para WhatsApp
Parabéns! Enviado para Instagram
Deletando mensagem enviada mensagem: Parabéns!
```

<br>


## Dependency Inversion Principle (Princípio da Inversão de Dependência):

Este princípio recomenda que uma classe deve depender abstrações e não de implementações concretas. 
Então vamos tornar nossa classe 'Mensageiro' dependente de uma interface 'Mensagem', assim podemos personalizar nossa mensagem por tipo de envio e ainda definir a utilização de interface ao usar nossa abstração 'Mensageiro'.

vamos atualizar nosso código para:

```python
from abc import ABC, abstractmethod


class Mensagem(ABC):
    @abstractmethod
    def __init__(self, mensagem: str):
        self.mensagem = mensagem

    @property
    @abstractmethod
    def texto(self):
        return self.mensagem


class WhatsAppMensagem(Mensagem):
    def __init__(self, mensagem: str):
        self.mensagem = mensagem

    @property
    def texto(self):
        return f"Olá pessoas do WhatsApp, {self.mensagem}"


class InstagramMensagem(Mensagem):
    def __init__(self, mensagem: str):
        self.mensagem = mensagem

    @property
    def texto(self):
        return f"Olá pessoas do Instagram, {self.mensagem}"


class Mensageiro(ABC):
    @abstractmethod
    def __init__(self, mensagem: Mensagem):
        ...

    @abstractmethod
    def enviar_mensagem(self):
        ...


class MensageiroEditor(Mensageiro):
    @abstractmethod
    def apaga_mensagem(self):
        ...


class EnviaWhatsApp(MensageiroEditor):
    def __init__(self, mensagem: Mensagem):
        self.mensagem = mensagem.texto

    def enviar_mensagem(self):
        print(self.mensagem)

    def apaga_mensagem(self):
        print("Deletando mensagem enviada", "mensagem:", self.mensagem)


class EnviaInstagram(Mensageiro):
    def __init__(self, mensagem: Mensagem):
        self.mensagem = mensagem.texto

    def enviar_mensagem(self):
        print(self.mensagem)


if __name__ == "__main__":
    mensagem = "Parabéns!"
    mensageiro_wpp = EnviaWhatsApp(WhatsAppMensagem(mensagem))
    mensageiro_insta = EnviaInstagram(InstagramMensagem(mensagem))
    mensageiro_wpp.enviar_mensagem()
    mensageiro_insta.enviar_mensagem()
    mensageiro_wpp.apaga_mensagem()
```

<br>

Podemos ver a diferença de cada tipo de mensageiro para cada tipo de envio:

```bash
Olá pessoas do WhatsApp, Parabéns!
Olá pessoas do Instagram, Parabéns!
Deletando mensagem enviada mensagem: Olá pessoas do WhatsApp, Parabéns!
```

<br>


## Conclusão
Espero que esses exemplos tenham sido úteis para ajudar a entender os princípios básicos do SOLID.
Conseguimos evoluir um pouco o código gerado pelo ChatGPT. É graças a essas boas práticas, que conseguimos um código mais limpo e reutilizável que pode ser muito útil quando diferentes colegas desenvolvem diferentes partes do software ou trabalharão nele no futuro. Entretando é bom que fiquemos atentos ao implementar novos padrões. Como acontece com todos os guias, também não é recomendado seguir os princípios S.O.L.I.D cegamente. Princípios, padrões e diretrizes podem facilmente se transformar em antipadrões e levar a novos problemas.


