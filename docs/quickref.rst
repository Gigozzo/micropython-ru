.. _quickref:

Краткое описание микроконтроллера pyboard
=========================================

.. image:: http://micropython.org/resources/pybv10-pinout.jpg
    :alt: PYBv1.0 pinout
    :width: 700px

Основные команды микроконтроллера
---------------------------------

Подробнее :mod:`pyb`. ::

    import pyb

    pyb.delay(50) # ожидание 50 миллисекунд
    pyb.millis() # количество миллисекунд с момента запуска
    pyb.repl_uart(pyb.UART(1, 9600)) # duplicate REPL on UART(1)
    pyb.wfi() # pause CPU, waiting for interrupt
    pyb.freq() # получить частоты центрального процессора и магистрали
    pyb.freq(60000000) # установить частоту центрального процессора 60MHz
    pyb.stop() # остановка центрального процессора, ожидание внешнего прерывания

Светодиоды (LEDs)
-----------------

Подробнее :ref:`pyb.LED <pyb.LED>`. ::

    from pyb import LED

    led = LED(1) # красный светодиод
    led.toggle()
    led.on()
    led.off()

Пины и интерфейсы ввода/вывода (GPIO)
-------------------------------------

Подробнее :ref:`pyb.Pin <pyb.Pin>`. ::

    from pyb import Pin

    p_out = Pin('X1', Pin.OUT_PP)
    p_out.high()
    p_out.low()

    p_in = Pin('X2', Pin.IN, Pin.PULL_UP)
    p_in.value() # получить значение, 0 или 1

Внешние прерывания
------------------

Подробнее :ref:`pyb.ExtInt <pyb.ExtInt>`. ::

    from pyb import Pin, ExtInt

    callback = lambda e: print("intr")
    ext = ExtInt(Pin('Y1'), ExtInt.IRQ_RISING, Pin.PULL_NONE, callback)

Таймеры
-------

Подробнее :ref:`pyb.Timer <pyb.Timer>`. ::

    from pyb import Timer

    tim = Timer(1, freq=1000)
    tim.counter() # получить значение счётчика
    tim.freq(0.5) # 0.5 Hz
    tim.callback(lambda t: pyb.LED(1).toggle())

Широтно-импульсная модуляция (PWM)
----------------------------------

Подробнее :ref:`pyb.Pin <pyb.Pin>` и :ref:`pyb.Timer <pyb.Timer>`. ::

    from pyb import Pin, Timer

    p = Pin('X1') # X1 это TIM2, CH1
    tim = Timer(2, freq=1000)
    ch = tim.channel(1, Timer.PWM, pin=p)
    ch.pulse_width_percent(50)

Конвертация аналогового в цифровой (ADC)
----------------------------------------

Подробнее :ref:`pyb.Pin <pyb.Pin>` и :ref:`pyb.ADC <pyb.ADC>`. ::

    from pyb import Pin, ADC

    adc = ADC(Pin('X19'))
    adc.read() # прочитать значение, 0-4095

Конвертация цифрового в аналоговый (DAC)
----------------------------------------

Подробнее :ref:`pyb.Pin <pyb.Pin>` и :ref:`pyb.DAC <pyb.DAC>`. ::

    from pyb import Pin, DAC

    dac = DAC(Pin('X5'))
    dac.write(120) # вывод от 0 до 255

Универсальный асинхронный приёмопередатчик (UART)
-------------------------------------------------

Подробнее :ref:`pyb.UART <pyb.UART>`. ::

    from pyb import UART

    uart = UART(1, 9600)
    uart.write('hello')
    uart.read(5) # читать 5 байт

Интерфейс системного программированния (SPI)
--------------------------------------------

Подробнее :ref:`pyb.SPI <pyb.SPI>`. ::

    from pyb import SPI

    spi = SPI(1, SPI.MASTER, baudrate=200000, polarity=1, phase=0)
    spi.send('hello')
    spi.recv(5) # получить 5 байт из шины
    spi.send_recv('hello') # send a receive 5 bytes

Интерфейсная шина IIC (I2C)
---------------------------

Подробнее :ref:`pyb.I2C <pyb.I2C>`. ::

    from pyb import I2C

    i2c = I2C(1, I2C.MASTER, baudrate=100000)
    i2c.scan() # возвращает список ведомых адресов
    i2c.send('hello', 0x42) # отправить 5 байт для ведомого устройства с адресом 0x42
    i2c.recv(5, 0x42) # получить 5 байт от ведомого устройства
    i2c.mem_read(2, 0x42, 0x10) # прочитать 2 байта от ведомого устройства 0x42, ведомого устройства памяти 0x10
    i2c.mem_write('xy', 0x42, 0x10) # написать 2 байта ведомому устройству 0x42, 0x10 ведомого устройства памяти
