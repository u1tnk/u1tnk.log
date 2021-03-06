+++
title = "EC2インスタンス価格の必要なところだけ表にしてみた"
date = "2014-04-17"
categories = ["aws", "money"]
+++
##  何？
EC2の価格表で以下が欲しかったので作った。

2月にほぼ手作業で同じようなデータ作ったけど、大幅値下げで既に使いものにならなくなったので業を煮やした。いや値下げは嬉しいんだけど。

同様にネットで見かけるまとめも見やすいものは古くなってたり…

以下公式の不満。

- 一画面でECUとかGiBとか比較できない
- リザーブドインスタンスにしたときのならし月額(一括払いを月額に按分して時間分と足す)が欲しい(見積りツールにはある)
- オンデマンドとリザーブドインスタンスどんぐらい違うか見たい
- ECUあたりとかメモリあたりの単価も比較したい
- 3年とかさすがに使えないのでいらない
- 中度とか軽度買うぐらいならオンデマンドで良いのでいらない
- このページのもデザイン的に見やすくないのでSpreadSheetなりに貼りつけて利用を推奨。(以下リンク先のCSSゼロの方がまだ見やすいという…)
    - やはりみづらいのでgoogle spread sheetに貼りつけた https://docs.google.com/spreadsheets/d/1QzsUHO4Nd07TL5FAnVM2yvLRa3uOslY7pDTrmhwuAf0/edit?usp=sharing

## どうやった？
以下ページのJSでHTML化。このページのはスナップショットとしてコピペ。
https://dl.dropboxusercontent.com/u/3400756/aws_price.html

- 料金表のページのソースを見るとJSONPで呼んでたので使わせてもらった。おそらく外部利用を想定してないので止まっても文句は言いません。(見れなくなったら困るのでコピペった)


## 表について
- RI = リザーブドインスタンス一年重度使用
- OD = オンデマンド(普通)
- m1.small比OD価格のm1.small比。
- 安い順にソートしている。ファミリー毎の方がコスパとか見やすかったかも。


## 内容感想
- m1.medium/m1.largeが消えてた…別ページにあるんだろうけど。
- m1.small、特殊用途のhs以外全てSSD化してた。HDDは死んだ…
- m1.small、単価見ると高い！しょぼい用途には使うけど…
- m3.mediumも2月時点のm1.small(RIならしで約$34だった)と大差無い。
- 2月時点のm1.mediumよりC3.largeが安いってw m1.mediumのリザーブドインスタンスどうしろと…
- c3.largeがオンデマンドでも月一万切る上、メモリ/ECU共にコスパ高い!
- large以上は特段コスパ良くなったりはしない。
- m3の中途半端さ。largeならc3かr3使うと思う。


## 4/17時点の価格表
<table id="result" border="1">
    <tbody><tr>
        <th>size</th>
        <th>vCPU</th>
        <th>ECU</th>
        <th>memoryGiB</th>
        <th>storageGB</th>
        <th>RI一括</th>
        <th>RI/時間</th>
        <th>RIならし月額</th>
        <th>OD時間</th>
        <th>OD月額</th>
        <th>OD m1.small比</th>
        <th>OD月額/ECU</th>
        <th>OD月額/GiB</th>
    </tr>
