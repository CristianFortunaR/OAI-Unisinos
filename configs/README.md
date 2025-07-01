# 📡 OpenAirInterface + USRP (Docker) EN

This project provides a ready-to-use Docker image for compiling and running OpenAirInterface (OAI) with USRP support (via UHD). Ideal for lab environments using USRP B210 or similar.

---

## ✅ Prerequisites

- Docker installed (version 20+)
- USRP B210 (or other compatible) connected via USB 3.0
- Host system with access to `/dev/bus/usb`
- Linux host (Ubuntu 20.04 or 22.04 recommended)

---

## 🧱 Building the Image

Clone this repository or copy the `Dockerfile` and run:

```bash
docker build -t oai-usrp .
```

---

## 🚀 Running the Container

Start the container with USB and host network access:

```bash
docker run -it --rm   --privileged   --net=host   --device=/dev/bus/usb   -v /dev:/dev   oai-usrp
```

---

## 📂 Container Structure

Inside the container, the working directory is:

```bash
/home/oaiuser/openairinterface5g
```

---

## 🛠️ Compiling OAI (gNB with USRP)

Once inside the container:

```bash
cd cmake_targets
./build_oai -w USRP --gNB
```

This builds the 5G gNB with USRP support.

---

## 🧪 Testing UHD and USRP

To check if the USRP is detected:

```bash
uhd_usrp_probe
```

Expected output: device info (serial, clock, etc).

---

## 💡 Tips

- UHD is pre-installed with images downloaded
- Runs as non-root user (`oaiuser`)
- Use `--privileged` to access USB devices

---

## 🧹 Cleanup

To remove the Docker image:

```bash
docker rmi oai-usrp
```

---

# OpenAirInterface (OAI) Deployment Explained

This document clarifies the relationship and interaction between the Docker components in your OAI-Unisinos repository, specifically concerning the `configs` and `core` folders.

## Understanding the Deployment Structure

It's important to note that you **would not** have containers created from the `core` step running *inside* a container created from the `configs` step. Docker containers are designed to be isolated, and the OAI deployment typically involves distinct components communicating over a network.

### 1. `configs` step (OAI with USRP)

* The Dockerfile located in your `configs` folder is used to build a Docker image. This image is intended for compiling and running **OpenAirInterface (OAI) with USRP (Universal Software Radio Peripheral) support**.
* When you create a container from this image, it will typically run the **OAI Radio Access Network (RAN) component**, such as a gNodeB (gNB) for 5G or an eNodeB (eNB) for 4G. This container is responsible for the radio interface and communication with User Equipment (UEs).
* **Result:** A single container dedicated to the OAI RAN functionality.

### 2. `core` step (5G Core Network)

* The `core` folder contains configuration files, most notably Docker Compose files like `docker-compose-basic-nrf.yaml`.
* This Docker Compose file is used to define and orchestrate **multiple, distinct Docker containers** that collectively form the **5G Core Network**. These containers represent individual 5G Network Functions (NFs) such as:
    * **NRF (Network Repository Function)**
    * **AMF (Access and Mobility Management Function)**
    * **SMF (Session Management Function)**
    * **UDM (Unified Data Management)**
    * **UDR (Unified Data Repository)**
    * **AUSF (Authentication Server Function)**
    * **UPF (User Plane Function)**
    * And other related services (e.g., MySQL for database).
* **Result:** A set of interconnected containers, each running a specific 5G Core Network function.

## How They Interact

Instead of nesting, these two major components – the OAI RAN container (from `configs`) and the OAI 5G Core Network containers (from `core`) – operate as **separate, interconnected entities**.

They are configured to communicate with each other over a network, simulating a complete mobile network environment. The gNB/eNB (RAN) will connect to the AMF/MME (Core), and data traffic will flow through the UPF/SGW-U.

**In summary, you will have:**

1.  A container running the OAI gNB/eNB (built from your `configs` Docker image).
2.  A collection of containers running the OAI 5G Core Network functions (orchestrated by Docker Compose from your `core` folder).

These two parts are designed to be deployed independently and then linked via networking to establish a fully functional OpenAirInterface cellular network.

---

## 🧪 Next Steps (optional)

- Install and configure a 5G Core (Open5GS or OAI Core)
- Use `gnb_nsa` or `enb` for UE connection
- Create a virtual bridge for integrated testing

---

## ✨ Credits

