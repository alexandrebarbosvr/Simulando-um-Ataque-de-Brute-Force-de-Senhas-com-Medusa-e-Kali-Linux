# Simulando-um-Ataque-de-Brute-Force-de-Senhas-com-Medusa-e-Kali-Linux
Desafio Santander Cybersegurança 2025

Nessa fase do Treinamento, aprofundamos um pouco mais sobre as ferramentas de ataque de força bruta, com lista de usuários e senhas em um arquiv.

# AMBIENTE
Meu ambiente está rodando em um Vmware Esxi, com uma rede totalmente separada da rede local, com o endereço dos hosts sendo 192.168.222.35, sendo 1 máquina com o Kali linux, (figura1) no diretório images, e uma máquina com o metasplitable2 (figrura2), com o ip 192.168.222.3. Imagem do teste de conectividade disponível na pasta imagens com o nome (giruta3)

# CRIAÇÃO DOS ARQUIVOS 
Feito a criação do arquivo de usuários e senhas para o teste, foi utilizado os comandos abaixo, ilustrado na imagem (figura4).

echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt

# Excutando o ataque com o comando abaixo:

medusa -h 192.168.222.3 -U users.txt -P pass.txt -M ftp -t 6
Onde o parametro -h especifica o host destino, o -U informa o arquivo com a lista de usuários, o - P informa o arquivo com a lista senhas, -M informa o módulo a ser utilizado, no caso o FTP, e o -t para abrir ne exemplo 6 conexões simultâneas.

A saída do comendo confirma uma combinação de usuário e senha como Usuário msfadmin e senha msfadmin, ilustrado na imagem (figura5).

# Efetuando o acesso para ver se a combinação realmente está funcionando.

Acesso concedido conformoe imagem (figura6).



