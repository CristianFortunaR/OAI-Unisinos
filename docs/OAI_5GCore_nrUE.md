## Introdução

Este tutorial tem como objetivo demonstrar o processo completo para a **configuração e execução de uma rede 5G funcional**, composta por um **Core Network (CN5G)**, um **gNB** e um **UE** utilizando o ecossistema **OpenAirInterface (OAI)**.

O cenário apresentado utiliza um **USRP B210** como estação base (gNB) e um segundo computador, também equipado com um USRP, atuando como **UE (User Equipment)**.  
Assim, em vez de um dispositivo comercial, o UE será emulado por outro host rodando o software do **OAI nrUE**.

Ao longo deste tutorial, serão abordadas todas as etapas necessárias para a execução do ambiente, incluindo a instalação dos pré-requisitos, a construção dos módulos do OAI e a inicialização dos componentes principais da rede 5G.

---

##  Requisitos do Sistema

Para a execução deste ambiente 5G completo, serão necessários **dois computadores** e, no mínimo, **dois rádios USRP** (por exemplo, USRP B210). Cada máquina desempenhará um papel específico dentro do cenário, conforme descrito abaixo.

### Computador 1 — OAI Core Network (CN5G) + gNB

Este computador será responsável por executar o **Core Network (CN5G)** e o **gNB (Next Generation NodeB)**.

**Especificações recomendadas:**
- **Sistema operacional:** Ubuntu 24.04 LTS  
- **Processador:** 8 núcleos x86_64 @ 3.5 GHz  
- **Memória RAM:** 32 GB  
- **Dispositivo de rádio:** USRP B210 (ou equivalente)  

Para instalação e configuração do núcleo 5G, siga o tutorial detalhado disponível em:  
👉 [OAI Core Network — CN5G (Develop)](https://github.com/CristianFortunaR/OAI-Unisinos/blob/v1/core/OAICoreNetwork-CN5G-Develop.md)

---

### Computador 2 — OAI nrUE

O segundo computador será utilizado para emular o **UE (User Equipment)**, também conectado a um USRP.  
Este equipamento será o responsável por simular o comportamento de um dispositivo 5G real.

**Especificações recomendadas:**
- **Sistema operacional:** Ubuntu 24.04 LTS  
- **Processador:** 8 núcleos x86_64 @ 3.5 GHz  
- **Memória RAM:** 8 GB  
- **Dispositivo de rádio:** USRP B210 (ou equivalente)

---

###  Configuração do UHD

Tanto o computador do **Core/gNB** quanto o do **UE** precisam ter o **UHD (USRP Hardware Driver)** devidamente instalado e configurado.  
O processo de instalação e compilação pode ser seguido conforme o guia oficial abaixo:

👉 [Tutorial de Instalação do UHD](https://github.com/CristianFortunaR/OAI-Unisinos/blob/v1/configs/TutorialUHD.md)

---

## Construção do OAI gNB e OAI nrUE

Após a instalação do UHD, prossiga com a obtenção e compilação dos componentes principais do OpenAirInterface.

### Compilação do OAI gNB e OAI nrUE

Baixe o código-fonte mais recente do **OpenAirInterface 5G**, instale as dependências e realize a compilação dos módulos necessários para o gNB e o nrUE nos dois computadores. 

```bash
# Obter o código-fonte do OpenAirInterface
git clone https://gitlab.eurecom.fr/oai/openairinterface5g.git ~/openairinterface5g
cd ~/openairinterface5g
git checkout develop

# Instalar dependências do OAI
cd ~/openairinterface5g/cmake_targets
./build_oai -I

# Instalar dependências do nrscope
sudo apt install -y libforms-dev libforms-bin

# Compilar o OAI gNB e o nrUE com suporte a USRP
cd ~/openairinterface5g/cmake_targets
./build_oai -w USRP --ninja --nrUE --gNB --build-lib "nrscope" -C
``` 
### Executar o OAI CN5G

Inicie o Core Network 5G utilizando o Docker Compose previamente configurado.
```bash
cd ~/oai-cn5g
docker compose up -d
```
### Executar o OAI gNB

Neste cenário será usado o USRP B210
```bash 
cd ~/openairinterface5g/cmake_targets/ran_build/build
sudo ./nr-softmodem -O ../../../targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf --gNBs.[0].min_rxtxtime 6 -E --continuous-tx
```

### Rodar o OAI nrUE

Este processo deve ser realizado em um segundo computador (host diferente do gNB), rodando Ubuntu 24.04 LTS e conectado ao seu próprio USRP B210.
```bash
cd ~/openairinterface5g/cmake_targets/ran_build/build
sudo ./nr-uesoftmodem -r 106 --numerology 1 --band 78 -C 3619200000 --ue-fo-compensation -E --uicc0.imsi 001010000000001
```
### Verificação da Conexão

Após a inicialização bem-sucedida do CN5G, gNB e UE, é possível verificar a conectividade fim a fim através de testes de ping.

## Teste — Comunicação entre UE e CN5G

No host do UE, execute:
```bash
ping 192.168.70.135 -I oaitun_ue1
```

## Teste 2 — Acesso à Internet (Google)

Após confirmar a comunicação com o core, teste o acesso externo:
```bash
ping google.com -I oaitun_ue1
```
Para mais informações, acesse o repositório oficial:  
👉 [https://gitlab.eurecom.fr/oai/openairinterface5g/-/blob/develop/doc/NR_SA_Tutorial_OAI_nrUE.md#5-oai-ue](https://gitlab.eurecom.fr/oai/openairinterface5g/-/blob/develop/doc/NR_SA_Tutorial_OAI_nrUE.md#5-oai-ue)
