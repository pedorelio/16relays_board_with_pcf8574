# 16relays_board_with_pcf8574
Projeto para implementar o uso do PCF8574 com a placa de reles de 16 canais.

Este projeto visa implementar o uso de um expansor de de entradas para uma placa de relés com 16 relés. 
A placa usada neste projeto usa um ESP8266, que é bem limitado em GPIOs, tornando assim impossível de adicionar interruptores físicos na mesma quantidade dos relés que tem na placa.
Este projeto foi feito no ESPHome, pois o uso com o tasmota é mais complicado de se implementar.

A placa usada neste projeto é essa da imagem abaixo.

<img width="1322" height="723" alt="Screenshot_20260425_092323_Mercado Libre" src="https://github.com/user-attachments/assets/c8540181-e66d-4fca-ae98-c923c1103625" />

Este modelo de placa tem relés com 5V, 12V e 24V. A que eu usei neste projeto foi a com 5V.

Para a expansão das entradas, utilizei módulos PCF8574, iguais ao da figura abaixo.

<img width="1386" height="1466" alt="Screenshot_20260425_092604_Mercado Libre" src="https://github.com/user-attachments/assets/12c27d73-ba61-4e74-8071-c4a5bd5eec61" />

Optei por usar a placa com relés de 5V por conta da fonte de alimentação. Fazendo isso, posso usar a mesma fonte de alimentação tanto para a placa de relés, quanto para o módulo PCF8574, já que os dois podem sem alimentados com 5V.

A placa de relés utiliza dois registradores de deslocamento do tipo 74HC595. Esses registradores estão associados a algumas GPIOs do ESP8266, e encontrei um problema quando precisei utilizar a comunicação I2C por limitação de GPIOs disponíveis no ESP. 
Esse registrador tem um pino chamado OE. Este pino tem que ficar sempre ligado ao GND, ele é o responsável por habilitar as saídas do registrador. Caso ele não esteja em nível baixo, não conseguimos acionar os relés.
Normalmente esse pino OE vai ligado ao GPIO5, porém para a ligação do barramento I2C se utiliza por padrão as GPIOs 4 e 5 para o SDA e o SCL.
A maneira que encontrei para contornar esse problema foi fazendo uma alteração física na placa.

Segundo o datasheet do CI 595, o pino referente ao OE é o pino 13.

<img width="1317" height="1644" alt="Screenshot_20260425_093424_Chrome" src="https://github.com/user-attachments/assets/68bbe949-4e85-4446-95bb-7f37ec7bd36d" />

O que fiz foi dessoldar esse pino da placa e conectá-lo diretamente ao GND do ESP.

<img width="4128" height="3096" alt="20260425_093733" src="https://github.com/user-attachments/assets/fec436cd-82dc-48f6-a7d0-c635c2707e03" />

Dessa forma libero o pino OE e posso usar o I2C sem problemas.

Os códigos utilizados estão neste repositório. 

A programação foi feita pensando em utilizar interruptores físicos para acionar os relés. Tudo foi feito visando utilização em uma automação residencial com um HUB de automação centralizado.

