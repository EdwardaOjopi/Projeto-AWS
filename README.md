<h1 align="center"> Projeto AWS :page_facing_up: <br>
Atividade de Linux PB AWS - UNICESUMAR UFOPA <br> </h1>
<br>

# Descrição
A atividade tem como objetivo praticar os conceitos abordados ao longo da sprint 1,2,3 e 4. Foram explorados conceitos gerais de Linux, além de comandos praticados no terminal. Também houveram práticas no console da Amazon AWS e outros serviços que este possui. Resumidamente, será criada uma instância EC2 com sistema operacional Amazon Linux 2, além de fornecer acesso público. Também, será realizado alguns pontos dentro do terminal da AWS(CloudShell), envolvendo a criação de script para verificar o status (online/offline) do Apache, e a execução automatizada do scrip deste a cada 5 minutos.

# Requisitos para iniciar
Para ocorrer a execução deste projeto, alguns pontos devem ser abordados.<br>
- Entraremos em dois ambientes, sendo o primeiro o console da Amazon AWS e posteriormente, a conexão SSH pelo CloudShell;<br>
- Após isso, é preciso seguir os seguintes pontos:<br>

**AWS** :computer:
1. Gerar uma chave pública para acesso ao ambiente;
2. Criar 1 instância EC2 com o sistema operacional Amazon Linux 2 (Família t3.small, 16 GB SSD);
3. Gerar 1 elastic IP e anexar à instância EC2;
4. Liberar as portas de comunicação para acesso público: 22/TCP, 111/TCP e UDP, 2049/TCP/UDP, 80/TCP, 443/TCP.

**Linux** :keyboard:
1. Configurar o NFS entregue;
2. Criar um diretorio dentro do filesystem do NFS com seu nome;
3. Subir um Apache no servidor - o apache deve estar online e rodando;
4. Criar um script que valide se o serviço esta online e envie o resultado da validação para o seu diretorio no NFS;
5. O script deve conter - Data HORA + nome do serviço + Status + mensagem personalizada de ONLINE ou offline;
6. O script deve gerar 2 arquivos de saida: 1 para o serviço online e 1 para o serviço OFFLINE;
7. Preparar a execução automatizada do script a cada 5 minutos;
8. Fazer o versionamento da atividade;
9. Fazer a documentação explicando o processo de instalação do Linux.
<br>

<h1 align="center"> Prática AWS <br> </h1>

<ul>
<li style="list-style-type: 🔔" ><h3>Gerar uma chave pública para acesso ao ambiente </h3>   </li>
- Para iniciar, iremos criar um chave pública, a qual vai conceder o acesso à instância EC2. <br>
1. Procure a opção do painel de EC2 do console AWS; <br>
2. Depois, acesse a opção Pares de Chaves, que está na seção <b>Rede e segurança</b>; <br>
3. Insira o nome e certifique que o formato de arquivo seja .ppk; <br>
4. Após isso, clique em Criar Par de Chaves; <br>
5. Salve o arquivo em algum lugar seguro. <br>

</ul>


<ul>
<li style="list-style-type: 🔔" ><h3> Criar 1 instância EC2 com o sistema operacional Amazon Linux 2 (Família t3.small, 16 GB SSD)</h3>   </li>
- No painel EC2, é possível realizar a criação de uma instância EC2:<br>
1. Vá para a seção de Instâncias;<br>
2. No lado superior direito, clique na opção de Executar instância;<br>
3. Depois, é necessário criar um nome, colocando as principais tags que foram fornecidas no ínicio do Estágio: Nome, CostCenter e Project;<br>
4. Agora selecione o sistema da Amazon Linux, em seguida a opção Amazon Linux 2 AMI(HVM);<br>
5. No Tipo de instância, selecione t3.small;<br>
6. No Par de Chaves, coloque a chave que foi criada na etapa anterior;<br>
7. Na Configuração de Rede, coloque a opção de Criar grupo de segurança, além de permitir tráfego SSH de Qualquer lugar(0.0.0.0/0);<br>
8. Na Configurar Armazenamento, selecione a opção de 16gb de armazenamento gp2(SSD);<br>
9. Ao lado direito, é possível revisar as informações selecionadas;<br>
10. Clique em Executar Instância.<br>
</ul>

<div align="center"> 
  
