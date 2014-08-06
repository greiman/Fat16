Read changes.txt if you have used previous releases of this library.

Please read the html documentation for this library.  Start with
html/index.html and read the Main Page.  Next go to the Files tab and
read the documentation for Fat16Config.h.  Finally go to the Classes
tab and read the Fat16 class documentation.

If you wish to report bugs or have comments, send email to
fat16lib@sbcglobal.net

Arduinos access SD cards using the cards SPI protocol.  PCs, Macs, and
most consumer devices use the 4-bit parallel SD protocol.  A card that
functions well on A PC or Mac may not work well on the Arduino.

Most cards have good SPI read performance but cards vary widely in SPI
write performance.  Write performance is limited by how efficiently the
card manages internal erase/remapping operations.  The Arduino cannot
optimize writes to reduce erase operations because of its limited RAM.

SanDisk cards generally have good write performance.  They seem to have
more internal RAM buffering than other cards and therefore can limit
the number of flash erase operations that the Arduino forces due to its
limited RAM.

Some Dane-Elec cards have a write speed that is only 20% as fast as
a good SanDisk card.


The hardware interface to the SD card should not use a resistor based
level shifter.  Fat16 sets the SPI bus frequency to 8 MHz which results
in signal rise times that are too slow for the edge detectors in many
newer SD card controllers when resistor voltage dividers are used.

The 5 to 3.3 V level shifter for 5 V arduinos should be IC based like
the  74HC4050N based circuit shown in the file SdLevel.png.  The
Adafruit Wave Shield uses a 74AHC125N.  Gravitech sells SD and MicroSD
Card Adapters based on the 74LCX245.


To install the library copy the Fat16 directory to the
libraries subdirectory of the Arduino user directory. On Windows 7
this is <User Name>\Documents\Arduino\libraries. On earlier versions of
the Arduino IDE use the hardware/libraries subdirectory of the
Arduino IDE.

For the AtMega168 be sure to reduce the serial buffer size by setting 
RX_BUFFER_SIZE to 32 or less in
hardware/cores/arduino/HardwareSerial.cpp.  I use 16.

Fat16 assumes chip select for the SD card is the hardware SS pin.  On a
168/328 Arduino this is pin 10 and on a Mega this is pin 53.  If you are
using another pin for chip select you will need call
SdCard::init(speed, chipSelectPin) with second parameter set to the
chip select pin.

If you have a shield like the SparkFun shield that uses pin 8 for chip
select you would change the line:
  card.init();
to
  card.init(0, 8);
in the SdFat examples.

You can also edit SdCard.h and change the default value for chip select.
Replace SPI_SS_PIN with the new value for chip select in the following
definition.

uint8_t const SD_CHIP_SELECT_PIN = SPI_SS_PIN;

For example to set SD_CHIP_SELECT_PIN to 8 for the SparkFun microSD shield:
uint8_t const SD_CHIP_SELECT_PIN = 8;

The best way to restore an SD card's format is to use SDFormatter
which can be downloaded from:

http://www.sdcard.org/consumers/formatter/

SDFormatter aligns flash erase boundaries with file
system structures which reduces write latency and file system overhead.

SDFormatter does not have an option for FAT type so it may format
small cards as FAT12.


The Fat16/examples directory has the following sketches.  They are my
debug sketches.

fat16AnalogLogger.pde - A simple data logger for one or more analog
                        pins.

fat16append.pde - This sketch creates a large file by successive
                  open/write/close operations using O_APPEND.

fat16bench.pde - A read/write benchmark.

fat16copy.pde - Copy the file created by fat16append.pde to the file
                ACOPY.TXT.

fat16GPS_CSVSensorLogger.pde - Ladyada's GPS logger modified to use
                               the Fat16 library.

fat16GPSLogger_v3.pde - Ladyada's GPS logger modified to use the Fat16
                        library.

fat16info.pde  - This Sketch attempts to initialize an SD card and
                 analyze its format.  Used for debug problems with
                 SD cards.

fat16ls.pde - A test of the ls() file list function.

fat16print.pde - This sketch shows how to use the Arduino Print class
                 with Fat16.

fat16read.pde  - This sketch reads and prints the file PRINT00.TXT
                 created by fat16print.pde or WRITE00.TXT created by
                 fat16write.pde.

fat16remove.pde - This sketch shows how to use remove() to delete the
                  file created by the fat16append.pde example.

fat16rewrite.pde - This sketch shows how to rewrite part of a line in
                   the middle of the file created by the
                   fat16append.pde example.

fat16tail.pde  - This sketch reads and prints the tail of all files
                 created by fat16append.pde, fat16print.pde and 
                 fat16write.pde.

fat16timestamp.pde - This sketch shows how to set file access, create,
                     and write/modify timestamps.

fat16truncate.pde - This sketch shows how to use truncate() to remove
                    the last half of the file created by the
                    fat16append.pde example.

fat16write.pde - This sketch creates a new file and writes 100 lines
                 to the file.

To access these examples from the Arduino development environment go to:
File -> Examples -> Fat16 -> <Sketch Name>

Compile, upload to your Arduino and click on Serial Monitor to run the
example.

You must use a standard SD card that has been formatted with a FAT16
file system.

Updated 2009-11-28
