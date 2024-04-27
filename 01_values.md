{{meta {docid: values}}}

# 值、型別和運算子

{{quote {author: "Master Yuan-Ma", title: "《編程之書》", chapter: true}

程式運行在機器的表面之下。它毫不費力地膨脹和收縮。在非常和諧的情況下，電子散射並重組。螢幕上的形式不過是水面上的漣漪。本質則隱藏在看不見的地方。

quote}}

{{index "Yuan-Ma", "Book of Programming"}}

{{figure {url: "img/chapter_picture_1.jpg", alt: "Illustration of a sea of dark and bright dots (bits) with islands in it", chapter: framed}}}

{{index "binary data", data, bit, memory}}

在電腦的世界裡，只有資料。你可以讀取資料、修改資料、建立新的資料——但無法提及不是資料的東西。所有這些資料都儲存為長長的位元序列，因此從本質上來說是相似的。

{{index CD, signal}}

_位元_ 是任何有兩種值的東西，通常被描述為 0 和 1。在電腦內部，它們可能以高低電壓、強弱訊號，或是 CD 表面的亮暗點等形式存在。任何離散的資訊都可以簡化為一串 0 和 1，從而用位元來表示。

{{index "binary number", radix, "decimal number"}}

舉例來說，我們可以用位元表示數字 13。這與十進位數字的運作方式相同，但我們只有 2 個數字，而不是 10 個 _不同_ 的數字，且每個數字的權重從右到左以 2 的倍數遞增。以下是構成數字 13 的位元，以及它們下方顯示的數字權重：

```{lang: null}
   0   0   0   0   1   1   0   1
 128  64  32  16   8   4   2   1
```

這是二進位數字 00001101。它的非零數字代表 8、4 和 1，總和為 13。

## 值

{{index [memory, organization], "volatile data storage", "hard drive"}}

想像一下，有一片、或者說一整個海洋——位元海。一台典型的現代電腦，其揮發性資料儲存（工作記憶體）中有超過 1,000 億個位元。非易失性儲存（硬碟或同等儲存裝置）的儲存量則往往還要高出幾個數量級。

為了能夠在如此龐大的位元量中工作而不迷失方向，我們將它們分成代表資訊片段的區塊。在 JavaScript 環境中，這些區塊被稱為值（value）。雖然所有的值都是由位元構成的，但它們扮演著不同的角色。每個值都有一個型別（type），決定了它的角色類型。有些值是數字，有些值是文字片段，有些值是函式等等。

{{index "garbage collection"}}

要建立一個值，你只需要呼叫它的名稱。這很方便。你不必為你的值收集建材，也不必付錢。你只要召喚一個值，嗖的一聲，它就出現了。當然，價值並非真的憑空而來。每個值都必須儲存在某個地方，如果你想同時使用大量的值，可能會用盡電腦的記憶體。幸運的是，只有當你需要同時使用所有的值時，這才是個問題。一旦你不再使用某個值，它就會消散，留下其碎片。作為下一代值的建材而被回收利用。

本章的其餘部分介紹了 JavaScript 程式的原子元素，也就是簡單的值型別，以及可以操作這些值的運算子。

## 數字

{{index [syntax, number], number, [number, notation]}}

數字（number）型別的值——不出所料——就是數值。在 JavaScript 程式中，它們是這樣寫的：

```
13
```

{{index "binary number"}}

在程式中使用上面的寫法，會讓表示數字 13 的位元模式在電腦的記憶體中產生。

{{index [number, representation], bit}}

JavaScript 使用固定數量的位元（64 位元）來儲存單一個數值。你能用 64 個位元產生的模式數量是有限的。這限制了能表示的不同數字的數量。使用 N 個十進位數字。你能表示 10^N^ 個數字。同樣，給定 64 個二進位數字。你可以表示 2^64^ 個不同的數字，大約是 18 百京（10^18^，18 後面跟 18 個零）。這是一個很大的數字。

