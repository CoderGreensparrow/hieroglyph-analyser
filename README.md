# hieroglyph-analyser
Hieroglyph romanizer and analyser tool.

## Specifications
Two-way conversion between raw hieroglyphs encoded in Unicode, and potentially multiple representations (or a mixed representation) of them in latin letters (like Gardiner or MdC, or a mix of the two).

## Implementation
A greedy matching algorithm forms the base of the project. It steps through the characters in the input string and matches as many characters forwards as it can, based on an ordered key-value map. If a match is found in the keys, it gets replaced with the related value. Characters already matched are skipped over in the input string, essentially splitting up the input string into blocks of matches. The exact entries in the key-value map should be in ordered by the keys' lengths in decreasing order, because that way the algorithm can simply step through the ordered key-value map from the start and the "greediness" emerges from the ordering of the key-value map.

The key-value map may be subdivided into submaps, which may contain different ways to romanize/encode hieroglphys. Gardiner's sign list's codes are one way to encode hieroglphys, and Manuel de Codage (MdC) can be another way. (The two could be used in the same text box, or in separate text boxes.)
