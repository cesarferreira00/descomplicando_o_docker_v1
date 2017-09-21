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


## Aula 4

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


## Aula 5

Instalando o Docker

Docker só roda em processadores de 64bits
Só pode ser instalado em versões do Kernel acima da 3.8
Seu kernel deve ter os módulos para suportar os storagesdriver - AUFS ou Device Mapper
Seu Kernel deve estar habilitado o cgroup e o namespace

curl -fsSL https://get.docker.com/ | sh


## Aula 6

Administrando Containers

Comando Docker - é um CLI (Comand Line)
docker run - utilizado para executar um continer

Se a imagem não existir na sua máquina ele vai fazer o pull dessa imagem. Ele vai l;a no Docker Hub e faz o download dessa imagem de continer para a sua máquina e executa.

docker images - o parâmetro images do docker é para mostrar todas as imagens que você já tenha na sua máquina.

docker ps -a - se eu quero visualizar todos os containers da minha máquina. Todos os containers que foram executados eles permanecem na máquina, ocupando espaço.

docker run -ti - o "t" é para que você tenha um terminal e o "i" para que você tenha uma interação com o seu container

docker run -d - para que você faça com o que o seu container rode como se fosse um deamon, um processo em backgorund

Exemplo: docker run -ti ubuntu /bin/bash
docker run (executa o container)
-ti (parâmetro para um terminal interativo)
ubuntu (nome da imagem)
/bin/bash (qual o comando que eu quero executar - nesse caso o shell)

Se eu der ctrl+d eu mato o container porque eu sairei do shell e o principal processo desse container é o shell
Mas se eu der ctrl+pq eu saio do container mas mantenho ele em execução

Se eu quiser der um nome para o container eu passo o parâmetro --name
docker run -ti --name teste debian


docker attach - é o comando que eu uso para voltar ao container - eu preciso do Container ID para isso - eu consigo o container ID digitando docker ps

docker create - crio o container, mas ele fica parado, não tenho nenhuma interação com ele - não foi colocado para executar

docker stop - eu paro o container - preciso passar o Container ID como parâmetro

docker start - eu "starto" novamente o container - passar o Container ID como parâmetro

docker pause - eu pauso o meu container - passar o Container ID como parâmetro

docker unpause - eu descongelo, retomo o container pausado - passar o Container ID como parâmetro

docker stats - mostra o consumo do meu container - passar o Container ID como parâmetro

docker top - mostra quais os processos estão rodando no meu container - passar o Container ID como parâmetro

docker logs - mostra os logs do container - o container loga tudo o que está em primeiro plano - passar o Container ID como parâmetro

docker rm - remove o container parado - passar o Container ID como parâmetro (passo o -f para forçar caso o container esteja em execução)

## Aula 07 - Limitando CPU e MEM dos containers e o Docker Update

Quando você cria um contianer e não passa nada, nem a quantidade max de mem e cpu, ele pode comprometer outros container, por utilizar demais outros recursos. É necessário tomar cuidado com isso e setar um limite para utilização dos recursos pelos containers de seu host

Como limitar a memória - (se eu der um 'free -m' no container, ele vai retornar a mesma quantidade de memória do host)

Para eu inspecionar um container, eu uso o comando docker inspect passado o container id como parâmetro
docker inspect a7aba39dd4b6

Para verificar a memória do container vou contar com o auxílio do grep
docker inspect a7aba39dd4b6 | grep -i mem

Se eu quero passar um limite de utilização de memória para o container é o parâmetro -m ou --memory
docker run -ti --memory 512m --name novo_teste debian

Se eu quero alterar um limite de utilização de memória já definido para um container eu uso o comando docker update com o parâmetro --memory o novo valor seguidos do docker id ou o nome do container
docker update --memory 256m novo_teste

Não vamos definir a cpu com %
Vamos pensar assim:
Digamos que o container1 tenha como o valor de cpu 1024, o container2 como 512 e o container3 como 512
Proporcionalmente o container1 terá como valor 50% de cpu, o container2 25% e o container3 25%
Simples como voar

Para limitar a utilização de cpu pelo container na criação usaremos o parâmetro --cpu-shares
docker run -ti --cpu-shares 1024 --name container1 debian

Para inspecionar o container utilizaremos o docker inspect mas desta vez com o | grep -i cpu - já que queremos as informações de cpu do container
docker inspect container1 | grep -i cpu

Para alterar o limite de cpu que já está definido para um container, utilizo novamente o comando docker update
docker update --cpu-shares 512 container1
