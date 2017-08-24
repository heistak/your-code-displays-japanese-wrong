---
title: Your code displays Japanese wrong
---

### Why am I here?

If you were brought here from a link given to you, the person who gave you the link probably thinks your code displays Japanese wrong. This page will give you a brief description of the glyph appearance problems that frequently arise with naive code implementations of Asian text display, why it happens, why it’s a big deal, and how to fix it.

### OK, what’s wrong?

Kanji (sometimes called Hanzi, Hanja or Han characters, but let’s call it Kanji for now) is a set of logographic characters that originated in China but are now also used in several other countries, such as Japan, Korea, and Taiwan.

The Kanji glyph sets used in Simplified Chinese, Traditional Chinese, Japanese, and Korean each look mostly similar to each other, but has large numbers of letters that must look distinctly different. For instance, here are the Japanese, Simplified Chinese, and Traditional Chinese glyph variants of the letter ():

  

Therefore, if text in Japanese is displayed using a Kanji glyph set meant for Chinese, it will immediately stand out to a native Japanese speaker as ugly, non-native, and plain bizarre due to the unfamiliar glyphs showing up in the text. This is most likely what’s happening with your program.

### Why does that happen?

Back when Unicode was being designed, a decision called Han Unification was made to create a single unified set of all the Chinese (Simplified/Traditional), Japanese, and Korean Kanji characters. This involved giving equivalent code points to characters that were deemed equivalent across languages, which allowed the size of the character set to be kept small. 

However, this also meant that characters which differ in appearance across languages, such as _ and _ and _, were given identical code points! It is up to the font and the program displaying the text to render them using the correct glyph set. Which, if the developer is not aware and don’t do anything about it, frequently fails.

  

In many cases, the default fallback behavior in an ambiguous situation is to choose the Chinese glyph set. Therefore, Japanese text tends to be incorrectly displayed using Chinese glyphs.

### How can I check if it’s happening or not?

Here are some characters that are known to have different glyph appearances between different languages.

  

Try copy-pasting them into your code, see the rendered results, and compare them with below. If the glyphs don’t look like the Japanese result sample below, your code is displaying Japanese wrong.

### How can I fix it?

In a nutshell, the way to fix it is to make your code and font be aware that it’s displaying Japanese when it is doing so. 

#### HTML 

#### Unity (TextMesh Pro)

  

### Do similar problems occur with other languages?

Likely, but I haven’t gone into details since the author is a Japanese who only speaks English/Japanese and doesn’t have too much insight on other languages. If you can offer assistance in problems that happen with other languages please drop me a line.

### Why isn’t there steps to fix it in INSERT_ENVIRONMENT_HERE?

### More Resources

Wikipedia: Han Unification

  

- Do not enable discretionary ligatures. 
- Line-wrapping rules.
