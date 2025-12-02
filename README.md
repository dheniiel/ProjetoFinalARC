# Projeto Final - Administração de Redes de Computadores
## Dheniel Rodrigues Luis e Felipe Ferreira Gomes
### Link: https://drive.google.com/drive/folders/1hRkuMkP-h2-ly6d5oKFZ04PtdLRZFjwm?usp=drive_link
Trabalho final da disciplina de Administração de Redes de Computadores, curso Sistemas de Informação do Instituto Federal Goiano Campus Ceres.
A prosposta do projeto final é "projetar, implantar e gerenciar uma rede empresarial usando tecnologia Linux, com ênfase em serviços como DHCP, DNS, Web, FTP, NFS e virtualização."

# Topologia (Visão geral)
A topologia consiste em três máquinas virtuais integradas em uma rede isolada para fins de testes. O ambiente utiliza um pfSense como servidor DHCP e DNS, e duas máquinas Linux Mint, onde uma funciona como servidor de serviços (NFS, FTP e Apache) e a outra como cliente.

### Máquina Virtual 1 - Servidor pfSense (DHCP e DNS)
O servidor pfSense está sendo utilizado para prestar serviços de DHCP e DNS. A máquina virtual está com duas placas de rede habilitada, a primeira em "Modo Bridge" e a segunda em "Rede Interna", sendo respectivamente atribuídas as interfaces WAN e LAN. Deste modo, o endereço IP da interface WAN é atribuído via DHCP pela rede em que a máquina nativa está e o IP da interface LAN é atribuído manualmente.

### Máquina Virtual 2 - Linux Mint (Servidor Apache/FTP/NFS)
A máquina virtual 2 usa o sistema operacinal do Linux Mint para ser servidor Apache (HTTP), FTP e NFS. Sua placa de rede está habilitada como "Rede Interna", bem como seu endereço IP é atribuído via DHCP do servidor pfSense configurado, sendo amarrado ao endereço MAC da máquina virtual por se tratar de um servidor. 

### Máquina Virtual 3 - Linux Mint (Cliente)
Por fim, a máquina virtual 3 serve como cliente para o serviços fornecidos pela máquina virtual 2 (Apache/FTP/NFS). Também tendo sua placa de rede habilitada como "Rede Interna", esta máquina virtual recebe IP dinamicamente via DHCP do servidor pfSense. 

# DHCP
Para configurar o servidor DHCP, foi necessário acessar a interface visual por meio do IP, recebido via DHCP na interface WAN, colocando-o no navegador. Ao acessar o pfSense no navegador, é necessário ir na aba Serviços -> Servidor DHCP. Após, é necessário marcar a opção de "Habilitar servidor DHCP na interface LAN". Por padrão, já vem pré-estabelecido com a mesma rede do IP configurado manualmente na LAN durante a configuração do pfSense, sendo neste caso a rede 172.16.0.0/24 com o range de sub-rede 172.16.0.1 - 172.16.0.254. Neste projeto, foi usado o range começando no IP 172.16.0.130 e acabando no IP 172.16.0.150. Para a configuração do DNS em que DHCP vai ofertar, foi colocado o IP do servidor pfSense na LAN (172.16.0.13), pois o servidor DNS também será configurado posteriormente. Na opção "Gateway" também é atribuído o IP do servidor pfSense, o dominío atribuído é o "lan". Em "Default Lease Time" é atríbuido 7200s (2 horas) como tempo de concessão padrão, caso o cliente não especificar, e em "Maximum Lease Time" é atribuído 86400s (24h) sendo o tempo máximo que o cliente pode ficar com o IP sem renová-lo. Por fim, para amarrar um IP ao endereço MAC dá máquina que vai ser servidor, é necessário a opção "DHCP Static Mappings" e procurar pelo botão "Add DHCP Static Mapping". Ao clicar adicionar no endereço MAC dá máquina virtual do servidor (Apache/FTP/NFS), sendo neste caso 08:00:27:17:ee:9b, atribuir um endereço IP fora do range escolhido, neste caso sendo 172.16.0.125 e adicionando nome ao host, nesse caso sendo "servidor".

