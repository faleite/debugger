# Como debugar código Python

## 1. Afinal, o que é debugar código?
- Conceitos básicos de depuração
    - Quando há erro no código
    - Descobrir o que está acontecendo
    - Fazer o debug
- Soluções
    - **print**
    - **logs**
        - Referência ---> [Live de Python](https://youtu.be/tZ2iJ5H99fg)
    - **trace**
        - Faz o print de toda a linha de execução
        - ``python -m trace --trace seu_arquivo.py``
    - **debugger**

## 2. Ferramentas para debugar códigos
- Python DeBugger (**PDB**)
   - Embutido
- IPython DeBugger (**IPDB**)
    - pip install ipdb
- Remote Python DeBugger (**RPDB**)
    - pip install rpdb
    - Debuger remoto, via netcat
- Web Python DeBugger (**Web PDB**)
    - pip install web_pdb.server
    - Debuger remoto utilizando o browser
- **PySnooper**
    - Misto entre trace e debugger
    - pip install pysnooper
- Integrações de **IDEs**
- ...

## 3. Forma mais simples de debugar com Python Debugger
Comando|Função
-------|------
**breakpoint()**| *Ponto de parada, é colocado na linha onde quer parar o interpretador*
**python seu_arquivo.py**| *Abrir o arquivo para entrar em debugger*

*O Python debbuger mostra o código até o breakpoint + a proxima linha\
a ser executada, junto com o (Pdb):*


``$ python exemplo_01.py``
```python
> /Users/fabricio/pyex/debugger/exemplo_01.py(11)<module>()
-> formatado = formatação(nome, programa, numero)
(Pdb)
```

***(Pdb)** --> Shell interativo, lincando dados do arquivo (parecido com python -i)*

```python
(Pdb) 1+1
2
```

*Podemos verificar todas as variáveis do nosso arquivo, até o breakpoint:*

```python
(Pdb) nome
'Eduardo'
```

*O debbuger mostra a proxima linha a ser executada mas não executa,\
ou seja não contempla o (Pdb)*
```python
(Pdb) formatado
*** NameError: name 'formatado' is not defined
```
---

### Comandos do Debugger

*Os comandos tem a sua forma completa e a forma abreviada.*

---|Abreviada|Completa|Função
---|---------|--------|------
h(elp)|**h**| **help**|*Lista os comandos do **Pdb***

---
### Comandos básicos do debugger

Comando|Função
-------|------
**l(ist)**|*Mostra 10 linhas de código. 5 linhas antes e 5 linhas depois do breakpoint*
**l 10**|*Coloca a linha 10 no centro*
**l 1,10**|*Mostra as linhas de 1 a 10*
**ll** (longlist)|*Mostra todo o contexto que estamos, ex:. corpo completo de uma função*
**n(ext)**|*Avança o debugger para a próxima linha e a executa*
**s(tep)**|*Entra no bloco, caso seja um bloco. Chamada de função, execução de método...*
**whatis**|*Diz o tipo de algum objeto*
**c(cont(inue))**|*Avança o debuger até o próximo breakpoint*
**q(uit) / exit**|*Saí do debugger*:whatis
**p**|*Da o print da variavel*
**pp**|*Da o print melhor formatado*
**w(here)**|*Diz onde estamos, em qual arquivo, de qual módulo em qual linha, etc...
**u(p\)**|*Sobe um nível na pilha. Em uma função, ele vai na linha onde ela foi chamada*
**d(own)**|*Desce um nível na pilha. Caso tenha subido pra ver quem chamou*
**dir( )**|*Mostra o que tem no namespace (funções, imports e etc ...)*

*Observação:*\
*Para acessar variáveis com o mesmo nome de um comando do **Pdb**, usamos o print:*
```py
n = 97  # "n" é uma variavel que recebe 97. Mas também representa "next" do Pdb
(Pdb) p n  # Para resolver o conflito usamos o p(rint)
97
```
*Ferramentas que não são do debuger, mas ajudam a debugar:*

Comando|Função
-------|------
**pp vars(  )**|*Mostra todas as variáveis no escopo do breakpoint*
**pp globals(  )**|*Mostra todas as coisas do módulo no escopo do breakpoint*
**type(variavel)**|*Mostra o tipo da variavel ...*

>*Evite usar variáveis de um único caracter\
use **p** ou **pp** quando isso acontecer*

---
### Post-mortem debugging

- É uma ferramenta de depuração do **Pdb**
    - O modo post-mortem leva o depurador direto para a execução do programa
    - Há diversas formas de usar o post_mortem, aqui uma forma pratica:
        - ``$ python3 -mpdb seu_script.py``
    - [*Saiba mais*](https://www.codementor.io/@stevek/\
advanced-python-debugging-with-pdb-g56gvmpfa#post-mortem-debugging)

---

## 4. Instalar e setar outras ferramentas de debugger

### PYTHONBREAKPOINT
*A variável de ambiente PYTHONBREAKPOINT é responsável por alterar o comportamento do\
python em relação ao debuger.*

*Setando a variável:*
Comando|Função
-------|------
**PYTHONBREAKPOINT=0**|*Desativa os breakpoints, caso alguém esqueça em produção*
**PYTHONBREAKPOINT=ipdb.set_trace**|*Troca o debuger para outra opção (ex:. ipdb).*

*Usando debbuger específico:*
Comando|Função
-------|------
**PYTHONBREAKPOINT=ipdb.set_trace python seu_script.py**|*IPython DeBugger*
**PYTHONBREAKPOINT=rpdb.set_trace python seu_script.py**|*Remote Python DeBugger*
**PYTHONBREAKPOINT=web_pdb.set_trace python seu_script.py**|*Web Python DeBugger*
**PYTHONBREAKPOINT=0 python seu_script.py**|*Desativar*

***Instalação do IPython Debugger (Ipdb)*** ---> ````pip install ipdb````

---
### Uso do Remote Python DeBugger
>*ipdb de maneira remota*

*Instalação:*
```sh
$ pip install rpdb  # poetry add rpdb
```

*Entrar em modo debugger:*
```sh
$ PYTHONBREAKPOINT=rpdb.set_trace python seu_script.py
pdb is running on 127.0.0.1:4444  # Gera link remoto
```

*Acessar remotamente:*
```sh
$ nc 127.0.0.1 4444  # Acesso via Netcat na porta 4444
```
---
### Uso do Web Python DeBugger
>*ipdb com layout externo, tipo pycharm/vscode*

*Instalação:*
```sh
$ pip install web-pdb  # poetry add web-pdb
```

*Entrar em modo debugger:*
```sh
$ PYTHONBREAKPOINT=web_pdb.set_trace python seu_script.py
# Gera link remoto
CRITICAL:root:Web-PDB: starting web-server on faleite.home:5555...
```

*Acessar remotamente:*
```sh
http://localhost:5555 # Acesso via Netcat na porta 55555
```
---

*Mais em [Live de Python](https://youtu.be/yffiyHEiUvo?t=4764)*
- Sequência da live:
    - Debugger no Jupyter Notebook
    - Debugger em containers
