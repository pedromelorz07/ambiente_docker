# Configuração de Ambiente Docker para ROS 🤖
Este repositório fornece um guia passo a passo e os arquivos necessários para configurar um ambiente de desenvolvimento isolado para ROS utilizando Docker.

## 1. Conceitos Fundamentais:
- **Docker:** É uma plataforma de containerização. Link para o Docker Engine https://docs.docker.com/engine/
- **Dockerfile:** É a "receita do bolo". Este arquivo diz o que instalar (instalar pacotes, configurar usuários, etc.).
- **Imagem:** É o bolo pronto. É um snapshot estático que contém o sistema operacional, bibliotecas e o ROS instalado.
- **Container:** É quando você corta uma fatia e serve (o ambiente rodando). É o processo em execução baseado na imagem. Você pode criar vários containers a partir de uma mesma imagem.
  - *Por que usar o Container?:* Portabilidade, isolamento de versões (ex: rodar ROS2 Humble e ROS Noetic na mesma máquina) e facilidade de recuperação se algo quebrar.
- **Docker Compose:** É o garçom que organiza a mesa. Ele define redes, volumes (pastas
compartilhadas entre seu PC e o Docker) e variáveis de ambiente de forma fácil.

## 2. Instalação do Docker Engine (Linux/Ubuntu):
Para rodar este projeto, você precisa do Docker Engine. No Ubuntu, o método recomendado é via repositório oficial:
- Atualize o apt e instale dependências:
  - `sudo apt-get update && sudo apt-get install ca-certificates curl gnupg`
- Adicione a chave GPG oficial do Docker:
  - `sudo install -m 0755 -d /etc/apt/keyrings`
  - `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg`
  - `sudo chmod a+r /etc/apt/keyrings/docker.gpg`
- Instale o Docker Engine e o Docker Compose:
  - `sudo apt-get update`
  - `sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`
- Etapas de pós-instalação do Docker Engine no Linux: https://docs.docker.com/engine/install/linux-postinstall

 ## 3. Dockerfile vs Docker Compose:
 Neste projeto, usamos ambos:
 - **Dockerfile:** Define o que tem dentro do container (ROS, Gazebo, Turtlebot).
 - **docker-compose.yaml:** Define como o container roda (mapeamento de pastas, permissões de interface gráfica, rede). É essencial para ROS pois evita comandos docker run gigantescos com dezenas de flags.

  ## 4. Fluxo de Trabalho e Comandos:
  O fluxo padrão é: Escrever Dockerfile ➡️ Buildar Imagem ➡️ Subir Container.
  - **Comandos para Imagens:**
    - `docker compose build`: buildar a Imagem.
    - `docker images`: listar todas as Imagens.
    - `docker pull <name>:<tag>`: é usado para baixar uma imagem Docker ou um conjunto de imagens de um registro.
    - `docker rmi -f <image_name>`: remove uma Imagem. (-f força a remoção).
  - **Comandos para Containers:**
    - `docker compose up -d`: subir o Container. -d executa em modo background/desacoplado.
    - `docker compose down`: para e remove containers, redes e volumes criados pelo up.
    - `docker run`: cria e inicia um novo Container a partir de uma Imagem. (Na primeira vez que você inicia o ambiente.)
    - `docker exec -it <container_name> bash`: entrar em um Container que já está em execução. (Para abrir vários terminais no mesmo ambiente ROS.)
    - `docker ps`: lista os containers em execução.
    - `docker ps -a`: -a lista todos os Containers em execução e os parados.
    - `docker start <container_name>`: inicia um Container parado.
    - `docker stop <container_name>`: para um Container pem execução.
    - `docker rm <container_name>`: remove um Container.
  - **Limpeza:**
    - `docker volume prune`: remove todos os Volume não utilizados.
    - `docker image prune`: remove todas as Imagens não utilizadas.
    - `docker system prune -a`: remove recursos não utilizados do sistema. (-a ou --all: remove também todas as imagens não utilizadas por containers).
    - `docker system prune --volumes`: --volumes: inclui volumes não utilizados na limpeza
      
      <img width="791" height="267" alt="image" src="https://github.com/user-attachments/assets/5574088c-db40-4175-a2c2-1bf45c333e98" />


   ## 5. Estrutura de Diretórios no Container:
   Dentro do container, seguimos a convenção do ROS:
   - `/ros_ws/`: o seu Workspace principal.
   - `/ros_ws/src/`: onde você coloca o código fonte dos seus pacotes.
   - ***Dica:*** usamos **Volumes** no Docker Compose para que o código que você edita na sua máquina (Ex: Linux) apareça instantaneamente dentro do container.

  ## 6. Integração com VSCode:
  -  Instale a extensão **Remote - Containers** (ou **Dev Containers**) no VSCode.
  -  Com o container rodando, clique no ícone azul no canto inferior esquerdo do VSCode.
  -  Selecione **"Attach to Running Container"**.
  -  Escolha o container do ROS. Agora você pode editar arquivos e usar o terminal do VSCode direto dentro do ambiente Docker!
  
  ## 7. Desenvolvendo:
  Depois de ter baixado o Docker Compose e o Docker Engine:
  - Crie o diretório de trabalho para Docker (Ex: `/docker_ws/`).
  - Dentro deste diretório, cole os arquivos `Dockerfile` e `docker-compose.yaml`
  - Agora, dê os seguintes comandos:
    - `docker compose build`: construir a imagem.
    - `docker compose up -d`: subir o container.
    - `docker exec -it ros2_humble_dev bash`: entrar no container.
      - aparecerá algo como `root@alguma_coisa:~#`, ou seja, você está dentro do container.
    - `ros2 run turtlesim turtlesim_node`: já dentro do container, rodar o turtlesim para ver se está funcionando.
    - `ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py`: rodar a simulação do turtlebot3
  - A estrutura do diretório `/docker_ws/` após estes comandos será algo como:
    
    <img width="278" height="209" alt="image" src="https://github.com/user-attachments/assets/f3a339e1-68a3-4016-be18-f1d9e44b2969" />

  

   
