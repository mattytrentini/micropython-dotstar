
MicroPython DotStar
===================

Higher level DotStar driver that presents the strip as a sequence. This is a port of the
`Adafruit CircuitPython DotStar library <https://github.com/adafruit/Adafruit_CircuitPython_DotStar>`_.
The primary change is to require an SPI object to be configured before creating the DotStar object. SPI
communciations are used to control the DotStars. 

.. note:: Be aware that SPI can be implemented using hardware support or
  purely in software. Hardware implementations allow higher rates of 
  transmission and can be desirable particularly for long strips of DotStars.
  SPI implementation is port-dependent so see your port-specific documentation
  for details. 

Colors are stored as tuples by default. However, you can also use int hex syntax
to set values similar to colors on the web. For example, ``0x100000`` (``#100000``
on the web) is equivalent to ``(0x10, 0, 0)``.

If you send a tuple with 4 values, you can control the brightness value, which appears in DotStar but not NeoPixels.
It should be a float. For example, (0xFF,0,0, 1.0) is the brightest red possible, (1,0,0,0.01) is the dimmest red possible.

.. note:: The int hex API represents the brightness of the white pixel when
  present by setting the RGB channels to identical values. For example, full
  white is 0xffffff but is actually (0xff, 0xff, 0xff) in the tuple syntax. 

.. note:: DotStar refers to an Adafruit product based on an electronic part
  known as APA102. 'Dotstar' and 'APA102' can be used interchangeably.

Usage Example
=============

This example demonstrates the library with a single DotStar connected to Pins 12 and 13. This 
matches the configuration of the `TinyPICO <http://tinpico.com>`_. On this platform (ESP32-based)
this is using software SPI.

.. code-block:: python

    from micropython_dotstar import DotStar
    from machine import SPI, Pin

    spi = SPI(sck=Pin(12), mosi=Pin(13), miso=Pin(18)) # Configure SPI - see note below
    dotstar = DotStar(spi, 1) # Just one DotStar
    dotstar[0] = (128, 0, 0) # Red
    dotstar[0] = (128, 0, 0, 0.5) # Red, half brightness
    dotstar.fill((0,0,128)) # Blue

.. note:: 'miso=Pin(18)' is only required due to a quirk in the current software SPI
  implementation where all of sck, mosi and miso must be set. It's expected that this
  limitation will be lifted in the future. In this example Pin(18) is simply unused since
  DotStars don't send data back to the master.

Raspberry Pi Pico Example
=========================

This example demonstrates the library with a single DotStar connected to SCK Pin 10 and Tx Pin 11 on the
`Raspberry Pi Pico <https://www.raspberrypi.com/documentation/microcontrollers/raspberry-pi-pico>`_.


.. code-block:: python

    from micropython_dotstar import DotStar
    from machine import SPI

    spi = SPI(id=1, sck=10, mosi=11) # Configure SPI
    dotstar = DotStar(spi, 1)        # Setup one DotStar

    dotstar[0] = (128, 0, 0)         # Red
    dotstar[0] = (128, 0, 0, 0.5)    # Red, half brightness
    dotstar.fill((0,0,128))          # Blue

