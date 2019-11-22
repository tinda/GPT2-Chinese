# GPT2-Chinese

## Description

- Chinese version of GPT2 training code, using BERT tokenizer or BPE tokenizer. It is based on the extremely awesome repository from HuggingFace team [Transformers](https://github.com/huggingface/transformers). Can write poems, news, novels, or train general language models. Support char level, word level and BPE level. Support large training corpus.
- 中文的GPT2訓練代碼，使用BERT的Tokenizer或Sentencepiece的BPE model（感謝[kangzhonghua](https://github.com/kangzhonghua)的貢獻，實現BPE模式需要略微修改train.py的代碼）。可以寫詩，新聞，小說，或是訓練通用語言模型。支援字為單位或是分詞模式或是BPE模式（需要略微修改train.py的代碼）。支持大語料訓練。
- 微信交流群：請見Issue第一條。

## NEWS 11.9

- [GPT2-ML](https://github.com/imcaspar/gpt2-ml)（與本項目無任何直接關聯）已發佈，包含1.5B中文GPT2模型。大家如有興趣或需要可將其轉換為本項目支持的Pytorch格式進行進一步訓練或生成測試。

## UPDATE 10.25

- 本專案第一個預訓練模型已公佈，為散文生成模型，具體可查看README模型分享部分。

## 專案狀態

- 目前專案主要架構已經穩定。如發現任何bug或是有功能意見與改進歡迎提交Issue，PR或是聯繫作者。

## 使用方法

- 在專案根目錄建立data資料夾。將訓練語料以train.json為名放入data目錄中。**train.json裡是一個json清單，清單的每個元素都分別是一篇要訓練的文章的文本內容（而不是檔連結）**。
- 運行train.py檔，勾選 --raw ，會自動預處理資料。
- 預處理完成之後，會自動執行訓練。

### 生成文本

``` bash
python ./generate.py --length=50 --nsamples=4 --prefix=xxx --fast_pattern --save_samples --save_samples_path=/mnt/xx
```
- **--fast_pattern** (由[LeeCP8](https://github.com/LeeCP8)貢獻）：如果生成的length參數比較小，速度基本無差別，我個人測試length=250時，快了2秒，所以如果不添加--fast_pattern，那麼預設不採用fast_pattern方式。
- **--save_samples**：預設將輸出樣本直接列印到控制台，傳遞此參數，將保存在根目錄下的**samples.txt**。
- **--save_samples_path**：可自行指定保存的目錄，預設可遞迴創建多級目錄，不可以傳遞檔案名稱，檔案名稱默認為**samples.txt**。

## 檔結構

- generate.py 與 train.py 分別是生成與訓練的腳本。
- train_single.py 是 train.py的延伸，可以用於一個很大的單獨元素清單（如訓練一本鬥破蒼穹書）。
- eval.py 用於評估生成模型的ppl分值。
- generate_texts.py 是 generate.py 的延伸，可以以一個清單的起始關鍵字分別生成若干個句子並輸出到檔中。
- train.json 是訓練樣本的格式範例，可供參考。
- cache 資料夾內包含若干BERT詞表，make_vocab.py 是一個協助在一個train.json語料文件上建立詞表的腳本。 vocab.txt 是原始BERT詞表， vocab_all.txt 額外添加了古文詞， vocab_small.txt 是小詞表。
- tokenizations 資料夾內是可以選用的三種tokenizer，包括默認的Bert Tokenizer，分詞版Bert Tokenizer以及BPE Tokenizer。 
- scripts 內包含了樣例訓練與生成腳本

## 注意

- 本專案使用Bert的tokenizer處理中文字元。
- 如果使用分詞版的tokenizer，不需要自己事先分詞，tokenizer會幫你分。
- 如果使用分詞版的tokenizer，最好先使用cache資料夾內的make_vocab.py文件建立針對你的語料的詞表。
- 模型需自行運算。各位如果完成了預訓練的話歡迎進行交流。
- 如果你的記憶體非常大或者語料較小的話，可以改掉train.py內build files內的對應代碼，不做拆分直接預處理語料。
- 若使用BPE Tokenizer，需自己建立中文詞表

## 語料

- 可以從[這裡](https://github.com/brightmart/nlp_chinese_corpus)與[這裡](http://thuctc.thunlp.org/#獲取連結)下載。
- 鬥破蒼穹語料可以從[這裡](https://github.com/GaoPeng97/transformer-xl-chinese/tree/master/data/doupo)下載。

## FP16與Gradient Accumulation支持

- 我在train.py檔中加入了fp16與gradient accumulation支持，如果你安裝了apex並且知道fp16是什麼的話，可以修改變數fp16=True來啟用。但是目前fp16可能不收斂，原因不明。

## 聯繫作者

- Mail：ned1991@gmail.com

## Citing

```
@misc{GPT2-Chinese,
  author = {Zeyao Du},
  title = {GPT2-Chinese: Tools for training GPT2 model in Chinese language},
  year = {2019},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/Morizeyao/GPT2-Chinese}},
}
```

## 模型分享
|  模型名稱 |   模型介紹|   分享者|  連結位址1 |  連結位址2 |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| 散文模型  | 使用130MB的名家散文、情感散文和散文詩歌訓練所得 。  |  [hughqiu](https://github.com/hughqiu "hughqiu") | [百度網盤【fpyu】](https://pan.baidu.com/s/1nbrW5iw34GRhoTin8uU2tQ)   | [GDrive](https://drive.google.com/drive/folders/1rJC4niJKMVwixUQkuL9k5teLRnEYTmUf?usp=sharing "GDrive") |


此處為熱情大方的git友訓練所得的模型檔，公開給所有朋友使用，同時也歡迎各位夥伴將自己訓練完畢的模型公開於此處。

## Demo

- 由用戶[JamesHujy](https://github.com/JamesHujy)根據本倉庫改版代碼訓練得到的模型作為律詩與絕句後臺，新版[九歌詩歌生成器](https://jiuge.thunlp.cn/lvshi.html)已經上線。
- 由[leemengtaiwan](https://github.com/leemengtaiwan)貢獻，提供[文章直觀介紹 GPT-2 以及如何視覺化自注意力機制](https://leemeng.tw/gpt2-language-model-generate-chinese-jing-yong-novels.html)。另提供 [Colab 筆記本與模型](https://colab.research.google.com/drive/1MaT8-HUHfZkdCra0OqZEIr0IFCq0MJBx)供任何使用者一鍵生成新樣例。

## 生成樣例

-以下為文學散文的生成樣例，由[hughqiu](https://github.com/hughqiu "hughqiu")貢獻，模型已經分享於模型分享清單。語料130MB，Batch size 16，10層深度下訓練10輪所得。
![avatar](sample/散文1.png)
![avatar](sample/散文2.png)
![avatar](sample/散文3.png)

- 下為鬥破蒼穹的生成樣例，使用約50M參數的GPT2以32Batch Size在16MB鬥破蒼穹小說內容上訓練得到。此處[SEP]表示換行。

![avatar](sample/doupo.jpeg)

- 下為古詩詞的生成樣例，由用戶[JamesHujy](https://github.com/JamesHujy)運算並貢獻。

![avatar](sample/poem_1.png)
![avatar](sample/poem_2.png)

- 下為古詩限定了生成體裁後的生成樣例，由用戶[JamesHujy](https://github.com/JamesHujy)運算並貢獻。

![avatar](sample/律詩絕句.png)
![avatar](sample/浣溪沙_江城子.png)
![avatar](sample/蝶戀花_滿江紅.png)

- 下為生成劇本的樣例文本，由使用者[chiangandy](https://github.com/chiangandy)運算並貢獻

[starttext]愛情遊戲劇情講述了鋼琴父女明致懷萌的愛情、個有著努力的熱情以及現實為人生的價值觀眾，獲得一系列愛情的故事。80後錄股媒體受到網友分享，是2014年主創陳拉昀出品牌總監于藍氏集團化驗師創業團門的哥哥大國度上海淮河畔，集入第一線公司青年度雖然沒有放到的事業，但是藍正是卻不到位主人拒絕瞭解，而在藍越的幫助理念出現，也因此開啟明朗的誤會而經營變成愛河。在一次偶然的編劇集電視劇之夏天上一改變了自命運環球頂樑，三人在創車禍中不知被記憶差網識分到創作，並被問流言敗，以及行業服務所有的低調教同才力，陳昭和唐詩詩妍展開了一段截然不同的“2014年間段感情”，兩人性格互相治癒的商業奮鬥故事，儘管是共90後北京華僑大學錄的一個宿舍小旅程和唐如、生等優秀青年，的人生活如何與願違3個國偶像，並且共同創作何以此他們互相有觀眾的成功和關心嗎?[endtext]

[starttext]學習愛情主要講述了兩對方小曼，經過啼笑皆非的考驗，終於選擇了三個孩子，攜手共同創業來四個孩子，在大城市裡創業的成功商。兩家內事業的加入了北京城市，經過了一次元城市融風雨故、差異後得到異的他們，最終收穫了夢想的真正屬於自己的愛情。贊助理想、電視劇、劇等主創業時代人物特點在北京舉行開機儀式，該劇以當下海南三個新人青年輕人面人海南梅竹馬的電視角，講述了幾個在北京、喜劇代人生活中增強非浪漫的年輕人，以獨特的雙時代年輕人從來到北京城市化中國大城市走出發展以海南方的變遷在語種城市闖關於人生態的同時，以及他們漸漸的生活方式為自己方向上演了那麼簡單俗，是當代際拍攝的就如何在這個城市裡都市里?那麼平靜的城市就是城市的風格特張嘉和支援工作打造，而這是一點就要打造出機場話劇組會。化身處處棋逢貌各種文化的人都非常獨特的煽情，交織了相，滑稽等來自外衣的東北漂亮、內地，者和兩位女孩子敢稱是啞女孩子。交織裡的人齊飛一開泰塊玩笑，令人印象太趨的氣質，讓人眼看這個性格非常喜劇，知道的是一個“東北漂”人的外國小養家，讓她耳熟練讀劇的外形象顯老大。之後齊飛、表示愛朗的齊飛、范兒、楚月子、白天傑。兩代人的生活裡友情似乎沒有結合、精彩表態的開朗和麗麗麗。[endtext]

- 下為金庸武俠小說的生成樣例，由[leemengtaiwan](https://github.com/leemengtaiwan)貢獻。模型大小約 82M，語料 50 MB，Batch size 16。提供[文章直觀介紹 GPT-2 以及如何視覺化自注意力機制](https://leemeng.tw/gpt2-language-model-generate-chinese-jing-yong-novels.html)。另提供 [Colab 筆記本與模型](https://colab.research.google.com/drive/1MaT8-HUHfZkdCra0OqZEIr0IFCq0MJBx)供任何使用者一鍵生成新樣例。

![avatar](sample/金庸_天龍八部.jpg)
![avatar](sample/金庸_倚天屠龍記.jpg)
![avatar](sample/金庸_鹿鼎記.jpg)
![avatar](sample/金庸_神鵰俠侶.jpg)




