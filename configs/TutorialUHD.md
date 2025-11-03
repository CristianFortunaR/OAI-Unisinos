# Instalação do UHD

Para utilizar dispositivos que necessitam do driver **UHD**, como o rádio **USRP B210**, é essencial seguir a instalação da próxima seção.

Este processo compila e instala o driver UHD (v4.8.0.0):

```bash
# [https://files.ettus.com/manual/page_build_guide.html](https://files.ettus.com/manual/page_build_guide.html)

# 1. Instalar dependências do UHD
sudo apt install -y autoconf automake build-essential ccache cmake cpufrequtils \
doxygen ethtool g++ git inetutils-tools libboost-all-dev libncurses-dev \
libusb-1.0-0 libusb-1.0-0-dev libusb-dev python3-dev python3-mako \
python3-numpy python3-requests python3-scipy python3-setuptools python3-ruamel.yaml

# 2. Clonar o repositório do UHD
git clone [https://github.com/EttusResearch/uhd.git](https://github.com/EttusResearch/uhd.git) ~/uhd
cd ~/uhd

# 3. Fazer checkout da versão específica (v4.8.0.0)
git checkout v4.8.0.0

# 4. Compilar e instalar
cd host
mkdir build
cd build
cmake ../
make -j $(nproc)
make test # Este passo é opcional

# 5. Instalar e carregar as bibliotecas
sudo make install
sudo ldconfig

# 6. Baixar as imagens de firmware/FPGA
sudo uhd_images_downloader
