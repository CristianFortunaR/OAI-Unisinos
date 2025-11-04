# Tutorial de Configuração da APN para o UE (COTS 5G)

Este tutorial descreve o processo de configuração da **APN (Access Point Name)** no **celular 5G (COTS UE)**, que será utilizado para se conectar à rede 5G privada implementada com o **OAI CN5G** e o **OAI gNB**.

---

## 1. Requisitos

- Celular 5G compatível com redes SA (Standalone).  
- Chip **Sysmocom** previamente programado (conforme o [TutorialSIM_UECots.md](https://github.com/CristianFortunaR/OAI-Unisinos/blob/v1/configs/TutorialSIM_UECots.md)).  
- Aplicativo que permita **fixar a rede 5G**, como *Force LTE Only* ou *5G Only Mode*.  

---

## 2. Configuração do Celular

1. Insira o chip Sysmocom no celular 5G.  
2. Ative o **modo roaming**, pois os códigos MCC/MNC utilizados (001/01) não pertencem a uma operadora comercial.  
3. Abra o menu de **Configurações de Rede Móvel → Nomes de Pontos de Acesso (APN)**.  
4. Crie uma nova APN com os seguintes parâmetros:

| Campo | Valor |
|-------|-------|
| **Nome** | oai |
| **APN** | oai |
| **MCC** | 001 |
| **MNC** | 01 |
| **Tipo de APN** | default,mms,supi,hipri,fota,cbs,xcap |
| **Protocolo APN** | IPv4 |
| **Protocolo de roaming** | IPv4 |
| **Demais campos** | Deixe em branco |

> ⚠️ **Dica:**  
> Caso o celular não permita editar todos os tipos de APN, mantenha pelo menos `default` e `hipri`.

---

## 3. Referência às DNNs configuradas no OAI Core

As **DNNs (Data Network Names)** definidas no núcleo 5G são utilizadas para configurar as APNs do celular.  
Essas informações podem ser verificadas diretamente no arquivo de configuração do OAI Core:
```bash
oai-cn5g/conf/config.yaml
``` 
