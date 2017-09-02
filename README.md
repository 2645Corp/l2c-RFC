This document introduces a format of lyrics. *Request for comments.*


1. Concept Explanation

    + Phonetic Notation, also referred to as PN. e.g. Pin-yin (Chinese), Kana (Japanese), Rōmaji (Japanese)
    + Translation: Translation.
    + Sentence: A line of lyrics.
    + Lyrics-part: Consisting of a single character, or a number of characters, or a word, or a number of words, or even a whole sentence. It's the smallest part in dividing the time of a sentence.
    + Extension: Some text attached to the whole or part of a sentence. It usually describes singer's gender or name for the sentence attached to.
    + Timelabel pair: A data structure with the form of (timeshift, duration).
    + Timeshift: The timeshift for a sentence (i.e. In a sentence mark `[]`) is the time between the very beginning of the song and the very beginning of this sentence represented in ms(millisecond). The timeshift for a lyrics-part (i.e. In a lyrics-part mark `<>`) is the time between the very beginning of the sentence and the very beginning of this lyrics-part represented in ms.
    + Duration: The time between the very beginning of a sentence/lyrics-part and the end of the sentence/lyrics-part represented in ms.

2. Operator

    + `[]` is used at the beginning of a sentence. One and only one timelabel pair is required in this operator. Multiple translations are allowed in this operator. Also, extensions are allowed in this operator.
    + `<>` is used at the beginning of a lyrics-part. One and only one timelabel pair is required in this operator. Multiple PNs are allowed in this operator.  Also, extensions are allowed in this operator.
    + `()` is used to make a timelabel pair which consisting of a timeshift and a duration.
    + `=` mostly means "equal to" or "be defined as".
    + `:=` is used to make various PNs together. It's also transitive. For example, <(1368,412) kana:="じ">自<(1780,123) kana:="ゆう">由<(1903,123) kana="じん">人 means the PN kana for the whole word "自由人" is "じゆうじん". This allows the player to display the PN well inline like `自由人(じゆうじん)`.

3. Keyword

    These keywords are pre-defined which have their specific meanings for the player, they are all written in UPPERCASE.
    
    PN, TRANSLATION, EXTENSION
    
    Extension keywords:
     
    SINGER, COLOR
     
    Author ID keywords:
     
    AR, TI, AL, LR, CP, TR, BY

4. Grammar
    
    A valid file consists of two parts. Definition part and Lyrics part. The definition part starts with a stand-alone line `@DEFINE`, and the lyrics part starts with a stand-alone line `@LYRICS`. The definition part tells the player what kind of PNs, translations, extensions are available in this file. PN, translation and extension keywords must be defined/declared in the definition part (if supported). 
        
    The following is a sample of the grammar. Words after a `#` mark are comments that exist for this introduction only, and are not allowed in an actual lyric file.  
    ```
    @DEFINE
    ### Definition start ###
    
    # PN Definition start
    PN = pn1, pn2, pn3, ... # pn1, pn2, pn3 are names of PNs given by editors
    # PN Definition end
    
    # Translation Language Definition start
    TRANSLATION = language1, language2, language3, ... # language1, language2, language3 are laguages given by editors
    # Translation Language Definition end
    
    # Extension Declaration start
    EXTENSION = SINGER, COLOR, ... # SINGER, COLOR are pre-defined keywords, editors declare here for supporting the extension.
    # Extension Declaration end
    
    # Author IDs start
    AR = Artist of the song
    TI = Title of the song
    AL = Album of the song
    LR = Lyrics writter of the song
    CP = Composer of the song
    TR = Translator
    BY = Editor of this file
    # Author IDs end
    
    ### Definition end ###
    
    @LYRICS
    ### Lyrics start ###
    # "=/:=" means operator = or := both work.
    [(timeshift1, duration1) language1="foobar1" language2="foobar2" ...]<(timeshift, duration) SINGER="bar" pn1=/:="foo1" pn2=/:="foo2" ...>barfoo<(timeshift, duration) SINGER="barf" pn1=/:="foo3" pn2=/:="foo4" ...>barfoobar ......
    [(timeshift2, duration2) language1="foobar1" language2="foobar2" ...]<(timeshift, duration) SINGER="bar" pn1=/:="foo1" pn2=/:="foo2" ...>barfoo<(timeshift, duration) SINGER="barf" pn1=/:="foo3" pn2=/:="foo4" ...>barfoobar ......
    ......
    ### Lyrics end ###
    ```
    
    Note: Extension marks (SINGER, COLOR, etc) can be added to either sentence marks (`[]`) or lyrics-part marks (`<>`). Translation marks should only be added to sentence marks (`[]`), and PN marks should only be added to lyrics-part mark (`<>`).
    
5. Instance

    The following is an instance. It demonstrates a use of the time labels, PN as well as Translation, Extension marks, and especially, the use of `:=` operator.
    
    It also shows that the lyric can work okay even with an ensemble of two or more singers, each singing one part.

    ```
    @DEFINE
    PN = kana, roma
    TRANSLATION = zh
    @LYRICS
    ...
    [(252660,5240) zh="匆匆忙出发 冷冷冬夜下 颤抖着一言不发"]<(0,250) kana="か" roma="ka" SINGER="絵里">駆<(250,240) roma="ke">け<(490,240) kana="だ" roma="da">出<(730,200) roma="shi">し<(930,250) roma="ta">た<(1180,400) roma="ra">ら <(1580,680) kana="つめ" roma="tsume">冷 <(2260,240) roma="ta">た<(2500,290) roma="sa">さ<(2790,400) roma="ni">に <(3190,250) roma="fu" SINGER="絵里 真姬">ふ<(3440,290) roma="ru">る<(3730,310) roma="e">え<(4040,310) roma="na">な<(4350,290) roma="ga">が<(4640,300) roma="ra">ら<(4940,300) roma="mo">も
    [(252720,5560) zh="这冬天赐予的感觉 相信你就会出现" SINGER="にこ"]<(0,500) kana="ふゆ" roma="fuyu">冬 <(500,340) roma="ga">が<(840,340) roma="ku">く<(1180,340) roma="re">れ<(1520,350) roma="ta">た<(1870,400) kana:="よ" roma:="yo">予<(2270,500) kana="かん" roma="kan">感 <(2770,300) roma="ki">き<(3070,350) roma="tto">っと<(3420,340) kana="く" roma="ku">来<(3760,400) roma="ru">る<(4160,660) kana="きみ" roma="kimi">君 <(4820,740) roma="ga">が
    ...
    ```