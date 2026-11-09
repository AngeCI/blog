---
date: "2026-11-07T20:12:06+00:00"
type: "post"
title: "Conversion algorithm from Wylie Transliteration to Tibetan Alphabet"
description: ""
categories:
  - "Project Lilac 丁香項目"
tags:
  - "Tibetan 藏語"
---

> [!WARNING]
> This algorithm is **not** complete yet. The validity of the program output has not been fully verified.

1. Convert digraphs to monographs.
1. Process all the force stackings.
1. If there exists at least one vowel letter `aeiou`, split the input into onset and codas first.
    1. If a syllable has a structure of `CCa`, put an extra `འ` at the end of the syllable.
1. If there exists no vowel letters:
    1. If there exist a `w`, this `w` would be the last letter of the prefix.
    1. Insert `a` between the two consonants when the whole syllable has exactly two consonants.
    1. Insert `a` between the first and second consonant when the whole syllable has four consonants.
    1. When there are three consonant in the whole syllable:
        1. Insert `a` between the first and second consonant, if the second and third consonant is one of the following combinations: `[gŋbm]s|[nrl]d`.
        1. Otherwise, insert `a` between the second and third consonant.
1. If `w` doesn’t come at the beginning of a syllable or a force non-stacking flag, convert it to a subscript form.
1. Process subscript letters.
1. Process superscript letters.
1. Process suffix letters.
1. If there’s nothing before a vowel letter `aeiou`, put an `ཨ` at the beginning of the output.
1. Convert vowel letters `eiou` into their respective symbols.
1. Delete any left over `a`’s.
1. Remove force non-stacking flags.

```js
String.prototype.batchReplace = function (table) {
  const keys = Object.keys(table).sort((a, b) => b.length - a.length);
  const regex = new RegExp(keys.join('|'), 'g');
  return this.replace(regex, match => table[match]);
};

let wylieToTibetan = function (wylie) {
  const consonantTable = {
    "k": 0xf40,
    "x": 0xf41,
    "g": 0xf42,
    "ŋ": 0xf44,
    "c": 0xf45,
    "ʨ": 0xf46,
    "j": 0xf47,
    "ɲ": 0xf49,
    "t": 0xf4f,
    "θ": 0xf50,
    "d": 0xf51,
    "n": 0xf53,
    "p": 0xf54,
    "ɸ": 0xf55,
    "b": 0xf56,
    "m": 0xf58,
    "ʦ": 0xf59,
    "ʩ": 0xf5a,
    "ʣ": 0xf5b,
    "w": 0xf5d,
    "ʑ": 0xf5e,
    "z": 0xf5f,
    "'": 0xf60,
    "v": 0xf60,
    "y": 0xf61,
    "r": 0xf62,
    "l": 0xf63,
    "ɕ": 0xf64,
    "s": 0xf66,
    "h": 0xf67,
  };
  const vowelTable = {
    "ai": "ཻ",
    "au": "ཽ",
    "i": "ི",
    "u": "ུ",
    "e": "ེ",
    "o": "ོ",
  };

  // Convert digraphs
  const digraphTable = {
    "kh": "x",
    "ng": "ŋ",
    "ch": "ʨ",
    "ny": "ɲ",
    "th": "θ",
    "ph": "ɸ",
    "ts": "ʦ",
    "tsh": "ʩ",
    "dz": "ʣ",
    "zh": "ʑ",
    "sh": "ɕ"
  };
  /*
  for (let digraph in digraphTable) {
    wylie = wylie.replaceAll(digraph, digraphTable[digraph]);
  };
  */
  wylie = wylie.batchReplace(digraphTable);

  // Process force stackings
  /*
  for (let stack of (wylie.match(/[\w']\+[\w']/g) || [])) {
    wylie = wylie.replace(stack, String.fromCharCode(consonantTable[stack[0]], consonantTable[stack[2]] + 0x50));
  };
  */
  wylie = wylie.replace(/[a-zA-Z'\u0f40-\u0fbf]+(?:\+[a-zA-Z'\u0f40-\u0fbf])+/g, (match) => {
    const parts = match.split('+');
    let result = "";
    parts.forEach((part, idx) => {
      if (consonantTable[part]) {
        const code = consonantTable[part] + (idx === 0 ? 0 : 0x50);
        result += String.fromCharCode(code);
      } else {
        result += part;
      };
    });
    return result;
  });

  let output = [];
  let syls = wylie.split(/\s+/g);

  for (let syl of syls) {
    if (!syl) continue;

    let prefix, vowel, suffix;
    const hasVowel = /[aeiou]/.test(syl);

    if (hasVowel) {
      // Split prefixes and suffixes
      [prefix, vowel, suffix] = syl.split(/([aeiou])/g);
    } else {
      vowel = "";

      if (syl.length === 2) {
        prefix = syl[0];
        suffix = syl[1];
      } else if (syl.length === 4) {
        prefix = Array.from(syl).slice(0, 2).join("");
        suffix = Array.from(syl).slice(2).join("");
      } else if (syl.length === 3) {
        if (/[gŋbm]s|[nrl]d/.test(syl.slice(1, 3))) {
          prefix = syl[0];
          suffix = Array.from(syl).slice(1).join("");
        } else {
          prefix = Array.from(syl).slice(0, 2).join("");
          suffix = syl[2];
        };
      } else {
        
      };
    };

    for (let match of (prefix.match(/[rls]\w[wyrl]/g) || [])) {
      syl = syl.replace(match, String.fromCharCode(consonantTable[match[0]], consonantTable[match[1]] + 0x50, consonantTable[match[2]] + 0x50));
    };

    // Process subscript
    for (let match of (prefix.match(/\w[wyrl]/g) || [])) {
      syl = syl.replace(match, String.fromCharCode(consonantTable[match[0]], consonantTable[match[1]] + 0x50));
    };

    // Process superscript
    for (let match of (prefix.match(/[rls]\w/g) || [])) {
      syl = syl.replace(match, String.fromCharCode(consonantTable[match[0]], consonantTable[match[1]] + 0x50));
    };

    // Process prefix and suffix
    for (let consonant in consonantTable) {
      syl = syl.replace(consonant, String.fromCharCode(consonantTable[consonant]));
    };
    // syl = syl.batchReplace(consonantTable);

    // Add vowel-initial filler
    if (/^[aeiou]/.test(syl)) {
      syl = "ཨ" + syl;
    };

    // Process vowels
    /*
    for (let vowel in vowelTable) {
      syl = syl.replace(vowel, vowelTable[vowel]);
    };
    */
    syl = syl.batchReplace(vowelTable);

    // Remove force non-stacking flags
    syl = syl.replaceAll(/[a\.]/g, "");

    output.push(syl);
  };

  return output.join("་");
};
```
