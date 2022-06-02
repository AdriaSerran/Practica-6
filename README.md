# Practica-6 LECTURA DE ETIQUETA RFID 

En aquesta part de la pràctica la intenció és fer que es detecti quan es passa la targeta pel sensor.

# Material 

- Targeta
- Sensor
- ESP32

# Funcionament

Primer de tot i com de costum s'afageixen els includes necessaris juntament amb els defines.

```c++
#include <SPI.h>
#include <MFRC522.h>

#define RST_PIN 17 //Pin 17 para el reset del RC522
#define SS_PIN 5 //Pin 5 para el SS (SDA) del RC522
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522
```

Al setup inicialitzem el port sèrie i també inicialitzem el Bus SPI i el MFRC522.

```c++
void setup() {
Serial.begin(115200); //Iniciamos la comunicación serial
SPI.begin(); //Iniciamos el Bus SPI
mfrc522.PCD_Init(); // Iniciamos el MFRC522
Serial.println("Lectura del UID");
}
```


Començem el loop i primer comprovem si hi ha una nova targeta present. Seguidament seleccionem la nova tarfeta i s'envia serialment al seu uid.
I seguidament acabem la lectura de la targeta actual.

```c++
void loop() {
// Revisamos si hay nuevas tarjetas presentes
if ( mfrc522.PICC_IsNewCardPresent())
{
//Seleccionamos una tarjeta
if ( mfrc522.PICC_ReadCardSerial())
{
// Enviamos serialemente su UID
Serial.print("Card UID:");
for (byte i = 0; i < mfrc522.uid.size; i++) {
Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
Serial.print(mfrc522.uid.uidByte[i], HEX);
}
Serial.println();
// Terminamos la lectura de la tarjeta actual
mfrc522.PICC_HaltA();
}
}
}
```
