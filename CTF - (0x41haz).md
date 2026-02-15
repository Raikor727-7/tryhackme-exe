# 🧩 Binary Analysis CTF – 0x41haz Write-Up
## 📌 Descrição

Este desafio consistia em analisar um binário ELF x86-64 do tipo crackme, cujo objetivo era descobrir a senha correta analisando sua lógica interna.

O binário inicialmente apresentava problemas estruturais, exigindo análise manual em hexadecimal, seguida de engenharia reversa com ferramentas apropriadas.

## 🛠️ Ferramentas Utilizadas

- Linux (Kali)

- hexeditor / ncurses-hexedit

- strings

- Ghidra

## 🔍 Etapa 1 – Análise Inicial do Binário

Primeiro, foi feita uma inspeção básica para identificar possíveis strings e entender o comportamento geral do binário:

`strings 0x41haz-1640335532346.0x41haz`


O output mostrava mensagens como:

```
"Hey, Can You Crackme?"

"Tell Me the Password :"

"Well Done !!"

"Nope"
```
Porém, as seções ELF (.text, .rodata, etc.) não apareciam corretamente, indicando que o binário estava corrompido ou com header inválido.

## 🧠 Etapa 2 – Correção Manual do Header ELF

Para corrigir o problema, o binário foi aberto em um editor hexadecimal:

`hexeditor -a 0x41haz-1640335532346.0x41haz`


Foi identificado um byte incorreto no header ELF.
O sexto byte foi alterado de 0x02 para 0x01, corrigindo o formato do arquivo.

Após a correção, o comando strings passou a exibir corretamente as seções ELF, confirmando que o binário agora era um ELF x86-64 válido.

## 🧬 Etapa 3 – Engenharia Reversa no Ghidra

Com o binário válido, ele foi importado no Ghidra.

No painel Symbol Tree → Functions, foi localizada a função principal do programa (main), descompilada automaticamente pelo Ghidra.

## 🔎 Etapa 4 – Análise da Lógica de Verificação

A função principal apresentava a seguinte lógica:

- O programa solicita uma senha ao usuário.

- A entrada é lida usando gets() (função insegura).

- O tamanho da entrada é verificado:

- A senha deve ter exatamente 13 caracteres.

- Cada caractere digitado é comparado individualmente com uma string armazenada internamente.

- Trecho relevante do código descompilado:

- builtin_strncpy(local_1e, "2@@25$gfsT&@L", 0xe);


- Em seguida, o programa compara cada caractere da entrada com essa string.
Se todos forem iguais, a mensagem "Well Done !!" é exibida.

## 🔐 Etapa 5 – Extração da Senha

A string copiada para a variável de comparação possui exatamente 13 caracteres:

`2@@25$gfsT&@L`


Essa é a senha esperada pelo binário.

- ✅ Etapa 6 – Validação

Após conceder permissão de execução:

```
chmod +x 0x41haz-1640335532346.0x41haz
./0x41haz-1640335532346.0x41haz
```

Inserindo a senha correta:

`2@@25$gfsT&@L`


O programa retorna:

Well Done !!


Confirmando a solução do desafio.

## ⚠️ Observações de Segurança

- O uso de gets() torna o binário vulnerável a buffer overflow.

- A senha é armazenada em texto claro, facilitando engenharia reversa.

- Não há hashing, ofuscação ou proteção contra análise estática.

mas tudo isso é esperado para um ctf de nivel basico.

## 🧠 Conclusão

Este desafio reforça conceitos importantes de:

- Estrutura de binários ELF

- Análise estática

- Leitura de código descompilado

- Engenharia reversa básica

- Identificação de falhas de segurança comuns

Um excelente exercício prático para iniciantes em reverse engineering e binary exploitation.
