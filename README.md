# compounds-that-inhibits-HIV-replication

binary classification

輸入feature為smile分子結構，輸出該分子有無對HIV活性，資料來源於http://moleculenet.ai/datasets-1 , https://wiki.nci.nih.gov/display/NCIDTPdata/AIDS+Antiviral+Screen+Data 。

除了使用rdtoolkit取得該分子的分子量、tpsa、價電子數量、異原子數量等，將smile分子結構使用[mol2vec](https://pubs.acs.org/doi/10.1021/acs.jcim.7b00616)可得到300維vector。
[mol2vec](https://pubs.acs.org/doi/10.1021/acs.jcim.7b00616)是透過word embedding方法，使用morgan fingerprint取得的各分子部位結構作為單字，一個分子視一個句子，建立neural 
network去預測句子中所空缺的單子(CBOW)或用單字預測去預測所連接的句子(skip-graw)。經過大量分子訓練後，neural network轉換分子的morgan fingerprint形成一個300維的vector。

比較Active和Non-active的數量差異，顯然Non-active數量稀少，應以True positive rate來評價模型較為合適。模型Xgboost中使用max_delta_step來減緩case imbalance之影響。


下圖為所建立之pipeline，Feature selection 考慮以下兩點:

1. 因rdtoolkit所取得的分子性質是有可能涵蓋在mol2vec所得到的300維度中，因此試著比較將分子性質排除之後的結果。

2. 試著用pca下降mol2vec所得到的300維度，以減緩overfit。

最後將結果儲存於MongoDB。


