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
  🔗 [Tutorial UHD - OAI Unisinos](https://github.com/CristianFortunaR/OAI-Unisinos/blob/v1/configs/TutorialUHD.md)

- **OAI Core Network (CN5G)**  
  Guia de instalação e configuração disponível em:  
  🔗 [OAI Core Network - CN5G Develop](https://github.com/CristianFortunaR/OAI-Unisinos/blob/v1/core/OAICoreNetwork-CN5G-Develop.md)

Esses tutoriais fornecem as bases necessárias para que o ambiente esteja pronto antes de prosseguir com a execução do gNB e a conexão com o COTS UE.
