<h1 align="center"> Projeto AWS :page_facing_up: <br>
Atividade de Linux PB AWS - UNICESUMAR UFOPA <br> </h1>
<br>

# Descrição
A atividade tem como objetivo praticar os conceitos abordados ao longo da sprint 1,2,3 e 4. Foram explorados conceitos gerais de Linux, além de comandos praticados no terminal. Também houveram práticas no console da Amazon AWS e outros serviços que este possui. Resumidamente, será criada uma instância EC2 com sistema operacional Amazon Linux 2, além de fornecer acesso público. Também, será realizado alguns pontos dentro do terminal Putty, envolvendo a criação de script para verificar o status (online/offline) do Apache, e a execução automatizada do scrip deste a cada 5 minutos.

# Requisitos para iniciar
Para ocorrer a execução deste projeto, alguns pontos devem ser abordados.
- Entraremos em dois ambientes, sendo o primeiro o console da Amazon AWS e posteriormente, a conexão SSH pelo Putty;
- Após isso, é preciso seguir os seguintes pontos:

**AWS** :computer:
1. Gerar uma chave pública para acesso ao ambiente;
2. Criar 1 instância EC2 com o sistema operacional Amazon Linux 2 (Família t3.small, 16 GB SSD);
3. Gerar 1 elastic IP e anexar à instância EC2;
4. Liberar as portas de comunicação para acesso público: (22/TCP, 111/TCP e UDP, 2049/TCP/UDP, 80/TCP, 443/TCP).

**Linux** :keyboard:
1. Configurar o NFS entregue;
2. Criar um diretorio dentro do filesystem do NFS com seu nome;
3. Subir um apache no servidor - o apache deve estar online e rodando;
4. Criar um script que valide se o serviço esta online e envie o resultado da validação para o seu diretorio no nfs;
5. O script deve conter - Data HORA + nome do serviço + Status + mensagem personalizada de ONLINE ou offline;
6. O script deve gerar 2 arquivos de saida: 1 para o serviço online e 1 para o serviço OFFLINE;
7. Preparar a execução automatizada do script a cada 5 minutos.
8. Fazer o versionamento da atividade;
9. Fazer a documentação explicando o processo de instalação do Linux.
<br>

<h1 align="center"> Prática AWS <br> </h1>
<br>

## 1. Gerar uma chave pública para acesso ao ambiente;
## 2. Criar 1 instância EC2 com o sistema operacional Amazon Linux 2 (Família t3.small, 16 GB SSD);
## 3. Gerar 1 elastic IP e anexar à instância EC2;
## 4. Liberar as portas de comunicação para acesso público: (22/TCP, 111/TCP e UDP, 2049/TCP/UDP, 80/TCP, 443/TCP).

Para iniciar, iremos criar um chave pública, a qual vai conceder o acesso à instância EC2. Com isso, procure a opção na página de EC2 do console AWS. Depois, acesse a opção Pares de Chaves, que está na seção **Rede e segurança**.


