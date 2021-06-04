# Hydra Brute Force Attack

## Scan das portas

```
nmap -sS 10.0.2.4 | tee nmap-ss.txt
```

Por que fazer um port scanning? a ideia de se fazer um tipo de scan desses e buscar portas abertas e suscetíveis a ataques. Usando o `tee` conseguimos salvar estas portas em um arquivo txt.

## Analise de vulnerabilidades

Depois da analise das portas vamos fazer uma analise de vulnerabilidades utilizando o Sparta que e uma ferramenta de scanner de vulnerabilidades. Com o Sparta ja instalado vamos rodar alguns comandos para iniciarmos ele.

Start in /usr/share/sparta

```
python3 sparta.py 
```

## Mitre Att&ck

Brute Force ID: T1110

Os adversários podem usar técnicas de força bruta para obter acesso a contas quando as senhas são desconhecidas ou quando os hashes de senha são obtidos. Sem o conhecimento da senha de uma conta ou conjunto de contas, um adversário pode adivinhar a senha usando um mecanismo repetitivo ou iterativo. A força bruta de senhas pode ocorrer por meio da interação com um serviço que verificará a validade dessas credenciais ou off-line em relação aos dados de credenciais adquiridos anteriormente, como hashes de senha.

## Brute Force Attack

Sabemos que existem portas abertas dentro do servidor e com isso podemos fazer um ataque de força bruta e tentar acessar o servidor. Fiz uma fork de um repositório com wordlists e vamos tentar buscar acesso com uma senha já conhecida por diversas pessoas.

Vamos utilizar a ferramenta Hydra para fazer esse brute force dentro do servidor e buscar entrar dentro do servidor apenas usando uma wordlist e batendo user e senha.

```
hydra -l msfadmin -P rockyou.txt telnet -V 
```

Atraves do hidra e de senhas ja conhecidas em uma wordlist conseguimos obter o acesso ao servidor 

## Tools using

- nmap
- hydra
- wordlists
- sparta

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
