# Programação de SIM/UICC (Sysmocom + Osmocom / pysim)

**Objetivo:** neste guia mostramos como programar um cartão SIM/UICC reprogramável (ex.: cartão Sysmocom compatível) utilizando as ferramentas da comunidade Osmocom (`pysim`). 
---

## Introdução

Os cartões utilizados neste cenário são cartões reprogramáveis compatíveis com as ferramentas Osmocom. Utilizamos o pacote **pysim** para gravar (flash) novas identidades — IMSI, ICCID, chaves (K), OPc/OP, SPN e demais parâmetros — no cartão UICC/SIM que será usado pelo COTS UE (celular 5G).

Este documento descreve desde a preparação do ambiente até o comando de programação e verificação básica. Os exemplos abaixo assumem um ambiente Ubuntu e um leitor de smartcard conectado ao PC.

---

## Avisos importantes

- **Dados sensíveis:** IMSI, K, OP/OPc e ICCID são dados sensíveis. **NÃO** publique esses valores em repositórios públicos ou logs.  
- **Risco:** programar cartões pode torná-los inutilizáveis se os parâmetros estiverem incorretos. Faça testes em cartões destinados a desenvolvimento.

---

## Pré-requisitos

- Computador com **Ubuntu 22.04/24.04 LTS** (ou equivalente Debian-based).  
- Leitor de smartcard USB compatível com PC/SC (ex.: ACS ACR122U ou leitor similar).  
- Cartão SIM/UICC reprogramável (ex.: cartão Sysmocom compatível).  
- Acesso root/sudo no sistema para instalar pacotes e trabalhar com o leitor.  
- Conexão à internet para baixar pacotes e clonar repositórios.

---

## Instalação do ambiente (pysim + dependências)

 Atualize pacotes e instale dependências básicas:

```bash
sudo apt update
sudo apt install -y git pcscd libpcsclite-dev python3 python3-setuptools python3-pip python3-pyscard
```
```bash
git clone https://github.com/osmocom/pysim
cd pysim
sudo apt-get install --no-install-recommends \
    pcscd libpcsclite-dev \
    python3 \
    python3-setuptools \
    python3-pyscard \
    python3-pip
pip3 install -r requirements.txt
```
## Comando de programação — Exemplo

Abaixo está um exemplo do comando utilizado para programar o cartão com o utilitário pySim-prog.py. Ajuste os parâmetros (IMSI, ICCID, K, OPc, MCC, MNC, SPN, etc.) conforme seu caso.
```bash
./pySim-prog.py \
  -p 0 \
  -n "TelecomUNISINOS" \
  -a 1234567 \
  -s 8988211000000280969 \
  --mcc=001 \
  --mnc=01 \
  --imsi=001010000000001 \
  -k fec86ba6eb707ed08905757b1bb44b8f \
  --opc=C42449363BBAD02B66D16BC975D77CC1
```
### Significado dos parâmetros (resumo)

- `-p 0` — porta / índice do leitor (ex.: leitor 0).  
- `-n "TelecomUNISINOS"` — **SPN** (*Service Provider Name*) — nome do operador.  
- `-a 1234567` — parâmetro administrativo usado pelo utilitário (ADM).  
- `-s 8988211000000280969` — **ICCID** do cartão (identificador de cartão).  
- `--mcc=001` — **MCC** (*Mobile Country Code*).  
- `--mnc=01` — **MNC** (*Mobile Network Code*).  
- `--imsi=001010000000001` — **IMSI** (identidade do assinante).  
- `-k <K>` — **K** (chave do assinante / *Ki*) usada para autenticação.  
- `--opc=<OPc>` — **OPc** (ou `--op`, dependendo da ferramenta) — parâmetro relacionado ao esquema de autenticação.  

> **Nota:** dependendo da versão do `pysim` que você usa, a flag para OPc pode ser `--opc` ou `--op`. Consulte `./pySim-prog.py --help` para confirmar.
