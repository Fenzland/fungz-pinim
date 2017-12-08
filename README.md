楓梓拼音輸入法 (beta)
================================

# 簡介

本項目是楓梓拼音輸入法在 rime 輸入法平臺上的實現，即一種基於 rime 的中文拼音輸入法。
楓梓拼音使用特有的拼音方案，與《現代漢語拼音》（更準確的叫法是“普通話拼音”，後文亦將如此稱呼）等主流方案不同，有更加精確，區分尖團音等特點。
整個項目包含　全拼，雙拼，並擊　三種輸入方案。
本項目仍在開發中，拼音規則還會變化。尤其是詞庫，由於手工區分尖團音等工作尚未完成，還會持續變化，


# 拼音方案

繼承於“普通話拼音”，並進行以　*系統化*、*精確化*、*分明化*　爲方向的演化。

## 演化調整

#### 1. 無異化和簡化寫法
“普通話拼音”中有一些多餘的異化(如 零聲母`i->yi`,`s->si`)和簡化(`jü->ju`,`uen->un`)寫法。
它們對於系統化和精確化有害無利，無謂地佔用了`y`,`w`兩個字母，甚至錯誤地佔用了其他拼音組合(`si`本因是“西”的拼音，卻用來表示根本沒有韻母i的“絲”)。
在楓梓拼音中，他們不存在。這些被去除的異化和簡化具體有：
```
i   -> y,yi
u   -> w,wu
uen -> un,wen
uei -> ui,wei
iou -> iu,you
üin -> ün,yun
z   -> zi
c   -> ci
s   -> si
zh  -> zhi
ch  -> chi
sh  -> shi
r   -> ri
jü  -> ju
qü  -> qu
xü  -> xu
```

#### 2. 補充遺漏拼音

“普通話拼音”作爲普通話（漢語的一個主要分支）並未完整收錄漢語中的聲母和韻母，甚至普通話中存在的韻母“ㄝ”也被遺漏了。
楓梓拼音中會補充一些遺漏的拼音。目前補充工作尚未完成，如濁音聲母、入聲韻尾等尚有待補充。此外由於作者尚未廣泛學習各個分支的漢語，不能詳盡。如有補充，請惠提 issue，感激不盡。
目前補充的拼音有:
```
聲母: ñ("娘"的聲母), r(一些漢語分支中出現的平舌音，對應的卷舌音寫爲ř)，ŋ(一些漢語分支中“我"的聲母)
韻母: ə(ㄝ)
```

#### 3. 貼近發音

“普通話拼音”中有一些拼音並不能忠實地表現發音，如“奧”的韻尾爲 `u` 卻寫作了 `ao`。“多”和“破”的韻母完全一樣，卻分別寫爲 `duo` 和 `po`　。“內”，“南”，“月”有相同的韻腹，卻分別寫爲 `nei`, `nan` 和 `yue` 等。
楓梓拼音對其進行調整，以求精確，具體如下：
```
ao    ->  au
ei    ->  əi
üe    ->  üə
an    ->  ən
bo    ->  buo
po    ->  puo
mo    ->  muo
fo    ->  fuo
feng  ->  fong
ni    ->  ñi
nü    ->  ñü
iong  ->  üong
```
精細化之後，我們就能表達不同的口音。比如“南”在普通話中發 `nən` 而在東北話中，則發 `nan` 。“定”在普通話中爲 `ding` ，京腔中則爲 `dieng` 。

#### 4. 區分普通話所混淆的發音

前面三步都是在普通話的範圍內進行調整，而這一步將撿回普通話所丟失的。目前的情況爲

已完成

* [bpmf]eng和[bpmfong]的區分

進行中

* 尖團音的區分
* 前鼻音和脣鼻音的區分

區分尖團後，不再使用 `j`, `q`, `x` 三個聲母，而使用 `g`, `k`, `h` 和 `z`, `c`, `s` 六個韻母。如“西”和“希”將分別拼作 `si` 和 `hi`。

在詞庫中，由於無需要人工修改，而法像前三步一樣進行批量替換，循序漸進進行。

此外，仍然會保留不區分這些混淆發音的純普通話詞庫版本。



## 聲母

分爲六組，每組四個音
```
// 標準寫法
b組:  b, p, m, f,  脣音/脣-齒音
d組:  d, t, n, l,  舌尖-齦音
g組:  g, k, ŋ, h,  舌根音
j組:  j, q, ñ, x,  舌面-顎音
z組:  z, c, r, s,  舌尖-齒音
ž組:  ž, č, ř, š,  卷舌音
```

爲了便於在拉丁鍵盤上打出，對於非拉丁字母表示的拼音，還有拉丁寫法。
```
// 拉丁寫法
b組:  b,  p,  m,  f,
d組:  d,  t,  n,  l,
g組:  g,  k, ng,  h,
j組:  j,  q,  y,  x,
z組:  z,  c,  r,  s,
ž組: zh, ch, rh, sh,
```


## 介音

流行的拼音方案都吧介音歸入韻母。而作者認爲介音獨立出來，或者歸入聲母更爲洽當。
因爲介音不影響韻腹和韻尾的發音，卻會影響聲母的發音。（例如“端”的發音，並不似“的 - 彎”，而似“都 - 安”）
而又因爲聲調會影響韻母和介音，而不影響聲母。介音獨立又優於歸入聲母。

介音一共有三個
```
  u, i, ü,
```
`ü` 的拉丁寫法爲 `v`

加上零介音，構成了 開口，合口，齊齒，撮口 四呼

## 韻母

韻母細分爲 韻腹，韻尾 兩部分
```
韻腹4個
  a, e, ə, o,
韻尾9個
  i, u, m, n, ŋ, p, t, k, r,
```
`ə`, `ŋ` 的拉丁寫法分別爲 `w`, `ng`

可任意組合。

我們目前只關心普通話中存在的組合(包含介音)：

```
ia,ua,
iə,üə,
uo,üo,
ai,uai,
əi,uəi,
au,iau,
ou,iou,
en,uen,
ən,iən,üən,uən,
in,üin,
aŋ,iaŋ,uaŋ，
oŋ,ioŋ,uoŋ,
eŋ,
iŋ,
er,
```


# License

MIT
