# samba-linux
Configurar o compartilhamento de arquivos pela rede entre Linux e Windows

## Passo 1: Instalação do Samba no Linux

Supondo que você esteja usando o Ubuntu ou outra distribuição baseada no Debian:

Verifique se o Samba não está instalado:

```
samba --version
```

### Instale o Samba:

```
sudo apt update sudo apt install samba
```

## Passo 2: Configuração do Samba

### Crie um diretório para compartilhar:

```
mkdir ~/compartilhado
```

### Ajuste as permissões do diretório:

```
sudo chown -R seu_usuario:seu_usuario ~/compartilhado sudo chmod -R 755 ~/compartilhado
```

### Edite o arquivo de configuração do Samba:

```
sudo vim /etc/samba/smb.conf
```

No final do arquivo, adicione o seguinte conteúdo (substitua seu_usuario pelo seu nome de usuário):

```
[compartilhado]
    path = /home/seu_usuario/compartilhado
    valid users = seu_usuario
    read only = no
```

### Defina uma senha para o usuário Samba:

```
sudo smbpasswd -a seu_usuario
```

### Reinicie o serviço Samba:

```
sudo systemctl restart smbd
```

## Passo 3: Acesso ao Compartilhamento no Windows

Encontre o endereço IP do seu computador Linux:Você pode fazer isso usando o comando ifconfig no terminal Linux e procurando por inet na interface de rede que você está usando (geralmente eth0 para Ethernet ou wlan0 para Wi-Fi).

No Windows, abra o Explorador de Arquivos:Na barra de endereços, digite \\IP_DO_LINUX (substitua IP_DO_LINUX pelo endereço IP do seu Linux) e pressione Enter.Por exemplo: \\192.168.1.100

Faça login: Insira o nome de usuário e senha que você definiu para o compartilhamento Samba no Linux.

Acesse o compartilhamento:Agora você deve ver o compartilhamento criado no Linux e poderá acessar os arquivos compartilhados.

Certifique-se de que ambos os sistemas estejam na mesma rede local e, se necessário, ajuste as configurações do firewall no Linux para permitir o tráfego do Samba.

Lembre-se de adaptar as instruções de acordo com a sua distribuição Linux específica e configurar os detalhes de permissões e usuários conforme necessário.

## Configuração do Firewall

Para permitir o compartilhamento de arquivos entre Linux e Windows através do Samba, é necessário ajustar as configurações de firewall em ambos os sistemas operacionais. Vou detalhar como fazer isso:

### No Linux:

Se você estiver usando o firewall ufw (padrão em muitas distribuições Ubuntu-based), siga estes passos:

Verifique o status do ufw:

```
sudo ufw status
```
Se o ufw estiver ativo, adicione uma regra para permitir o tráfego Samba:

```
sudo ufw allow Samba
```

### Verifique se a regra foi adicionada corretamente:

```
sudo ufw status
```

### No Windows:

Para permitir o acesso ao compartilhamento do Samba através do firewall do Windows:

Abra o Painel de Controle e vá para "Configurações de Firewall":

Clique em "Configurações avançadas":

Crie uma nova regra de entrada:Selecione "Nova Regra".

Escolha o tipo de regra como "Porta" e clique em "Avançar".

Selecione "TCP" e especifique a porta do Samba (o padrão é a porta 445).

Permita a conexão e clique em "Avançar".

Escolha "Todos os perfis" (pública, privada e domínio) e dê um nome para a regra.

Clique em "Concluir" para adicionar a nova regra.

### Observações:

Certifique-se de que ambos os sistemas (Linux e Windows) estejam na mesma rede local.

Use as ferramentas de firewall apropriadas para configurar as regras de acesso ao Samba, garantindo que o tráfego na porta usada pelo Samba (por padrão, a porta 445) seja permitido.

Adicionar exceções nos firewalls permite que o tráfego Samba seja recebido e enviado, facilitando o compartilhamento de arquivos entre os sistemas.

Lembre-se de que, por motivos de segurança, é recomendável restringir o acesso apenas às portas e serviços necessários. Certifique-se de entender os riscos ao abrir portas em seus firewalls e aplique as regras com base nas suas necessidades de compartilhamento de arquivos.
