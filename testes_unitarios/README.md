# Testes

Antes de qualquer coisa, é sempre interessante verificar se esta ou não em um ambiente virtual, caso não esteja, rode os seguintes comandos no terminal:

```python
python3 -m venv venv
source venv/bin/activate
```

Caso não tenha o arquivo requiriments, rode o comando:

```python
pip freeze > requirements.txt
```

## Observações

- Os arquivos devem estar em uma pasta chamada tests;
- Lembre de ter o arquivo com dunder **init**.py no arquivo;
- O nome do arquivo deve começão com test\_;
- Os nomes das funções devem começar com test\_;

Para rodar testes em python, é necessario rodar o comando

```python
pip3 install pytests
```

## Rodando os testes

Para rodar os testes, use o comando no terminal:

```python
pytest
```

Para rodar os testes de forma verbosa (detalhada), use o comando no terminal:

```python
pytest -v
```

# Métodos da classe

Quando há métodos que serão utilizados apenas dentro da classe, se cploca um ‘\_’ na frente do nome do método.

## TDD

É sempre bom aplicar as técnicas de TDD (Test Driven Development). Essa técnica se aplica em:

- Criação de testes
- Geração de códigos
- Refatoração do código
  ![Untitled](Testes%20428a4919b5e944a9a3e33da60296ec2b/Untitled.png)

## Testes de Exception

obs.: É necessário importar o pytest

Para testar Exception, é necessário colocar a seguinte linha de código:

```python
with pytest.raises(Exception):
```

Para verificar uma exception, o assert dentro do with comentado acima, não deve ter uma verificação, apenas o resultado, exemplo:

```python
with pytest.raises(Exception):
            entrada = 100000000

            funcionario_teste = Funcionario("Teste", "11/11/20000", entrada)
            resultado = funcionario_teste.calcular_bonus()   # When

            assert resultado  # Then
```

## Rodando um teste específico

**(Método não aconselhável, pois se tem muitos testes, pode ter muitos testes que contem aquele nome)**

Basta inserir a tag k que ele rodara os testes que tem no nome a palavra idade:

```python
pytest -k idade
```

Para rodar de uma forma verbosa, basta, rodar o seguinte comando:

```python
pytest -v -k idade
```

**(Método aconselhável)**

Criando marks específicos para os testes

Em cima de cada teste, deve-se colocar um mark, como mostrado a seguir:

```python
from pytest import mark

@mark.calcular_bonus
def test_quando_xx_deve_retornar_yy(self):
	*
	*
	*
```

Ou

```python
import pytest

@pytest.mark.calcular_bonus
def test_quando_xx_deve_retornar_yy(self):
	*
	*
	*
```

E então no terminal, deve-se rodar o seguinte comando para rodar apenas os testes que tem essa mark (sim, você pode colocar a mesma mark para diversas funções):

```python
pytest - v -m calcular_bonus
```

Esse comando ele te retorna alguns warnings, para remover esses warnings, siga os seguintes passos:

Aqui você estará criando marks personalizados

- Na pasta root, crie um arquivo chamado pytest.ini
- Dentro desse arquivo coloque a seguinte configuração:
  ```python
  [pytest]
  markers =
      calcular_bonus: Teste para o metodo calcular_bonus
  ```

**Para pular um teste, basta colocar o seguinte mark**

```python
@mark.skip
def .....
```

e rodar em seguida no terminal o comando:

```python
pytest -v
```

Existem diversos tipos de markes, para descobrir todos, rode o comando no temrinal:

```python
pytest -- markers
```

