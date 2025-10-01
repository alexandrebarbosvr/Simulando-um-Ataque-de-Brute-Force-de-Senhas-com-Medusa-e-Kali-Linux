# Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux
Desafio Santander Cybersegurança 2025

Nessa fase do Treinamento, aprofundamos um pouco mais sobre as ferramentas de ataque de força bruta, com lista de usuários e senhas em um arquiv.

# AMBIENTE
Meu ambiente está rodando em um Vmware Esxi, com uma rede totalmente separada da rede local, com o endereço dos hosts sendo 192.168.222.35, sendo 1 máquina com o Kali linux, (figura1) no diretório images, e uma máquina com o metasplitable2 (figrura2), com o ip 192.168.222.3. Imagem do teste de conectividade disponível na pasta imagens com o nome (giruta3)

# ATAQUE A SERVIÇO FTP
# CRIAÇÃO DOS ARQUIVOS 
Feito a criação do arquivo de usuários e senhas (Wordlists) para o teste, foi utilizado os comandos abaixo, ilustrado na imagem (figura4).

echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt

# Excutando o ataque com o comando abaixo:

medusa -h 192.168.222.3 -U users.txt -P pass.txt -M ftp -t 6
Onde o parametro -h especifica o host destino, o -U informa o arquivo com a lista de usuários, o - P informa o arquivo com a lista senhas, -M informa o módulo a ser utilizado, no caso o FTP, e o -t para abrir ne exemplo 6 conexões simultâneas.

A saída do comendo confirma uma combinação de usuário e senha como Usuário msfadmin e senha msfadmin, ilustrado na imagem (figura5).

# Efetuando o acesso para ver se a combinação realmente está funcionando.

Acesso concedido conforme imagem (figura6).

# ATAQUE A PÁGINA WEB COM WORDLISTS


Nesse caso iremos criar os arquivos com os usuário e as senhas conforme feito para o teste de ataque ao serviço de FTP, usamos os mesmos comandos.

echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt

Após gerar os arquivos, iremos ao ataque de fato.

Iremos usar o coamnado abaixo para ralizar as tentativas de acesso ao site em questão, ilustrado na figura7.

medusa -h 192.168.222.3 -U users.txt -P pass.txt -M http \
-m PAGE:'dvwa/login.php' \
-m FORM:'username=^USER^&password=^PASS^$Login=login' \
-m 'FAIL=Login failed' -t 6

Após a execução do comando, pudemos ver entre as senhas testadas que o usuário admin e a senha password foi válida para acesso ao site, conforme imagem8.
Na imagem9 podemos ver o site aberto e com o acesso feito com o usuário e senha em questão.

# ATAQUE SERVIÇO SMB

Vamos começar rodando o comando enum4linux -a 192.168.222.3 | tee enum4_output.txt

Com o comando obtivemos várias informações sobre o host em questão, conforme ilustrado na imagem10.

Vamos criar os aquivos de usuário e senha para realizar o ataque, com os comandos abaixo:
  echo -e "user\nmsfadmin\nservice" > smb_users.txt
  echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt

Arquivos ilustrados na imagem11.

Agora vamos ao ataque propriamente digo, utilizando o comando abaixo:

medusa -h 192.168.222.3 -U smb_users.txt -P senhas_spray -M smbnt -t2 -T 50 onde:
  -h host albo
  -U indica qual arquivo ele vai buscar os usuários
  -P indica qual arquivo ele vai buscar as senhas
  -M indica o módulo que vai ser usado, no caso o módulo smbnt
  -t número de tentativas simultâneas, no caso 2
  - T hosts em paralelo

Executando o comando temos a saída ilustrada na imagem12.
Analisando a saída do comando percebemos que temos uma saida AACOUNT FOUND com o usuário msfadmin e senha msfadmin, na qual será usada para acesso ao compartilhamento de rede.

Agora iremos testar o acesso ao compartilhamento no host destino com o comando abaixo:

smbclient -L //192.168.222.3 -U msfadmin e posteriormente colocando a senha msfadmin.
Processo ilustrado na imagem13.

Com isso finalizamos o desafio, obtendo ótimos aprendizados.
