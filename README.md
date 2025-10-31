# Semaforo_Offline

## Introdução
&emsp;Neste projeto, foi desenvolvido um semáforo eletrônico para controle de tráfego de veículos e pedestres em uma via movimentada do bairro Butantã. O objetivo foi aplicar conhecimentos de eletrônica básica e programação em Arduino para criar um sistema que simule o comportamento de um semáforo real, com temporização adequada de cada fase: vermelho, verde e amarelo.

&emsp;A atividade envolveu a montagem física dos LEDs na protoboard, a programação das sequências de tempo de cada luz e a verificação do funcionamento correto do sistema.

---

### Parte 1: Montagem Física do Semáforo

Imagem 01: Montagem do semáforo na protoboard
![Montagem do semáforo](https://res.cloudinary.com/drog2ehgw/image/upload/v1761850897/semaforo_verde_b7nicg.jpg)  
*Fonte: Imagem autoral, 2025.*

### Materiais Utilizados

| Componente | Quantidade | Função |
|-------------|-------------|--------|
| **Arduino UNO** | 1 | Placa responsável por controlar o circuito do semáforo |
| **Protoboard (breadboard)** | 1 | Utilizada para montar o circuito sem necessidade de solda |
| **LED vermelho** | 2 | Representa o sinal de pare |
| **LED amarelo** | 1 | Representa o sinal de atenção |
| **LED verde** | 2 | Representa o sinal de siga |
| **Botão (push button)** | 1 | Simula o botão de pedestre para mudar o estado do semáforo |
| **Resistores** | 5 | Limitam a corrente dos LEDs, evitando que queimem |
| **Jumpers (fios de conexão)** | Vários | Fazem as conexões entre os pinos do Arduino e os componentes |
| **Cabo USB** | 1 | Alimenta e envia o código do computador para o Arduino |
| **Estrutura de madeira (mini poste de semáforo)** | 1 | Suporte físico para os LEDs do semáforo |
| **Notebook com Arduino IDE** | 1 | Usado para programar e monitorar o funcionamento do circuito |

---

### Passo a passo resumido de montagem

1. **Preparar o material:**  
   - Arduino, protoboard, LEDs (vermelho, amarelo, verde), LEDs extras (vermelho e verde), resistores, botão, fios de conexão.

2. **Montar o circuito base:**  
   - Posicione a protoboard sobre a mesa.  
   - Conecte o Arduino à protoboard via cabo USB.

3. **Conectar os LEDs principais:**  
   - LED vermelho ao pino 13  
   - LED amarelo ao pino 12  
   - LED verde ao pino 11  
   - Coloque um resistor em série com cada LED.  
   - Ligue os cátodos (perna menor) de todos os LEDs ao GND.
   - Coloque eles no molde do semáforo.

4. **Conectar os LEDs extras:**  
   - LED extra vermelho ao pino 9  
   - LED extra verde ao pino 10  
   - Coloque um resistor em série com cada LED.  
   - Conecte os cátodos ao GND.

5. **Conectar o botão:**  
   - Ligue um terminal do botão ao pino A2 e o outro ao GND.  
   - Configure no código usando `pinMode(botao, INPUT_PULLUP)`.

6. **Verificar conexões:**  
   - Confirme que o GND da protoboard está conectado ao GND do Arduino.  
   - Garanta que não há fios soltos ou LEDs invertidos.

7. **Enviar o código para o Arduino:**  
   - Abra o Arduino IDE, insira o código do semáforo e faça o upload.  
   - Teste o funcionamento:  
     - O semáforo deve alternar entre verde → amarelo → vermelho.  
     - O LED extra vermelho acende junto com o amarelo e verde.  
     - O LED extra verde acende junto com o vermelho principal.  
     - Ao apertar o botão, a sequência muda imediatamente para vermelho e o LED extra verde acende.

8. **Organizar os fios:**  
   - Use cores diferentes para facilitar identificação (ex: vermelho = energia, preto = GND, azul = sinal).  



### Parte 2: Programação e Lógica do Semáforo

&emsp;O semáforo foi programado seguindo a sequência:

1. **Vermelho:** 6 segundos  
2. **Verde:** 4 segundos  
3. **Amarelo:** 2 segundos  

&emsp;O ciclo se repete continuamente. Toda vez que o vermelho fica aceso, o verde para pedestres (que está na placa de protoboard) fica aceso também.

&emsp;Ao clicar no botão, o semáforo fica vermelho e o de pedestres fica verde, ambos por 5 segundos. Depois disso, o ciclo volta normalmente.

### Código Arduino utilizado:

```cpp

int vermelho = 13;
int amarelo = 12;
int verde = 11;
int botao = A2;
int ledExtra = 10;
int ledExtra2 = 9;

int* pVermelho = &vermelho;
int* pAmarelo = &amarelo;
int* pVerde = &verde;
int* pBotao = &botao;
int* pLedExtra = &ledExtra;
int* pLedExtra2 = &ledExtra2;

void setup() {
  pinMode(*pVerde, OUTPUT);
  pinMode(*pAmarelo, OUTPUT);
  pinMode(*pVermelho, OUTPUT);
  pinMode(*pLedExtra, OUTPUT);
  pinMode(*pLedExtra2, OUTPUT);
  pinMode(*pBotao, INPUT_PULLUP);
}

void loop() {
  // Verde
  digitalWrite(*pVerde, HIGH);
  digitalWrite(*pLedExtra2, HIGH);
  for (int i = 0; i < 40; i++) {  
    if (digitalRead(*pBotao) == LOW) {
      acionarVermelhoEmergencia();
      return;
    }
    delay(100);
  }
  digitalWrite(*pVerde, LOW);
  digitalWrite(*pLedExtra2, LOW);

  // Amarelo
  digitalWrite(*pAmarelo, HIGH);
  digitalWrite(*pLedExtra2, HIGH);
  for (int i = 0; i < 20; i++) {  
    if (digitalRead(*pBotao) == LOW) {
      acionarVermelhoEmergencia();
      return;
    }
    delay(100);
  }
  digitalWrite(*pAmarelo, LOW);
  digitalWrite(*pLedExtra2, LOW);

  // Vermelho
  digitalWrite(*pVermelho, HIGH);
  digitalWrite(*pLedExtra, HIGH);
  for (int i = 0; i < 40; i++) {  
    if (digitalRead(*pBotao) == LOW) {
      acionarVermelhoEmergencia();
      return;
    }
    delay(100);
  }
  digitalWrite(*pVermelho, LOW);
  digitalWrite(*pLedExtra, LOW);
}

void acionarVermelhoEmergencia() {
  digitalWrite(*pVerde, LOW);
  digitalWrite(*pAmarelo, LOW);
  digitalWrite(*pVermelho, LOW);
  digitalWrite(*pLedExtra2, LOW);
  digitalWrite(*pLedExtra, LOW);

  digitalWrite(*pVermelho, HIGH);
  digitalWrite(*pLedExtra, HIGH);
  delay(5000);

  digitalWrite(*pVermelho, LOW);
  digitalWrite(*pLedExtra, LOW);
}
```
### Funcionamento do código

&emsp;O programa controla os LEDs do semáforo, acendendo apenas a cor correspondente à fase atual (verde, amarelo ou vermelho). Cada fase permanece pelo tempo definido no `delay()`. O ciclo se repete continuamente, garantindo o funcionamento constante do semáforo.  

&emsp;Além disso, o botão permite alterar a sequência: ao ser pressionado, o semáforo muda imediatamente para o vermelho e acende o LED extra verde, simulando situações de prioridade ou parada de emergência.


### Parte 3: Avaliação de Pares
A atividade foi submetida à avaliação de pelo menos dois colegas, considerando:

- Organização da protoboard
- Clareza das conexões
- Funcionamento correto do semáforo
- Correção dos tempos das fases

#### Avaliações recebidas:

| Avaliador | Montagem física (4) | Pontos | Temporização (3) | Pontos | Código & estrutura (3) | Pontos | Observações gerais                                                         | Total (10) |
|-----------|----------------------|--------|-------------------|--------|------------------------|--------|----------------------------------------------------------------------------|------------|
| Luiz     | Contempla completamente | 4.0    | Contempla parcialmente         | 2.9   | Contempla               | 3    | Estrutura de jumpers bem organizados, bom funcionamento do botão para pedestres, código sem comentários. | 9.9        |
| Amanda Cristina Martinez da Rosa     | Contempla  parcialmente | 3.5    | contempla          | 3    | Contempla               | 3    |  Semáforo funcionou bem e conseguiu implementar um elemento a mais na montagem. Porém, senti que o contexto do elemento a mais, botãoe mais dois LEDs, poderia ser mais claro com o sentido de um semáforo | 9.5       |

### Conclusão

&emsp;A realização desta atividade permitiu integrar conhecimentos de eletrônica e programação, proporcionando uma compreensão prática do funcionamento de LEDs e resistores em um circuito controlado por Arduino.  

&emsp;Além disso, o experimento reforçou conceitos de lógica sequencial e demonstrou como automatizar sistemas simples, consolidando a aplicação prática de programação em dispositivos físicos.


### Vídeo Demonstração:
[(Vídeo do funcionamento do semáforo.)](https://youtu.be/bTZ7D_PirdQ?si=twu-Gi7-NBGxgIhW)