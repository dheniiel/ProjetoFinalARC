# Projeto Final - Administração de Redes Computadores
Trabalho final da disciplina de Administração de Redes de Computadores, curso Sistemas de Informação do Instituto Federal Goiano Campus Ceres.
A prosposta do projeto final é "projetar, implantar e gerenciar uma rede empresarial usando tecnologia Linux, com ênfase em serviços como DHCP, DNS, Web, FTP, NFS e virtualização."

# Topologia 
A topologia consiste em três máquinas virtuais integradas em uma rede isolada para fins de testes. O ambiente utiliza um pfSense como servidor DHCP e DNS, e duas máquinas Linux Mint, onde uma funciona como servidor de serviços (NFS, FTP e Apache) e a outra como cliente.

## Máquina Virutal 1 - Servidor pfSense
O servidor pfSense está sendo utilizado para prestar serviços de DHCP e DNS. A máquina virtual está com duas placas de rede habilitada, a primeira em Modo Bridge e a segunda em Rede Interna, sendo respectivamente atribuídas as interfaces WAN e LAN. Deste modo, o endereço IP da interface WAN é atribuído via DHCP pela rede em que a máquina nativa e o IP da interface LAN é atribuído manualmente, sendo 172.16.0.13/24 nesta rede em questão.
