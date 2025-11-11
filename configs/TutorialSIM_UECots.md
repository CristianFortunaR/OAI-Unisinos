# SIM/UICC Programming (Sysmocom + Osmocom / pysim)

**Objective:** this guide explains how to program a reprogrammable SIM/UICC card (e.g., Sysmocom-compatible card) using the Osmocom community tools (`pysim`).  
---

## Introduction

The cards used in this scenario are reprogrammable cards compatible with Osmocom tools. We use the **pysim** package to flash new identities — IMSI, ICCID, keys (K), OPc/OP, SPN, and other parameters — into the UICC/SIM card that will be used by the COTS UE (5G phone).

This document covers the entire process from environment setup to the programming command and basic verification. The examples below assume an Ubuntu environment and a smartcard reader connected to the PC.

---

## Important Warnings

- **Sensitive data:** IMSI, K, OP/OPc, and ICCID are sensitive values. **DO NOT** publish them in public repositories or logs.  
- **Risk:** programming cards may render them unusable if parameters are incorrect. Perform tests on cards intended for development.

---

## Prerequisites

- Computer running **Ubuntu 22.04/24.04 LTS** (or equivalent Debian-based).  
- USB smartcard reader compatible with PC/SC (e.g., ACS ACR122U or similar).  
- Reprogrammable SIM/UICC card (e.g., Sysmocom-compatible card).  
- Root/sudo access to install packages and operate the reader.  
- Internet connection to download packages and clone repositories.

---

## Environment Installation (pysim + dependencies)

Update system packages and install the basic dependencies:

```bash
sudo apt update
sudo apt install -y git pcscd libpcsclite-dev python3 python3-setuptools python3-pip python3-pyscard

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
## Programming Command — Example

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
