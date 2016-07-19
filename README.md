Avalanche
=========

[![Build Status](https://api.travis-ci.org/MozillaSecurity/avalanche.svg)](https://travis-ci.org/MozillaSecurity/avalanche)

Avalanche is a document generator which uses context-free grammars to generate
randomized outputs for fuzz-testing.


Syntax Cheatsheet
-----------------

```
# BinSymbol
SymName        x"41414141"      # generate b"AAAA" in the output (cannot be used with TextSymbol)

# TextSymbol
SymName         "text"          # generate u"text" in the output (cannot be used with BinSymbol)

# ChoiceSymbol (must be named, no inline form)
SymName   1     Defn1           # choose between generating Defn1 (1:3 odds)
          2     Defn2           #                        or Defn2 (2:3 odds)

SymName2  +     SymName         # '+' imports choices & weights from SymName into SymName2
          1     Defn3           # ie. choices are Defn1 (1:4), Defn2 (2:4) or Defn3 (1:4)

# ConcatSymbol
SymName         SubSym1 SubSym2             # concat, generate SubSym1 then SubSym2
SymName         SubSym1 ( SubSym2 SubSym3 ) # inline concat (grouping), SubSym2 & SubSym3 can be
                                            # repeated using RepeatSymbol

# RepeatSymbol
SymName         SubSym{a,b}     # repeat SubSym a random number of times, between a and b
SymName         SubSym{a}       # repeat SubSym 'a' times
SymName         SubSym?         # shorthand for {0,1}
# RepeatSampleSymbol
SymName         SubSym<a,b>     # repeatedly generate SubSym between [a,b] unique choices
                                # SubSym must be a choice or a concat with exactly
                                # one choice followed or preceded by text/bin

# RegexSymbol
SymName         /[A-Za-z]{0,4}..?[^a-f]{2}/  # simple regex generator, generate from A-Za-z [0,4]
                                             # times, '.' generates printable ASCII. [^] inverts
                                             # characters. ? generates [0,1] instances, etc.

# RefSymbol
SymName         @SymName2       # returns a previously generated instance of SymName2

# FuncSymbol
SymName         rndint(a,b)     # rndint, rndflt, rndpow2 are built-in,
                                # others can be passed as keyword args to the Grammar constructor
                                # args can be numeric literals, or symbol definitions

# Import
Blah            import('another.gmr')    # can use imported symnames like Blah.SymName
```


Quickstart
----------

Define your grammar in a text file (UTF-8 encoding). 'root' is the default start symbol.

```
with open('my.gmr') as fd:
    g = Grammar(fd)
result = g.generate()
```


About the name
--------------

Avalanche is French for "Avalanche".

