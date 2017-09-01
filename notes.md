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

O Docker utiliza dois storage drivers - um é o AUFS e o outro é o Device Mapper

AUFS = Another Union File System
Priemiro File System do Docker
Vai sobrepondo uma camada em cima da outra, todas as camadas exceto a última são Read Only
Quando seu arquivo está na camada Read Only, ele carrega, faz uma cópia para a cada Read Write

Copy-on-Write
É como se você fosse fazer uma anotação em um livro e no momento que você vai iniciar essa anotação, viesse uma cópia do livro para você anotar ao invés do original, preservando assim o original - Dessa forma o arquivo que você altera é uma cópia e o original é preservado

O Device Mapper foi criado pela Red Hat


# Aula 4

Docker Internals

**namespaces** - Foi incorporado no kernel Linux na V 2.6.24
ele é o principal responsável pelo o container tenha o seu próprio ambiente, seu enviroment, seu isolamento, seu próprio IP

PID namespace responsável por isolar os PID`s, fazer com que o container tenha sua própria árvore de Processor Indentificator

netnamespace - responsável por fazer o isolamento das interfaces de rede - você tem uma eth0 no host e uma eth0 no continer - ele faz esse isolamento e também responsável por fazer a comunicação

mntnamespace - ele faz o isolamento de filesystem, o filesystem do continer é só dele ele não vai compartilhar com ninguém

ipcnamespace - ipcnamespace - semáforo, padrão posix - a fila de mensagem no padrão posix

utsnamespece - fornece o isolamento de hostname do container, versão do OS, isola essa parte

usernamespace - ela é responsável por fazer o isolamento de usuários - ela foi adicionada ao kernel linux a partir da v3.8 - recente - é responsável pelo mapa de identificação de usuários no container

o namespaces faz o isolamento de um monte de coisas mas não faz o isolamento de cpu, memória... que faz isso é o cgroups

**cgroups** - é o responsável por limitar, fazer o isolamento e liberar recursos para os containers, seja cpu, seja memória, IO (permitindo limitar a quantidade de leitura ou escrita de IO por tamanho)

**netfilter** - nada mais é do que o módulo do kernel do iptables - quase toda a parte de rede, rotas e tudo mais o docker utiliza o iptables, os redirects de porta

Para que serve o Docker?
Se você é desenvolvedor - fica mais fácil para você criar sua aplicação, testar na sua máquina no ambiente de dev, você vai poder pegar esse único binário, poder ser passado para o ambiente de stage, de prod, muito mais rápido

Para o sysadmin - você não precisa ficar preparando o server para a aplicação em cada linguagem diferente - é possivel gerenciar melhor os recursos e a utilização dos componentes - disponibilidade se cair um container, 5 seg já tem outro em pé

Para empresa - ela vai economizar recursos - não será necessário ficar comprando servidores que muitas vezes ficam sub utilizados