### Em síntese:
#### 1 - Habilitar servidor DHCP na interface LAN
#### 2 - Subrede: 172.16.0.0/24, Subrede range: 172.16.0.1 - 172.16.0.254 (Vem pré-estabelecido de acordo com o IP da LAN, atribuído manulmente no pfSense)
#### 3 - Address Pool Range: 172.16.0.130 - 172.16.0.150 (Usado neste exemplo de rede)
#### 4 - Servidores DNS: 172.16.0.13 (IP do próprio servidor pfSense, pois também será configurado um servidor DNS)
#### 5 - Gateway: 172.16.0.13 (IP do próprio servidor)
#### 6 - Domain name: lan (Dominío criado para esta rede)
#### 7 - Default lease time: 7200s/2h (Tempo em que o DHCP atribui o IP antes que o clinte precise renovar)
#### 8 - Maximum  lease time: 86400s/24h (Tempo máximo em que um cliente pode ficar com IP antes que precise renovar)
#### 9 - DHCP Static Mappings 
#### 9.1 - Endereço MAC: 08:00:27:17:ee:9b (Endereço MAC da máquina que vai ser o servidor)
#### 9.2 - Endereço IP: 172.16.0.125 (IP que vai ser amarrado ao servidor)
#### 9.3 - Nome de host: servidor (Nome atribuido ao servidor)

# DNS 
Para configurar o DNS resolver, é necessário ir na barra de navegação e procurar por Serviços -> DNS Resolver. Ao acessar esta opção é necessário marcar a opção "Ativar o resolvedor de DNS" e a porta de escuta permanece a padrão 53. Em "Interface de Rede" deixar a opção "Todos", estabelecendo que o DNS vai aceitar e responder consultas de todas a interfaces. Em "Interfaces de Rede de Saída" configurar como "WAN", definindo que a WAN vai ser a interface que vai enviar as consultas a DNS externos. Por fim, na opção "Sobrescrever Host" clicando em "Adicionar", foi configurado o host "cliente" como o domínio "lan" ao endereço IP 172.16.0.132 e o host "servidor" com domínio "lan" ao endereço IP 172.16.0.125.
### Em síntese: 
#### 1 - Ativar o resolvedor de DNS
#### 2 - Porta de escuta: 53 (Porta padrão do DNS)
#### 3 - Interface de rede: Todos (O servidor DNS vai aceitar e responder consultar de todas as interfaces)
#### 4 - Interface de rede de saída: WAN (A WAN vai ser a interface responsável por enviar consultas a DNS externos)
#### 5 - Sobrescrever Host
#### 5.1 - Host: cliente (Nome apenas representativo)
#### 5.2 - Domínio lan (Domínio criado no servidor pfSense)
#### 5.3 - Endereço IP: 172.16.0.132 (Endereço IP da máquina que desejo atribuir esse host)
#### 5.4 - Host: servidor (Nome apenas representativo)
#### 5.5 - Domínio lan (Domínio criado no servidor pfSense)
#### 5.6 - Endereço IP: 172.16.0.125 (Endereço IP amarrado a máquina que desejo atribuir esse host)

# Apache 
Nesta rede, o servidor Apache foi criado e testado de modo simples e pragmático. Iniciando-se com a instalação do próprio servidor, na máquina virtual que é servidor (172.16.0.125). Após a instalação verificar se o serviço está rodando e testá-lo colocando o IP  172.16.0.125 e aparecendo a página padrão do servidor Apache.

### Passo a passo: 
#### 1 - sudo apt install apache2 -y (Instalar o apache)
#### 2 - systemctl status apache2 (Consultar se está rodando, se estiver terá algo parecido com "Active: active (running)")
#### 3 - Para testar é só colocar o IP do servidor no navegador (172.16.0.125), na máquina servidor ou cliente, aparecendo assim a página HTML padrão do Apache.

# FTP
De forma inicial, para configurar o servidor FTP e utilizar do compartilhamento de arquivos, é necessário que a máquina virtual esteja com a placa de rede habilitada como "Rede Interna".

### Passo a passo: 
### Na máquina servidor:  
#### 1 - sudo apt update (Para atualização dos repositórios)
#### 2 - sudo apt upgrade (Para atualizar os pacotes já instalados no sistema operacional)
#### 3 - sudo apt install vsftpd -y (Para a instalação do VSFTPD)

Após a instalação, foi verificado se o serviço estava em execução corretamente, utilizando o comando 
#### 4 - sudo systemctl status vsftpd. O resultado esperado para funcionamento adequado é: **Active: running.**

Em caso de erro como **Unit vsftpd.service not found**, é necessário reinstalar o VSFTPD utilizando o comando: 
#### 5 - sudo apt reinstall vsftpd -y. 

Caso ocorra o erro **500 OOPS: refusing to run with writable root inside chroot()**, que impede o login via FTP, deve-se acessar o arquivo de configuração com: 
#### 6 - sudo nano /etc/vsftpd.conf.

Em seguida, com CTRL + W, localizar as configurações e inserir a diretiva: 
#### 7 - allow_writeable_chroot=YES. 

