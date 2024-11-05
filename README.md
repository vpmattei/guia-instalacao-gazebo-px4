# Guia de instalação Gazebo e PX4 no Ubuntu 20.04 LTS

![Ubuntu 20.04](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1280,h_768/https://ubuntu.com/wp-content/uploads/a728/2020-04-23-13.05.21.jpg)

Vinícius Mattei - Última atualização: 05/11/2024

---

## 1. Instalar Ubuntu 20.04.6 LTS

  Instale a versão 20.04.6 LTS de Ubuntu seguindo este [link](https://releases.ubuntu.com/focal/), é importante instalar esta versão e não uma versão posterior ou anterior pois muitas das bibliotecas só funcionam nesta versão de Ubuntu.
  
  Instale o Ubuntu em uma máquina virtual ou com um dual boot, caso
não saiba como instalar como um dual boot et queira seguir esta maneira,
siga [este tutorial](https://youtu.be/mXyN1aJYefc?si=b5Smt-pQjrhisa16) em inglês explicando. E caso tenha dificuldades ao particionar disco rígido, sugiro instalar este [programa gratuito](https://www.partitionwizard.com/free-partition-manager.html).

---

## 2. Instalar ROS

  Agora que já tem o sistema operacional Ubuntu 20.04.6 instalado,
podemos seguir para a instalação de Gazebo, mas antes disso temos que
instalar suas dependências, no caso são o ROS e o PX4. [Aqui está o guia](<https://docs.px4.io/main/en/ros/mavros_installation.html#ros-noetic-(ubuntu-22.04)>) oficial para esta instalação para usar como referência, nós vamos seguir este guia mas vamos fazer as coisas um pouquinho diferente.

  Primeiramente vamos instalar o ROS Noetic usando este guia, use-o
como referência caso precise de mais informações, caso contrário, siga os próximos
passos :

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

  Depois escolha o quanto do ROS deseja instalar, neste exemplo
escolhemos instalar todos os pacotes e bibliotecas, isto deve demorar algum
tempo para ser executado, então já prepare um cafezinho ☕ bem quente.

```bash
sudo apt install ros-noetic-desktop-full
```

### 2.d Configuração do ambiente

  Você deve então configurar o ambiente usando o comando source do
setup.bash do ROS.

```bash
source /opt/ros/noetic/setup.bash
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

```zsh
echo "source /opt/ros/noetic/setup.zsh" >> ~/.zshrc
source ~/.zshrc
```

### 2.e Dependências para construir os pacotes

  Até agora você instalou o que precisa para executar os pacotes
principais do ROS. Para criar e gerenciar seus próprios espaços de trabalho
ROS, existem várias ferramentas e requisitos distribuídos separadamente. Por
exemplo, “rosinstall” é uma ferramenta de linha de comando usada com
frequência que permite baixar facilmente muitas “source trees” para pacotes
ROS com um comando.

  Para instalar esta ferramenta e outras dependências para construir
pacotes ROS, execute:

```bash
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
```

### 2.f Inicializar o rosdep

  Antes de poder usar muitas ferramentas ROS, você precisará inicializar o
rosdep. rosdep permite que você instale facilmente dependências do
sistema para a fonte que deseja compilar e é necessário para executar alguns
componentes principais no ROS. Se você ainda não instalou o rosdep, faça o
seguinte.

```bash
sudo apt install python3-rosdep
```

  Com o seguinte, você pode inicializar o rosdep.

```bash
sudo rosdep init
rosdep update
```

  Perfeito! Se tudo deu certo, agora você deve ter instalado todas as
dependências necessárias do ROS Noetic.

---

## 3. Instalar o PX4

  Agora que você ja tem o ROS Noetic instalado, ja pode instalar o PX4.
Crie um repertório para desenvolvimento e faça o download do PX4 com o
seguinte comando (certifique-se que tenha GIT instalado).

```bash
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
```

Rode o comando ubuntu.sh

```bash
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
```

  Pode ser que você encontre um erro relacionado a versão de
numpy>=1.20.3, para resolver este erro, instale a versão 1.20.3 com o seguinte
comando.

```bash
pip install numpy==1.20.3
```

  Para verificar que esta com a versão correta instalada use o seguinte
comando.

```bash
python3 -c "import numpy; print(numpy.__version__)”
```

### 3.a Instalar as dependências

```bash
sudo apt-get install protobuf-compiler libeigen3-dev libopencv-dev -y
```

  Dentro do diretório do PX4, rode os seguintes comandos

```bash
make
make px4_sitl gazebo
```

  Caso continue encontrando erros, reinicie seu computador. Depois vá
até o repertório do PX4 novamente e reinstale o PX4 :


```bash
make clean
make distclean
bash ./Tools/setup/ubuntu.sh
make
make px4_sitl gazebo
```

  Você deve ver a uma nova janela do Gazebo se abrir com a simulação do
drone. Dentro do terminal pode executar o seguinte comando para fazer o
drone decolar.

```bash
commander takeoff
```

---

## 4. Instalar QGroundControl

  Finalmente você pode instalar QGroundControl no Ubuntu 20.04.6
executando os seguintes comandos:

```bash
sudo usermod -a -G dialout $USER
sudo apt-get remove modemmanager -y
sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav
gstreamer1.0-gl -y
sudo apt install libfuse2 -y
sudo apt install libxcb-xinerama0 libxkbcommon-x11-0 libxcb-
cursor0 -y
```

  Depois disso faça o [download do arquivo](https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.AppImage) e dê as permissões necessárias
para poder rodá-lo.

```bash
chmod +x ./QGroundControl.AppImage
```

  Finalmente só basta rodar QGroundControl, pode rodar usando o
terminal ou clicando duas vezes no arquivo baixado.

```bash
./QGroundControl.AppImage
```

  Para fechar com chave 🔑 de ouro, se dirija até o repertório do PX4 e
execute o comando seguinte para rodar uma simulação com um avião VTOL
(note que você tem que rodar o QGroundControl antes de rodar o
Gazebo+PX4).

```bash
make px4_sitl gazebo-classic_standard_vtol
```
