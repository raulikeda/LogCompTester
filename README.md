# MonitoriaLogComp
Repositorio de ferramentas de avaliacao de compiladores feitos na materia

## Como rodar:

0. Instalar python (versão 3.7 ou acima) e a dependência gitPython:
```
$ pip install gitpython
```

1. copiar path do github dos compiladores dos alunos em um arquivo json;
2. Para puxar a release x.x de cada um, basta usar, por exemplo:
```
$ python fetch_releases.py git_paths.json 2.0
```

Isso criara uma pasta da release src/x.x com uma pasta por aluno, contendo o codigo fonte de compilador.

3. Para rodar testes de input de string no terminal, use, por exemplo:
```
$./run_test_lines.sh 1.2-tests.txt 1.2/aluno1/   
```

4. Para rodar testes de input de arquivos no terminal, use, por exemplo:
```
$./run_test_files.sh 2.0-tests/ 2.0/aluno1/   
```

Assume-se que o aluno possui um arquivo main.py no diretorio raiz de seu compilador. 

Se o aluno nao deu o release ou errou a tag da release, seu template de review devera estar vazio




versão 2.0

## Dependências:


Existem duas dependências além do python

1. Gitpython usado para fazer o pull, ele pode ser instalado com:

```
$ pip install gitpython
```

2. Ghi usado para criar as issues ele precisa ser instalado e configurado:
```
$ sudo apt install ghi
$ ghi config --auth "username"
```
## Configurando projeto:

O codigo esta divido em três partes, fetch_releases.py, auto_test.py e issuer_pusher.py elas em ordem fazem pull de todos os repositórios, fazem todos os testes para os repositórios e criam as issues.

O fetch_releases precisa de uma chave SSH configurada para se comunicar com o github é esperado que ela esteja no path "~/.ssh/id_rsa" que é o padrão.
Um tutorial de como criar uma chave ssh para o github pode ser encontrado [aqui](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

Tanto o fetch_releases e o auto_test precisam de um git_paths configurado, o git_paths.json contem uma lista de dicionarios com as informaçoes de todos os alunos,
ele tem os seguintes parâmetros:
1. student_username - nome do usuario git do aluno
2. repository_name - nome do repositorio que contem o compilador
3. run_args - é os argumentos que um cmd precisa para executar o compilador
4. compile_args - é os argumentos que um cmd precisa para compilar o compilador, so é necessário se linguagem precisar de compilação
5. language - é a língua que o compilador está escrita

Um exemplo pode ser visto a seguir:

```
[
    {
        "student_username": "lucassa3",
        "repository_name" : "compilador",
        "run_args" : "python3 somador.py",
	"language" : "python3"
    },
    {
        "student_username": "raulikeda",
        "repository_name" : "rep",
        "run_args" : "./out/compylador",
        "compile_args": "dotnet build --nologo -o out",
	"language" : "C#"
    }
]
```

## Como rodar:

Importante: todos os caminhos do codigo estão relativos, assim eles precisam ser executados por um cmd que esta dentro da pasta Compiler

Ao executar o fetch_releases ele deleta todos os reports e os codigos dos alunos que estão em src
1. Para puxar a release x.x de cada aluno, basta usar:
```
$ python fetch_releases.py git_paths.json 2.0
```
Se uma realese não for encontrada é criado um report dentro da pasta reports

2. Para rodar o os testes para todos os alunos
```
$ python3 auto_test.py php/2.4
```
O auto_testes executa os testes para todos os alunos que não tem um report na pasta de reports 
