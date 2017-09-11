---
title: Your code displays Japanese wrong
---

### Why am I here?

If you were brought here from a link given to you, the person who gave you the link probably thinks your code displays Japanese wrong. This page will give you a brief description of the glyph appearance problems that often arise with implementations of Asian text display, why it happens, why it’s a big deal, and how to fix it.

### OK, what’s wrong?

Kanji, also known as Hanzi, Hanja or just Han Characters, is a set of logographic characters that originated in China but are now also used in Japan, Korea, and Taiwan. It consists of several thousand characters, each having their own glyph (glyph is a typographical term which refers to the appearance of a character, as opposed to the meaning.)

The Kanji glyph sets used in Chinese, Japanese, and Korean -- commonly abbreviated to CJK -- each look mostly similar to each other, but have large numbers of characters that must look distinctly different. For instance, here are the Japanese, Simplified Chinese, and Traditional Chinese glyph variants of the character that represents *knife edge*:

| Language            | Glyph                                                             |
|---------------------|-------------------------------------------------------------------|
| Japanese            | <span class="emkanji" xml:lang="ja" lang="ja">刃</span>           |
| Simplified Chinese  | <span class="emkanji" xml:lang="zh-Hans" lang="zh-Hans">刃</span> |
| Traditional Chinese | <span class="emkanji" xml:lang="zh-Hant" lang="zh-Hant">刃</span> |

Therefore, if text in Japanese is displayed using a Kanji glyph set meant for Chinese, it will look to a native Japanese speaker as ugly, non-native, and plain bizarre due to the unfamiliar glyphs showing up in the text. This is most likely what’s happening with your program.

### Why does this happen?

