Testing Marimo stopwords on a corpus of traditional Chinese HK Gov press
releases, tokenized using quanteda::tokens
================
Chung-hong Chan

``` r
require(rio)
require(quanteda)
require(tidyverse)
hk_pr <- corpus(rio::import("hongkonggov_press_releases.csv")$content)
saveRDS(hk_pr, "hk_pr.RDS")
```

``` r
require(rio)
```

    ## Loading required package: rio

``` r
require(quanteda)
```

    ## Loading required package: quanteda

    ## Package version: 2.0.0

    ## Parallel computing: 2 of 4 threads used.

    ## See https://quanteda.io for tutorials and examples.

    ## 
    ## Attaching package: 'quanteda'

    ## The following object is masked from 'package:rio':
    ## 
    ##     convert

    ## The following object is masked from 'package:utils':
    ## 
    ##     View

``` r
require(tidyverse)
```

    ## Loading required package: tidyverse

    ## ── Attaching packages ────────────────────────────────── tidyverse 1.3.0 ──

    ## ✔ ggplot2 3.3.0     ✔ purrr   0.3.3
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.5
    ## ✔ tidyr   1.0.2     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ───────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
hk_pr <- tokens(readRDS('hk_pr.RDS'))
hk_pr
```

    ## Tokens consisting of 47,849 documents.
    ## text1 :
    ##  [1] "差"   "餉"   "及"   "地租" "須"   "於"   "七月" "三十" "一日" "或"  
    ## [11] "之前" "繳交"
    ## [ ... and 399 more ]
    ## 
    ## text2 :
    ##  [1] "社會" "保障" "受"   "助人" "獲"   "發"   "一筆" "過"   "額外" "援助"
    ## [11] "金"   "＊"  
    ## [ ... and 273 more ]
    ## 
    ## text3 :
    ##  [1] "酒"   "牌局" "周二" "舉行" "會議" "＊"   "＊"   "＊"   "＊"   "＊"  
    ## [11] "＊"   "＊"  
    ## [ ... and 152 more ]
    ## 
    ## text4 :
    ##  [1] "賽馬" "日"   "沙田" "特別" "交通" "措施" "＊"   "＊"   "＊"   "＊"  
    ## [11] "＊"   "＊"  
    ## [ ... and 144 more ]
    ## 
    ## text5 :
    ##  [1] "泳"   "灘"   "懸掛" "紅旗" "＊"   "＊"   "＊"   "＊"   "＊"   "＊"  
    ## [11] "電台" "及"  
    ## [ ... and 79 more ]
    ## 
    ## text6 :
    ##  [1] "香港"     "特別"     "行政區"   "第"       "四屆"     "政府"    
    ##  [7] "就職典禮" "（"       "附圖"     "／"       "短"       "片"      
    ## [ ... and 147 more ]
    ## 
    ## [ reached max_ndoc ... 47,843 more documents ]

``` r
stopwords <- unlist(yaml::read_yaml("stopwords_zh_traditional.yml"))
chinese_stopwords <- as.list(unlist(yaml::read_yaml("stopwords_zh_traditional.yml")))
names(chinese_stopwords) <- chinese_stopwords
matched_words <- tokens_lookup(hk_pr, dictionary(chinese_stopwords))
```

Used stopwords

``` r
matched_words %>% dfm %>% textstat_frequency -> stopwords_freq
stopwords_freq %>% knitr::kable()
```

| feature | frequency | rank | docfreq | group |
| :------ | --------: | ---: | ------: | :---- |
| 的       |    866354 |    1 |   42826 | all   |
| 及       |    260642 |    2 |   38124 | all   |
| 在       |    202865 |    3 |   36213 | all   |
| 和       |    145437 |    4 |   27595 | all   |
| 二       |    111175 |    5 |   26018 | all   |
| 我們      |     94632 |    6 |   11887 | all   |
| 年       |     89817 |    7 |   44733 | all   |
| 時       |     87819 |    8 |   44733 | all   |
| 是       |     84829 |    9 |   20642 | all   |
| 為       |     79801 |   10 |   25950 | all   |
| 一       |     78732 |   11 |   23113 | all   |
| 有       |     74381 |   12 |   21183 | all   |
| 日       |     73463 |   13 |   44733 | all   |
| 或       |     72517 |   14 |   22398 | all   |
| 與       |     64936 |   15 |   21365 | all   |
| 有關      |     57112 |   16 |   21088 | all   |
| 以       |     56897 |   17 |   21932 | all   |
| 亦       |     56271 |   18 |   19556 | all   |
| 月       |     56106 |   19 |   44733 | all   |
| 分       |     52983 |   20 |   44733 | all   |
| 至       |     52250 |   21 |   19429 | all   |
| 於       |     51883 |   22 |   22959 | all   |
| 我       |     45979 |   23 |    8634 | all   |
| 並       |     39609 |   24 |   20295 | all   |
| 中       |     39120 |   25 |   16716 | all   |
| 對       |     38385 |   26 |   16754 | all   |
| 上       |     37741 |   27 |   17351 | all   |
| 就       |     37268 |   28 |   14752 | all   |
| 該       |     36924 |   29 |   16037 | all   |
| 由       |     35234 |   30 |   17221 | all   |
| 後       |     34819 |   31 |   16654 | all   |
| 而       |     34666 |   32 |   16300 | all   |
| 下       |     33755 |   33 |   18703 | all   |
| 以及      |     33494 |   34 |   15770 | all   |
| 向       |     31322 |   35 |   15538 | all   |
| 內       |     29093 |   36 |   14779 | all   |
| 三       |     28495 |   37 |   13644 | all   |
| 到       |     24767 |   38 |   10928 | all   |
| 其他      |     24748 |   39 |   12996 | all   |
| 這       |     21073 |   40 |    7454 | all   |
| 前       |     21012 |   41 |   12038 | all   |
| 其       |     20503 |   42 |   11077 | all   |
| 說       |     19707 |   43 |   10629 | all   |
| 根據      |     19119 |   44 |   10786 | all   |
| 如       |     19098 |   45 |   10568 | all   |
| 但       |     18777 |   46 |    8741 | all   |
| 四       |     18558 |   47 |   10575 | all   |
| 這個      |     18137 |   48 |    5281 | all   |
| 透過      |     16987 |   49 |   10120 | all   |
| 多       |     16257 |   50 |    8849 | all   |
| 這些      |     16130 |   51 |    7844 | all   |
| 一些      |     15760 |   52 |    5305 | all   |
| 不       |     15521 |   53 |    8307 | all   |
| 報告      |     15106 |   54 |    8458 | all   |
| 提出      |     14988 |   55 |    6636 | all   |
| 任何      |     13993 |   56 |    8256 | all   |
| 表示      |     13816 |   57 |   10207 | all   |
| 所以      |     13735 |   58 |    4285 | all   |
| 受       |     13708 |   59 |    7108 | all   |
| 他       |     13688 |   60 |    7032 | all   |
| 所有      |     13628 |   61 |    8653 | all   |
| 都       |     13473 |   62 |    5213 | all   |
| 非       |     12964 |   63 |    5739 | all   |
| 期間      |     12889 |   64 |    8298 | all   |
| 經       |     12471 |   65 |    7224 | all   |
| 否       |     12351 |   66 |    4589 | all   |
| 均       |     12244 |   67 |    8366 | all   |
| 共       |     12241 |   68 |    7646 | all   |
| 五       |     11815 |   69 |    7510 | all   |
| 若       |     11804 |   70 |    5720 | all   |
| 更       |     11752 |   71 |    7233 | all   |
| 因為      |     11290 |   72 |    4765 | all   |
| 從       |     11189 |   73 |    7465 | all   |
| 星期三     |     11093 |   74 |   10241 | all   |
| 由於      |     11042 |   75 |    8831 | all   |
| 星期五     |     11017 |   76 |    9901 | all   |
| 七月      |     11007 |   77 |    6842 | all   |
| 入       |     10941 |   78 |    4177 | all   |
| 十二月     |     10841 |   79 |    6259 | all   |
| 六月      |     10791 |   80 |    6497 | all   |
| 外       |     10761 |   81 |    6324 | all   |
| 三月      |     10572 |   82 |    6445 | all   |
| 一月      |     10482 |   83 |    6323 | all   |
| 九月      |     10473 |   84 |    6097 | all   |
| 十月      |     10453 |   85 |    5990 | all   |
| 進一步     |     10397 |   86 |    7391 | all   |
| 四月      |     10327 |   87 |    6134 | all   |
| 令       |     10299 |   88 |    6071 | all   |
| 自       |     10149 |   89 |    6695 | all   |
| 可能      |     10082 |   90 |    5990 | all   |
| 六       |     10054 |   91 |    6322 | all   |
| 十一月     |      9898 |   92 |    5874 | all   |
| 五月      |      9875 |   93 |    5963 | all   |
| 涉及      |      9816 |   94 |    5633 | all   |
| 其中      |      9510 |   95 |    7212 | all   |
| 前往      |      9421 |   96 |    4701 | all   |
| 作為      |      9366 |   97 |    6074 | all   |
| 也       |      9311 |   98 |    4598 | all   |
| 很       |      9306 |   99 |    4380 | all   |
| 再       |      9010 |  100 |    5508 | all   |
| 如何      |      8989 |  101 |    4919 | all   |
| 二月      |      8955 |  102 |    5217 | all   |
| 更多      |      8915 |  103 |    6143 | all   |
| 超過      |      8894 |  104 |    5533 | all   |
| 因此      |      8748 |  105 |    5376 | all   |
| 八月      |      8614 |  106 |    5092 | all   |
| 你       |      8460 |  107 |    2390 | all   |
| 七       |      8351 |  108 |    5280 | all   |
| 因       |      8340 |  109 |    5977 | all   |
| 星期四     |      8273 |  110 |    7532 | all   |
| 年度      |      8261 |  111 |    3268 | all   |
| 晚上      |      8198 |  112 |    3546 | all   |
| 星期一     |      8070 |  113 |    7164 | all   |
| 九       |      7783 |  114 |    4453 | all   |
| 通過      |      7732 |  115 |    4258 | all   |
| 星期二     |      7708 |  116 |    7031 | all   |
| 如果      |      7484 |  117 |    3126 | all   |
| 八       |      7465 |  118 |    4913 | all   |
| 非常      |      7263 |  119 |    4293 | all   |
| 將會      |      7260 |  120 |    4661 | all   |
| 甚麼      |      5950 |  121 |    2754 | all   |
| 屬       |      5906 |  122 |    4259 | all   |
| 星期六     |      5881 |  123 |    5155 | all   |
| 至於      |      5764 |  124 |    4209 | all   |
| 過       |      5473 |  125 |    3636 | all   |
| 最       |      5469 |  126 |    3669 | all   |
| 個別      |      5192 |  127 |    3473 | all   |
| 關於      |      5055 |  128 |    3563 | all   |
| 星期日     |      5027 |  129 |    4590 | all   |
| 同       |      4582 |  130 |    3466 | all   |
| 之間      |      4514 |  131 |    2750 | all   |
| 往       |      4483 |  132 |    2364 | all   |
| 成為      |      4474 |  133 |    3266 | all   |
| 她       |      4187 |  134 |    2190 | all   |
| 除       |      4180 |  135 |    3420 | all   |
| 給       |      4141 |  136 |    2573 | all   |
| 之前      |      4036 |  137 |    3009 | all   |
| 對於      |      3938 |  138 |    2605 | all   |
| 應該      |      3632 |  139 |    2284 | all   |
| 跟       |      3520 |  140 |    2101 | all   |
| 當時      |      3455 |  141 |    2325 | all   |
| 又       |      3439 |  142 |    2672 | all   |
| 講       |      3260 |  143 |    1591 | all   |
| 直至      |      3223 |  144 |    2932 | all   |
| 不是      |      3200 |  145 |    1769 | all   |
| 進入      |      3054 |  146 |    2079 | all   |
| 或者      |      2966 |  147 |    1387 | all   |
| 之後      |      2948 |  148 |    1974 | all   |
| 為了      |      2865 |  149 |    2380 | all   |
| 全部      |      2845 |  150 |    2229 | all   |
| 完全      |      2833 |  151 |    2144 | all   |
| 擁有      |      2809 |  152 |    1826 | all   |
| 怎樣      |      2748 |  153 |    1432 | all   |
| 然後      |      2673 |  154 |    2078 | all   |
| 只       |      2638 |  155 |    2184 | all   |
| 那       |      2458 |  156 |    1406 | all   |
| 他的      |      2338 |  157 |    1565 | all   |
| 但是      |      2313 |  158 |    1284 | all   |
| 不過      |      2268 |  159 |    1675 | all   |
| 比       |      2211 |  160 |    1617 | all   |
| 大量      |      2139 |  161 |    1754 | all   |
| 報道      |      2130 |  162 |    1433 | all   |
| 你們      |      2096 |  163 |    1133 | all   |
| 然而      |      2000 |  164 |    1594 | all   |
| 十       |      1994 |  165 |    1440 | all   |
| 我的      |      1985 |  166 |    1376 | all   |
| 其餘      |      1822 |  167 |    1626 | all   |
| 匯報      |      1816 |  168 |    1077 | all   |
| 只是      |      1745 |  169 |    1286 | all   |
| 裏面      |      1701 |  170 |     709 | all   |
| 少       |      1699 |  171 |    1256 | all   |
| 中午      |      1669 |  172 |    1232 | all   |
| 反對      |      1638 |  173 |     939 | all   |
| 相同      |      1632 |  174 |    1388 | all   |
| 少數      |      1598 |  175 |     699 | all   |
| 離       |      1593 |  176 |    1044 | all   |
| 屬於      |      1584 |  177 |    1234 | all   |
| 早上      |      1521 |  178 |    1238 | all   |
| 同樣      |      1477 |  179 |    1264 | all   |
| 曾經      |      1467 |  180 |    1245 | all   |
| 每一      |      1438 |  181 |    1081 | all   |
| 何時      |      1428 |  182 |    1042 | all   |
| 差       |      1366 |  183 |     568 | all   |
| 即使      |      1240 |  184 |    1021 | all   |
| 復       |      1236 |  185 |     621 | all   |
| 處於      |      1133 |  186 |     877 | all   |
| 之外      |      1115 |  187 |     929 | all   |
| 以免      |      1065 |  188 |    1021 | all   |
| 最多      |      1029 |  189 |     839 | all   |
| 那些      |       983 |  190 |     713 | all   |
| 以前      |       966 |  191 |     782 | all   |
| 你的      |       908 |  192 |     518 | all   |
| 她的      |       881 |  193 |     650 | all   |
| 之下      |       870 |  194 |     654 | all   |
| 年代      |       864 |  195 |     565 | all   |
| 如此      |       858 |  196 |     743 | all   |
| 太       |       836 |  197 |     585 | all   |
| 像       |       835 |  198 |     703 | all   |
| 透露      |       813 |  199 |     761 | all   |
| 覆蓋      |       778 |  200 |     501 | all   |
| 兼       |       758 |  201 |     628 | all   |
| 大概      |       678 |  202 |     497 | all   |
| 那個      |       673 |  203 |     413 | all   |
| 講述      |       666 |  204 |     521 | all   |
| 若干      |       664 |  205 |     505 | all   |
| 高於      |       661 |  206 |     503 | all   |
| 那麼      |       659 |  207 |     507 | all   |
| 皆       |       650 |  208 |     501 | all   |
| 正是      |       649 |  209 |     566 | all   |
| 各自      |       614 |  210 |     537 | all   |
| 一度      |       568 |  211 |     540 | all   |
| 極       |       559 |  212 |     468 | all   |
| 剩餘      |       535 |  213 |     404 | all   |
| 轉為      |       521 |  214 |     370 | all   |
| 以防      |       512 |  215 |     489 | all   |
| 世紀      |       484 |  216 |     389 | all   |
| 陳述      |       467 |  217 |     281 | all   |
| 之內      |       463 |  218 |     423 | all   |
| 中間      |       443 |  219 |     340 | all   |
| 向上      |       434 |  220 |     309 | all   |
| 多於      |       431 |  221 |     351 | all   |
| 之中      |       421 |  222 |     378 | all   |
| 彼此      |       401 |  223 |     371 | all   |
| 少量      |       379 |  224 |     323 | all   |
| 大多數     |       373 |  225 |     327 | all   |
| 也不      |       364 |  226 |     320 | all   |
| 沒       |       361 |  227 |     306 | all   |
| 朝       |       347 |  228 |     295 | all   |
| 頗       |       342 |  229 |     281 | all   |
| 換句話說    |       328 |  230 |     248 | all   |
| 假如      |       319 |  231 |     261 | all   |
| 靠       |       301 |  232 |     243 | all   |
| 換言之     |       282 |  233 |     266 | all   |
| 年份      |       278 |  234 |     213 | all   |
| 跟隨      |       263 |  235 |     216 | all   |
| 哪個      |       247 |  236 |     203 | all   |
| 誰       |       243 |  237 |     186 | all   |
| 也沒有     |       241 |  238 |     220 | all   |
| 哪一      |       234 |  239 |     209 | all   |
| 周六      |       232 |  240 |     204 | all   |
| 周二      |       222 |  241 |     216 | all   |
| 多數      |       216 |  242 |     183 | all   |
| 遠離      |       206 |  243 |     185 | all   |
| 部份      |       189 |  244 |     167 | all   |
| 屢       |       169 |  245 |     162 | all   |
| 之上      |       167 |  246 |     160 | all   |
| 掉       |       163 |  247 |     148 | all   |
| 若果      |       154 |  248 |     127 | all   |
| 對立      |       149 |  249 |     117 | all   |
| 直到      |       135 |  250 |     119 | all   |
| 過於      |       133 |  251 |     125 | all   |
| 周五      |       133 |  251 |     114 | all   |
| 什麼      |       126 |  253 |      87 | all   |
| 周日      |       124 |  254 |     108 | all   |
| 周一      |       121 |  255 |      93 | all   |
| 幾乎      |        96 |  256 |      91 | all   |
| 那樣      |        81 |  257 |      75 | all   |
| 去掉      |        80 |  258 |      79 | all   |
| 始於      |        70 |  259 |      68 | all   |
| 向下      |        67 |  260 |      53 | all   |
| 那時      |        61 |  261 |      54 | all   |
| 距       |        57 |  262 |      52 | all   |
| 除掉      |        56 |  263 |      56 | all   |
| 靠近      |        54 |  264 |      51 | all   |
| 幾時      |        50 |  265 |      43 | all   |
| 極其      |        45 |  266 |      44 | all   |
| 那一      |        43 |  267 |      42 | all   |
| 穿越      |        39 |  268 |      33 | all   |
| 牠       |        37 |  269 |      23 | all   |
| 從前      |        37 |  269 |      35 | all   |
| 周四      |        32 |  271 |      30 | all   |
| 報導      |        31 |  272 |      28 | all   |
| 又一      |        29 |  273 |      24 | all   |
| 周三      |        28 |  274 |      25 | all   |
| 往上      |        24 |  275 |      22 | all   |
| 裡面      |        15 |  276 |      12 | all   |
| 哪裡      |        11 |  277 |      11 | all   |
| 倚       |        11 |  277 |       9 | all   |
| 往下      |         6 |  279 |       6 | all   |
| 那裡      |         5 |  280 |       5 | all   |
| 何方      |         5 |  280 |       5 | all   |
| 那末      |         1 |  282 |       1 | all   |
| 朝著      |         1 |  282 |       1 | all   |

Unused stopwords

``` r
setdiff(stopwords, stopwords_freq$feature) ### mostly are those dative and processive pronouns
```

    ##  [1] "把我"     "對我"     "我自己"   "把我們"   "對我們"   "我們自己"
    ##  [7] "把你"     "對你"     "你自己"   "把你們"   "對你們"   "你們自己"
    ## [13] "把他"     "對他"     "他自己"   "把她"     "對她"     "她自己"  
    ## [19] "把牠"     "對牠"     "牠自己"   "我們的"   "你們的"   "牠的"    
    ## [25] "他們的"   "哪一個"   "那一個"   "誰人"     "誰人的"   "哪時"    
    ## [31] "哪裏"     "那裏"     "確有"     "或能"     "同樣地"   "迄"      
    ## [37] "這地"     "這處"     "這地方"   "這個地方" "那地"     "那處"    
    ## [43] "那地方"   "那個地方" "兩者都"   "更大量"   "大部份"   "固此"    
    ## [49] "在上"     "在下"     "對著"     "再一次"   "自己的"   "獨立地"  
    ## [55] "獨立的"   "本人的"   "相同的"   "同樣的"

Potentially missing stopwords

``` r
hkpr_top_features <- topfeatures(dfm(hk_pr), n = 500)
setdiff(names(hkpr_top_features), stopwords)
```

    ##   [1] "，"       "＊"       "。"       "）"       "（"       "、"      
    ##   [7] "香港"     "１"       "："       "會"       "「"       "」"      
    ##  [13] "２"       "０"       "；"       "政府"     "○"        "時間"    
    ##  [19] "局"       "署"       "》"       "《"       "發展"     "服務"    
    ##  [25] "生"       "人士"     "今日"     "中心"     "完"       "計劃"    
    ##  [31] "市民"     "/"        "包括"     "５"       "工作"     "３"      
    ##  [37] "已"       "提供"     "４"       "委員會"   "道"       "局長"    
    ##  [43] "了"       "一個"     "進行"     "區"       "人"       "立法"    
    ##  [49] "情況"     "措施"     "６"       "他們"     "可"       "%"       
    ##  [55] "等"       "將"       "港"       "？"       "處"       "二十"    
    ##  [61] "需要"     "活動"     "方面"     "個案"     "所"       "議員"    
    ##  [67] "特別"     "宗"       "條例"     "經濟"     "７"       "大家"    
    ##  [73] "問題"     "資料"     "事務"     "零"       "社會"     "應"      
    ##  [79] "申請"     "行"       "灣"       "繼續"     "-"        "內地"    
    ##  [85] "會議"     "８"       "相關"     "安排"     "被"       "新"      
    ##  [91] "可以"     "約"       "調查"     "食物"     "機構"     "灘"      
    ##  [97] "影響"     "泳"       "建議"     "環境"     "要"       "９"      
    ## [103] "地"       "第"       "發言人"   "者"       "個"       "不同"    
    ## [109] "安全"     "請"       "公眾"     "主席"     "以下"     "使用"    
    ## [115] "處理"     "人員"     "三年"     "須"       "大"       "交通"    
    ## [121] "希望"     "警方"     "意見"     "業"       "街"       "公司"    
    ## [127] "車輛"     "醫院"     "工程"     "按"       "名"       "沒有"    
    ## [133] "基金"     "病人"     "現時"     "行政長官" "記者"     "市場"    
    ## [139] "公布"     "作出"     "考慮"     "舉行"     "管"       "防護"    
    ## [145] "讓"       "政策"     "合作"     "本地"     "項目"     "社區"    
    ## [151] "國際"     "五年"     "加強"     "獲"       "文化"     "是否"    
    ## [157] "諮詢"     "教育"     "作"       "增加"     "管理"     "四年"    
    ## [163] "公共"     "商"       "將於"     "勞工"     "參與"     "／"      
    ## [169] "健康"     "地區"     "研究"     "綜合"     "正"       "一日"    
    ## [175] "為何"     "分別"     "元"       "─"        "學生"     "運輸"    
    ## [181] "乎"       "性"       "部分"     "出席"     "路"       "金融"    
    ## [187] "房屋"     "發出"     "今年"     "同時"     "各"       "下午"    
    ## [193] "土地"     "介"       "主要"     "感染"     "即"       "網頁"    
    ## [199] "線"       "曾"       "出現"     "部門"     "長者"     "詳情"    
    ## [205] "過去"     "屋"       "第二"     "修訂"     "行動"     "投資"    
    ## [211] "要求"     "設施"     "學校"     "當局"     "較"       "一二"    
    ## [217] "之"       "流感"     "支持"     "重要"     "兩"       "特區"    
    ## [223] "統計"     "十分"     "資助"     "則"       "起"       "號"      
    ## [229] "單位"     "採取"     "結果"     "去"       "協助"     "管理局"  
    ## [235] "最新"     "上午"     "水"       "場"       "地方"     "司長"    
    ## [241] "高"       "宣布"     "此外"     "開始"     "能夠"     "醫"      
    ## [247] "陳"       "小組"     "業界"     "實施"     "注意"     "南"      
    ## [253] "站"       "旅遊"     "企業"     "張"       "供應"     "整體"    
    ## [259] "各位"     "．"       "上述"     "已經"     "接受"     "例如"    
    ## [265] "法"       "評估"     "傳"       "當中"     "︰"       "本港"    
    ## [271] "產品"     "能"       "食"       "成立"     "專業"     "總"      
    ## [277] "科技"     "一直"     "完成"     "院"       "支援"     "一名"    
    ## [283] "推出"     "十二"     "東"       "保持"     "系統"     "改善"    
    ## [289] "上升"     "程序"     "保障"     "樣本"     "查詢"     "家"      
    ## [295] "國家"     "."        "把"       "條"       "呼籲"     "監察"    
    ## [301] "登記"     "億元"     "討論"     "必須"     "全文"     "期"      
    ## [307] "來"       "決定"     "用"       "數字"     "法律"     "規劃"    
    ## [313] "第一"     "檢"       "盡快"     "資訊"     "顯示"     "很多"    
    ## [319] "跟進"     "去年"     "規定"     "牌照"     "媒"       "推廣"    
    ## [325] "發現"     "比較"     "一年"     "代表"     "醫生"     "基本"    
    ## [331] "居民"     "避免"     "了解"     "確保"     "就業"     "機會"    
    ## [337] "推動"     "日期"     "檢討"     "病毒"     "剛才"     "貿易"    
    ## [343] "首"       "接觸"     "投標"     "個人"     "症"       "九龍"    
    ## [349] "未來"     "留意"     "司"       "醫療"     "風險"     "旅客"    
    ## [355] "三十"     "現"       "位於"     "持續"     "一段"     "合"      
    ## [361] "一六"     "十五"     "組織"     "標準"     "內容"     "其實"    
    ## [367] "相信"     "財政"     "梁"       "致電"     "控"       "青年"    
    ## [373] "涌"       "小"       "附圖"     "委"       "懷疑"     "民政"    
    ## [379] "樓"       "黃"       "禽"       "提升"