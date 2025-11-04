## OAI-5GCORE with COTS_UE

Este tutorial descreve como configurar e executar uma rede 5G **monolítica** utilizando os componentes do **OpenAirInterface (OAI)** — especificamente o **OAI CN5G** (core) e o **OAI gNB** — em um **único computador**.  

O objetivo é demonstrar o funcionamento completo de uma rede 5G Standalone (SA), conectando-a a um **COTS UE (Commercial Off-The-Shelf User Equipment)**, ou seja, um **celular comercial compatível com 5G**.  

O tutorial cobre desde a preparação do ambiente e compilação dos componentes até os testes de conectividade com o dispositivo 5G real.

---
## Requisitos do Sistema

Antes de iniciar a configuração, verifique se seu ambiente atende aos requisitos mínimos de hardware e software para executar a rede 5G monolítica com o OpenAirInterface.

### Hardware mínimo recomendado

- **Computador único** para execução conjunta do OAI CN5G (core) e OAI gNB:  
  - **Sistema operacional:** Ubuntu 24.04 LTS  
  - **CPU:** 8 núcleos x86_64 @ 3.5 GHz  
  - **Memória RAM:** 32 GB  
  - **Interface USRP:** B210, N300 ou X300  
  - Identifique corretamente a interface de rede do USRP e configure-a no arquivo do gNB.  
  - **Equipamento de usuário (UE):** neste cenário foi utilizado um **celular 5G comercial (COTS UE)**, equipado com um **chip reprogramável da Sysmocom**,

### Dependências e tutoriais de referência

Este tutorial assume que você já seguiu as instruções básicas de instalação e configuração dos seguintes componentes:

