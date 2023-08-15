---
title: 日本語の表示がおかしいよ
---

## ここはどこ？

このページを誰かが紹介してきたのであれば、おそらくその人はあなたのコードが日本語を正しく表示していないと思っているでしょう。端的に言うと、日本人の目から見ると、あなたのテキストはこんな風に見えています。本ページは、アジアのテキスト表示の実装によく起こる文字の表示問題、それがどうして起こるのか、それが大問題である理由、そしてそれをどのように修正するかについての簡単な説明を提供します。
If someone gave you a link to this page, that person probably thinks your code displays Japanese wrong. In short, from a native Japanese eye, yѳur ҭєxҭ lѳѳκs κιnd ѳf lικє ҭЋιs. This page will give you a brief description of the glyph appearance problems that often arise with implementations of Asian text display, why it happens, why it’s a big deal, and how to fix it.

## OK, what’s wrong?

Kanji, also known as Hanzi, Hanja or just Han Characters, is a set of characters that originated in China but are also used in Japan, Korea, Taiwan, etc. The Kanji sets used in those countries each look mostly similar to each other, but also have large numbers of characters that have different-looking glyphs. (*Glyph* is a typographical term which refers to the appearance of a character, as opposed to the meaning.) 

For instance, here are the Japanese, Simplified Chinese, and Traditional Chinese glyph variants of the character that represents *knife edge*:

| Language            | Glyph                                               | Unicode Code Point |
|---------------------|-----------------------------------------------------|--------------------|
| Japanese            | ![knife edge, Japanese](img/knife-jp.png)           | U+5203             |
| Simplified Chinese  | ![knife edge, Simplified Chinese](img/knife-sc.png) | U+5203             |
| Traditional Chinese | ![knife edge, Traditional Chinese](img/knife-tc.png)| U+5203             |

Therefore, if text in Japanese is displayed using a Kanji glyph set meant for other languages, it will look to a native Japanese reader as non-native, vaguely shady, and plain bizarre due to the unfamiliar glyphs showing up in the text. This is most likely what’s happening with your program.

## Why does this happen?

Back when Unicode was being designed, a decision called [Han Unification](https://en.wikipedia.org/wiki/Han_unification) was made to create a single unified set of all the Chinese (Simplified/Traditional), Japanese, and Korean Kanji characters. This involved giving equivalent code points to characters that were deemed equivalent across languages, which allowed the size of the character set to be kept small. 
<style><!-- span.emkanji { font-size: 200%; line-height: 100%;} --></style>
However, this also meant that characters which differ in appearance across languages, such as <span xml:lang="ja" lang="ja">刃</span> and <span  xml:lang="zh-Hans" lang="zh-Hans">刃</span> and <span xml:lang="zh-Hant" lang="zh-Hant">刃</span>, were given **identical code points!** You can see in the earlier chart that the three "knife edges" were ALL assigned U+5203. It is up to the program displaying the text to render them using a font that can display the correct glyph set. 

In many cases, the default fallback behavior in an ambiguous situation is to choose the Simplified Chinese glyph set. Therefore, if the developer isn't aware of it, Japanese text tends to be incorrectly displayed using Chinese glyphs.

## Is it that much of a big deal?
As the app is not exactly unreadable in this state, it may be tempting to consider this issue minor and give it low priority. However, this issue is much more than the difference between, say, the lowercase A with the overhang (a) or without (α). Like the example at the beginning of this article, if the equivalent symptom was happening with English text, ιҭ wѳuld bє lѳѳκιng sѳmєҭЋιng lικє ҭЋιs. 

Much like how the previous sentence immediately jumps out at you as appearing *weird* and *wrong*, Japanese text written in incorrect glyph sets will stand out similarly to any native speaker of Japanese, and will give off a connotation that whoever developed this app does not care about this (often large) subset of the global user population. I hope you agree in that this apathy is not the message you want to be sending.

## How can I check if it’s happening or not?

Here are some characters that are known to have different glyph appearances between different languages.

<span class="emkanji" xml:lang="ja" lang="ja">刃直海角骨入</span>

Try copy-pasting them into your code, see the rendered results, and compare them with below. If the glyph shapes look different from the Japanese result sample below (aside from differences due to the font's styling), your code is displaying Japanese wrong.

![刃直海角骨入](img/testtext-correct.png)

## How can I fix it?

In a nutshell, the way to fix it is to make your code and font be aware that it’s displaying Japanese when it is doing so. 

### Web development: Mark elements as lang=ja

On the web, browser rendering engines are usually smart enough to choose the correct fonts from generic font family declarations like `font-family: sans-serif`. However, it may choose a wrong font if the `lang` or `xml:lang` property of your DOM elements are not specified to `ja`. Make sure that when you switch the output language of your pages to Japanese, the `lang` property also changes to `ja`.

Also, if explicitly specifying fonts in CSS, be sure to specify a font that is designed for the language. The following `font-family` statement covers most standard Japanese fonts preinstalled in modern devices (courtesy of [ICS Media](https://ics.media/entry/200317/)):

    body {
      font-family: "Helvetica Neue",
        Arial,
        "Hiragino Kaku Gothic ProN",
        "Hiragino Sans",
        Meiryo,
        sans-serif;
    }

### Game development: Generate separate font atlases from language-specific fonts

Games often store and display fonts using a system that generates font texture atlases from a font file, such as Unity's [TextMesh Pro](https://docs.unity3d.com/Manual/com.unity.textmeshpro.html).

If you are using such a system, make sure you are generating separate font atlases for each Asian language, and that each of the source fonts used to generate them are specifically designed for that language. [Google's Noto project](https://fonts.google.com/noto) provides great open-licensed fonts specifically designed for [Japanese](https://fonts.google.com/noto/specimen/Noto+Sans+JP), [Simplified Chinese](https://fonts.google.com/noto/specimen/Noto+Sans+SC), [Traditional Chinese](https://fonts.google.com/noto/specimen/Noto+Sans+TC), [Korean](https://fonts.google.com/noto/specimen/Noto+Sans+KR), etc.

## Any other things to be careful of?

A few other things. [I broke them off to a separate page](otherthings.html).

## Do similar problems occur with other languages? / Why aren’t there steps to fix it in [insert environment here]?

The author is a Japanese native who only speaks English/Japanese, and made this site out of a personal peeve. I don't have much insight on other languages, sorry. If you can offer assistance in problems that happen with other languages/environments, or find any mistakes, please drop me a line.

## More Resources

* Wikipedia: [Han Unification](https://en.wikipedia.org/wiki/Han_unification), [Variant Chinese Character](https://en.wikipedia.org/wiki/Variant_Chinese_character)
* Model View Culture: [I Can Text You A Pile of Poo, But I Can’t Write My Name](https://modelviewculture.com/pieces/i-can-text-you-a-pile-of-poo-but-i-cant-write-my-name)

## Author
Kenji Iguchi
* [Email](mailto&#58;%&#54;Ee%65d&#108;&#101;&#64;&#104;eistak%2&#69;%63om), [Twitter](https://twitter.com/needle_e), [Facebook](http://heistak.com/fb)