電腦記憶體過去要小得多，人們傾向於用 8 或 16 個位元的組合來表示數字。這樣很容易意外地溢位（overflow）——產生一個無法用給定位元數表示的數字。如今，即使是裝在口袋裡的電腦也有充足的記憶體，所以你可以自由地使用 64 個位元的區塊，只有在處理真正天文數字般大的數值時才需要擔心溢位問題。

{{index sign, "floating-point number", "sign bit"}}

不過，並非所有小於 18 百京（10^18^）的整數都能放進一個 JavaScript 數字中。這些位元也用於儲存負數，因此有一個位元表示數字的正負號。更大的問題是表示非整數。為了做到這一點，一些位元被用來儲存小數點的位置。實際上能儲存的最大整數大約在 9 千兆（10^15^，15 個零）的範圍內，這仍然是一個讓人愉快的巨大數字。


{{index [number, notation], "fractional number"}}

分數用小數點寫出來：

```
9.81
```

{{index exponent, "scientific notation", [number, notation]}}

對於非常大或非常小的數字，你也可以使用科學記號，在數字後面加上一個 _e_ (表示指數 _exponent_ ),後面再跟著數字的指數：

```
2.998e8
```

那是 2.998 × 10^8^ = 299,800,000。

{{index pi, [number, "precision of"], "floating-point number"}}

對小於前面提到的 9 千兆的整數（也叫做整數 _integer_ ）進行計算，可以保證總是精確的。不幸的是，對分數的計算通常就不那麼精確了。正如 π(pi)無法用有限的十進位數字精確表示，許多數字在只用 64 個位元儲存時會失去一些精準度。這很遺憾，但只有在特定情況下才會造成實際問題。重要的是要意識到這一點，把分數型的數位數字視為近似值，而不是精確值。

### 算術運算

{{index [syntax, operator], operator, "binary operator", arithmetic, addition, multiplication}}

The main thing to do with numbers is arithmetic. Arithmetic operations such as addition or multiplication take two number values and produce a new number from them. Here is what they look like in JavaScript:
對數字最主要的操作就是算術運算。像是加法或乘法等算術運算子會接受兩個數字值，並從中產生一個新的數字。在 JavaScript 中，它們看起來像這樣：

```{meta: "expr"}
100 + 4 * 11
```

{{index [operator, application], asterisk, "plus character", "* operator", "+ operator"}}

`+` 和 `*` 符號被稱為運算子。第一個代表加法，第二個代表乘法。將運算子置於兩個值之間，就會將運算子套用到那些值，並產生一個新值。

{{index grouping, parentheses, precedence}}

這個範例是表示「把 4 加到 100,然後將結果乘以 11」，還是先做乘法再做加法？你可能已經猜到了，乘法會先進行。就像在數學中一樣，你可以用括號將加法括起來，改變運算的順序：

```{meta: "expr"}
(100 + 4) * 11
```

{{index "hyphen character", "slash character", division, subtraction, minus, "- operator", "/ operator"}}

減法的運算子是 `-`。除法則是用 `/` 運算子。

當運算子沒有被括號括起來時，它們套用的順序取決於運算子的優先順序。這個範例顯示，乘法優先於加法。`/` 運算子與 `*` 具有相同的優先順序。同樣地，`+` 和 `-` 也有相同的優先順序。當具有相同優先順序的多個運算子並排出現時，例如：`1 - 2 + 1`，它們會從左到右依序套用：`(1 - 2) + 1`。

不用太擔心這些優先順序規則。有疑慮時，直接加上括號就好。

{{index "modulo operator", division, "remainder operator", "% operator"}}

還有一個算術運算子,你可能不會立刻認出它。`%` 符號用來表示取餘數的運算。`X % Y` 是 `X` 除以 `Y` 的餘數。例如：`314 % 100` 會得到 `14`，而 `144 % 12` 會得到 `0`。餘數運算子的優先順序與乘法和除法相同。你也常會看到這個運算子被稱為模數( _modulo_ )。

### 特殊數字

{{index [number, "special values"], infinity}}

JavaScript 中有三個被視為數字的特殊值，但它們的行為與一般數字不同。前兩個是 `Infinity` 和 `-Infinity`，分別表示正無窮大和負無窮大。`Infinity - 1` 仍然是 `Infinity`，依此類推。但不要太信任以無窮大為基礎的計算。它在數學上並不合理，而且很快就會導致下一個特殊數字：`NaN`。

