Microsoft Sculpt Ergonomic Desktop keyboard firmware
======================
Ergonomic keyboard by Microsoft modified to work with Teensy 2.0.

## Microsoft Sculpt Ergonomic Desktop keyboard resources
- [Microsoft Sculpt Ergonomic Desktop support page](https://www.microsoft.com/hardware/en-us/d/sculpt-ergonomic-desktop)
- [Photos of completed wired mod](https://photos.app.goo.gl/KOjkhbifV5bxhWOU2)

## Build
Move to this directory and run `make`:

    $ make

## Matrix
Only keyboard with US/UK layout was traced. The US matrix ribbon cable had the following writing:

SN5822 US 852-41441-00A V0.4 GODA-3 130601

The US/UK controller PCB had the following writing:

SK3360 V1.1 2013/03/18

812-01311-00A

E157925

KB-6160

### Original US/UK layout keyboard matrix
Original US layout keboard matrix is 8 rows and 18 columns with UK extra keys in () brackets:

          A     B     C     D     E     F     G     H     I
       ----------------------------------------------------
    1        PAUS         DEL     0     9           8  BSPC  -->
    2        PGUP         F12  LBRC  MINS        RBRC   INS  -->
    3        HOME        CALC     P     O           I        -->
    4        SLCK         ENT  SCLN     L           K  BSLS  -->
    5                     APP  SLSH  QUOT  RALT        LEFT  -->
    6        END   RSFT  PGDN (NUHS)  DOT        COMM        --> 
    7  LCTL  RGHT          UP  DOWN                    RSPC  -->
    8        PSCR         F11   EQL    F9          F8   F10  -->

                          J     K     L     M     N     O     P     Q     R
                          -------------------------------------------------
                  -->     7   TAB     Q     2     1                          1
                  -->     Y    F5    F3     W     4          F6              2
                  -->     U     R     E  CAPS     3           T              3
                  -->     J     F     D (NUBS)    A        LGUI              4
                  -->     H     G    F4     S   ESC              LALT        5
                  -->     M     V     C     X     Z  LSFT                    6
                  -->     N     B  LSPC                                RCTL  7
                  -->    F7     5    F2    F1   GRV           6              8

Note that original matrix requires 8+18=26 pins on micro-controller and will
not work verbatim on Teensy 2.0 because it only has 25 pins.

### Original US/UK layout flex cable pinout

There are 30 pins on the US layout Sculpt flex cable. Pin 1 is the outermost
pin (closest to the rounded corner with the writing) and lines up with the
pin marked "1" underneath a triangle on the US controller PCB.

| Pin | Matrix Connection        |
| --- | ------------------------ |
| 1   | Fn Switch                |
| 2   | row 1                    |
| 3   | row 2                    |
| 4   | row 3                    |
| 5   | row 4                    |
| 6   | row 5                    |
| 7   | row 6                    |
| 8   | row 7                    |
| 9   | row 8                    |
| 10  | col A                    |
| 11  | col B                    |
| 12  | col C                    |
| 13  | col D                    |
| 14  | col E                    |
| 15  | col F                    |
| 16  | col G                    |
| 17  | col H                    |
| 18  | col I                    |
| 19  | col J                    |
| 20  | col K                    |
| 21  | col L                    |
| 22  | col M                    |
| 23  | col N                    |
| 24  | col O                    |
| 25  | col P                    |
| 26  | col Q                    |
| 27  | col R                    |
| 28  | n/a (Q2 LED)             |
| 29  | n/a (Q1 LED)             |
| 30  | GND (LEDs and fn switch) |

### Modified US/UK layout keyboard matrix

To make all keys work on Teensy 2.0 it is best to merge keys on column C and G
of orininal matrix (see above). To achieve that simply connect colums C and G
to the same pin on Teensy.

Modified matrix will have 8 rows and 17 columns and will require 25 pins:

          A     B     C     D     E     F     G     H
       ----------------------------------------------
    1        PAUS         DEL     0     9     8  BSPC  -->
    2        PGUP         F12  LBRC  MINS  RBRC   INS  -->
    3        HOME        CALC     P     O     I        -->
    4        SLCK         ENT  SCLN     L     K  BSLS  -->
    5              RALT   APP  SLSH  QUOT        LEFT  -->
    6        END   RSFT  PGDN (NUHS)  DOT  COMM        --> 
    7  LCTL  RGHT          UP  DOWN              RSPC  -->
    8        PSCR         F11   EQL    F9    F8   F10  -->

                          I     J     K     L     M     N     O     P     Q
                          -------------------------------------------------
                  -->     7   TAB     Q     2     1                          1
                  -->     Y    F5    F3     W     4          F6              2
                  -->     U     R     E  CAPS     3           T              3
                  -->     J     F     D (NUBS)    A        LGUI              4
                  -->     H     G    F4     S   ESC              LALT        5
                  -->     M     V     C     X     Z  LSFT                    6
                  -->     N     B  LSPC                                RCTL  7
                  -->    F7     5    F2    F1   GRV           6              8

Note that RALT and RSFT are now sitting on the same column (C).


## Keymap

### 1  Original Sculpt keymap
[keymap.c](keymap.c) represents original Sculpt layout

    Fn + {F1, F2, F3} = {play/pause, mute, volume down, volume up}
    Fn + {left, down, up, right}  = {home, pgdown, pgup, end}

#### 1.0 Default layer US
    ,---------------------------.    ,-----------------------------------------------.
    |Esc| F1| F2| F3| F4| F5| F6|    | F7| F8| F9|F10|F11|F12|PrSc|ScLk|Pau|Calc|    |
    |---------------------------|    |-----------------------------------------------|
    |  `|  1|  2|  3|  4|  5|  6|    |    7|  8|  9|  0|  -|  =|      Bcksp| Del|Home|
    |---------------------------|    |-------------------------------------|    |----|
    |  Tab|  Q|  W|  E|  R|    T|    |     Y|  U|  I|  O|  P|  [|  ]|     \|    | End|
    |---------------------------|    |-----------------------------------------------|
    |  Caps|  A|  S|  D|  F|   G|    |      H|  J|  K|  L|  ;|  '|    Enter| Ins|PgUp|
    |---------------------------|    |-----------------------------------------------|
    |  Shift|  Z|  X|  C|  V|  B|    |       N|  M|  ,|  .|  /|       Shift|  Up|PgDn|
    |---------------------------`----'-----------------------------------------------|
    |   Ctrl| Gui| Alt|            |            |  Alt|  Fn|      Ctrl|Left|Down|Righ|
    `--------------------------------------------------------------------------------'
    
    
 #### 1.1 Default layer UK
    ,---------------------------.    ,-----------------------------------------------.
    |Esc| F1| F2| F3| F4| F5| F6|    | F7| F8| F9|F10|F11|F12|PrSc|ScLk|Pau|Calc|    |
    |---------------------------|    |-----------------------------------------------|
    |  `|  1|  2|  3|  4|  5|  6|    |    7|  8|  9|  0|  -|  =|      Bcksp| Del|Home|
    |---------------------------|    |-------------------------------------|    |----|
    |  Tab|  Q|  W|  E|  R|    T|    |     Y|  U|  I|  O|  P|  [|  ]| Enter|    | End|
    |---------------------------|    |-------------------------------      |---------|
    |  Caps|  A|  S|  D|  F|   G|    |      H|  J|  K|  L|  ;|  '|  #|     | Ins|PgUp|
    |---------------------------|    |-----------------------------------------------|
    |Shft| \|  Z|  X|  C|  V|  B|    |       N|  M|  ,|  .|  /|       Shift|  Up|PgDn|
    |---------------------------`----'-----------------------------------------------|
    |   Ctrl| Gui| Alt|            |            |  Alt|  Fn|      Ctrl|Left|Down|Righ|
    `--------------------------------------------------------------------------------'
