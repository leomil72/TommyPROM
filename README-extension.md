# Extending the design to new chip families

## Hardware
There are currently two hardware flavors - one for 28C series EEPROMs and one specifically for the Intel 8255A UV EPROM. The 8755A chip uses a multiplexed set of address and data lines that are directly driven by the Arduino. Most other chips will not use this addressing method. The 8755A design aslso includes a circuit to switch the programming voltage under software control.  This may be useful for other chips that use non-5V programming voltages, although many of these chips, like the 27 series EPROMS, have a programming voltage that remains for the entire write and verify cycle. For those chips, it may be easier to simply switch the programming voltage manually.

The basic hardware design, used for the 28C family. is much more adaptable to additional chip families. This design uses two shift registers to create 16 dedicated address lines from only 3 arduino pins. This design, plus manual switching of the program voltage, would be very adaptable to EPROMs like the 2716, 2764, 27040, and 272001. The hardware has already been used with these chips for read-only operations.

# Software
The software design is modular, allowing easy extenstion for chips with different programming algoritms.  A new class can be added for each new chip family. This class will include code for byte reads, byte writes, and optional block writes, if the chip supports it.  All of the chip-specific code will be in this single class.

The basic steps to support a new chip are as follows:
* Create or copy an existing PromDeviceXX class to create a new .cpp and .h file for the chip-specific code.
* Define the pin assignments and implement the chip-specific setAddress, readByte, and burnByte code in the new files. The setAddress code can call the PromAddressDriver code if the shift register address hardware is used.
* If supported, also add chip-specific burnBlock code.
* Be sure that the new files have unique #if defined code that matches the class name.
* Edit Configure.h to add the conditional code includes the new PromDevice subclass.
* Edit TommyPROM.ino to add the conditional code that declares the new device.

Note that after files are added or renamed in the TommyPROM directory, the project needs to be reopened in Arduino IDE to get the changes.
