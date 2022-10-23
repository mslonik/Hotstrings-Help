# GUI (Graphical User Interface) in Hotstrigns application.

## Why GUIs?

To bring user experience over configuration and edition of d(t, o, h).

## What GUIs do we have?

| short name | long name | description | function(s) |
| :---:      | ---       | ---         | :---        |
| HS3        | main (**default**)      | One, to rule them all.    | Full functionality.   |
| HS4        | left      | Subset of main GUI (HS3): left part of main GUI.  | Edition of d(t, o, h) in one window. Vertically fits to Laptop (*) screen. Full menus. | 
| HS2        | right     | Subset of main GUI (HS3): right part of main GUI. | 1. Future. 2. Utilize full vertical capacity of display to enable learning of definitions from within particular library. Full menus.|

**default**: visible one GUI is called for the very first time.

## How default GUI is organized?

Main components of the default GUI:
```
+---------------------------------------------------------+                
|                      TOP MENU                           |                
+---------------------------------------------------------+                
+-----------------++---++---------------------------------+                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   ||       TABLE (ListView)          |                
|                 ||   ||                                 |                
| CONSTANTS       ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 || SW||                                 |                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   |+---------------------------------+                
|                 ||   |+---------------------------------+                
|                 ||   || SANDBOX LABEL |                 |                
|                 ||   |+---------------------------------+                
|                 ||   |+---------------------------------+                
|                 ||   ||       SANDBOX                   |                
|                 ||   ||                                 |                
+-----------------++---++---------------------------------+                
```
- HS4: left part of the screen: CONSTANTS.
- HS2: right part of the screen: TABLE.
- SB:  belongs to all GUIs (Switching Button).
&nbsp;

&nbsp;

&nbsp;


## Scalability

- HS3:
   - TABLE component is scalable by width and height.
   - SANDBOX component is scalable by width.
- HS4: **Not** scalable by width and height.
- HS2: scalable by width and height.

&nbsp;

&nbsp;

&nbsp;


## Menus

- Top menu.
- Context menu - available only for TABLE component.
&nbsp;

&nbsp;

&nbsp;

## Feature: floating components
Floating: based on screen size are moved automatically to utilize height of the screen.
- SANDBOX LABEL + SANDBOX

or
- SANDBOX LABEL
Flag: ini_SandboxMoved. Values: true (moved to the left part of window) or false (not moved to the left part of window).

Remark: SANDBOX LABEL is always visible to keep it visible anytime.
&nbsp;

&nbsp;

&nbsp;

## Feature: toggle components
Toggle: make component visible or invisible (hidden).
- SANDBOX
Flag: ini_Sandbox. Values: true (visible) or false (hidden).
&nbsp;

&nbsp;

&nbsp;


## Features of floating and switchability are combined.
&nbsp;

&nbsp;

&nbsp;

## Events
- window scaling (by mouse)
- toggle visibility of SANDBOX (by keyboard)

On time of window scaling **floating components** float automatically in order to keep as much free space on left or right screen as possible.

&nbsp;

&nbsp;

&nbsp;

## How it works
Couple of words about software development:

What do we need to know to display Gui window initially?
* X, Y, W, H <- read from ini file,
* if SANDBOX is visible, if SANDBOX is shifted to the left <- read from ini file.

| function | →     | description | event source |
| :---     | :---: | :---        | :---:        |
| initial drawing | → | response to user event "show / restore GUI" | keyboard / mouse |
| redrawing1 | → | response to user event "change GUI size" | mouse |
| redrawing2 | → | response to user event "toggle component" | keyboard |

| function | variable(s) | description |
| :---     | :---:       | :---        |
| initial drawing | ini_Sandbox | read |
| | ini_SandboxAtLeft | read |
| redrawing1 | ini_Sandbox | read |
| | ini_SandboxAtLeft | write |
| redrawing2 | ini_Sandbox | write |
| | ini_SandboxAtLeft | write |

redrawing1: HS3GuiSize(GuiHwnd, EventInfo, Width, Height)

Initial concept was to separate each function. Actually it should be separated, but at different layers:
- separate control from actual drawing,
- "atomic" drawing functions, for each out of just **4** components separately (SW, TABLE, SANDBOX LABEL, SANDBOX),
- name all functions according to any scheme.

Example:
| code | comment |
| ---  | ---     |
| ```if (condition)``` | control |
| ```{ F_Redrawing_XYZ(a, b) }``` | drawing |
| ```else``` | control |
| ```{ F_Redrawing_ABC(d, e) }``` | drawing |
| | |
| ```F_Redrawing_XYZ(a, b)``` | |
| ```{ F_Atomic1() F_Atomic2() … F_Atomicn() }``` | atomic function |

Initial sequence:

1. ```Gui,	% ini_WhichGui . ": Show", % "X" . ini_HS3WindoPos.X . A_Space . "Y" . ini_HS3WindoPos.Y . A_Space . "W" . ini_HS3WindoPos.W . A_Space . "H" . ini_HS3WindoPos.H```
2. Gui Show initiates **GuiSize**.
3. **GuiSize** already "knows" GUI size thanks to arguments "X" and "Y" from Gui call.
&nbsp;

&nbsp;

&nbsp;


## Examples

### HS3, SANDBOX is off
Remarks: 
- SANDBOX is off (hidden),
- default window width and height (not changed),
- TABLE automatically scalled down,
- SANDBOX LABEL automatically moved down.
```
+---------------------------------------------------------+                
|                      TOP MENU                           |                
+---------------------------------------------------------+                
+-----------------++---++---------------------------------+                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   ||       TABLE (ListView)          |                
|                 ||   ||                                 |                
| CONSTANTS       ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 || SW||                                 |                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   ||                                 |                
|                 ||   |+---------------------------------+                
|                 ||   |+---------------------------------+                
|                 ||   || SANDBOX LABEL |                 |
+-----------------++---++---------------------------------+                
```

### HS3, changed window width
Remarks:
- default window width (not changed),
- TABLE width is changed, adjusted to new window width.
```
+-----------------------------------------------------------------+                
|                      TOP MENU                                   |                
+-----------------------------------------------------------------+                
+-----------------++---++-----------------------------------------+                
|                 ||   ||                                         |                
|                 ||   ||                                         |                
|                 ||   ||                                         |                
|                 ||   ||                                         |                
|                 ||   ||       TABLE (ListView)                  |                
|                 ||   ||                                         |                
| CONSTANTS       ||   ||                                         |                
|                 ||   ||                                         |                
|                 ||   ||                                         |                
|                 || SW||                                         |                
|                 ||   ||                                         |                
|                 ||   ||                                         |                
|                 ||   ||                                         |                
|                 ||   ||                                         |                
|                 ||   ||                                         |                
|                 ||   ||                                         |                
|                 ||   |+-----------------------------------------+                
|                 ||   |+-----------------------------------------+                
|                 ||   || SANDBOX LABEL |                         |
+-----------------++---++-----------------------------------------+                
```

### HS3, changed window height