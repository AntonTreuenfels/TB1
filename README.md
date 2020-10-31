TB1 – A 6502 Implementation of Tiny BASIC

by Anton Treuenfels
October 2020

TB1 is a lightly extended version of Tiny BASIC based on the orignal design described by Dennis Allison.

Extensions and Modifications

- line numbers may range from 1 to 63999
- case of keywords is not significant
- the LET keyword is optional in assignment statements
- the INPUT statement accepts an optional literal string followed by a semi-colon as a prompt
 - - in immediate mode INPUT accepts consecutive variable names and expressions,
 - - - eg., after INPUT a,1000,b,a+1000,c,b
 - - - a = 1000, b = 2000, c = 2000
- in program mode INPUT accepts only one or more variable names. It prompts for expressions to assign values to them. Multiple expressions separated by commas can be entered in response to one prompt to assign values to more than one variable at a time. If there are fewer variables than expressions, the excess is preserved for the next INPUT statement. If there are fewer expressions than variables, a new prompt is issued for each as-yet unassigned variable.
- the PRINT statement is enhanced
a PRINT statement by itself with no arguments generates a carriage return
 - - a semi-colon separating string literal or numeric expression arguments does not change the print position 
 - - a semi-colon or colon at the end of a list of arguments suppresses a carriage return
- the THEN keyword in IF..THEN statements is not optional
 - a REM statement has been added to mark a line as non-executable (a REMark)
 - the CLEAR statement is replaced by NEW
 - the END statement is optional if it would otherwise be the last line of a program
 - GOSUB nesting depth is limited to 32
 - there are no built-in functions
 - starting program execution by any of RUN, GOTO or GOSUB clears the user input buffer
 - ending progam execution either normally or by error stop clears the user input buffer
 - a modulus operator (“%”) has been added to produce the remainder of an integer division
 - error messages attempt to be slightly more specific about the cause
 - errors occuring during program execution list the line that caused the problem

Implementation

Functionally TB1 uses a two-level interpreter in the spirt of Tom Pittman's versions of Tiny BASIC (more of his comments here and some OCR scanned Tiny BASIC programs here). However TB1 uses a much less elaborate encoding scheme for the opcodes of the inner interpreter.

TB1 is meant to be executed by 6502-family processors. Although it is supposed to be fairly portable to 6502-family processors in general, no effort is made in regard to portability to other processor families .

Additionally, no consideration is given to portability of the source code. It is meant to be assembled by the HXA assembler, which has some features that make writing and modifying TB1 easier than using assemblers without those features.

One feature in particular, segmentation of the source code, allows the separation of the logical arrangement of the source code from the physical arrangement of the object code. This makes it possible, for instance, for HXA to assign token values to the “opcodes” of the inner interpreter automatically. What they actually are is usually of no real consequence to the operation of the inner interpreter, thus there is no need for the programmer to know what they are or to manage all their details (the only exceptions are for the CALL and JUMP opcodes. Even here the tweaking is only minor in order to give them a range of -255..+255, enough to reach every part of the inner interpreter).

The source code provided produces a 16K ROM image meant to be used with the Symon memory map of Seth Morabito's Symon NMOS 6502-simulator. Although TB1 itself is only a little over 2K of code, Symon proved to be a very convenient test bed during its development.

A main drawback, both in Symon and TB1 itself, is an inability to save or load a BASIC program. This  limits testing to short programs that can be easily typed in. This means testing of TB1 may not be as consistent or thorough as desirable.

It is assumed that in many if not most cases modifications will be made to the source code to produce a binary object that runs in RAM rather than ROM. The main details that need to be changed from one machine to another are the memory map (where TB1 itself resides and where any of the variables it manages are kept) and the hardware details of how single character input and output is accomplished. Making any changes should be fairly straightforward, as most important values are assigned to variables which are grouped together in the source code