Também é necessário garantir que a linha **chroot_local_user** esteja como **YES**. Ao confirmar ambos, salvar com CTRL + O, pressionar ENTER e sair com CTRL + X. 
Para o erro "**500 OOPS: run two copies of vsftpd for IPv4 and IPv6"**, deve-se retornar ao arquivo de configuração 
#### 8 - sudo nano /etc/vsftpd.conf 
e ajustar as diretivas para:
#### 9 - listen=YES e listen_ipv6=NO. 

Após salvar, o serviço deve ser reiniciado com: 
#### 10 - sudo systemctl restart vsftpd. 

Listando o IP do servidor com o comando
#### 11 - ip a 
foi identificado o endereço: 172.16.0.125. 

### Na máquina cliente
É feito o acesso utilizando o comando: 
#### 1 - ftp 172.16.0.125 
que corresponde ao IP do servidor previamente configurado. 
Após inserir o nome de usuário e senha definidos no servidor, o acesso é estabelecido. 
Para listar e verificar os arquivos disponíveis, utiliza-se o comando: 
#### 2 - ls

# NFS
Para configurar o servidor NFS, foi necessário instalar o pacote nfs-kernel-server na máquina Servidor, configurando-o corretamente e aplicando as configurações. Na máquina cliente, é necessário baixar o pacote nfs-common para utilizar o serviço do NFS e após, montar o diretério compartilhado em um diretório local. 

### Passo a passo: 

### Na máquina servidor
#### 1 - sudo apt install nfs-kernel-server (Comando para instalar o servidor NFS)
#### 2 - sudo mkdir -p /servidor/pastacomp (Comando para criar o diretório a ser compartilhado)
#### 3 - sudo chmod 777 /servidor/pastacomp (Comando para atribuir todas permissões no diretório)
#### 4 - sudo nano /etc/exports (Comando para acessar e editar o arquivo "exports", responsável pela configuração do servidor NFS)
#### 4.1 - /servidor/pastacomp 172.16.0.0/24(rw,sync,no_root_squash)
/servidor/pastacomp = diretório a ser compartilhado; <br> 
172.16.0.0/24: ficará disponível para toda a rede local; <br>
rw: permissão de leitura e escrita; <br>
sync: escrita síncrona; <br> 
no_root_squash: libera privilégios para o root. 
#### 5 - sudo exportfs -ra (Comando para aplicar as configurações estabelecidas no arquivo /etc/exports)
#### 6 - sudo exportfs -v (Comando para verificar se o diretório realmente foi compartilhado)
#### 7 - systemctl status nfs-kernel-server (Comando para ver se o serviço está realmente ativo, se estiver aparecerá com "Active: active (running)")

### Na máquina cliente
#### 1 - sudo apt install nfs-common (Comando para instalar o NFS para os clientes que vão utilizar o serviço)
#### 2 - sudo mkdir -p /cliente/pastacomp (Comando para criar o diretório que será montado o compartilhamento)
#### 3 - sudo mount 172.16.0.125:/servidor/pastacomp /cliente/pastacomp: 172.16.0.125 = Endereço IP do servidor NFS, /servidor/pastacomp = diretório do servidor que vai ser compartilhado, /cliente/pastacomp = direótiro do cliente em que será montada a pasta compartilhada.
#### 4 - sudo nano /etc/fstab (Comando para configurar o arquivo /etc/fstab garantindo que a montagem sempre seja feita automaticamente)
#### 4.1 - 172.16.0.125:/servidor/pastacomp   /cliente/pastacomp   nfs   defaults,noatime,_netdev   0   0
172.16.0.125:/servidor/pastacomp = diretório do servidor que vai ser compartilhado; <br>
/cliente/pastacomp = diretório de montagem da máquina cliente; <br>
nfs = comando para utilizar o sistema de arquivos nfs; <br>
defaults = opção padrão de configuração; <br>
noatime = melhora o desempenho não escrevendo no disco; <br>
_netdev = monta o diretório apenas se os serviços de rede estiverem funcionando;<br>
0 0 = comando para não ser realizado o dump nem ser verificado pelo fsck. 

#### 5 - mount -a (Comando para ser montado tudo, aplicando as configurações no cliente).

### Teste
Para testar o servidor NFS basta procurar no gerecidor de arquivos o diretório compartilhado e/ou montado, criando um arquivo em algum dos diretórios e verificando no outro. Por exemplo: procurar pela pasta /servidor/pastacomp no gerenciador de arquivos do servidor e criar um arquivo txt, depois acessar a máquina cliente e verificar se na pasta /cliente/pastacomp está o arquivo txt criado. 
