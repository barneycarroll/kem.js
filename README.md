kem.js
======

Keyboard Event Manager aims to provide a powerful keyboard input abstraction layer for JavaScript.

# Rationale

Abstracting keyboard input from native events in JS is a nightmare: one [Jan Wolter](http://unixpapa.com) has done the world a service in [detailing a good number of the most basic low level problems](http://unixpapa.com/js/key.html); [PPK](htt://quirksmode.org)'s ever-invaluable [compatibility tables](http://www.quirksmode.org/js/keys.html) give us a quick statistical glance at what we can and can't do.

Various small libraries (todo: provide examples) exist that attempt to provide a unified, coherent interface for keyboard events, but desirable high level problems exclusive to the world of web are often left unaddressed: what about key combinations, and sequences? What about user configuration? Something more is needed.

## Aims

KEM aims to provide a human-readable and -writable end-to-end interface layer that addresses the problems on both ends:

* Built-in awareness and responsible mitigation of user-agent deficiencies & ambiguity
* Several optional degrees of advanced capture methods to compensate for native keyboard event blindspots
* An expressive, RegExp-like symbolic language for matching keyboard input that takes account of
    * Combinations and sequences
    * Throttling, deduplication, etc
    * Interchangeable capture groups
    * Dynamically-resolved configurable labels for keys, groups, sequences

## Use case scenarios

### Simple example: Keyboard-controlled directional movement

WASD is awesome, right? Tell that to AZERTY keyboard users. Using KEM's abstraction layer, your program only has to hook up to directional keywords. A decoupled module would allow user-configuration of input methods, optionally enhanced with presets for generic international keyboards from KEM's library.

### General problem: Modifier keys

`KEM.on('Ctrl + click')`, anyone? It's a common enough paradigm for context menus. What about `'Alt + click'` to cycle through the options of a menu-associated button? But `Alt` often acts as a trigger, too: you don't want keyboard input to trigger the default behaviour while it's active. And conventions with desktop apps dictates that `Alt` can be held down, to put its effect into action until it's released, or toggled with a quick keypress. That's something you should be able to turn on or off and configure without dealing with native I/O. As with previous examples, you probably want the user to be able to re-bind these depending on preference. So maybe `'Mod1 + click'`, where `Mod1` is `Ctrl` by default, but configurable. And, as ever, this is absolutely not something you want tightly coupled to your application's internal event handling. KEM should sit as far or as near as you want it: simply offer a cleaner syntax for handling keystrokes, use a custom taxonomy for abstracts, or provide savvy advanced capture methods and an interface for total user control of input-function bindings.

### Intractable problems resolved: Keyboard-accessible, feature-rich web apps

You shouldn't have to choose between user- and feature-centric interface design philosophies. Keyboard accessibility has often gone down the pan as software developers make the hard business choice of quickly developping something that's awesome for most of their target market and unusable by some, or sinking collosal design & development budget into an attempt to make it awesome for everyone. Google have more resources than most to throw at industry-leading web apps, yet GMail's keyboard shortcuts menu isn't keyboard accessible itself. Even if you do have a mouse, those shortcuts are lacking and arbitrary. What if they conflict with the user's accessibility pre-sets? What if they'd rather put in easier-to-remember keys, or combinations of keys, for their favourite functions? KEM wants to make this possible.

# Components / sub-projects

## KEM keyboard-input syntax

Anywhere you find someone typing out what a user should be hitting on their keyboard, whether in a harcopy manual, GUI config menu or power user forum, you'll find a fairly generic convention: `Ctrl + S`, `Ctrl + Alt + S`. The KEM KIS sub-project should allow for this kind of string-based event abstraction at the minimum.

### Abstractions: labels, groups, sequences

For a richer level of abstraction, KEM's KIS should allow RegExp-like expressions. `Ctrl + S|O`, `Fire + Jump`, `Shift + (\w+)` (with a returned match for event handling). This isn't half as straightforward, and involves some creative thought. But some kind of implementation of this features would make for incredibly powerful user-input handling a doddle.

## Software & cultural polyfill features

A library of modular components to dissapear cultural and software bug issues: hidden textareas (or similar) to pick up inputs that don't trigger native keyboard events, or produce ambiguous ones; helpful dictionaries of common keyboard analogies like WASD with regional variations to aid common i18n issues.

## Internal tests and lookups

When the user sets `Alt + Ctrl + Shit + C` as their desired trigger for function X, they should probably be told that this will override a previous binding for function Y, and be given the opportunity to override that. Or should they? Venturing into a philosophy that compares to the KEM KIS ideal as jQuery UI does to jQuery, a higher-level input management interface for keyboard input intensive rich applications that need to magic away a highly customisable UI.
