# T/C0 실습 - Normal 모드

이번엔 8Bit T/C0을 사용해서 TCNT0의 값이 128에 도달하면 LED를 켜고, 오버플로우되면 LED를 끄는 프로그램을 작성해볼 것이다.

먼저 CPU 클럭과 인터럽트를 필요하는데 필요한 라이브러리를 적용해줄 것이다.

```c
#define F_CPU 16000000UL // 16MHz 클럭 설정
#include <avr/io.h> // I/O 기본 라이브러리
#include <avr/interrupt.h> // 인터럽트 라이브러리
```

A포트 출력을 위해 DDRA를 0xff로 설정하고, SREG를 0x80으로 설정해서 전체 인터럽트를 허용해준다.

```c
SREG = 0x80;
DDRA = 0xff;
```

그 다음으로, T/C0을 Normal 모드로 동작하도록 설정하고, 또한 비교를 위한 OCR0과 타이머 인터럽트 등을 추가로 설정해준다.

```c
TCCR0 = 0b00001001; // 분주를 사용하지 않고, Normal 모드를 사용
TIMSK = 0b00000011; // T/C0의 오버플로우, 비교 일치 인터럽트를 사용
TCNT0 = 0; // 0부터 시작 (Optional)
OCR0 = 128; // T/C0의 비교 값을 128로 설정
```

OCR0과 비교해서 값이 같을 때 LED를 켜는 인터럽트를 추가해준다.

```c
ISR(TIMER0_COMP_vect)
{
    PORTA = 0xff;
}
```

오버플로우 되었을 때의 인터럽트를 추가해준다.

```c
ISR(TIMER0_OVF_vect)
{
    PORTA = 0;
}
```

## 전체 코드

```c
#define F_CPU 16000000UL
#include <avr/io.h>
#include <avr/interrupt.h>

int main()
{
    SREG = 0x80;
    DDRA = 0xff;

    TCCR0 = 0b00001001;
    TIMSK = 0b00000011;
    TCNT0 = 0;
    OCR0 = 128;

    while(1);
}

ISR(TIMER0_COMP_vect)
{
    PORTA = 0xff;
}

ISR(TIMER0_OVF_vect)
{
    PORTA = 0;
}
```
