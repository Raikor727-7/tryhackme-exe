# CTF collection Vol.1

# 1 TASK
 um simples hash base 64 para decodificar: `THM{ju57_d3c0d3_7h3_b453}`

# 2 TASK
 analisar o metadados de uma imagem  
 fazemos o download  
 usamoss o exiftool na imagem  
 no dono temos a resposta: `THM{3x1f_0r_3x17}`

# 3 TASK
esse é outra imagem  
fazemos o download, 
analiso o metadados, e nada  
penso que talvez seja util tentar ver se tem algo zipado,
entao tento o comando binwalk
nenhum resultado
entao uso o **stegseek** e extraio algo de dentro (se tiver iria sair)  

entao conseguimos o txt e com isso a resposta: `THM{500n3r_0r_l473r_17_15_0ur_7urn}`

# 4 TASK
esse é bem facil,
ta escrito no site apenas tem um sobreescrito q "impede" a visualizacao  
selecionando a parte q tava com algo por cima aparece a senha: `THM{wh173_fl46}`

# 5 TASK
esse é um qr code, escaneando ele aparece a senha: `THM{qr_m4k3_l1f3_345y}`

# 6 TASK
esse sexto ja é bem mais dificil  
ele entrega um arquivo .hello apenas  
faço o download e executo  
ele retorna: 
```
./hello_1577977122465.hello       
Hello there, wish you have a nice day
```

continuo na procura  
abro a ghidra, e analiso, nao encontro nada de util

entao faço o comando `file`
e me retorna:
```
file hello_1577977122465.hello 
hello_1577977122465.hello: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=02900338a56c3c8296f8ef7a8cf5df8699b18696, for GNU/Linux 3.2.0, not stripped
```

nada muito util tbm  

entao tento usar o comando `Strings | grep "THM{"`  
e isso me retorna a flag: `THM{345y_f1nd_345y_60}`
> dificil

# 7 TASK
esse é um hash tbm  
coloco nos identificadores, eles nao descobrem, entao coloco no google e diz q se parece com o base58
entao peço um comando para tentar visualizar e ele me passa  
`echo "3agrSy1CewF9v8ukcSkPSYm3oKUoByUpKG4L" | base58 -d`
`THM{17_h45_l3553r_l3773r5}`                                                                             

> solucionado

# 8 TASK
é um rot so nao sabia qual  
mas como sabia q os 3 primeiros sempre sao o THM
entao eu mesmo testei a rotacao q seria  
deu **rot19**  
como nao achei na internet um decoder, 
eu msm decodifiquei:  
MAF{atbe_max_vtxltk}
para a esquerda 19 vezes fica:
`THM{hail_the_caesar}`

# 9 TASK
esse ele avisa para vermos o comentario no html  
entao vamos vendo todo codigo html, e uma hora acha:

`THM{4lw4y5_ch3ck_7h3_c0m3mn7}`

# 10 TASK
esse ele manda um png quebrado para nos corrigirmos  
uso o pngcheck para saber oq ta quebrado:
```
└─# pngcheck -v spoil_1577979329740.png                    
File: spoil_1577979329740.png (70759 bytes)
  this is neither a PNG or JNG image nor a MNG stream
ERRORS DETECTED in spoil_1577979329740.png
```

logo ve q ta quebrado
uso o visualizador de hexadecimal para ver se o arquivo ta assinado correto
```
└─# head -c 16 spoil_1577979329740.png | xxd
00000000: 2333 445f 0d0a 1a0a 0000 000d 4948 4452  #3D_........IHDR
```

jogo no google e vejo q ta errado
entao eu vou e entro no hexeditor e modifico os primeiros 8 bytes do arquivo

dps disso a imagem desquebra e temos a senha:
`THM{y35_w3_c4n}`

# 11 TASK
esse tinha que pesquisa no reddit pq tinha algo escondido la  
mas como faz muito tempo q o room saiu ele acabou ficando perdido  
entao fui num write up e vi como achava, achei e a flag é: `THM{50c14l_4cc0un7_15_p4r7_0f_051n7}`
> (vai salvar pq quanto mais tempo passar mais perdido fica)

# 12 TASK
esse é um binariofuck  
entao procuro um decodificador online e uso.  
senha: `THM{0h_my_h34d}`

# 13 TASK
esse é s1 xor s2  
entao fiz o comando python (que pesquisei)  
```
python3 -c "s1=0x44585d6b2368737c65252166234f20626d; s2=0x1010101010101010101010101010101010; print(hex(s1 ^ s2)[2:])"
```
resultado = **54484d7b3378636c75353176335f30727d**

entao peguei o resultado e coloquei no comando para ver o hex (pesquisei tbm)
```
python3 -c "print(bytes.fromhex('54484d7b3378636d75353176335f30727d').decode())"
```

senha: `THM{3xclu51v3_0r}`

# 14 TASK
esse ele da uma dica daora  
> "Binary walk"  
entao baixo o arquivo e uso o binwalk
```
──(root㉿kali)-[/home/kali/Desktop]
└─# binwalk hell_1578018688127.jpg 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.02
30            0x1E            TIFF image data, big-endian, offset of first image directory: 8
265845        0x40E75         Zip archive data, at least v2.0 to extract, uncompressed size: 69, name: hello_there.txt
266099        0x40F73         End of Zip archive, footer length: 22
```

com isso sei q tem coisa dentro

entao uso o comando: `binwalk -e`
para extrair

extraindo ele me da im txt
que tem a senha: `THM{y0u_w4lk_m3_0u7}`

# 15 TASK
esse ele me da uma imagem toda preta  
e me recomenda usar o stegsolve  
nao tenho e faço o download  
depois do dowload eu abro ele com `java -jar stegsolve.jar`  
ele aberto coloco a imagem toda escura  
e ele aparece no filtro blue plane 0 (ou gray bits) a senha: `THM{7h3r3_15_h0p3_1n_7h3_d4rkn355}`

# 16 TASK
esse ele passa um qr code e voce escaneia vai para o soundcloud e ele tem um som q cita qual a flag  
super facil ate pq tem comentarios la transcrevendo o audio  
senha: `THM{SOUNDINGQR}`

# 17 TASK
esse ele passa um link e fala sobre o passado  
o passado da internet ta no internet archive, 
entramos la
pesquisamos  
vamos no dia que ele fornece  
temos o site la com a senha: `THM{ch3ck_th3_h4ckb4ck}`

# 18 TASK
esse ele so da uma cifra louca  
tento rot de 0 a 365 e vejo que nao funciona e nao tem futuro  
entao pesquiso e vejo q pode ser uma tal de cifra de vigenere  
entao eu procuro um decode para isso  
e coloco la, com a chave de saida o **thm**
e a senha aparece: `tryhackme{you_found_the_key}`


# 19 TASK
esse ele da uma dica boa  
voce pega a base q ele da  
transforma em hex  
e de hexadecimal voce transforma em ascii  
e temos a senha : `THM{17_ju57_4n_0rd1n4ry_b4535}`

# 20 TASK
e ultimo ele passa uns pacotes capturado pelo wireshark  
nos abrimos no kalilinux e procuro algum http onde teria algo vazado  
acho uma conexao http  
coloco para seguir o pacote
e acho a ultima senha: `THM{d0_n07_574lk_m3}`

### e finalizo mais um ctf