Ou consultar a [documentação](https://docs.pytest.org/en/7.1.x/reference/reference.html#ini-options-ref).

## Cobertura de testes (Coverage)

Para isso, é necessário instalar uma biblioteca chamada pytest-cov, com o seguinte comando no terminal (Lembre de estar no ambiente virtual)

```python
pip install pytest-cov
pip freeze > requirements.txt
```

para rodar o coverage, rode no terminal o comando:

```python
pytest --cov
```

Porém, dessa maneira rodará arquivos da própria pasta de testes, como mostrado na figura a seguir:

- Stmts(STATEMENTS): linhas de código que se tem naquele arquivo
- Miss: Existe a perda de x linhas para a cobertura de testes
- Cover: A porcentagem de cobertura de testes no arquivo

![Untitled](Testes%20428a4919b5e944a9a3e33da60296ec2b/Untitled%201.png)

Estrutura do projeto:

![Untitled](Testes%20428a4919b5e944a9a3e33da60296ec2b/Untitled%202.png)

Para solucionar o problema de rodar onde não precisa, podemos especificar a diretorio que queremos rodar os testes com o seguinte comando:

```python
pytest --cov=codigo tests/
```

**Primeiro você especifica aonde quer rodar os testes, que no caso está na pasta “codigo” e onde estão os testes que é na pasta “tests/”**

Assim, temos o seguinte resultado:

![Untitled](Testes%20428a4919b5e944a9a3e33da60296ec2b/Untitled%203.png)

## Como descobrir qual linha não foi testada

Para descobrir qual linha está faltando, rode o seguinte comando no terminal:

```python
pytest --cov=codigo tests/ --cov-report term-missing
```

Ao rodar esse comando, tem-se a linha que ainda falta ser coberta pelo codigo em uma coluna extra no terminal

![Untitled](Testes%20428a4919b5e944a9a3e33da60296ec2b/Untitled%204.png)

Ao solucionar o problema da linha faltante, o seu terminal ao rodar o mesmo comando ficará da seguinte maneira:

![Untitled](Testes%20428a4919b5e944a9a3e33da60296ec2b/Untitled%205.png)

Para facilitar a leitura, podemos jogar a cobertura para um arquivo html da seguinte maneira:

```python
pytest --cov=codigo tests/ --cov-report html
```

Isso gerará arquivos, você pode abrir o arquivo html gerado na pasta “htmlcov” e isso o levará para uma pagina parecida com essa, **lembre de adicionar a pasta em questão ao gitignore**:

![Untitled](Testes%20428a4919b5e944a9a3e33da60296ec2b/Untitled%206.png)

- Item 1
  - Ao clicar em run, você verá todos as linhas cobertas
  - Ao clicar em missing, você verá todos as linhas de código não cobertas
- Item 2
  Caso você não esteja nessa tela, você pode navegar com os “[” “]”.
  Existem mais comandos, para verificar os comandos existentes, clique no teclado que aparece em cima da mesma mencionada na imagem.

Porém, a linha que o terminal acusou que não foi testada, foi uma função **str**, que é uma coisa específica da linguagem python, não faz sentido testar algo pra ver se a linguagem esta funcionando.

Para solucionar isso, podemos “excluir” a linha em questão do teste, crie um arquivo chamado “.coveragerc”

Dentro dele, coloque as linhas que não deseja testar.

```bash
[run]

[report]
exclude_lines =
    def __str__
```

Ao rodar o comando:

```python
pytest --cov=codigo tests/ --cov-report term-missing
```

Você vai obter o seguinte resultado:

![Untitled](Testes%20428a4919b5e944a9a3e33da60296ec2b/Untitled%207.png)

Note que agora, você tem 33 stmts e não mais 34, pois ele não está mais cobrindo o dunder **str**.

## Gerando relatórios

- Podemos gerar através de arquivos html como comentado anteriormente

Para diminuir a verbosidade do que você vai rodar no terminal, você pode editar seu arquivo .coveragerc para

```python
[run]
source = ./codigo

[report]
exclude_lines =
    def __str__
```

Assim, sempre que rodar o comando a seguir, ele rodará diretamente na pasta código, não precisando mais escrever a linha gigante “pytest --cov=codigo tests/”, você pode agora colocar apenas:

```python
pytest --cov
```

Para gerar o html em outra pasta, basta adicionar o seguinte comando no arquivo .coveragerc

```python
[run]
source = ./codigo

[report]
exclude_lines =
    def __str__

[html]
directory = coverage_relatorio_html
```

Assim, ele gerará o relatorio na pasta “coverage_relatorio_html”

- Para que quando eu digite apenas pytest ele gere tudo, eu posso simplesmente, alterar o arquivo pytest.ini para:
  ```python
  [pytest]
  addopts = -v --cov=codigo tests/ --cov-report term-missing
  markers =
      calcular_bonus: Teste para o metodo calcular_bonus
  ```
  Assim, toda vez que rodar o comando no terminal:
  ```python
  pytest
  ```
  ele gerará um teste verboso com coverage e report missing sem a necessidade de escrever no terminal um comando gigante (você também. pode gerar os arquivos html caso queira)
- Relatório xml
  rode o seguinte comando no terminal:
  ```python
  pytest --junitxml report.xml
  ```
  Relatório de coverage:
  ```python
  pytest --cov-report xml
  ```

## Links úteis

[Montando cenários de testes com o Pytest | Alura](https://www.alura.com.br/artigos/montando-cenarios-de-testes-com-o-pytest?_gl=1*1ii7h1k*_ga*NzIzOTA0NjI5LjE2NTQwMDA5NDc.*_ga_1EPWSW3PCS*MTY5NjM1MzU2MS4xNC4xLjE2OTYzNTgxMTAuMC4wLjA.*_fplc*V0g0ajJDOWJaekFrMDZPeEhIWkUyeTZaaTdseW9CWHJLZVBrTyUyQlFWS043ektzMU9HcDIxVmZCQjJEU1FnWkRvaTZPNTREdyUyRmNDSE1UWFRoRlRsTXY3b1lQWlltbUt6anQlMkJrQVpOWjNFN3pjUFdIV0w0NXRwem1GVDNlY2JRJTNEJTNE)

[Full pytest documentation — pytest documentation](https://docs.pytest.org/en/7.1.x/contents.html)
