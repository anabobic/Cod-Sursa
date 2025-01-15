# Cod-Sursa
Aplicație: Măsurarea Temperaturii şi a Umidității Relative
#include <LiquidCrystal.h>
//includerea în program a bibliotecii comenzilor pentru LCD

LiquidCrystal lcd(7, 6, 5, 4, 3, 2);
//inițializarea bibliotecii și a variabilei lcd cu numerele pinilor utilizați de

shield-ul LCD
byte grad[8] = {
//definirea variabilei grad de tip byte, ca fiind o matrice cu 8 rânduri ce au
valorile din acolade

 B01000,
 B10100,
 B01000,
 B00011,
 B00100,
 B00100,
 B00011,
 B00000
};
const int portTemperatura = 0;
//definirea variabilei portTemperatura corespunzătoare portului analogic

A0 unde va fi conectat pinul OUT al senzorului de temperatură
const int portUmiditate = 1;
//definirea variabilei portUmiditate corespunzătoare portului analogic A1

unde va fi conectat pinul OUT al senzorului de umiditate
float temp = 0.0;
//definirea variabilei pentru temperatură

int umid = 0.0;
//definirea variabilei pentru umiditate

float val_dig_temp = 0.0;
//definirea variabilei val_dig_temp ce va avea valoarea digitală

corespunzătoare temperaturii citite
int val_dig_umid = 0.0;
//definirea variabilei val_dig_umid ce va avea valoarea digitală

corespunzătoare umidității citite
float U_temp = 0.0;
//definirea variabilei pentru tensiunea analogică furnizată de traductorul

de temperatură
float ICT = 0.0;
//definirea variabilei pentru ICT (indicele de confort termic)

float Vcc = 5.0;
//definirea variabilei pentru tensiunea Vcc ce va avea valoarea inițială 5V

void setup(){
 lcd.begin(16, 2);
//inițializarea interfeței cu ecranul LCD și specificarea numărului de

rânduri și de coloane al acestuia
 lcd.createChar(1, grad);
//crearea caracterului personalizat ce va avea conținutul matricei grad și
alocarea poziției 1

}
void loop(){
 for (int i=0;i<500;i++) {
 val_dig_temp = val_dig_temp + analogRead(portTemperatura);
//citirea valorii digitale corespunzătoare temperaturii de 500 de ori și
însumarea tuturor valorilor

 val_dig_umid = val_dig_umid + analogRead(portUmiditate);
//citirea valorii digitale corespunzătoare umidității de 500 de ori și
însumarea tuturor valorilor

 delay(1);
//întârziere 1 milisecundă

 }
 val_dig_temp = val_dig_temp/500;
//calculul valorii digitale medii a temperaturii

 val_dig_umid = val_dig_umid/500;
//calculul valorii digitale medii a umidității

 U_temp = (val_dig_temp * Vcc)/1023;
//calculul tensiunii analogice echivalentă valorii digitale citite – formula

(2)
 temp = (U_temp - 0.5)/0.01;
//calculul temperaturii – formula (3)

 switch (val_dig_umid) {
//determinarea valorii umidității în funcție de valoarea digitală citită

 case 0 ... 1: umid = 95; break;
 case 2: umid = 90; break;
 case 3 ... 4: umid = 85; break;
 case 5 ... 7: umid = 80; break;
 case 8 ... 11: umid = 75; break;
 case 12 ... 19: umid = 70; break;
 case 20 ... 32: umid = 65; break;
 case 33 ... 52: umid = 60; break;
 case 53 ... 92: umid = 55; break;
 case 93 ... 148: umid = 50; break;
 case 149 ... 261: umid = 45; break;
 case 262 ... 387: umid = 40; break;
 case 388 ... 559: umid = 35; break;
 case 560 ... 721: umid = 30; break;
 case 722 ... 852: umid = 25; break;
 case 853 ... 1023: umid = 20; break;
 }
ICT = (temp * 1.8 + 32) - (0.55 - 0.0055 * umid)*((temp * 1.8 + 32) - 58);
//calculul Indicelui de Confort Termic – formula (1)

lcd.clear();
//ștergere conținut ecran LCD și poziționare cursor în colțul din stânga

sus
lcd.print("t=");
//afișează pe ecranul LCD textul dintre ghilimele

lcd.print(temp,1);
//afișează pe ecranul LCD valoarea variabilei temp, cu o zecimală după

virgulă
lcd.write(1);
//afișează pe ecranul LCD caracterul personalizat având poziția 1

lcd.print(" h=");
//afișează pe ecranul LCD textul dintre ghilimele

lcd.print(umid);
//afișează pe ecranul LCD valoarea variabilei umid

lcd.print("%");
//afișează pe ecranul LCD textul dintre ghilimele

lcd.setCursor(0, 1);
//mutare cursor pe coloana 1, rândul 2

lcd.print("ICT=");
//afișează pe ecranul LCD textul dintre ghilimele

lcd.print(ICT,1);
//afișează pe ecranul LCD valoarea variabilei ICT, cu o zecimală după

virgulă
} 
