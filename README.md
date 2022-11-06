<h1 align="center">Instalação de Docker e Opencv em Arquitetura 
<p> ARM( RaspberryPi , Android ) </p> </h1>

<h2 align="center">
<img src="https://user-images.githubusercontent.com/40872405/200147615-49e7eb11-14d8-4f3c-bb7e-b02f5a184eaf.png"/>
<h2/>
  
## Motivação 
##### 
A motivação começa com a necessidade em resolver um problema de logistica e de HardPower,
Eu possui um Pi 3b+ comprado em 2020 com memoria 'ram' limitada e memoria em 'disco' curta de um cartão SD 8GB.
Então eu trabalho em um ambiente bem controlado/limitado a onde não posso fugir do Escopo, um simples ap-get update/upgrade da distro do raspbian já lotaria meus 8GB.
  Ainda mais com libs de c, c++ que precisam ser buildadas e compiladas como no caso do Opencv para ser usado em Python

## Solução
#### Enfrentando um ambiente tão Limitado** eu tive que optar pelo menos provalvel que seria instalar Docker e dentro dele criar outro ambiente Linux com tudo que necessitaria para Builda o Opencv, Python, Cmake, GCC7. que por incrivel que pareça colocar todo esse sistema em uma 'Bolha' em Docker só demanda 700M de disco. Coisa que se eu fosse fazer no PI direito daria uns 8GB+ só de pacotes para o PI rodar Atualizado.

## Instalção
  
#### Primeiro instalar o Docker em seu Pi

# Clone o repositorio
```sh
$ git clone https://github.com/carlinhoshk/buildando_opencv_docker.git

# Abra o projeto e a pasta de instalação

$ cd buildando_opencv_docker/InstalarDocker
``` 
```sh  
# Ou de um Curl direto no site oficial da Docker
$ curl -fsSL https://get.docker.com -o get-docker.sh

# Depois execute o arquivo 
$ sudo sh get-docker.sh
```
```sh  
# Quando a instalção termina adicione o Docker ao seu grupo de usuario meu caso carlinhoshk já que troquei o nome de usuario do pi
$ sudo usermod -aG docker [user_name]
# Exemplo 
$ sudo usermod -aG docker carlinhoshk
  
$ sudo reboot
  
$ docker version 
```  
### nesse momento não é para aparecer nenhum erro se apareceu mande um print do terminal e o modelo da placa em [issues](https://github.com/carlinhoshk/buildando_opencv_docker/issues)

  
####
  Agora você precisa Aumentar a memoria do seu Pi para ele ter capacidade de buildar usando cmake para o opencv, eu já tentei varias vezes builda sem aumenta a memoria, mas sempre da erro em 96% <- confesso que quebrei muita cabeça até entender que era falta de memoria. Acho que como eu você não gosta muito de mexer nas configurações e ficar com medo de quebrar algo. mas como no meu caso eu fui Obrigado a modificar para atigir meu Objetivo. vou posta o comando para aumentar a memoria swap

```sh 
# mudando a memoria de swap
# abra o aquivo
$ sudo nano /etc/dphys-swapfile
```
![Screenshot_20221105_093407](https://user-images.githubusercontent.com/40872405/200148632-d1bcd2fd-e35c-4c7d-95aa-836ff06a5931.png)
## modifque colocando 512 onde está a seta vermelha e observe as 2 setas azuis que demonstram o comando para Salvar e Sair do editor nano: Ctrl+O | Ctrl+X
```sh
# reiniciando serviço
$ sudo /etc/init.d/dphys-swapfile stop
$ sudo /etc/init.d/dphys-swapfile start
```
![Screenshot_20221105_093951](https://user-images.githubusercontent.com/40872405/200148731-0092d033-99cf-4de2-945d-cd100b737f66.png)

#### se você der o commando $free -m é para estar como o meu, agora sim você poderá dar procedimento a instalção da imagem opencv
  
## Após você ter certeza que seu Docker está funcionando você precisa builda sua Imagem Docker, volte uma pasta ou abra novamente o repositorio
  
```sh
$ cd ../

# Agora builde a imagem passando ela no parametro 

$ docker build -f Dockerfile.arm .

```
## Nesse momento a imagem vai ser buildada no meu Pi 3b+ demorou em torno de 4 Horas a 6 Horas. eu aconselho realmente esperar tudo isso, mesmo quando parecer que travou só cheque depois de dar exatos 4 horas no minimo. 

# Após terminar escreva 
```sh
$ docker image list
# aqui é para aparecer suas duas imagem docker, uma de build e outra de execução
```
![Screenshot_20221105_094738](https://user-images.githubusercontent.com/40872405/200149294-61a3ae84-e42c-4415-81ee-7649f900939c.png)


## Em IMAGE ID o seu vai está diferente, como eu não dei uma TAG para minha imagem ela fica como none marcado em vermelho. mas é possivel usar o numero da TAG ( eu acho mais facil )


## Bom agora com tudo pronto podemos executar a imagem em modo interativo
 ```sh
$docker run -it 7187a834a450 bash
 ```
![Screenshot_20221105_095230](https://user-images.githubusercontent.com/40872405/200149104-d39c3fef-7517-4f41-987f-d469737cca1d.png)

## Pronto lembrando que no Print acima eu passei o parametro --rm, no momento que eu fechar o container ele vai ser excluido. lembrando que a imagem vai continuar, Se você quiser deixar o container 'vivo' é só escrever o comentario $docker run -it 7187a834a450 bash 

### pronto agora se quiser voltar a memoria swap é por sua conta propria 
  
  
![Screenshot_20221105_100414](https://user-images.githubusercontent.com/40872405/200149250-89ed8ecf-79ae-4330-ab08-72e1b8f56f35.png)

##Links e Documentação usada no processo:
  
  
  -[Instalando Docker](https://www.simplilearn.com/tutorials/docker-tutorial/raspberry-pi-docker)
  
  -[Swap de memoria](https://qengineering.eu/install-opencv-lite-on-raspberry-pi.html)
  
  -[Imagem Docker OpenCv](https://www.linkedin.com/pulse/opencv-e-raspberry-pi-jantando-dentro-de-um-cont%C3%AAiner-s-bezerra/?originalSubdomain=pt)
  
  
  
