---
layout: post
title:  "在github pages上面使用jekyll架設部落格"
date:   2015-07-19
image:  github-pages-jekyll/github-pages.png
---

又開始寫部落格囉!! 其實一直想要寫，之前有個[落格站](http://popcornylu.blogspot.tw/)，但那邊已經長蚊子長蜘蛛網很久了。其實沒有寫的原因有幾個:

1. 編寫介面還是比較麻煩，我喜歡在local寫，直接在local預覽。但是Blogspot做不到。
1. 上面不支援Markdown，而且對於寫程式碼比較不Friendly。
1. 自己懶 (主因...)

## Markdown Editor
再來之前用[MOU](http://25.io/mou/)寫Markdown，最近改用[MacDown](http://macdown.uranusjr.com/)，發現有幾個好用的功能。

1. 支援[Fenced Code Block](https://help.github.com/articles/github-flavored-markdown/#fenced-code-blocks)，這個很重要啊!!
2. 有支援TOC (Table Of Content)
3. 支援[Github Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/)

有了這個利器後，之後我剛好又看到一個github.io結尾的部落格。研究一下[github pages](https://pages.github.com/)可以搭配[jekyll](http://jekyllrb.com/)架設個人部落格，而且可以在local預覽，支援Markdown。天啊!! 根本就全部我需要的。這就興起了我寫部落格的念頭。最後就架設起這個部落格。

## Github Pages and Jekyll

我大概紀錄一下流程。對於有興趣在**Github Pages**上面架站的可以參考一下

1. 根據[Github Pages](https://pages.github.com/)上面的指示。架設你的第一個`<mypage>.github.io`的網站。上面只允許靜態頁面。
2. 再根據[Using Jekyll with Github Pages](https://help.github.com/articles/using-jekyll-with-pages/)裡面的內容。在自己電腦裝起Jekyll，這個可以讓你的個人部落格跑在`localhost`，並且邊編輯邊預覽，方長方便。
3. 去[Jekyll Themes](http://jekyllthemes.org/)找一個想要的Theme複製到自己的blog根目錄。馬上就是一個美美的部落格。

## 設定_config.yml

前面一步一步來，應該都可以很快搞定。但是通常到這時候，就開始微調自己的部落格了。`_config.yml`是**jekyll**的設定檔。在預設的Configuration沒有支援**Fenced Code Block**。所以我加上了以下的設定。

``` yaml
markdown: Kramdown
kramdown:
  input: GFM
  use_coderay: true
  coderay:
    coderay_css: class
```

這邊是改用了[Karmdown](http://kramdown.gettalong.org/)這個Markdown渲染器。而Coderay是其中處理Syntax Highlighter的部分。但是這邊我卡很久的是，我原本以為加了就可以用，但是其實不是，他只是把Fenced Code Block的部分，產生出來的*code* tag加上`language-xxx`這個class。下面是個例子:

```java
	```java
	System.out.println("");
	```
```

結果是加上`language-java`這個class

```markup
<pre>
<code class="language-java">
System.out.println("");
</code>
</pre>
```

好吧。其實這樣也不差，我可以選用自己想要的syntax hightlihger。畢竟這方面有很多javascript。我很喜歡**MacDown**輸出的Syntax Highlight，研究之後發現它用的是[prismjs](http://prismjs.com/)這套。


### Prismjs

要安裝也不會太困難。**Prismjs**把resource分成兩部分。

1. javascript: 有包含核心的邏輯，各個語言的lexer描述，還有Plugins。我選用了`line numbers`plugin。
2. css: 包含各種theme的定義。我選擇的是github的theme。

如下面的畫面
![]({{site.baseurl}}/assets/img/github-pages-jekyll/prismjs.png)

把自己想要的語言download一下，並且放到`assets/js`跟`assets/css`裡面。

在`_includes/head.html`這個目錄下加上javascript跟css。

```markup
    <!-- Syntax Hightlight -->
    <link rel="stylesheet" href="{{ "/assets/css/prism.css" | prepend: site.baseurl }}">
    <script src="{{ "/assets/js/prismjs.js" | prepend: site.baseurl }}" type="text/javascript"></script>    
```

### Line Numbers
最後，因為我要有`line numbers`的功能。而**prismjs**會把有定義`line-numbers`的class加上line numbers。所以我在`_includes/footer.html`加上這一段code。

```javascript
<script>
var pres = document.getElementsByTagName("pre");
for (var i = 0; i < pres.length; i++) {
   var pre = pres[i];
   pre.className = pre.className + " line-numbers";
}

</script>
```
此小撇步會讓所有的`pre`後面加上`line-numbers`的class。讓**prismjs**知道要去render line numbers。


## 後記
當然市面上還是有很多好用的blog service，且有些支援markdown，但還是有一種小眾(傲嬌)需求，是喜歡自己來，刻出自己想要的Blog，而Github Pages加上Jekyll確實發揮了這個彈性。靜態網站的設計，讓你覺得一切掌握度會比較高，沒有什麼Server Side Magic。但是另一方面就是你要自己多下點功夫，像我研究了Syntax Highlight就研究好一陣子才弄到自己想要的。當然寫這篇除了自己實驗寫一篇以外，最主要也給真的想要寫技術文章部落格，尤其常常會有很多程式碼要貼的人，我覺得這個Solution目前覺得還不錯，推薦給各位。





