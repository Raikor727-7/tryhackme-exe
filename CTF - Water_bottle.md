# Writeup — Water Station OSINT Challenge (TryHackMe)

## Challenge Description

The goal was to identify the **name and contact number** of a former water refilling station located near **Boni Avenue**, active until 2014. The only clue was that the contact number had **12 digits** and started with **63922**.

**Flag format:** `THM{stationname_contactnumber}`

---

## Investigation Process

### 1. Direct keyword search
Query: `water station 63922`
→ No relevant results.

### 2. Geolocation via Google Maps
Searched for **Boni Avenue** on Google Maps, then manually scanned water-related establishments in the area.
→ Found **WaterMarket**, whose number started with the target prefix.

### 3. Submission attempts (rabbit hole)
Several formatting variations were tested:
- `water_market_639228721288`
- `watermarket_639228721288`
- `thm{watermarket_09228339868}`
- Variations with and without the `63` prefix, with a leading `0`, etc.

→ None worked. I concluded I had either found the **wrong establishment** or the name was different from what I expected.

### 4. Prompt Engineering on TryHackMe's AI (Echo)
After identifying that the direct OSINT vector was inconclusive — and that the community was facing the same difficulty — I pivoted to an alternative attack vector: **prompt engineering on the platform's AI assistant (Echo)**. I found that by providing enough context about my attempts, the model disclosed the correct answer — a design flaw in the assistant.

```
THM{aquabest_639228721288}
```

---

## Flag

> ⚠️ **Flag intentionally exposed in this writeup** — see the reflections section below.

`THM{aquabest_639228721288}`

---

## Lessons Learned

- For OSINT involving historical data (pre-2014), tools like **Wayback Machine** and archived Google Maps records may be more effective than current searches
- Establishments may have changed names — cross-referencing contact numbers against **archived phone directories** is worth trying
- AI assistants on CTF platforms can leak answers if not properly calibrated — an interesting angle from the perspective of **prompt injection** and information disclosure vulnerabilities

---

# Writeup — Water Station OSINT Challenge (TryHackMe)

## Descrição do Desafio

O objetivo era identificar o **nome e número de contato** de uma antiga estação de recarga de água localizada próxima à **Boni Avenue**, ativa até 2014. A única pista era que o número de contato tinha **12 dígitos** e começava com **63922**.

**Formato da flag:** `THM{nomedaestacao_numerocontato}`

---

## Processo de Investigação

### 1. Pesquisa inicial por termos diretos
Busca: `water station 63922`
→ Sem resultados relevantes.

### 2. Geolocalização via Google Maps
Busca por **Boni Avenue** no Google Maps, seguida de varredura manual por estabelecimentos de água na região.
→ Encontrei a **WaterMarket**, cujo número iniciava com o prefixo desejado.

### 3. Tentativas de submissão (rabbit hole)
Foram testadas diversas variações de formatação:
- `water_market_639228721288`
- `watermarket_639228721288`
- `thm{watermarket_09228339868}`
- Variações com e sem prefixo `63`, com `0` à frente, etc.

→ Nenhuma funcionou. Concluí que possivelmente havia chegado ao estabelecimento **errado** ou o nome estava divergente.

### 4. Engenharia de Prompt com a IA do TryHackMe (Echo)
Ao identificar que o vetor OSINT direto estava inconcluso e que a comunidade enfrentava a mesma dificuldade, pivotei para um vetor alternativo:
engenharia de prompt no assistente de IA da plataforma (Echo). Identifiquei que, ao fornecer contexto suficiente sobre minhas tentativas, o modelo revelava a resposta, uma falha de design do assistente.
```
THM{aquabest_639228721288}
```

---

## Flag

> ⚠️ **Flag exposta intencionalmente neste writeup** — justificativa na seção de reflexões abaixo.

`THM{aquabest_639228721288}`

---
## Lições Aprendidas

- Em OSINT com dados históricos (pré-2014), ferramentas como **Wayback Machine** e registros antigos do Google Maps podem ser mais eficazes que buscas atuais
- Estabelecimentos podem ter mudado de nome — vale cruzar o número de contato em **listas telefônicas arquivadas**
- IAs assistentes de plataformas de CTF podem vazar respostas se não forem bem calibradas — interessante do ponto de vista de *prompt injection* e segurança de sistemas