Back when Unicode was being designed, a decision called [Han Unification](https://en.wikipedia.org/wiki/Han_unification) was made to create a single unified set of all the Chinese (Simplified/Traditional), Japanese, and Korean Kanji characters. This involved giving equivalent code points to characters that were deemed equivalent across languages, which allowed the size of the character set to be kept small. 
<style><!-- span.emkanji { font-size: 200%; line-height: 100%;} --></style>
However, this also meant that characters which differ in appearance across languages, such as <span xml:lang="ja" lang="ja">刃</span> and <span  xml:lang="zh-Hans" lang="zh-Hans">刃</span> and <span xml:lang="zh-Hant" lang="zh-Hant">刃</span>, were given identical code points! It is up to the program displaying the text to render them using a font that can display the correct glyph set. Which, if the developer is not aware and doesn’t do anything about it, tends to fail.

In many cases, the default fallback behavior in an ambiguous situation is to choose the Simplified Chinese glyph set. Therefore, Japanese text tends to be incorrectly displayed using Chinese glyphs.

### Is it that much of a big deal?
As the app is not exactly unreadable in this state, it may be tempting to consider this issue minor and give it low priority. However, this issue is much more than the difference between, say, the lowercase A with or without the overhang - imagine your єnglιsЋ ҭєxҭ lѳѳks lικє ҭЋιs. 

Much like how the previous sentence immediately jumps out at you as appearing *weird*, *wrong*, and *begging to be fixed*, Japanese text written in incorrect glyph sets will stand out similarly to any native speaker of Japanese, and will give off a strong connotation that whoever developed this app does not care about this (often large) subset of the global user population. I hope you agree in that this apathy is not the message you want to be sending.

### How can I check if it’s happening or not?

Here are some characters that are known to have different glyph appearances between different languages.

<span class="emkanji" xml:lang="ja" lang="ja">刃直海角骨入</span>

Try copy-pasting them into your code, see the rendered results, and compare them with below. If the glyphs don’t exactly look like the Japanese result sample below, your code is displaying Japanese wrong.

![刃直海角骨入](img/testtext-correct.png)

### How can I fix it?

In a nutshell, the way to fix it is to make your code and font be aware that it’s displaying Japanese when it is doing so. 

#### Web development

On the web, browser rendering engines are usually smart enough to choose the correct fonts from generic font family declarations like `font-family: sans-serif`. However, it may choose a wrong font if the `lang` or `xml:lang` property of your DOM elements are specified to `en`, `en-US` or some other value that does not match the actual language of the text within. Make sure that when you switch the output language of your pages to Japanese, the `lang` property also changes to `ja`.

Also, if explicitly specifying fonts in CSS, be sure to specify a font that is designed for the language. The following `font-family` statement covers most standard Japanese fonts preinstalled in Mac OS, iOS and Windows:

#### Game development (Unity, Unreal, etc.)

Games often store and display fonts using a system that generates font texture atlases from a font file, such as Unity's [TextMesh Pro](http://digitalnativestudios.com/).

If you are using such a system, make sure you are generating separate font atlases for each CJK language, and that each of the source fonts used to generate them are specifically designed for that language. [Google's Noto project](https://www.google.com/get/noto/help/cjk/) provides great open-licensed fonts specifically designed for [Japanese](https://www.google.com/get/noto/#sans-jpan), [Simplified Chinese](https://www.google.com/get/noto/#sans-hans), [Traditional Chinese](https://www.google.com/get/noto/#sans-hant), [Korean](https://www.google.com/get/noto/#sans-kore), etc.

### Any other things to be careful of?

#### Web: Do not enable discretionary ligatures
Discretionary ligatures is a feature of modern fonts, separate from normal ligatures, that allow certain character sequences to be rendered in a more decorative, ornamental style. Due to its often fanciful appearance, it is to be used with discretion, hence the name. Discretionary ligatures can be enabled in CSS as either `font-feature-settings: "dlig" 1` or `font-variant-ligatures: discretionary-ligatures`.

However, do *not* blanket-enable them in Japanese text, as doing so can inadvertently trigger all sorts of obscure weirdo ligatures such as the word <span xml:lang="ja" lang="ja">マンション</span> (mansion) turning into the single-letter <span xml:lang="ja" lang="ja">㍇</span> variant, <span xml:lang="ja" lang="ja">ます</span> (a very common way to end sentences in Japanese) turning into <span xml:lang="ja" lang="ja">〼</span>, etc. There would be *very* few situations where you would want to intentionally trigger this. Please don't enable them.

#### Games: Use composite fonts

In cases where you generate font atlases, you may run into a situation where you are already using a specific font in the Western-text version of your game, and that font does not contain CJK glyphs. In such cases, you may want to go look for a font that does contain both Western and CJK glyphs, like the aforementioned Google Noto. 

When you do this, note that you do *not* need to switch your western glyphs into that CJK font just because you are using that font - in fact, please don't. It is normal, and even desired, for Asian graphic design in situations like this to use *composite fonts*, where one typeface is used for western text and another typeface for CJK text (preferably with matching font weights,) in order to retain the visual appearance of the original western-font-based design. Therefore, the best course of action would be to keep your western characters still sourced from your original font, but use a dedicated CJK font for CJK character code points.

![Screenshot from Pokémon GO demonstrating composite fonts](img/pokemongo.jpg "Screenshot from Pokémon GO demonstrating composite fonts.")
The above screenshot from Pokémon GO shows composite fonts in use. Note that the numbers and units are still displayed using [Lato](https://fonts.google.com/specimen/Lato), Pokémon GO's English font, while the Japanese text are displayed in Hiragino Kaku Gothic, a Japanese font built into iOS.


#### Line-wrapping rules
Line-wrapping in Japanese contain a few new rules as well. Web browsers and TextMesh Pro both have correct JP line-wrapping rules built in, but if you are rolling your own text display routines, you will also want to implement language-aware line-wrapping rules as well.

* The familiar western wrapping rule of using spaces as opportunities to break into a new line should still be in effect.
* Points where the writing script switches between western and Asian, eg. "例えばこういっ**たe**xampl**eに**おいて" should also be used as opportunities to break as well, despite the lack of an explicit space character. 
* Certain characters like 。、） must not be the first character in a line, and should wrap at the character before it. For details, see [Wikipedia's article on Line breaking rules in East Asian languages](https://en.wikipedia.org/wiki/Line_breaking_rules_in_East_Asian_languages) *(Kinsoku Shori)*.

### Do similar problems occur with other languages? / Why aren’t there steps to fix it in [insert environment here]?

I haven’t gone into details since the author is a Japanese native who only speaks English/Japanese, and made this site out of a personal peeve. Honestly I do not have too much insight on other languages. If you can offer assistance in problems that happen with other languages or environments, or find any mistakes, please drop me a line.

### More Resources

* Wikipedia: [Han Unification](https://en.wikipedia.org/wiki/Han_unification), [Variant Chinese Character](https://en.wikipedia.org/wiki/Variant_Chinese_character)
* Model View Culture: [I Can Text You A Pile of Poo, But I Can’t Write My Name](https://modelviewculture.com/pieces/i-can-text-you-a-pile-of-poo-but-i-cant-write-my-name)

### Author
Kenji Iguchi
* [Email](mailto&#58;%&#54;Ee%65d&#108;&#101;&#64;&#104;eistak%2&#69;%63om), [Twitter](https://twitter.com/needle_e), [Facebook](http://heistak.com/fb)
* Donate BTC: 13rbSDJ5Xp5zS4ecjtXSUH9isW1FNZXUU6
* Donate ETH: 0xEccc036049c56055e1f0D3152ec0f152a89ADD7F 