Based on the official [OpenAirInterface repository](https://gitlab.eurecom.fr/oai/openairinterface5g).

Maintained by: Unisinos

---

# 📡 OpenAirInterface + USRP (Docker) PT

Este projeto fornece uma imagem Docker pronta para compilar e executar o OpenAirInterface (OAI) com suporte a USRP (via UHD). Ideal para uso em laboratório com USRP B210 ou similar.

---

## ✅ Pré-requisitos

- Docker instalado (versão 20+)
- USRP B210 (ou outro compatível) conectado via USB 3.0
- Sistema host com acesso a `/dev/bus/usb`
- Linux recomendado (Ubuntu 20.04 ou 22.04)

---

## 🧱 Build da Imagem

Clone este repositório ou copie o `Dockerfile`.

```bash
docker build -t oai-usrp .
```

---

## 🚀 Executando o Container

Use o comando abaixo para iniciar o container com acesso à USB e rede do host:

```bash
docker run -it --rm   --privileged   --net=host   --device=/dev/bus/usb   -v /dev:/dev   oai-usrp
```

---

## 📂 Estrutura no Container

Dentro do container, o diretório padrão de trabalho será:

```bash
/home/oaiuser/openairinterface5g
```

---

## 🛠️ Compilando o OAI (gNB com USRP)

Após entrar no container, execute:

```bash
cd cmake_targets
./build_oai -w USRP --gNB
```

Esse comando compila o OAI para modo 5G (gNB) com suporte ao USRP.

---

## 🧪 Testando o UHD e USRP

Para testar se o USRP está sendo detectado:

```bash
uhd_usrp_probe
```

Saída esperada: Detecção do dispositivo USRP com informações de serial, clock, etc.

---

## 💡 Dicas úteis

- O UHD já está instalado e com imagens baixadas
- O container é iniciado com um usuário não-root (`oaiuser`)
- Use `--privileged` no `docker run` para garantir o acesso ao USRP

---

## 🧹 Limpando

Para remover a imagem criada:

```bash
docker rmi oai-usrp
```

---

# Implantação do OpenAirInterface (OAI) Explicada

Este documento esclarece a relação e a interação entre os componentes Docker no seu repositório OAI-Unisinos, especificamente em relação às pastas `configs` e `core`.

## Compreendendo a Estrutura da Implantação

É importante notar que você **não** teria contêineres criados a partir da etapa `core` sendo executados *dentro* de um contêiner criado a partir da etapa `configs`. Os contêineres Docker são projetados para serem isolados, e a implantação do OAI tipicamente envolve componentes distintos se comunicando por uma rede.

### 1. Etapa `configs` (OAI com USRP)

* O Dockerfile localizado na sua pasta `configs` é usado para construir uma imagem Docker. Esta imagem é destinada à compilação e execução do **OpenAirInterface (OAI) com suporte a USRP (Universal Software Radio Peripheral)**.
* Ao criar um contêiner a partir desta imagem, ele normalmente executará o **componente OAI Radio Access Network (RAN)**, como um gNodeB (gNB) para 5G ou um eNodeB (eNB) para 4G. Este contêiner é responsável pela interface de rádio e comunicação com os User Equipments (UEs).
* **Resultado:** Um único contêiner dedicado à funcionalidade OAI RAN.

### 2. Etapa `core` (Rede de Núcleo 5G)

* A pasta `core` contém arquivos de configuração, notavelmente arquivos Docker Compose como `docker-compose-basic-nrf.yaml`.
* Este arquivo Docker Compose é usado para definir e orchestrar **múltiplos e distintos contêineres Docker** que, coletivamente, formam a **Rede de Núcleo 5G**. Estes contêineres representam funções individuais da Rede 5G (NFs), tais como:
    * **NRF (Network Repository Function)**
    * **AMF (Access and Mobility Management Function)**
    * **SMF (Session Management Function)**
    * **UDM (Unified Data Management)**
    * **UDR (Unified Data Repository)**
    * **AUSF (Authentication Server Function)**
    * **UPF (User Plane Function)**
    * E outros serviços relacionados (por exemplo, MySQL para banco de dados).
* **Resultado:** Um conjunto de contêineres interconectados, cada um executando uma função específica da Rede de Núcleo 5G.

## Como Eles Interagem

Em vez de aninhamento, esses dois principais componentes – o contêiner OAI RAN (da `configs`) e os contêineres da Rede de Núcleo OAI 5G (da `core`) – operam como **entidades separadas e interconectadas**.

Eles são configurados para se comunicar entre si por uma rede, simulando um ambiente de rede móvel completo. O gNB/eNB (RAN) se conectará ao AMF/MME (Core), e o tráfego de dados fluirá através do UPF/SGW-U.

**Em resumo, você terá:**

1. Um contêiner executando o OAI gNB/eNB (construído a partir da sua imagem Docker `configs`).
2. Uma coleção de contêineres executando as funções da Rede de Núcleo OAI 5G (orquestrados pelo Docker Compose da sua pasta `core`).

Essas duas partes são projetadas para serem implantadas independentemente e, em seguida, interligadas via rede para estabelecer uma rede celular OpenAirInterface totalmente funcional.

---

## 🧪 Próximos passos (opcional)

- Instalar e configurar o Core 5G (como Open5GS ou OAI Core)
- Usar `gnb_nsa` ou `enb` para integração com UE físico ou simulado
- Criar uma bridge virtual para testes integrados

---

## ✨ Créditos

Baseado no repositório oficial do [OpenAirInterface](https://gitlab.eurecom.fr/oai/openairinterface5g).

Mantido por: Unisinos
