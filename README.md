# Codul Master
Codul master controlează un joc în care jucătorii concurează reacționând la o culoare afișată printr-un sistem RGB.
Interacțiunea are loc printr-un afișaj LCD, un servomotor și o comunicare cu un dispozitiv slave folosind SPI (Serial Peripheral Interface).
Jocul urmărește scorurile jucătorilor, gestionează rundele și actualizează stările jocului pe afișaj.

## Componente utilizate
### Biblioteci Incluse

- LiquidCrystal.h: Pentru controlul afișajului LCD.
- Servo.h: Pentru controlul servomotorului.
- SPI.h: Pentru comunicarea SPI între master și dispozitivul slave.
- 
### Pini și Definiții

- BAUD_RATE: Viteza de transmisie serială (9600 baud).
- Pini pentru RGB: Definesc pini pentru controlul componentelor RGB.
- Pini pentru Butoane: Definesc pini pentru controlul butoanelor RGB.
- Definiri pentru Controlul Jocului: GAME_START, GAME_STOP, IGNORE pentru a controla starea jocului.
- Constante de Timp: SECOND, GAME_DURATION, ROND_DURATION pentru durata jocului și a rundelor.
- Definiri pentru Servomotor: END_ROTATION, START_ROTATION, ROTATION_DISTANCE controlează poziția servomotorului.

### Structuri și Variabile Globale

Structura Players stochează numele și punctele fiecărui jucător.
Variabilele globale gestionareaza stările jocului, punctajele, timpul, și comunicarea SPI.
