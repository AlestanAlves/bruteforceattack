![banner GS cyber](https://user-images.githubusercontent.com/48387196/121257879-9a9ab300-c884-11eb-9a9a-4923081f3d28.png)

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

## TelNet

O TELNET é um serviço muito vulnerável. Por não possuir nenhum tipo de criptografia, permite a descoberta de senhas e captura de informações.Através de uma sessão TELNET é possível disparar ataques, descobrir portas desprotegidas e serviços que estão sendo executados no servidor. O TELNET também é usado freqüentemente para gerar correio e notícias falsas (fakemail e fakenews).

## Brute Force Attack

Sabemos que existem portas abertas dentro do servidor e com isso podemos fazer um ataque de força bruta e tentar acessar o servidor. Fiz uma fork de um repositório com wordlists e vamos tentar buscar acesso com uma senha já conhecida por diversas pessoas.

Vamos utilizar a ferramenta Hydra para fazer esse brute force dentro do servidor e buscar entrar dentro do servidor apenas usando uma wordlist e batendo user e senha.

Vamos entrar na pasta do kali /user/share/wordlists e fazer um gunzip do rockyou.(mas como vamos utilizar um exemplo eu baixei uma wordlist da internet mais simples para não demorar tanto esse brute force).

```
hydra -l funcionario -P rockyou.txt 10.0.2.4 telnet -V 
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

Logo após sabermos que o nmap possui privilégio vamos utilizar ele para ter acesso root ao servidor, utilizando o código abaixo para ter acesso ao modo interativo do nmap.

```
nmap --interactive 
```
Rodar o shell

```
!sh
```

Agora se eu der um `id` ou `whoami` eu apareco como root

Entao agora bora na pasta principal do servidor e clonar nosso repositorio mas antes excluir o motd que ja existe.

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

## Ascii Hacked

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
