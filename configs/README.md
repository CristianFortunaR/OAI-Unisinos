# 📡 OpenAirInterface + USRP ((Docker) EN

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

## 🧪 Próximos passos (opcional)

- Instalar e configurar o Core 5G (como Open5GS ou OAI Core)
- Usar `gnb_nsa` ou `enb` para integração com UE físico ou simulado
- Criar uma bridge virtual para testes integrados

---

## ✨ Créditos

Baseado no repositório oficial do [OpenAirInterface](https://gitlab.eurecom.fr/oai/openairinterface5g).

Mantido por: Unisinos
