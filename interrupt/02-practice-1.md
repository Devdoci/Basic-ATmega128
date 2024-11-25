# 3. 실습

이번엔 인터럽트를 사용해서 LED를 반복해서 토글시키는 프로그램을 작성해볼 것이다.

CPU 클럭과 인터럽트를 사용하기 위해서 필요한 라이브러리들을 적용해준다. 또한 LED의 상태를 저장하기 위해서 is_toggle 변수를 정의해준다.

```c
#define F_CPU 16000000UL // 16MHz 클럭 설정
#include <avr/io.h> // I/O 기본 라이브러리
#include <avr/interrupt.h> // 인터럽트 라이브러리

volatile unsigned int is_toggle = 0; // LED 상태 저장
```

PORTA의 LED를 출력시키기 위해서 DDRA를 0xff로 설정하고, SREG를 0x80으로 설정해서 전체 인터럽트를 허용한다.
그 다음에 1번 인터럽트를 하강 에지로 사용하기 위해서 EICRA와 EIMSK를 설정해준다.

```c
DDRA = 0xff; // A포트 출력으로 설정
SREG |= 0x80; // 전체 인터럽트 허용
EIMSK |= 0x02; // 0b00000010, 1번 인터럽트 허용
EICRA |= 0x08; // 0b00001000, 1번 인터럽트 하강 에지 사용
```

LED 출력을 위한 반복문도 작성해준다.

```c
while(1)
{
    PORTA = is_toggle; // LED 상태 출력
}
```

마지막으로 1번 외부 인터럽트가 실행되었을 때 is_toggle을 반전시키는 코드를 작성해준다.

```c
ISR(INT1_vect)
{
    if(is_toggle == 0) is_toggle = 0xff; // 꺼져있는 경우 Set
    else is_toggle = 0; // 켜져있는 경우 Clear
}
```

## 전체 코드

```c
#define F_CPU 16000000UL

#include <avr/io.h>
#include <avr/interrupt.h>

volatile unsigned int is_toggle = 0;

int main(){
    DDRA = 0xff;
    SREG |= 0x80;
    EIMSK |= 0x02;
    EICRA |= 0x08;

    while(1){
        PORTA = is_toggle;
    }
}

ISR(INT1_vect){
    if(is_toggle == 0) is_toggle = 0xff;
    else is_toggle = 0;
}
```
