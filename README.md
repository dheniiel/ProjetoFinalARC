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
