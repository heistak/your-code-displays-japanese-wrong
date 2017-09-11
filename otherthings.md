---
title: Other things to be careful of
---

## Other things to be careful of

### Line breaking rules
Line breaking, or word-wrapping, in Japanese contain a few new rules as well. Web browsers and TextMesh Pro both have correct JP line breaking rules built in, but if you are rolling your own text display routines, you will also want to implement language-aware line breaking rules as well.

* The familiar western line-break rule of using spaces as opportunities to break into a new line should still be in effect.
* Points where the writing script switches between western and Asian, eg. "例えばこういっ**たe**xampl**eに**おいて" should also be used as opportunities to break as well, despite the lack of an explicit space character. 
* Certain characters like 。、） must not be the first character in a line, and should wrap at the character before it. For details, see [Wikipedia's article on Line breaking rules in East Asian languages](https://en.wikipedia.org/wiki/Line_breaking_rules_in_East_Asian_languages) *(Kinsoku Shori)*.


### Web: Do not enable discretionary ligatures
*Discretionary ligatures* is a feature of modern fonts, separate from normal ligatures, that allow certain character sequences to be rendered in a more decorative, ornamental style. Due to its often fanciful appearance, it is to be used with discretion, hence the name. Discretionary ligatures can be enabled in CSS as either `font-feature-settings: "dlig" 1` or `font-variant-ligatures: discretionary-ligatures`.

However, do *not* blanket-enable them in Japanese text! Doing so can inadvertently trigger all sorts of obscure weirdo ligatures such as the word <span xml:lang="ja" lang="ja">マンション</span> (mansion) turning into the single-letter <span xml:lang="ja" lang="ja">㍇</span> variant, <span xml:lang="ja" lang="ja">ます</span> (a very common way to end sentences in Japanese) turning into <span xml:lang="ja" lang="ja">〼</span>, etc. There would be *very* few situations where you would want to intentionally trigger this. Please don't enable them.

### Games: Use composite fonts

In cases where you generate font atlases, you may run into a situation where you are already using a specific font in the Western-text version of your game, and that font does not contain CJK glyphs. In such cases, you may want to go look for a font that does contain both Western and CJK glyphs, like the aforementioned Google Noto. 

When you do this, note that you do *not* need to switch your western glyphs into that CJK font just because you are using that font - in fact, please don't. It is normal, and even desired, for Asian graphic design in situations like this to use *composite fonts*, where one typeface is used for western text and another typeface for CJK text (preferably with matching font weights,) in order to retain the visual appearance of the original western-font-based design. Therefore, the best course of action would be to keep your western characters still sourced from your original font, but use a dedicated CJK font for CJK character code points.

![Screenshot from Pokémon GO demonstrating composite fonts](img/pokemongo.jpg "Screenshot from Pokémon GO demonstrating composite fonts.")
The above screenshot from Pokémon GO shows composite fonts in use. Note that the numbers and units are still displayed using [Lato](https://fonts.google.com/specimen/Lato), Pokémon GO's English font, while the Japanese text are displayed in Hiragino Kaku Gothic, a Japanese font built into iOS.



[Back to Main Page](index.html)