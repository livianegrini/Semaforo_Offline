# Semaforo_Offline

## Introdução
&emsp;Neste projeto, foi desenvolvido um semáforo eletrônico para controle de tráfego de veículos e pedestres em uma via movimentada do bairro Butantã. O objetivo foi aplicar conhecimentos de eletrônica básica e programação em Arduino para criar um sistema que simule o comportamento de um semáforo real, com temporização adequada de cada fase: vermelho, verde e amarelo.

&emsp;A atividade envolveu a montagem física dos LEDs na protoboard, a programação das sequências de tempo de cada luz e a verificação do funcionamento correto do sistema.

---

### Parte 1: Montagem Física do Semáforo

Imagem 01: Montagem do semáforo na protoboard
![Montagem do semáforo](https://res.cloudinary.com/drog2ehgw/image/upload/v1761850897/semaforo_verde_b7nicg.jpg)  
*Fonte: Imagem autoral, 2025.*


### Parte 2: Programação e Lógica do Semáforo

O semáforo foi programado seguindo a sequência:

1. **Vermelho:** 6 segundos  
2. **Verde:** 4 segundos  
3. **Amarelo:** 2 segundos  

O ciclo se repete continuamente.

Toda vez que o vermelho fica aceso, o verde para pedestres (que está na placa de protoboard) fica aceso também.

Ao clicar no botão, o semáforo fica vermelho e o de pedestres fica verde, ambos por 5 segundos. Depois disso, o ciclo volta normalmente.

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
### Funcionamento do código:

O programa alterna entre os LEDs, acendendo apenas a cor correspondente à fase atual do semáforo.

Cada fase é mantida pelo tempo definido no delay().

O ciclo se repete infinitamente, garantindo que o semáforo funcione de forma contínua.

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
A realização da atividade permitiu integrar conhecimentos de eletrônica e programação. Foi possível compreender o funcionamento básico de LEDs e resistores em um circuito controlado por Arduino.

O experimento também reforçou conceitos de lógica sequencial e de automatização de sistemas simples.

### Vídeo Demonstração:
[(O vídeo deve mostrar o funcionamento do semáforo.)](https://youtu.be/OeQ-svH8c24?si=wKR4JJZf6fzLiXFF)