- **UHD (USRP Hardware Driver)**  
  Guia completo disponível em:  
   [Tutorial UHD - OAI Unisinos](https://github.com/CristianFortunaR/OAI-Unisinos/blob/v1/configs/TutorialUHD.md)

- **OAI Core Network (CN5G)**  
  Guia de instalação e configuração disponível em:  
   [OAI Core Network - CN5G Develop](https://github.com/CristianFortunaR/OAI-Unisinos/blob/v1/core/OAICoreNetwork-CN5G-Develop.md)

Esses tutoriais fornecem as bases necessárias para que o ambiente esteja pronto antes de prosseguir com a execução do gNB e a conexão com o COTS UE.

---
## Compilação do OAI gNB

Nesta etapa, será feita a **obtenção do código-fonte do OpenAirInterface 5G (OAI)** e a **compilação do gNB (gNodeB)** com suporte ao hardware USRP.

### Passos para compilação:

Clone o repositório oficial do OAI:

```bash
# Obter o código-fonte do OAI
git clone https://gitlab.eurecom.fr/oai/openairinterface5g.git ~/openairinterface5g
cd ~/openairinterface5g
git checkout develop
# Instalar dependências do OAI
cd ~/openairinterface5g/cmake_targets
./build_oai -I
# Compilar o OAI gNB
cd ~/openairinterface5g/cmake_targets
./build_oai -w USRP --ninja --gNB -C
```
---

## Configuração do UE (User Equipment)

Antes de executar o **Core 5G (OAI CN5G)** e o **gNB**, é necessário realizar a configuração do **UE (User Equipment)**, ou seja, o **celular 5G** que será conectado à rede.

Essa etapa é fundamental para garantir que o dispositivo consiga autenticar-se corretamente e estabelecer comunicação com a rede 5G.  
Para isso, será preciso configurar:

- O **chip SIM reprogramável** (Sysmocom), com os parâmetros de autenticação corretos;  
- A **APN (Access Point Name)** no próprio celular, de forma que a conexão de dados seja direcionada ao núcleo 5G configurado no ambiente.

Nas próximas etapas, serão descritos todo o tutorial para a configuração do UE.

## Programação do SIM Card (Sysmocom) 

Antes de iniciar a execução da rede 5G, é necessário **programar o chip SIM** que será utilizado pelo dispositivo 5G (COTS UE).

Neste cenário, foi utilizado um **celular 5G** equipado com um **chip reprogramável da Sysmocom**.  
O processo de programação desse cartão é essencial para definir parâmetros como IMSI, MCC, MNC, e chaves de autenticação (K e OPC), garantindo a autenticação correta do dispositivo na rede.

O passo a passo completo para realizar a programação do SIM pode ser consultado no tutorial abaixo:

📘 **Tutorial de Programação do SIM (Sysmocom)**  
👉 [TutorialSIM_UECots.md](https://github.com/CristianFortunaR/OAI-Unisinos/blob/v1/configs/TutorialSIM_UECots.md)

> ⚠️ **Importante:** Certifique-se de realizar a programação do SIM Card antes de iniciar o processo de conexão do celular à rede 5G, pois o chip precisa estar configurado com as credenciais corretas do OAI Core.

## Gerenciamento dos Registros de UE no Banco de Dados

Após programar o SIM Card, é necessário garantir que as informações do **UE (User Equipment)** estejam registradas corretamente no banco de dados do **OAI Core Network (CN5G)**.

Você pode optar por duas abordagens:

1. **Adicionar novos registros de UE** manualmente no banco de dados, ou  
2. **Utilizar um registro já existente**, previamente configurado.

Para realizar essa configuração, acesse o arquivo SQL responsável pelos dados da rede: 
```bash
oai-cn5g/database/oai_db.sql
```
Dentro desse arquivo, localize as seções correspondentes aos dados de autenticação e de gerenciamento de sessão do usuário:

- `-- Dumping data for table \`SessionManagementSubscriptionData\``
- `-- Dumping data for table \`AuthenticationSubscription\``

Essas tabelas armazenam as informações de assinatura, IMSI, chaves de autenticação (K e OPC), e parâmetros necessários para a autenticação do UE na rede.

> ⚠️ **Dica:** Certifique-se de que os valores programados no chip Sysmocom (IMSI, MCC, MNC, K e OPC) coincidam com os valores registrados no banco de dados.  
> Caso contrário, o dispositivo não conseguirá autenticar-se no núcleo 5G.

## Configuração da APN

Antes de conectar o celular à rede 5G, é necessário configurar a **APN (Access Point Name)** para garantir que o tráfego de dados seja roteado corretamente para o núcleo 5G (OAI CN5G).

O passo a passo completo dessa configuração está documentado no tutorial abaixo:

📘 **Tutorial de Configuração da APN no Celular 5G**  
👉 [TutorialAPN.md](https://github.com/CristianFortunaR/OAI-Unisinos/blob/v1/configs/TutorialAPN.md)

> ⚠️ **Importante:**  
> Verifique se a APN utiliza o mesmo **MCC**, **MNC** e **DNN** configurados no chip Sysmocom e no arquivo `config.yaml` do OAI Core.  
> Esses valores precisam coincidir para que o UE consiga autenticar-se corretamente na rede.

## Run OAI CN5G and OAI gNB

Feito a configuração do **UE**, poderemos executar o **Core 5G (OAI CN5G)** e o **gNB**.

---

### Run OAI CN5G

Para iniciar o Core 5G:

```bash
cd ~/oai-cn5g
docker compose up -d
```
### Run OAI gNB
```bash
cd ~/openairinterface5g/cmake_targets/ran_build/build
sudo ./nr-softmodem -O ../../../targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf -E --continuous-tx
```

## Conexão do UE à Rede 5G

Após configurar o **UE** (SIM e APN) e iniciar o **Core 5G** e o **gNB**, o próximo passo é conectar o celular à rede 5G.

### Passos para conexão:

1. No celular, busque **manualmente as redes disponíveis**.  
2. Selecione a rede correspondente à combinação de **MCC e MNC** configurada anteriormente (por exemplo, MCC=001 e MNC=01).  

> ⚠️ **Importante:**  
> Certifique-se de que o celular esteja forçando conexão exclusiva à **rede 5G**, caso contrário ele pode tentar se conectar a redes 4G ou 3G próximas.

### Acompanhamento da Conexão

Podemos monitorar o registro do UE e o tráfego de conexão através dos **logs do gNB e do Core**.

#### Logs do Core

Para acessar os logs de um container Docker do Core:

```bash
docker ps #Para acessar o nome dos containers
docker logs <nome_do_container> -f
```
Recomendado acessar os logs do AMF, para realizar debugs. 
```bash
docker logs oai-amf -f
```
#### Logs do gNB

Os logs do gNB podem ser visualizados diretamente no terminal onde o nr-softmodem foi executado.
Eles permitem acompanhar o processo de registro do UE e identificar possíveis falhas de autenticação ou configuração.

### 6. Teste da Conexão do UE

Após o UE se registrar corretamente na rede 5G, é possível testar a conectividade:

1. **Acessar a Internet** diretamente pelo celular (UE), navegando ou utilizando aplicativos que dependam de dados.  
2. **Testar a conectividade via ping** a partir do próprio host do UE, utilizando o **IP atribuído pelo Core**.

Exemplo de teste de ping no UE:

```bash
docker exec -it oai-ext-dn ping 12.0.0.2
```
