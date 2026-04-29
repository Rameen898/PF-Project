#include <avr/io.h>
#include <util/delay.h>

#define trigPin PB1   
#define echoPin PB0   
#define buzzPin PB2   

long duration;
int distance;

void setup() {
    DDRB |= (1 << trigPin);   
    DDRB &= ~(1 << echoPin);  
    DDRB |= (1 << buzzPin);   
}

long pulseIn() {
    long count = 0;
    
    while (!(PINB & (1 << echoPin)));

    while (PINB & (1 << echoPin)) {
        _delay_us(1);
        count++;
    }

    return count;
}

int main(void) {
    setup();

    while (1) {
      
        PORTB &= ~(1 << trigPin);
        _delay_us(2);

        PORTB |= (1 << trigPin);
        _delay_us(10);
        PORTB &= ~(1 << trigPin);

        duration = pulseIn();

        distance = duration * 0.034 / 2;

        if (distance < 30) {
            PORTB |= (1 << buzzPin);  
        } else {
            PORTB &= ~(1 << buzzPin);  
        }

        _delay_ms(100);
    }
}
