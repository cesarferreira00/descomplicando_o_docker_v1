# Descomplicando o Docker v1

## Aula 1

O que é um container?

Container - reduz custos
Container - pense na aplicação - sua app precisa de libs, binaries, etc - ele gera binário
Ele vai usar o kernel da máquina física, o conteiner não tem kernel

O Container não é VM
O container é a virtualização da sua aplicação o não do OS como um todo

Como o container é bem menor que uma VM a portabilidade é bem mais fácil, posso até trocar de distro que o container vai funcionar normalmente 

Em 2013 foi que surgiu o Docker.


## Aula 2

O que é Docker?

Queria ser um PaaS (Plataform as a service)
em 2013 a Dotcloud pegou o core de adm de containers e tornar opensource

Depois de opensource virou Docker

Levou apenas 15 meses para sair da v 0.1 para a versão estável

Empresas grandes que utilizam: Google, Netflix, Spotify, Facebook
Fizeram papers e palestras evagelizando na comunidade
A Red Hat apoia também o Docker

A Microsoft vai rodar Docker nas próximas versões do Windows Server


## Aula 3

Camadas

O Docker utiliza dois storage drivers - um é o AUFS e o outro é o Device Map

AUFS = Another Union File System
Priemiro File System do Docker
Vai sobrepondo uma camada em cima da outra, todas as camadas exceto a última são Read Only
Quando seu arquivo está na camada Read Only, ele carrega, faz uma cópia para a cada Read Write

Copy-on-Write
É como se você fosse fazer uma anotação em um livro e no momento que você vai iniciar essa anotação, viesse uma cópia do livro para você anotar ao invés do original, preservando assim o original - Dessa forma o arquivo que você altera é uma cópia e o original é preservado

O Device Map foi criado pela Red Hat
