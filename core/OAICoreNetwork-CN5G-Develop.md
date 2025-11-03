# 📶 Tutorial: Configuração de um Core 5G com OpenAirInterface (OAI)

Este tutorial descreve como configurar e executar um Core 5g *OAI*.  


---

## 🖥️ Requisitos mínimos de hardware

Para executar o **OAI CN5G** e o **OAI gNB**, recomenda-se o seguinte:

- **Sistema Operacional:** Ubuntu 24.04 LTS  
- **CPU:** 8 núcleos x86_64 @ 3.5 GHz  
- **Memória RAM:** 32 GB  
- **Equipamento:** Laptop, desktop ou servidor  

---

## 🧩 1. OAI CN5G

### 1.1 Pré-requisitos do OAI CN5G

Instale os pacotes necessários:

```bash
sudo apt install -y git net-tools putty

# Repositório oficial do Docker
sudo apt update
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Adiciona o repositório Docker
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo \"${UBUNTU_CODENAME:-$VERSION_CODENAME}\") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Instalação dos pacotes Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Adiciona o usuário atual ao grupo docker
sudo usermod -a -G docker $(whoami)

reboot
``` 
### 1.2 Arquivos de configuração do OAI CN5G

```bash
wget -O ~/oai-cn5g.zip https://gitlab.eurecom.fr/oai/openairinterface5g/-/archive/develop/openairinterface5g-develop.zip?path=doc/tutorial_resources/oai-cn5g
unzip ~/oai-cn5g.zip
mv ~/openairinterface5g-develop-doc-tutorial_resources-oai-cn5g/doc/tutorial_resources/oai-cn5g ~/oai-cn5g
rm -r ~/openairinterface5g-develop-doc-tutorial_resources-oai-cn5g ~/oai-cn5g.zip
```
### 1.3 Baixar as imagens Docker do OAI CN5G

```bash
cd ~/oai-cn5g
docker compose up -d
```
---

## ▶️ 2. Executar o OAI CN5G

### 2.1 Iniciar o OAI CN5G
```bash
cd ~/oai-cn5g
docker compose pull
```

### 2.2 Parar o OAI CN5G
```bash
cd ~/oai-cn5g
docker compose down
```
