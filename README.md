# EvilCrowRF-V2

![EvilCrow](https://github.com/joelsernamoreno/EvilCrowRF-V2/blob/main/images/Logo1.png)

**Ideia, desenvolvimento e implementação:** Joel Serna (@JoelSernaMoreno).

**Colaborador principal:** Little Satan (https://github.com/LSatan/)

**Outros colaboradores:** Eduardo Blázquez (@_eblazquez), Federico Maggi (@phretor), Andrea Guglielmini (@Guglio95) e RFQuack (@rfquack).

**Design de PCB:** Ignacio Díaz Álvarez (@Nacon_96), Forensic Security (@ForensicSec) e April Brother (@aprbrother).

**Fabricante e distribuidor:** April Brother (@aprbrother).

**Distribuidor do Reino Unido:** KSEC Worldwide (@KSEC_KC).

Os desenvolvedores e colaboradores deste projeto não ganham dinheiro com isso.
Você pode me convidar para um café para desenvolver ainda mais dispositivos de hackers de baixo custo. Se você não me convidar para um café, nada acontece, continuarei desenvolvendo dispositivos.

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/E1E614OA5)

**À venda com a April Brother (envio da China):**

* Evil Crow RF V2 Aliexpress: https://aliexpress.com/item/1005004019072519.html
* Evil Crow RF V2 Lite (sem NRF2401L) Aliexpress: https://aliexpress.com/item/1005004032930927.html
* Evil Crow RF V2 Alibaba: https://www.alibaba.com/product-detail/Evil-Crow-RF2-signal-receiver-with_1600467911757.html

**Para venda com KSEC Worldwide (envio do Reino Unido):**

* Evil Crow RF V2: https://labs.ksec.co.uk/product/evil-crow-rf-v2/
* Evil Crow RF V2 Lite: https://labs.ksec.co.uk/product/evil-crow-rf2-lite/

**Grupo do Discord:** https://discord.gg/jECPUtdrnW

**Resumo:**

1. Isenção de responsabilidade
2. Introdução
3. Firmware
* Instalação
* Primeiros passos com Evil Crow RF V2
* Exemplo de Configuração RX
* Exemplo de Log RX
* Exemplo de configuração RAW TX
* Exemplo de configuração de TX binário
* Configuração de botões
* Abridor de porta de carga Tesla
* Exemplo de análise de URH
* Atualização OTA
* Configuração Wi-Fi
* Gerenciamento de energia
* Outros esboços
4. Suporte Evil Crow RF V2

# Isenção de responsabilidade

O Evil Crow RF V2 é um dispositivo básico para profissionais e entusiastas de cibersegurança.

Não somos responsáveis pelo uso incorreto do Evil Crow RF V2.

Recomendamos o uso deste dispositivo para testes, aprendizado e diversão :D

**Cuidado com este aparelho e com a transmissão de sinais. Certifique-se de seguir as leis que se aplicam ao seu país.**

![EvilCrowRF](https://github.com/joelsernamoreno/EvilCrowRF-V2/blob/main/images/evilcrowrfv2.jpg)

# Introdução

Evil Crow RF V2 é um dispositivo de hacking de radiofrequência para operações de pentest e Red Team, este dispositivo opera nas seguintes bandas de radiofrequência:

* 300Mhz-348Mhz
* 387Mhz-464Mhz
* 779Mhz-928Mhz
* 2,4 GHz

O Evil Crow RF V2 possui dois módulos de radiofrequência CC1101, esses módulos podem ser configurados para transmitir ou receber em diferentes frequências ao mesmo tempo. Além disso, o Evil Crow RF V2 possui um módulo NRF24L01 para outros ataques.

Evil Crow RF V2 permite os seguintes ataques:

* Receptor de sinal
* Transmissor de sinal
* Ataque de repetição
* análise URH
* Mousejacking
* ...

**OBSERVAÇÃO:**

* Todos os dispositivos foram atualizados com o firmware básico Evil Crow RF V2 antes do envio.
* Por favor, não me peça para implementar novas funções neste código. Você pode desenvolver código para Evil Crow RF V2 e enviar PR com seu novo código.

#Firmware

O firmware básico permite receber e transmitir sinais. Você pode configurar os dois módulos de rádio através de um painel web via WiFi.

## Instalação

1. Instale o esptool: sudo apt install esptool
2. Instale o pyserial: sudo pip install pyserial
3. Baixe e instale o Arduino IDE: https://www.arduino.cc/en/main/software
4. Baixe o repositório Evil Crow RF V2: git clone https://github.com/joelsernamoreno/EvilCrowRF-V2.git
5. Baixe a biblioteca ESPAsyncWebServer no diretório da biblioteca Arduino: git clone https://github.com/me-no-dev/ESPAsyncWebServer.git
6. Baixe a biblioteca AsyncElegantOTA no diretório da biblioteca do Arduino: git clone https://github.com/ayushsharma82/AsyncElegantOTA.git
7. Baixe a biblioteca ESP32-targz no diretório da biblioteca do Arduino: git clone https://github.com/tobozo/ESP32-targz.git
8. Baixe a biblioteca AsyncTCP no diretório da biblioteca do Arduino: git clone https://github.com/me-no-dev/AsyncTCP.git
9. Edite AsyncTCP/src/AsyncTCP.he altere o seguinte:

* #define CONFIG_ASYNC_TCP_USE_WDT 1 a #define CONFIG_ASYNC_TCP_USE_WDT 0

10. Abra o Arduino IDE
11. Vá para Arquivo - Preferências. Localize o campo "Additional Board Manager URLs:" Adicione "https://dl.espressif.com/dl/package_esp32_index.json" sem aspas. Clique OK"
12. Selecione Ferramentas - Placa - Gerenciador de Placas. Procure por "esp32". Instale "esp32 by Espressif system version 1.0.6". Clique em "Fechar".
13. Abra o esboço EvilCrowRF-V2/firmware/v1.3.2/EvilCrow-RFv2/EvilCrow-RFv2.ino
14. Selecione Ferramentas:
     * Placa - "Módulo ESP32 Dev".
     * Tamanho do Flash - "4MB (32Mb)".
     * Frequência da CPU - "80MHz (WiFi/BT)".
     * Frequência do Flash - "40MHz"
     * Modo Flash - "DIO"
15. Carregue o código para o dispositivo Evil Crow RF V2
16. Copie a pasta EvilCrowRF-V2/firmware/v1.3.2/SD/HTML para um cartão MicroSD.
17. Copie a pasta EvilCrowRF-V2/firmware/v1.3.2/SD/URH para um cartão MicroSD.
    
![SD](https://github.com/joelsernamoreno/EvilCrowRF-V2/blob/main/images/sd.png)

**Observações sobre SD:**

* O servidor Web não carrega, é uma página em branco ou não exibe nada:

Verifique se você copiou os arquivos relevantes para o cartão SD e se o cartão SD está inserido no dispositivo Evil Crow RF V2. Verifique se você está conectado ao ponto de acesso Wi-Fi do Evil Crow RF V2.

* Meus arquivos estão no cartão SD, mas o servidor web não funciona:

Verifique o tamanho do seu cartão SD. Recomenda-se usar um cartão pequeno. 32 GB ou menos é suficiente para a operação. Cartões maiores do que isso demonstraram causar problemas e não funcionar.

* Não consigo acessar a internet quando estou conectado ao Evil Crow RF V2:

Por padrão, o Evil Crow opera como um ponto de acesso. Quando você se conecta a ele, ele não tem acesso à internet, pois não está conectado à internet. Se você precisar de internet ao mesmo tempo, leia a seção Wi-Fi Config deste repositório para configurar o Evil Crow RF V2 no modo STATION.

## Primeiros passos com Evil Crow RF V2

0. Verifique e verifique se você copiou os arquivos relevantes para o seu cartão SD.
1. Insira o cartão MicroSD no Evil Crow RF V2 e conecte o dispositivo a uma bateria externa ou laptop.
2. Visualize as redes wi-fi ao seu redor e conecte-se ao Evil Crow RF V2 (SSID padrão: Evil Crow RF v2).
3. Digite a senha da rede wi-fi (senha padrão: 123456789).
4. Abra um navegador e acesse o painel web (IP padrão: 192.168.4.1).
5. Vá!

![Webpanel](https://github.com/joelsernamoreno/EvilCrowRF-V2/blob/main/images/webpanel.png)

## Exemplo de Configuração RX

* Módulo: (1 para o primeiro módulo CC1101, 2 para o segundo módulo CC1101)
* Modulação: (exemplo ASK/OOK)
* Frequência: (exemplo 433.92)
* Largura de banda RxBW: (exemplo 58)
* Desvio: (exemplo 0)
* Taxa de dados: (exemplo 5)

![RX](https://github.com/joelsernamoreno/EvilCrowRF-V2/blob/main/images/rx.png)

**NOTAS 2-FSK:**

* Evil Crow RF V2 permite modulação 2-FSK (RX/TX), isso é configurado para uso com CC1101 módulo 2. Não use CC1101 módulo 1 para 2-FSK RX.

* Você pode usar 2-FSK TX com o módulo 1 ou com o módulo 2.

* Evil Crow RF V2 permite que você receba sinais ao mesmo tempo em duas frequências diferentes, mas isso não funciona corretamente se você usar 2-FSK. Certifique-se de usar o módulo 2 para 2-FSK RX, ao fazer isso não use o módulo 1 para nada ou você não receberá os sinais 2-FSK corretamente.

* Você pode receber dois sinais em frequências diferentes com ASK/OOK.

## Exemplo de Log RX

![RXLog](https://github.com/joelsernamoreno/EvilCrowRF-V2/blob/main/images/rx-log.png)

## Exemplo de Configuração RAW TX

* Módulo: (1 para o primeiro módulo CC1101, 2 para o segundo módulo CC1101)
* Modulação: (exemplo ASK/OOK)
* Transmissões: (número de transmissões)
* Frequência: (exemplo 433.92)
* RAW Data: (dados brutos ou dados brutos corrigidos exibidos no RX Log)
* Desvio: (exemplo 0)

![TXRAW](https://github.com/joelsernamoreno/EvilCrowRF-V2/blob/main/images/txraw.png)

## Exemplo de configuração de TX binário

* Módulo: (1 para o primeiro módulo CC1101, 2 para o segundo módulo CC1101)
* Modulação: (exemplo ASK/OOK)
* Transmissões: (número de transmissões)
* Frequência: (exemplo 433.92)
* Dados Binários: (dados binários exibidos no RX Log)
* Sample Pulse: (amostras/símbolo exibido no RX Log)
* Desvio: (exemplo 0)

![TXBINARY](https://github.com/joelsernamoreno/EvilCrowRF-V2/blob/main/images/txbinary.png)

## Configuração de Botões

* Botão: (1 para o primeiro botão, 2 para o segundo botão)
* Modulação: (exemplo ASK/OOK)
* Transmissões: (número de transmissões)
* Frequência: (exemplo 433.92)
* RAW Data: (dados brutos ou dados brutos corrigidos exibidos no RX Log)
* Desvio: (exemplo 0)

![TXBUTTON](https://github.com/joelsernamoreno/EvilCrowRF-V2/blob/main/images/pushbutton.png)

## Tesla Charge Door Opener

Demonstração: https://www.youtube.com/watch?v=feNokjfEGgs

## Exemplo de análise de URH

Demonstração: https://youtube.com/watch?v=TAgtaAnLL6U

## Atualização OTA

Demonstração: https://www.youtube.com/watch?v=YQFNLyHu42A

## Configuração WiFi

O Evil Crow RF V2 é configurado no modo AP com um SSID e senha padrão. Você pode alterar o modo para STATION ou AP, alterar o SSID, alterar a senha e alterar o canal Wi-Fi remotamente a partir do painel da web.

As alterações serão armazenadas no dispositivo, toda vez que você reiniciar o Evil Crow RF V2, as novas configurações de Wi-Fi serão aplicadas. Se quiser retornar às configurações padrão, você pode excluir a configuração de Wi-Fi armazenada no painel da web.

**NOTA:** Ao alterar a configuração do Wi-Fi, você deve preencher todos os campos corretamente, caso contrário, você bloqueou o dispositivo.

## Gerenciamento de energia

1. No modo normal, pressione push2 + reset e solte reset: Evil Crow RF v2 pisca várias vezes e vai dormir.
2. No modo de hibernação, pressione push2 + reset e solte reset para acordá-lo.

Demonstração: https://www.youtube.com/shorts/K_Qkss6-pEY

**NOTA:** Se o Evil Crow RF v2 estiver dormindo e você acidentalmente pressionar reset, ele voltará a dormir. Se ele não estiver dormindo e você pressionar reset, ele também ficará acordado.

## Outros esboços

* Mousejacking: EvilCrowRF-V2/firmware/other/standalone-mousejacking
* ...

# Suporte Evil Crow RF V2

* Você pode perguntar no grupo do Discord: https://discord.gg/jECPUtdrnW
* Você pode abrir a issue ou me enviar uma mensagem via twitter (@JoelSernaMoreno).
