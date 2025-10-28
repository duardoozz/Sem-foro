# Projeto: Semáforo com LCD

Este é o meu projeto de **semáforo** usando Arduino.  
O sistema controla os LEDs **vermelho**, **amarelo** e **verde** e exibe o status no LCD.

## Funcionalidades
- Acende os LEDs de acordo com a sequência de um semáforo real.
- Mostra no LCD qual luz está acesa.
- Implementa ponteiros para controle dos pinos.

## Demonstração
Assista ao vídeo do projeto no Google Drive:

[▶️ Assistir vídeo](https://drive.google.com/file/d/1GsnsAoV50aGaHzbxbgoiei3MkEbsiEWj/view?usp=drive_link)


## Código do Arduino
```cpp
#include <LiquidCrystal_I2C.h>
#include <Wire.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

// Variáveis dos pinos
int red = 9;
int yellow = 8;
int green = 7;

// ponteiros para os pinos
int *pRed = &red;
int *pYellow = &yellow;
int *pGreen = &green;

void setup() {
  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("Semaforo Iniciado");

  // ponteiros no pinMode
  pinMode(*pRed, OUTPUT);
  pinMode(*pYellow, OUTPUT);
  pinMode(*pGreen, OUTPUT);

  delay(2000);
}

void loop() {
  // Vermelho 6s
  digitalWrite(*pRed, HIGH);
  digitalWrite(*pYellow, LOW);
  digitalWrite(*pGreen, LOW);
  contagem("Vermelho", 6);

  // Amarelo 2s
  digitalWrite(*pRed, LOW);
  digitalWrite(*pYellow, HIGH);
  digitalWrite(*pGreen, LOW);
  contagem("Amarelo", 2);

  // Verde 2s
  digitalWrite(*pRed, LOW);
  digitalWrite(*pYellow, LOW);
  digitalWrite(*pGreen, HIGH);
  contagem("Verde", 2);

  // Verde Extra +2s
  contagem("Verde Extra", 2);

  // Amarelo 2s
  digitalWrite(*pRed, LOW);
  digitalWrite(*pYellow, HIGH);
  digitalWrite(*pGreen, LOW);
  contagem("Amarelo", 2);
}

void contagem(const char* fase, int segundos) {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Fase: ");
  lcd.print(fase);
  for (int i = segundos; i > 0; i--) {
    lcd.setCursor(0, 1);
    lcd.print("Tempo: ");
    lcd.print(i);
    lcd.print("s  ");
    delay(1000);
  }
}