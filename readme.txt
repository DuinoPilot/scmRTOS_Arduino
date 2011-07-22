
scmRTOS for Arduino
===================

scmRTOS (http://scmrtos.sourceforge.net/ScmRTOS) is a lightweight RTOS
well suited to the AVR.  This library provides the core scmRTOS
functionality in an easy-to-use fashion for use with Arduino.

No modifications to the Arduino base are required.

Arduino functions that are interrupt-safe can be used safely in more
than one process.  This is a (partial) list of those functions:

	pinMode	   	(each pin should be used by only one process)
	digitalWrite	(each pin should be used by only one process)
	digitalRead	(each pin should be used by only one process)
	millis
	micros
	delay		(use OS::delay to give time to other processes)
	delayMicroseconds
	shiftIn		(pins must not be shared between processes)
	shiftOut	(pins must not be shared between processes)
	all Math functions
	all Trigonometry functions
	all Bits and Bytes functions
	noInterupts	(also stops process switching)
	interrupts	  	

The user is responsible for ensuring that other functions and
libraries called from different processes do not conflict; in general
it is safest to limit most Arduino library calls to a single process.

To use the library, include <scmRTOS.h>.  The library provides three
processes, which must be configured for stack size and the loop function
that they will call respectively.

Processes are configured with the scmRTOS_PROCESS() macro:

	scmRTOS_PROCESS(<process number>, <stack size>, <loop function>);

The available process numbers are 0, 1 and 2.  Process 0 has the
lowest priority, and process 2 the highest priority.  There is no
sharing of the CPU between processes; a lower priority process will
only run if the higher-priority process is blocked (e.g. in OS::delay).

The stack size should be sufficient for the process' needs, but bear
in mind that the sum of all stack sizes must fit in RAM along with
other variables.

The loop function behaves the same as the Arduino loop() function -
the process will call it repeatedly.

At the end of the Arduino setup() function, call

	scmRTOS_START();

This call will not return.

For more details, see the scmRTOS documentation.  Note, however, that 
this library is based on scmRTOS pre-v400, and the existing English 
documentation is rather out of date.
