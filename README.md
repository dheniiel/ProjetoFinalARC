# Projeto Final - Administração de Redes de Computadores
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
### Em síntese
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
