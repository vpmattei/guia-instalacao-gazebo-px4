# Guia de instalação Gazebo e PX4 no Ubuntu 20.04 LTS

## Vinicius Mattei - 02/10/2024

---

## Índice

1. [Instalar Ubuntu 20.04.6 LTS](#instalar-ubuntu-20046-lts)
2. [Instalar ROS](#instalar-ros)
    1. [Configurar o sources.list](#configurar-o-sourceslist)
    2. [Configurar suas chaves](#configurar-suas-chaves)
    3. [Instalação](#instalacao)
    4. [Configuração do ambiente](#configuracao-do-ambiente)
    5. [Dependências para construir os pacotes](#dependencias-para-construir-os-pacotes)
    6. [Inicializar o rosdep](#inicializar-o-rosdep)
3. [Instalar o PX4](#instalar-o-px4)
    1. [Instalar as dependências](#instalar-as-dependencias)
4. [Instalar QGroundControl](#instalar-qgroundcontrol)

---

## 1. Instalar Ubuntu 20.04.6 LTS

Instale a versão 20.04.6 LTS de Ubuntu seguindo este [link](https://releases.ubuntu.com/focal/), é importante instalar esta versão e não uma versão posterior ou anterior pois muitas das bibliotecas só funcionam nesta versão de Ubuntu.

---

## 2. Instalar ROS

Agora que já tem o sistema operacional Ubuntu 20.04.6 instalado, podemos seguir para a instalação de Gazebo, mas antes disso temos que instalar suas dependências, no caso são o ROS e o PX4.

[Guia oficial para esta instalação](https://docs.px4.io/main/en/ros/mavros_installation.html#ros-noetic-(ubuntu-22.04))

### 2.a Configurar o sources.list

Dentro do terminal, execute os seguintes comandos:

```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

### 2.b Configurar suas chaves

```bash
sudo apt install curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

### 2.c Instalação

Certifique-se que todos seus pacotes Debian estejam atualizados:

```bash
sudo apt update
```

Para instalar o ROS Noetic, execute o seguinte comando:

```bash
sudo apt install ros-noetic-desktop-full
```

### 2.d Configuração do ambiente

```bash
source /opt/ros/noetic/setup.bash
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### 2.e Dependências para construir os pacotes

```bash
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
```

### 2.f Inicializar o rosdep

```bash
sudo rosdep init
rosdep update
```

---

## 3. Instalar o PX4

```bash
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
```

### 3.a Instalar as dependências

```bash
sudo apt-get install protobuf-compiler libeigen3-dev libopencv-dev -y
```

---

## 4. Instalar QGroundControl

```bash
sudo apt install libfuse2 -y
chmod +x ./QGroundControl.AppImage
./QGroundControl.AppImage
```

[Baixar QGroundControl](https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.AppImage)

