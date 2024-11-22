# Basic ATmega128

일반적으로 타이머는 시간을 정해두고, 시간이 되었을 때 알려주는 프로그램을 말한다.

타이머는 내부 클럭 (clk I/O)을 세는 장치로, MCU의 내부 클럭을 세어 일정시간 간격의 펄스를 만들어 내거나 일정시간 경과 후에 인터럽트를 발생시킨다. 카운터는 MCU 외부에서 입력되는 클럭을 세는 장치이다.

ATmega128은 8BIT Timer/Counter 0과 2, 16BIT Timer/Counter 1과 3, 총 4개의 Timer/Counter를 가지고 있다.

|종류|설명|
|---|---|
|타이머|펄스에 따라 주기적으로 발생하기 때문에 시간을 유추할 수 있다.|
|카운터|외부에서 입력되는 클럭에 따라 발생하기 때문에 시간을 유추하기 어렵다.|
