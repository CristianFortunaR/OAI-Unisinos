# Guia Detalhado de Implantação — OpenAirInterface 5G Core Network com UERANSIM

Este repositório apresenta instruções para configuração, compilação e execução do **OpenAirInterface (OAI) 5G Core Network** integrado ao **UERANSIM**, visando testes de conectividade, desempenho e validação da rede. Este tutorial é recomendado para estudar o Core5G da OAI, e entender seus fundamentos.

---

## 1. Requisitos

Antes de iniciar, é necessário concluir o **Basic Deployment** do Core do OAI versão 1.5:

* [OAI Core Network V1.5](https://github.com/CristianFortunaR/OAI-Unisinos/tree/main/core)

---

## 2. Configuração do Core do OAI

### 2.1 Parâmetros do AMF

Acesse o arquivo de configuração principal:

```bash
cd oai-cn5g-fed/docker-compose/docker-compose-basic-vpp-nrf.yaml
```

Adicione os seguintes parâmetros ao bloco `oai-amf`:

```yaml
- INT_ALGO_LIST=["NIA1","NIA2"]
- CIPH_ALGO_LIST=["NEA1","NEA2"]
```

Esses parâmetros definem os algoritmos de integridade e criptografia permitidos para comunicação segura entre as funções de rede.

---

## 3. Instalação e Compilação do UERANSIM

Execute os comandos:

```bash
git clone -b docker_support https://github.com/orion-belt/UERANSIM.git
cd UERANSIM
docker build --target ueransim --tag ueransim:latest -f docker/Dockerfile.ubuntu.18.04 .
```

---

## 4. Inicialização do Core do OAI

Para iniciar:

```bash
python3 core-network.py --type start-basic-vpp
```

### 4.1 Correção de Erro Comum

Caso ocorra o erro:

```
SMF not receiving heartbeats from UPF...
```

Execute:

```bash
docker restart oai-smf
python3 core-network.py --type start-basic-vpp
```

### 4.2 Parar o Core

```bash
python3 core-network.py --type stop-basic-vpp
```

---

## 5. Inicialização do UERANSIM

Para iniciar:

```bash
docker-compose -f docker-compose-ueransim-vpp.yaml up -d
```

### 5.1 Parar o UERANSIM

```bash
docker-compose -f docker-compose-ueransim-vpp.yaml down
```

---

## 6. Verificação de Funcionamento

### 6.1 Listar Contêineres Ativos

```bash
docker ps
```

### 6.2 Consultar Logs do UERANSIM

```bash
docker logs ueransim
```

No final do log, deve aparecer algo semelhante a:

```
[2025-08-26 19:00:20.176] [nas] [info] PDU Session establishment is successful PSI[1]
[2025-08-26 19:00:20.186] [app] [info] Connection setup for PDU session[1] is successful, TUN interface[uesimtun0, 12.1.1.3] is up.
```

> **Observação:** Anote o IP (`12.1.1.x`), pois ele será necessário para os testes de conectividade.

---

## 7. Testes de Conectividade

### 7.1 Teste UE → Internet

Comando:

```bash
docker exec ueransim ping -c 3 -I uesimtun0 google.com
```

Exemplo de saída bem-sucedida:

```
PING google.com (172.217.18.238) from 12.2.1.2 : 56(84) bytes of data.
64 bytes from par10s10-in-f238.1e100.net (172.217.18.238): icmp_seq=1 ttl=115 time=5.12 ms
64 bytes from par10s10-in-f238.1e100.net (172.217.18.238): icmp_seq=2 ttl=115 time=7.52 ms
64 bytes from par10s10-in-f238.1e100.net (172.217.18.238): icmp_seq=3 ttl=115 time=7.19 ms

--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 4ms
rtt min/avg/max/mdev = 5.119/6.606/7.515/1.064 ms
```

### 7.2 Teste Internet → UE

> **Observação:** Certifique-se de utilizar o IP indicado nos logs do contêiner, pois ele pode variar a cada inicialização.

Comando:

```bash
docker exec -it oai-ext-dn ping -c 3 12.2.1.2
```

Exemplo de saída bem-sucedida:

```
PING 12.2.1.2 (12.2.1.2) 56(84) bytes of data.
64 bytes from 12.2.1.2: icmp_seq=1 ttl=64 time=0.235 ms
64 bytes from 12.2.1.2: icmp_seq=2 ttl=64 time=0.145 ms
64 bytes from 12.2.1.2: icmp_seq=3 ttl=64 time=0.448 ms

--- 12.1.1.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2036ms
rtt min/avg/max/mdev = 0.145/0.276/0.448/0.127 ms
```

### 7.3 Testes Avançados

Para testes adicionais (velocidade, estresse, etc.), consulte:

[Documentação Oficial — DEPLOY\_SA5G\_WITH\_UERANSIM](https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-fed/-/blob/v1.5.0/docs/DEPLOY_SA5G_WITH_UERANSIM.md?ref_type=tags)