<tr><td align="left">t1.micro</td><td align="right">1</td><td align="right">variable</td><td align="right">0.615</td><td align="right">ebsonly</td><td align="right">62</td><td align="right">0.009</td><td align="right">11.64</td><td align="right">0.026</td><td align="right">18.72</td><td align="right">0.42</td><td align="right">NaN</td><td align="right">30.43</td></tr>
<tr><td align="left">m1.small</td><td align="right">1</td><td align="right">1</td><td align="right">1.7</td><td align="right">1 x 160</td><td align="right">135</td><td align="right">0.017</td><td align="right">23.49</td><td align="right">0.061</td><td align="right">43.92</td><td align="right">1</td><td align="right">43.92</td><td align="right">25.83</td></tr>
<tr><td align="left">m3.medium</td><td align="right">1</td><td align="right">3</td><td align="right">3.75</td><td align="right">1 x 4 SSD</td><td align="right">244</td><td align="right">0.03</td><td align="right">41.93</td><td align="right">0.101</td><td align="right">72.72</td><td align="right">1.65</td><td align="right">24.24</td><td align="right">19.39</td></tr>
<tr><td align="left">c3.large</td><td align="right">2</td><td align="right">7</td><td align="right">3.75</td><td align="right">2 x 16 SSD</td><td align="right">357</td><td align="right">0.047</td><td align="right">63.59</td><td align="right">0.128</td><td align="right">92.16</td><td align="right">2.09</td><td align="right">13.16</td><td align="right">24.57</td></tr>
<tr><td align="left">m3.large</td><td align="right">2</td><td align="right">6.5</td><td align="right">7.5</td><td align="right">1 x 32 SSD</td><td align="right">487</td><td align="right">0.061</td><td align="right">84.5</td><td align="right">0.203</td><td align="right">146.16</td><td align="right">3.32</td><td align="right">22.48</td><td align="right">19.48</td></tr>
<tr><td align="left">r3.large</td><td align="right">2</td><td align="right">6.5</td><td align="right">15</td><td align="right">1 x 32 SSD</td><td align="right">777</td><td align="right">0.048</td><td align="right">99.31</td><td align="right">0.210</td><td align="right">151.19</td><td align="right">3.44</td><td align="right">23.26</td><td align="right">10.08</td></tr>
<tr><td align="left">c3.xlarge</td><td align="right">4</td><td align="right">14</td><td align="right">7.5</td><td align="right">2 x 40 SSD</td><td align="right">713</td><td align="right">0.094</td><td align="right">127.09</td><td align="right">0.255</td><td align="right">183.6</td><td align="right">4.18</td><td align="right">13.11</td><td align="right">24.48</td></tr>
<tr><td align="left">m3.xlarge</td><td align="right">4</td><td align="right">13</td><td align="right">15</td><td align="right">2 x 40 SSD</td><td align="right">973</td><td align="right">0.123</td><td align="right">169.64</td><td align="right">0.405</td><td align="right">291.6</td><td align="right">6.63</td><td align="right">22.43</td><td align="right">19.44</td></tr>
<tr><td align="left">r3.xlarge</td><td align="right">4</td><td align="right">13</td><td align="right">30.5</td><td align="right">1 x 80 SSD</td><td align="right">1554</td><td align="right">0.096</td><td align="right">198.62</td><td align="right">0.420</td><td align="right">302.39</td><td align="right">6.88</td><td align="right">23.26</td><td align="right">9.91</td></tr>
<tr><td align="left">c3.2xlarge</td><td align="right">8</td><td align="right">28</td><td align="right">15</td><td align="right">2 x 80 SSD</td><td align="right">1427</td><td align="right">0.188</td><td align="right">254.27</td><td align="right">0.511</td><td align="right">367.91</td><td align="right">8.37</td><td align="right">13.13</td><td align="right">24.52</td></tr>
<tr><td align="left">m3.2xlarge</td><td align="right">8</td><td align="right">26</td><td align="right">30</td><td align="right">2 x 80 SSD</td><td align="right">1948</td><td align="right">0.246</td><td align="right">339.45</td><td align="right">0.810</td><td align="right">583.2</td><td align="right">13.27</td><td align="right">22.43</td><td align="right">19.44</td></tr>
<tr><td align="left">r3.2xlarge</td><td align="right">8</td><td align="right">26</td><td align="right">61</td><td align="right">1 x 160 SSD</td><td align="right">3108</td><td align="right">0.192</td><td align="right">397.24</td><td align="right">0.840</td><td align="right">604.79</td><td align="right">13.77</td><td align="right">23.26</td><td align="right">9.91</td></tr>
<tr><td align="left">g2.2xlarge</td><td align="right">8</td><td align="right">26</td><td align="right">15</td><td align="right">60 SSD</td><td align="right">3406</td><td align="right">0.21</td><td align="right">435.03</td><td align="right">0.898</td><td align="right">646.55</td><td align="right">14.72</td><td align="right">24.86</td><td align="right">43.1</td></tr>
<tr><td align="left">i2.xlarge</td><td align="right">4</td><td align="right">14</td><td align="right">30.5</td><td align="right">1 x 800 SSD</td><td align="right">2670</td><td align="right">0.228</td><td align="right">386.66</td><td align="right">1.001</td><td align="right">720.71</td><td align="right">16.4</td><td align="right">51.48</td><td align="right">23.63</td></tr>
<tr><td align="left">c3.4xlarge</td><td align="right">16</td><td align="right">55</td><td align="right">30</td><td align="right">2 x 160 SSD</td><td align="right">2854</td><td align="right">0.375</td><td align="right">507.83</td><td align="right">1.021</td><td align="right">735.11</td><td align="right">16.73</td><td align="right">13.36</td><td align="right">24.5</td></tr>
<tr><td align="left">r3.4xlarge</td><td align="right">16</td><td align="right">52</td><td align="right">122</td><td align="right">1 x 320 SSD</td><td align="right">6216</td><td align="right">0.384</td><td align="right">794.48</td><td align="right">1.680</td><td align="right">1209.59</td><td align="right">27.54</td><td align="right">23.26</td><td align="right">9.91</td></tr>
<tr><td align="left">i2.2xlarge</td><td align="right">8</td><td align="right">27</td><td align="right">61</td><td align="right">2 x 800 SSD</td><td align="right">5339</td><td align="right">0.456</td><td align="right">773.23</td><td align="right">2.001</td><td align="right">1440.72</td><td align="right">32.8</td><td align="right">53.36</td><td align="right">23.61</td></tr>
<tr><td align="left">c3.8xlarge</td><td align="right">32</td><td align="right">108</td><td align="right">60</td><td align="right">2 x 320 SSD</td><td align="right">5708</td><td align="right">0.75</td><td align="right">1015.66</td><td align="right">2.043</td><td align="right">1470.96</td><td align="right">33.49</td><td align="right">13.62</td><td align="right">24.51</td></tr>
<tr><td align="left">r3.8xlarge</td><td align="right">32</td><td align="right">104</td><td align="right">244</td><td align="right">2 x 320 SSD</td><td align="right">12432</td><td align="right">0.768</td><td align="right">1588.96</td><td align="right">3.360</td><td align="right">2419.19</td><td align="right">55.08</td><td align="right">23.26</td><td align="right">9.91</td></tr>
<tr><td align="left">i2.4xlarge</td><td align="right">16</td><td align="right">53</td><td align="right">122</td><td align="right">4 x 800 SSD</td><td align="right">10677</td><td align="right">0.91</td><td align="right">1544.95</td><td align="right">4.002</td><td align="right">2881.44</td><td align="right">65.6</td><td align="right">54.36</td><td align="right">23.61</td></tr>
<tr><td align="left">hs1.8xlarge</td><td align="right">16</td><td align="right">35</td><td align="right">117</td><td align="right">24 x 2048</td><td align="right">12822</td><td align="right">1.317</td><td align="right">2016.73</td><td align="right">5.400</td><td align="right">3888</td><td align="right">88.52</td><td align="right">111.08</td><td align="right">33.23</td></tr>
<tr><td align="left">i2.8xlarge</td><td align="right">32</td><td align="right">104</td><td align="right">244</td><td align="right">8 x 800 SSD</td><td align="right">21354</td><td align="right">1.822</td><td align="right">3091.34</td><td align="right">8.004</td><td align="right">5762.88</td><td align="right">131.21</td><td align="right">55.41</td><td align="right">23.61</td></tr>
</tbody></table>




