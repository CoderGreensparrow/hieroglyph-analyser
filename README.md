# hieroglyph-analyser
Hieroglyph romanizer and analyser tool.
One of the most important functions: automatic "aesthetic" arrangement of Egyptian words into quadrats, based on what the historical scribes have written. (If I can find that out.)

## Specification and implementation details
Two-way conversion between raw hieroglyphs encoded in Unicode, and potentially multiple representations (or a mixed representation) of them in latin letters (like Gardiner or MdC, or a mix of the two).
One of the transliterations used will definitely be a JSesh-compatible, extended MdC-like representation.

The program will also include an "Aesthetic word arranger" tool, which will help with arranging a list of hieroglyphics, which should form a word, into "aesthetically pleasing" quadrats, based on a set of "aesthetic rules" (which should probably be derived from the ancient aesthetic rules of scribes, if those exist).

### General overview of implementation (computer science oriented text)

#### Where does the app live? (software stack, localization)

The program is mainly meant to be a lightweight webapp that can run on even simpler machines. It should run completely locally.

Localization: the program should support localization through some simple method (like json files for menu text). The translations menu options, buttons, info messages should always be consistent and should use correct egyptological vocabulary.
Currently planned localizations for the UI:
- English first, this is the base language
- Hungarian second
- German third
- French fourth (once I find someone that can translate it to French)
In places where using the French or German terms would be the convention (like saying "Manual de Codage" instead of "Coding Manual"), those should be used *always*.

#### Transliteration
A greedy matching algorithm forms the base of the project. It steps through the characters in the input string and matches as many characters forwards as it can, based on an ordered key-value map. If a match is found in the keys, it gets replaced with the related value. Characters already matched are skipped over in the input string, essentially splitting up the input string into blocks of matches. The exact entries in the key-value map should be in ordered by the keys' lengths in decreasing order, because that way the algorithm can simply step through the ordered key-value map from the start and the "greediness" emerges from the ordering of the key-value map.

The key-value map may be subdivided into submaps, which may contain different ways to romanize/encode hieroglphys. Gardiner's sign list's codes are one way to encode hieroglphys, and Manuel de Codage (MdC) can be another way. (The two could be used in the same text box, or in separate text boxes.)

However, after giving it some thought, I arrived at the conclusion that using JSesh-compatible transliterations would be the best. This should provide the best accessibility for all academics studying Egyptology.
However, since this program has extra functions that JSesh doesn't have (the only one being the Aesthetic word arranger), we need to extend the functionality of JSesh's transliteration. (JSesh itself is based on MdC (Manuel de Codage), but it also "propose\[s\] a number of extensions" (from the [JSesh documentation](https://jseshdoc.qenherkhopeshef.org/en/jsesh/introduction)), so this app also has to support those.)

#### Aesthetic quadrat arranger for words
Also called the Aesthetic word arranger for short.

Unanswered problem: What were the aesthetic rules employed by egyptian scribes?
Once we solve that problem, we can start implementing the following algorithm:

Encode the aesthetic rules, by which we can grade individual arrangements of words' hieroglyphs into quadrats. Then simply look through all possible options for ways to arrange characters. The one with the highest score is the winner.
Admittedly, this is a naïve implementation of mathematical optimization. This could probably be improved in a lot of ways once we actually know the exact rules used by scribes.

(Future idea: Since this is an optimization problem, a simple machine learning algorithm could also be employed, given we have enough hieroglyph arrangements of different words as data.)

#### Database of Egyptian words and grammar

It is currently NOT in the scope of this project that the software should have an internal list of words with transliterations and translations, and is also NOT meant to be a natural language processor, so no grammatical analysis.
However, the project should be built in a way where adding new tools is possible.

However, since the job of Aesthistic arrangement of hieroglyphs within words may warrant the creation of a list of "Conventional Arrangements" (arrangements of the hieroglyphs of certain words into quadrats which became kind of fixed throughout centuries of use, sometimes with different conventions for different historical eras and kingdoms). Hence it may turn out that the program *will eventually* contain a list of possible Egyptian words with correct conventional arrangement.
But the ultimate goal of the project is to create an Aesthetic arranger that has rules made in such a way that such a list of Convential Arrangements is not needed at all.*

*On that note however: it can be true that scribes from different time periods, from different kingdoms had different defitions of "aesthetically arranging hieroglyphs", and this could be reflected in the program as *multiple seperate rulesets for different eras*, if such a requirement is made by the egyptologist community.

### How to use the software (aka. exact software specification; easier read)

#### Transliterating egyptian characters

TODO

### Exact implementation (computer science oriented text)

#### Rendering egyptian characters

Unless I use some external tool for rendering, I have to build one myself. There are a few ideas:
- HTML with flexboxes for quadrats
- Using \<canvas\>
  - svg, png, jpeg rendering (or any other format)
- Unicode text with Egyptian Hieroglyph Format Controls*
- And the non-rendering option: JSesh-style MdC transliteration (this should be directly pastable into JSesh and should work)

*As the [eglyf](https://github.com/kbinani/eglyf) project writes: "Egyptian Hieroglyph Format Controls (U+13430–U+1343F) are Unicode characters that specify the layout of hieroglyphic text. While these controls are defined in the Unicode standard, they typically require specialized software to be interpreted correctly."
Nevertheless, since some fonts that support them *do* exist (the linked project tries to make the fonts that don't support them actually start supporting them), one of the rendering options will be simply UTF-8 text with format control characters.