![image](https://github.com/EdwardaOjopi/Projeto-AWS/assets/114951492/f1964190-5a67-4859-96f9-05f49326a40a)
</div>



<ul>
<li style="list-style-type: 🔔" ><h3> Gerar 1 Elastic IP e anexar à instância EC2</h3>   </li>
- Para gerar um IP Elástico, é preciso ir na mesma seção que se encontra Pares de chaves:<br>
1. Clique em IPs elásticos, na seção de <b>Rede e segurança</b> e selecione Alocar endereço IP elástico;<br>
2. Não será necessário alterar nenhuma opção, então selecione Alocar;<br>
3. Com isso, selecione o IP criado, clique em Ações(parte superior) e escolha Associar endereço IP elástico;<br>
4. Selecione a instância EC2 que foi criada anteriormente, a qual terá um IP fornecido pela própria AWS;<br>
5. Aperte na opção Permitir que o endereço IP elástico seja reassociado e crie o Elastic IP.<br>
</ul>


<ul>
<li style="list-style-type: 🔔" ><h3> Liberar as portas de comunicação para acesso público: 22/TCP, 111/TCP e UDP, 2049/TCP/UDP, 80/TCP, 443/TCP</h3>   </li>
- Para liberar as portas de comunicação, é necessário acessar o Security groups:<br>
1. Na página do EC2, clique em Security groups, na seção <b>Rede e segurança</b>;<br>
2. Selecione o grupo de segurança criado com a instância EC2;<br>
3. Clique em Regras de entrada, na parte inferior, e depois, em Editar regras de entrada;<br>
4. Por padrão, já temos do Tipo SSH, no Intervalo de portas 22, Protocolo TCP;<br>
5. Adicione mais regras, que serão necessárias para liberar os acessos: 22/TCP, 111/TCP e UDP, 2049/TCP/UDP, 80/TCP, 443/TCP;<br>
6. Para finalizar, clique em Salvar regras.<br>
</ul>

Para realizar o acesso ao EFS, iremos criar um servidor NFS a partir do EFS:<br>
1. Acesse o painel EC2 e clique em Security groups;<br>
2. Clique em Criar grupo de segurança;<br>
3. Coloque um nome e selecione uma VPC com uma rota de destino 0.0.0.0/0 com alvo para Gateway de internet;<br>
4. Nas regras de entrada, coloque a seguinte regra: 2049/NFS com origem ao grupo de segurança criado para a instância EC2;<br>
5. Finalize apertando Criar grupo de segurança.<br>

Por fim, criar um serviço EFS:<br>
1. Acesse o serviço EFS pelo console AWS;<br>
2. Clique na seção Sistema de arquivos e em Criar sistema de arquivos;<br>
3. Coloque um nome ao sistema e clique em Personalizar;<br>
4. Marque a opção One Zone e coloque a zona de disponibilidade de sua instância EC2;<br>
5. Depois de avançar, selecione o grupo de segurança feito para o acesso NFS;<br>
6. Clique em Criar para terminar;<br>
7. Vá ate os sistemas e selecione o que foi criado;<br>
8. Clique em Anexar para visualizar as opções de montagem(IP ou DNS), escolhendo a montagem via DNS com o cliente NFS;<br>
9. Depois que finalizar, selecione o sistema e clique em Visualizar detalhes;<br>
9. Salve o comnado que será usado para montar via DNS usando o cliente do NFS.<br>
**Obs:** Foi grifado por motivos de segurança!<br>
<div align="center"> 
  
![image](https://github.com/EdwardaOjopi/Projeto-AWS/assets/114951492/1cd1fccb-f4b2-4e2a-971d-2f3163d2c71d)
</div>

Para dar continuidade, é preciso acessar o terminal da AWS, chamado CloudShell. Por ele, é possível ter acesso público à instância EC2 criada. Para isso:<br>
1. Vá para a seção de Instâncias e selecione a instância criada anteriormente;<br>
2. Após isso, selecione a opção Conectar;<br>
3. Depois, é possível visualizar como será acessado. No caso, iremos pela EC2. O usuário é padrão, então não há necessidade de mudar;<br>
4. Ao conectar, uma página web será aberta, mostrando o acesso da instância.<br>
<div align="center"> 
  
![image](https://github.com/EdwardaOjopi/Projeto-AWS/assets/114951492/6a3584bc-6517-42a1-8b82-9aeb7afa92cf)<br>
</div>
<br>
<h1 align="center"> Prática Linux via CloudShell <br> </h1>

<ul>
<li style="list-style-type: 🔔" ><h3>Configurar o NFS entregue </h3>  </li>
- Ao entrar na conexão da instância EC2, coloque o comando <code>sudo su</code>, que faz ganhar privilégios administrativos: <br>
1. Use <code>sudo yum update -y</code> para atualizar o sistema e instalar versão atualizadas dos arquivos Linux;<br>
2. Use <code>sudo yum install -y amazon-efs-utils</code> para instalar o pacote do NFS;<br>
3. Crie um diretório com o comando <code>sudo mkdir /mnt/efs</code>, destinado a ser o local para montagem;<br>
4. Agora, use o comando de montagem que foi fornecido pela AWS: <code>sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2 [DNS-EFS]:/ [caminho local/diretório]</code>. <br>
<br>
Também é preciso fazer a configuração automática da montagem, para que não seja necessário montar novamente:<br>
1. Coloque o comando <code>sudo nano /etc/ftsab</code>, assim abrindo o arquivo para editar;<br>
2. Dentro, coloque a linha: <code>[ID do sismeta de arquivos]:/ [caminho local/diretório] nfs4 defaults 0 0</code>;<br>
<br>
<div align="center"> 
  
![image](https://github.com/EdwardaOjopi/Projeto-AWS/assets/114951492/7e1e9354-5e59-4363-bfc0-3685d78e0350)
</div>

3. Para confirmar se está montado, utilize o comando <code>df -h</code>.<br>
<br>
<div align="center"> 
  
![image](https://github.com/EdwardaOjopi/Projeto-AWS/assets/114951492/646a0a71-1705-41d9-b648-9a6b972b5d05)
</div>
</ul>

<ul>
<li style="list-style-type: 🔔" ><h3>Subir um apache no servidor</h3>   </li> 
- Para instalar o Apache no servidor, é preciso atualizar o sistema com o comando <code>sudo yum update -y</code>, depois é feito o seguinte:<br>
1. Use o comando de instalação <code>sudo yum install httpd -y</code>;<br>
2. Inicie o Apache com o comando <code>sudo systemctl start httpd</code>. Caso não dê, use <code>sudo /bin/systemctl start httpd.service</code>;<br>
3. Para iniciar de forma automática, use o comando <code>sudo systemctl enable httpd</code>;<br>
4. Para saber se o Apache está executando, use o comando <code>sudo systemctl status httpd</code>.<br>
<br>
<div align="center"> 
  
![image](https://github.com/EdwardaOjopi/Projeto-AWS/assets/114951492/6af733e1-95b0-4bf1-af2c-c5db5167d584)
</div>

</ul>

<ul>
<li style="list-style-type: 🔔" ><h3>Criar um diretorio dentro do filesystem do NFS com seu nome </h3> </li>
- Para criar um diretório dentro do sistema de arquivos do NFS, é preciso ir no caminho do diretório feito para o efs:<br>
1. Use o comando <code>cd /mnt/efs</code>;<br>
2. Crie um novo diretório, com o comando <code>sudo mkdir [seu nome]</code>;<br>
3. Após isso, confirme usando o comando <code>ls</code>.<br>

<div align="center">
  
![image](https://github.com/EdwardaOjopi/Projeto-AWS/assets/114951492/aea712e1-b80e-4517-a203-7d5f6f210918)
</div>
</ul>

<ul>
<li style="list-style-type: 🔔" ><h3>Criar um script que valide se o serviço esta online e envie o resultado da validação para o seu diretorio no nfs, além de possuir Data HORA + nome do serviço + Status + mensagem personalizada de ONLINE ou offline e gerar 2 arquivos de saida: 1 para o serviço online e 1 para o serviço OFFLINE; </h3>   </li>
- Para isso, será preciso criar um script com o <b>nano</b>, além de colocar a extensão <b>sh.</b> para o final do nome do arquivo: <br>
1. Devemos editar o status do serviço no script, usando <code>sudo nano service_status.sh</code>. É importante criar o script no diretório EFS. Iremos salvar no arquivo com o nosso nome, como /mnt/efs/edwarda;<br>
2. Dentro do arquivo, digite o script abaixo:<br>
<pre><code>
#!/bin/bash

service_name="Apache"
timestamp=$(date +"%Y-%m-%d %H:%M:%S")

if systemctl is-active httpd; then
    echo "Data/Hora: $timestamp - $service_name - Serviço HTTPD - Status: o serviço está online" >> "/mnt/efs/edwarda/httpd-online.txt"
else
   echo "Data/Hora: $timestamp - $service_name - Serviço HTTPD - Status: o serviço está offline " >> "/mnt/efs/edwarda/httpd-offline.txt"
fi
</code></pre>

3. Depois, salve o arquivo.<br>
4. Agora, é necessário torna o script executável, colocando o comando <code>sudo chmod +x [nome do script]</code>, onde o nome do script é equivalente ao <b>service_status.sh</b>; <br>
5. Agora, veja se está no diretório com seu nome. Caso não esteja, utilize <code>cd /mnt/efs/edwarda</code> para acessar. Com isso, use o comando <code>./service_status.sh</code>;<br>
6. Se precisar ler, é possível com o comando <code>cat httpd-online.txt</code>.<br>

Também é pedido para automatizar a execução do script a cada 5 minutos. Para isso, iremos editar o arquivo crontab:<br>
1. Use o comando <code>EDITOR=nano crontab -e</code>, que irá acessar o arquivo crontab;<br>
2. Lá dentro, digite a seguinte linha: <pre><code>\*/5 \* \* \* \* /\[caminho de onde está o script/nome do script]</code></pre>, onde o que ficaria desse jeito: <pre><code>*/5 \* \* \* \* /mnt/efs/edwarda/service_status.sh</code></pre><br>
3. Aperte no ESC e salve digitando <code>:wq</code>;<br>
4. A seguir, temos as duas figuras que demonstram o serviço online e offline, além dos scripts criados dentro do diretório /mnt/efs/edwarda:
<br>
<div align="center">
 
![image](https://github.com/EdwardaOjopi/Projeto-AWS/assets/114951492/b6e73375-19c7-43c3-9872-23d23c0be44b)
</div>
<br>
<div align="center">
  
![image](https://github.com/EdwardaOjopi/Projeto-AWS/assets/114951492/68ccd201-af7c-49b4-8f05-6ca728c29134)
</div>
</ul>