{{index NaN, "not a number", "division by zero"}}

`NaN` 表示「不是一個數字」(not a number)，儘管它是 number 型別的一個值。當你嘗試計算 `0 / 0`(零除以零)、`Infinity - Infinity` 或任何其他沒有有意義結果的數值運算時，就會得到這個結果。

## 字串

{{indexsee "grave accent", backtick}}

{{index [syntax, string], text, character, [string, notation], "single-quote character", "double-quote character", "quotation mark", backtick}}

下一個基本資料型別是 _((字串))_ 。字串用於表示文字。它們是以將內容括在引號中的方式編寫。

```
`Down on the sea`
"Lie on the ocean"
'Float on the ocean'
```

你可以使用單引號、雙引號或反引號來標記字串，只要確保字串開頭和結尾的引號相匹配即可。

{{index "line break", "newline character"}}

你可以將幾乎任何東西放在引號之間，讓 JavaScript 將其轉換為字串值。但有些字元會比較難處理。你可以想像，要在引號之間放入引號可能會很困難，因為它們看起來像是字串的結尾。僅當字串用反引號（`` ` ``）括住的時候，才能包含換行字元（當你按下 [enter 鍵]{keyname}時得到的字元）。

{{index [escaping, "in strings"], ["backslash character", "in strings"]}}

為了讓這些字元可以包含在字串中，使用以下表示法：在引號中的反斜線 （`\`） 表示其後的字元具有特殊含義。這被稱為跳脫（escaping）字元。在反斜線之後的引號不會結束字串，而是成為字串的一部分。當 `n` 字元出現在反斜線之後時，它會被解釋為換行。同樣，反斜線後面的 `t` 表示定位字元（tab）。看看下面的字串：

```
"This is the first line\nAnd this is the second"
```

這是該字串中的實際文字：

```{lang: null}
This is the first line
And this is the second
```
然而在某些情況下，你會想在字串中使用反斜線，而不是特殊字元。如果兩個反斜線相連，它們會合併在一起，而在結果字串值中只會留下一個反斜線。這就是字串换行字元的寫法，比如 " _A newline character is written like `"`\n`"`._ "。可以表示為：

```
"A newline character is written like \"\\n\"."
```

{{id unicode}}

{{index [string, representation], Unicode, character}}

為了能夠存在於電腦中，字串也必須被建模為一系列的位元。JavaScript 採用了以 Unicode 標準為基礎的方式來做到這一點。該標準為你可能需要的幾乎每個字元分配了一個數字，包括希臘文、阿拉伯文、日文、亞美尼亞文等的字元。如果每個字元都有一個數字，就可以用數字序列來描述字串。這就是 JavaScript 所做的。

{{index "UTF-16", emoji}}

不過這裡有一個複雜之處：JavaScript 的表示法每個字串元素使用 16 位元，最多可以描述 2^16^ 個不同的字元。然而，Unicode 定義的字元比這個還要多——目前大約是兩倍。所以有些字元（例如許多表情符號），在 JavaScript 字串中佔據了兩個「字元位置」。我們會在第[章節 ?](higher_order#code_units)再回到這個主題。

{{index "+ operator", concatenation}}

字串不能被除、乘或減。`+` 運算子可以用在字串上，但不是用來相加，而是用來串接（concatenate）——將兩個字串粘合在一起。下面這行會產生字串 `"concatenate"`:

```{meta: "expr"}
"con" + "cat" + "e" + "nate"
```

字串值有一些相關的函式（ _methods_ ），可用於在字串上執行其他操作。我會在第[章節 ?](data#methods)詳細說明這些函式。

{{index interpolation, backtick}}

用單引號或雙引號編寫的字串行為非常相似——唯一的區別在於你需要在其中跳脫哪種類型的引號。用反引號括住的字串通常被稱為模板字串（template literal），它們可以做更多花樣。除了能夠跨行之外，它們還可以嵌入其他值。

```{meta: "expr"}
`half of 100 is ${100 / 2}`
```

當你在模板字串中編寫 `${}` 內的內容時，其結果將被計算出來，轉換為字串，並包含在該位置。這個範例會產生 " _half of 100 is 50_ "。

## 一元運算子

{{index operator, "typeof operator", type}}

Not all operators are symbols. Some are written as words. One example is the `typeof` operator, which produces a string value naming the type of the value you give it.
並非所有運算子都是符號。有些是用文字編寫的。一個範例是 `typeof` 運算子，它會產生一個表示你提供給它的值的型別的字串值。

```
console.log(typeof 4.5)
// → number
console.log(typeof "x")
// → string
```

{{index "console.log", output, "JavaScript console"}}

{{id "console.log"}}

我們將在範例程式碼中使用 `console.log`，表示我們想看到某些東西求值的結果。[下一章](program_structure)會詳細說明這一點。

{{index negation, "- operator", "binary operator", "unary operator"}}

到目前為止，本章中顯示的其他運算子都對兩個值進行操作，但 `typeof` 只接受一個值。使用兩個值的運算子稱為二元運算子（ _binary_  operator），而只接受一個值的運算子稱為一元運算子（ _unary_ operator）。減號運算子可以同時用作二元運算子和一元運算子。

```
console.log(- (10 - 2))
// → -8
```

## 布林值

{{index Boolean, operator, true, false, bit}}

通常區分只有兩種可能性的值會很有用，例如「是」和「否」，或「開」和「關」。為此，JavaScript 有一個布林（Boolean）型別，它只有兩個值，true 和 false，以這些文字編寫。

### 比較

{{index comparison}}

以下是產生布林值的一種方式：

```
console.log(3 > 2)
// → true
console.log(3 < 2)
// → false
```

{{index [comparison, "of numbers"], "> operator", "< operator", "greater than", "less than"}}

`>` 和 `<` 符號分別是「大於」和「小於」的傳統符號。它們是二元運算子。套用它們會得到一個布林值，表示在這種情況下它們是否成立。

字串也可以用同樣的方式比較：

```
console.log("Aardvark" < "Zoroaster")
// → true
```

{{index [comparison, "of strings"]}}

字串的排序方式大致上是按字母順序，但不完全是你在字典中看到的順序：大寫字母總是「小於」小寫字母，所以 `"Z" < "a"`，而非字母的字元（!、- 等）也包括在排序中。在比較字串時，JavaScript 會從左到右遍歷字元，逐一比較 Unicode 程式碼。

{{index equality, ">= operator", "<= operator", "== operator", "!= operator"}}

其他類似的運算子是 `>=`（大於或等於）、`<=`（小於或等於）、`==`（等於）和 `!=`（不等於）。

```
console.log("Garnet" != "Ruby")
// → true
console.log("Pearl" == "Amethyst")
// → false
```

{{index [comparison, "of NaN"], NaN}}

There is only one value in JavaScript that is not equal to itself, and that is `NaN` ("not a number").
JavaScript 中只有一個值不等於自身，那就是 NaN（不是一個數字）。

```
console.log(NaN == NaN)
// → false
```

`NaN` 應該表示無意義計算的結果，因此它不等於任何其他無意義的計算結果。

### 邏輯運算子

{{index reasoning, "logical operators"}}

也有一些運算可以套用到布林值本身。JavaScript 支援三個邏輯運算子： _and_ 、 _or_ 和 _not_ 。這些運算子可用於對布林值進行「推理」。


{{index "&& operator", "logical and"}}

`&&` 運算子表示邏輯 and。它是一個二元運算子，只有在給它的兩個值都是 true 時，其結果才為 true。

```
console.log(true && false)
// → false
console.log(true && true)
// → true
```

{{index "|| operator", "logical or"}}

`||` 運算子表示邏輯 _or_。如果給它的值中有一個是 true，它就會產生 true。

```
console.log(false || true)
// → true
console.log(false || false)
// → false
```

{{index negation, "! operator"}}

_Not_ is written as an exclamation mark (`!`). It is a unary operator that flips the value given to it—`!true` produces `false` and `!false` gives `true`.
_Not_ 寫作驚嘆號（`!`）。它是一個一元運算子，會反轉給它的值——`!true` 產生 `false`，`!false` 會給出 `true`。

{{index precedence}}

當將這些布林運算子、算術運算子以及其他運算子混合使用時的情況，什麼時候需要括號就不明顯了。實際上，通常你只需要知道到目前為止我們看過的運算子中，`||` 的優先順序最低，然後是 `&&`，然後是比較運算子（`>`、`==` 等），最後是其餘的運算子。這個順序是精心選擇的，使下方這樣典型的表達式中，儘可能少用括號：

```{meta: "expr"}
1 + 1 == 2 && 10 * 10 > 50
```

{{index "conditional execution", "ternary operator", "?: operator", "conditional operator", "colon character", "question mark"}}

我們要看的最後一個邏輯運算子不是一元的，也不是二元的，而是三元的，對三個值進行操作。它寫作問號和冒號，像這樣：

```
console.log(true ? 1 : 2);
// → 1
console.log(false ? 1 : 2);
// → 2
```

這個運算子被稱為條件運算子（有時也簡稱為三元運算子，因為它是語言中唯一的三元運算子）。它使用問號左邊的值來決定「選擇」另外兩個值中的哪一個。如果你寫 `a ? b : c`，當 `a` 為 true 時結果將是 `b`，否則結果將是 `c`。

## 空值

{{index undefined, null}}

有兩個特殊的值，寫作 `null` 和 `undefined`，用於表示沒有有意義的值。它們本身就是值，但不攜帶任何資訊。

語言中許多不產生有意義值的操作會產生 `undefined`，僅僅是因為它們必須產生某個值。

`undefined` 和 `null` 在意義上的區別是 JavaScript 設計上的一個意外，大多數時候這並不重要。在實際必須關注這些值的情況下，我建議將它們視為大致可以互換。

## 自動型別轉換

{{index NaN, "type coercion"}}

在介紹中，我提到 JavaScript 竭盡全力接受你提供給它的幾乎任何程式，即使是做奇怪事情的程式。下面的表達式很好地展示了這一點：

```
console.log(8 * null)
// → 0
console.log("5" - 1)
// → 4
console.log("5" + 1)
// → 51
console.log("five" * 2)
// → NaN
console.log(false == 0)
// → true
```

{{index "+ operator", arithmetic, "* operator", "- operator"}}

當運算子套用到「錯誤」型別的值時，JavaScript 會悄悄地將該值轉換為它需要的型別，使用一組通常不是你想要或期望的規則。這被稱為型別強制轉換（type coercion）。第一個表達式中的 `null` 變成了 0，第二個表達式中的 `"5"` 變成了 `5`（從字串到數字）。然而在第三個表達式中，`+` 會在進行數值相加之前嘗試字串串接，所以 `1` 被轉換為 `"1"`（從數字到字串）。

{{index "type coercion", [number, "conversion to"]}}

當一些無法以明顯方式對應到數字的東西（例如 `"five"` 或 undefined）被轉換為數字時，你會得到值 `NaN`。對 `NaN` 進行進一步的算術運算會持續產生 `NaN`，所以如果你在意外的地方得到了 `NaN`，請尋找意外的型別轉換。

{{index null, undefined, [comparison, "of undefined values"], "== operator"}}

當使用 `==` 運算子比較相同型別的值時，結果很容易預測：當兩個值相同時，你應該得到 true，除了 `NaN` 的情況。但是當型別不同時，JavaScript 會使用一組複雜且令人困惑的規則來決定要做什麼。在大多數情況下，它只是試圖將其中一個值轉換為另一個值的型別。然而，當運算子任一側出現 `null` 或 `undefined` 時，只有在兩側都是 `null` 或 `undefined` 的情況下，它才會產生 true。

```
console.log(null == undefined);
// → true
console.log(null == 0);
// → false
```

當你想測試一個值是否有真正的值，而不是 `null` 或 `undefined` 時，你可以使用 `==` 或 `!=` 運算子將其與 `null` 進行比較，這個方法很有用。

{{index "type coercion", [Boolean, "conversion to"], "=== operator", "!== operator", comparison}}

但如果你想測試某個東西是否準確地引用值 `false` 呢？由於自動型別轉換，像 `0 == false` 和 `"" == false` 這樣的表達式也為 true。當你不希望發生任何型別轉換時，還有兩個額外的運算子：`===` 和 `!==`。第一個運算子測試一個值是否精確地等於另一個值，第二個運算子測試它是否不精確地等於另一個值。因此， `"" === false` 如預期般為 false。

我建議防禦性地使用三個字元的比較運算子，以防止意外的型別轉換絆倒你。但是當你確定兩側的型別將是相同的時候，使用較短的運算子就沒有問題。

### 邏輯運算子的特殊情況

{{index "type coercion", [Boolean, "conversion to"], operator}}

邏輯運算子 `&&` 和 `||` 以一種特殊的方式處理不同型別的值。它們會將其左側的值轉換為布林型別，以決定要做什麼，但根據運算子和轉換的結果，它們將返回原始的左側值或右側值。

{{index "|| operator"}}

例如，`||` 運算子在其左側的值可以轉換為 true 時將返回該值，否則將返回其右側的值。當值是布林值時，這具有預期的效果，並對其他型別的值進行類似的操作。
```
console.log(null || "user")
// → user
console.log("Agnes" || "user")
// → Agnes
```

{{index "default value"}}


我們可以利用這個功能作為退回到預設值的一種方式。如果你有一個可能為空的值，你可以在它後面放一個 `||` 和一個替代值。如果初始值可以轉換為 `false`，你將得到替代值。將字串和數字轉換為布林值的規則規定，`0`、`NaN` 和空字串(`""`) 視為 false,而所有其他值視為 true。這意味著 `0 || -1` 會產生 `-1`，而 `"" || "!?"` 會產生 `"!?"`。

{{index "?? operator", null, undefined}}

運算子 `??` 類似於 ||，但只有當左側的值是 null 或 undefined 時才返回右側的值，而不是在它可以轉換為 `false` 的其他值時。通常，這比 `||` 的行為還要好。
```
console.log(0 || 100);
// → 100
console.log(0 ?? 100);
// → 0
console.log(null ?? 100);
// → 100
```

{{index "&& operator"}}

運算子 `&&` 的工作方式類似，但方向相反。當其左側的值是可以轉換為 false 的東西時，它返回該值，否則它返回右側的值。

這兩個運算子的另一個重要特性是，只有在必要時才會評估它們右側的部分。在 `true || X` 的情況下，無論 `X` 是什麼——即使它是一段做可怕事情的程式——結果都將是 true，而且永遠不會評估 `X`。`false && X` 也是如此，它是 false 並且會忽略 `X`。這被稱為短路求值 _(short-circuit evaluation)_。

{{index "ternary operator", "?: operator", "conditional operator"}}

條件運算子的工作方式類似。第二個和第三個值中，只有被選中的那個會被求值。

## 總結

We looked at four types of JavaScript values in this chapter: numbers, strings, Booleans, and undefined values. Such values are created by typing in their name (`true`, `null`) or value (`13`, `"abc"`).
在本章中,我們研究了四種 JavaScript 值：數字、字串、布林值和未定義值。通過輸入它們的名稱（`true`、`null`）或值（`13`、`"abc"`）來建立這些值。

你可以用運算子組合和轉換值。我們看到了用於算術（`+`、`-`、`*`、`/` 和 `%`）、字串串接 (`+`)、比較（`==`、`!=`、`===`、`!==`、`<`、`>`、`<=` 和 `>=`）和邏輯（`&&` 、`||` 和 `??`）的二元運算子，以及幾個一元運算子(`-` 對數字取負、`!` 對邏輯值取反和 `typeof` 查找值的型別)和一個三元運算子(`?:`)根據第三個值選擇兩個值之一。

這給了你足夠的資訊來將 JavaScript 用作袖珍計算機，但還不夠多。下一章將開始將這些表達式串在一起，組成基本的程式。