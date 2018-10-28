# JapaneseRiichiMahjongKit
|Author|Jianyang Tang (Thomas)|
|---|---
|E-mail|jian4yang2.tang1@gmail.com

# Contents
* [1. Game log crawler: GameLogCrawler](#crawl)
* [2. Game log processor: PreProcessing ](#pre)
* [3. Deterministic algorithms: Partition, WinWaitCal](#deter)

*** 

# <a name="crawl"></a>1. Game log crawler: GameLogCrawler
| function  | Description |
| --------- | ----------- |
| [db_show_tables()](#showtable) | Display the structure of the database. |
| [batch_crawl_refids(gr_level, ite=5)](#batchrefids) | Crawl game log referral ids of players who havn't been explored yet. The crawled ids will be then inserted into the TABLE refids. |
| [batch_crawl_levels(self, ite=5)](#batchlevels) | Crawl levels for players who havn't have the value for level and pt in the database. The crawled information will be updated in the TABLE players.|
| [batch_crawl_logs(self, gr_lv, ite=10)](#batchlogs) | Crawl game logs, in which players with level higher than gr_lv are involved. The game log will be inserted into TABLE log as text. |
| [db_get_logs_where_players_lv_gr(self, gr_lv)](#dblogs) | Return a generator of game logs, in which player with level higher than gr_lv are involved. |
| [prt_log_format(log)](#printlog) | Print the game log in a user friendly format |

### <a name="showtable"></a>db_show_tables()
```python
glc = GameLogCrawler()
glc.db_show_tables()
```
```console
TABLE 'player': 7497 rows
      ------------------------------------------- 
    | Column     Name       Type       Primary   |
    | -------------------------------------------|
    | 0          name       text       1         |
    | 1          level      text       0         |
    | 2          pt         text       0         |
    | 3          lv         INTEGER    0         |
    | 4          retrieved  BOOLEAN    0         |
      ------------------------------------------- 

TABLE 'refids': 75575 rows
      ------------------------------------------- 
    | Column     Name       Type       Primary   |
    | -------------------------------------------|
    | 0          ref        text       1         |
    | 1          p1         text       0         |
    | 2          p2         text       0         |
    | 3          p3         text       0         |
    | 4          p4         text       0         |
      ------------------------------------------- 

TABLE 'logs': 60 rows
      ------------------------------------------- 
    | Column     Name       Type       Primary   |
    | -------------------------------------------|
    | 0          refid      text       1         |
    | 1          log        text       0         |
      ------------------------------------------- 
```

### <a name="batchrefids"></a>batch_crawl_refids(gr_level, ite=5)]
```python
glc = GameLogCrawler()
glc.batch_crawl_refids(17, ite=1)
```
```console
Player Seria's level-十段 and pt-2000pt is updated.
   Refid 2018102710gm-00a9-0000-084f8165 - ['神風てぃっしゅ', 'トキ雀狂人戦代表', 'Seria', 'モロヘイヤ'] inserted into table refids
   Refid 2018102713gm-00a9-0000-ef452bb7 - ['真エトペン', '武威', 'Seria', 'pincers'] inserted into table refids
   Refid 2018102714gm-00a9-0000-bd0a7584 - ['銀色いがぐり', '木漏れ日', 'Seria', 'どいーん'] inserted into table refids
   Refid 2018102715gm-00a9-0000-4b3121a0 - ['ぷんぷん！', 'MrFeng', '早大ダメ修士', 'Seria'] inserted into table refids
   Refid 2018102715gm-00a9-0000-d5d084d8 - ['Seria', '懐く家畜', '★ホース★', 'とびまこ'] inserted into table refids
   Refid 2018102716gm-00a9-0000-14022cce - ['-★独眼鉄★-', '鈴侍', '至如信モノ', 'Seria'] inserted into table refids
   Refid 2018102716gm-00a9-0000-682ca8bc - ['Seria', 'びりびり☆ビリー', 'kazu-', 'ふじかつ'] inserted into table refids
   Refid 2018102717gm-00a9-0000-5385aa06 - ['Seria', 'ryo17', 'つらい。', 'とらいち'] inserted into table refids
   Refid 2018102718gm-00a9-0000-4c334959 - ['Hachinal', 'ちゃる子', 'Seria', 'のぶのぶ'] inserted into table refids
   Refid 2018102719gm-00a9-0000-5aae805b - ['Seria', 'G.K.', '生しらす', 'エスケー'] inserted into table refids
   Refid 2018102720gm-00a9-0000-260f8960 - ['Seria', 'アスタナ', 'くうたろ', 'MJの最高峰'] inserted into table refids
Player Seria's playing history was totally retrieved
```

### <a name="batchlevels"></a>batch_crawl_levels(self, ite=5)
```python
glc = GameLogCrawler()
glc.batch_crawl_levels(ite=1)
```
```console
Player 新世界の紙's level-七段 and pt-10pt is updated.
```

### <a name="batchlogs"></a>batch_crawl_logs(self, gr_lv, ite=10)
```python
glc = GameLogCrawler()
glc.batch_crawl_logs(17, ite=2)
```
```console
Game log of 2018102720gm-00a9-0000-260f8960 crawled and inserted into TABLE logs.
Game log of 2018102719gm-00a9-0000-5aae805b crawled and inserted into TABLE logs.
```

### <a name="dblogs"></a>db_get_logs_where_players_lv_gr(self, gr_lv)
```python
glc = GameLogCrawler()
log_generator = glc.db_get_logs_where_players_lv_gr(19)
for log in log_generator:
    print(log)
```
```console
{'ver': 2.3, 'ref': '2017120321gm-00a9-0000-34e876fb', 'log': [[[0, 0, 0], [25000, 25000, 25000, 25000], [46], [], [12, 13, 13, 17, 22, 22, 27, 34, 37, 42, 45, 46, 47], [32, 39, 44, 22, 47, 19, 11, 11, 35, 24, 11, 18, 16, 16, 24, 28, 21, 39], [42, 46, 60, 45, 27, 60, 13, 17, 32, 37, 39, 60, 60, 60, 35, 60, 34, 60], [15, 18, 19, 26, 28, 31, 31, 32, 33, 33, 53, 38, 38], [47, 21, 14, 13, 26, 17, 43, 36, 39, 27, 46, 41, 23, 26, 11, 12, 21, 22], [19, 60, 18, 47, 31, 60, 60, 28, 33, 39, 60, 60, 38, 38, 53, 31, 32, 36], [21, 23, 25, 25, 26, 27, 28, 29, 32, 36, 38, 42, 44], [27, 15, 38, 14, 45, 37, 37, 17, 33, 29, 43, 25, 46, 14, 35, 16, 37], [32, 44, 42, 36, 60, 60, 60, 60, 60, 60, 60, 38, 38, 46, 60, 29, 60], [12, 15, 17, 19, 23, 23, 52, 28, 29, 31, 36, 41, 43], [18, 13, 41, 24, 34, 14, 24, 34, 42, 12, 41, 19, 42, 18, 44, 39, 34], [43, 31, 15, 36, 60, 29, 'r28', 60, 60, 60, 60, 60, 60, 60, 60, 60, 60], ['流局', [-1000, -1000, -1000, 3000]]], [[1, 1, 1], [24000, 24000, 24000, 27000], [11], [42], [15, 22, 24, 24, 29, 32, 33, 34, 53, 43, 44, 45, 47], [35, 23, 22, 23, 12, 39, 34, 47, 46, 46, 25, 15, 32], [43, 29, 44, 47, 45, 60, 12, 60, 60, 60, 32, 25, 60], [14, 18, 19, 19, 21, 23, 52, 27, 28, 31, 38, 42, 46], [27, 17, 41, 38, 13, 42, 24, 43, 41, 22, 43, 47, 34, 33], [42, 31, 19, 46, 41, 60, 21, 60, 60, 13, 60, 60, 52, 28], [13, 16, 24, 27, 28, 31, 36, 37, 38, 39, 39, 44, 47], [32, 17, 25, 41, 26, 46, 21, 21, 23, 12, 36, 12, 37, 18], [44, 47, 31, 32, 13, 60, 60, 60, 'r41', 60, 60, 60, 60], [51, 16, 17, 19, 21, 25, 26, 28, 28, 29, 33, 38, 39], [37, 44, 44, 18, 26, 36, 18, 26, 31, 29, 35, 29, 15], [21, 60, 60, 33, 29, 39, 26, 60, 60, 60, 38, 60, 28], ['和了', [-2100, -4100, 10300, -2100], [2, 2, 2, '満貫2000-4000点', '立直(1飜)', '門前清自摸和(1飜)', '平和(1飜)', '三色同順(2飜)']]], [[2, 0, 0], [21900, 19900, 33300, 24900], [26], [], [14, 17, 18, 19, 22, 25, 26, 33, 34, 34, 44, 45, 46], [37, 17, 11, 12, 28, 43, 13, 36, 32], [44, 46, 60, 45, 12, 17, 60, 14, 37], [12, 14, 22, 23, 26, 27, 28, 29, 36, 42, 45, 45, 46], [52, 29, 22, 'p454545', 32, 29, '29p2929', 47, 12, 18], [36, 12, 14, 46, 60, 42, 23, 60, 60, 60], [14, 15, 16, 19, 24, 24, 25, 31, 35, 37, 41, 42, 43], [16, 23, 32, 21, 38, 42, 38, 31, 36], [31, 19, 42, 60, 43, 60, 32, 35, 38], [17, 24, 27, 28, 29, 35, 39, 39, 41, 41, 46, 46, 47], [13, 41, 13, 17, '46p4646', 16, 26, 42, 44, 37], [60, 47, 60, 35, 24, 17, 29, 60, 60, 60], ['和了', [0, -5200, 0, 5200], [3, 1, 3, '40符3飜5200点', '場風 東(1飜)', '役牌 發(1飜)', 'ドラ(1飜)']]], [[3, 0, 0], [21900, 14700, 33300, 30100], [24], [], [16, 22, 24, 25, 27, 33, 34, 35, 36, 42, 44, 46, 47], [32, 31, 16, 14, 33, 33], [44, 47, 42, 46, 22, 27], [14, 16, 23, 25, 29, 31, 32, 33, 35, 38, 42, 44, 45], [36, 26, 27, 29], [44, 42, 45, 38], [12, 12, 15, 17, 18, 19, 22, 23, 26, 28, 43, 45, 46], [21, 46, 43, '46p4646', 38], [43, 15, 45, 43, 60], [11, 11, 11, 14, 17, 22, 23, 24, 29, 32, 37, 37, 38], [21, 31, 44, 12, 13, 19], [29, 14, 60, 37, 17, 60], ['和了', [-1000, 0, 1000, 0], [2, 0, 2, '30符1飜1000点', '役牌 發(1飜)']]], [[4, 0, 0], [20900, 14700, 34300, 30100], [51], [], [12, 12, 13, 14, 14, 16, 18, 19, 22, 31, 35, 39, 41], [33, 39, 28, 23, 16, 44, 37, 39, 47, 27, 21, 46, 21, 21, 45, 33], [39, 60, 19, 41, 28, 60, 31, 60, 18, 60, 47, 60, 60, 60, 60, 60], [11, 12, 14, 15, 17, 17, 22, 32, 38, 38, 39, 46, 47], [11, 26, 19, 34, 24, 53, 34, 18, 45, 34, 29, 36, 23, 16, 42, 52], [32, 22, 47, 60, 46, 39, 11, 17, 60, 60, 60, 12, 11, 23, 60], [13, 15, 19, 19, 23, 23, 24, 28, 38, 41, 42, 44, 47], [14, 31, 25, 27, 46, 17, 35, 26, 26, 13, 37, 36, 45, 13, 16], [44, 60, 41, 42, 60, 38, 60, 47, 19, 60, 60, 60, 60, 60, 19], [15, 22, 22, 26, 29, 31, 32, 33, 34, 36, 37, 44, 47], [11, 27, 28, 18, 21, 44, 43, 42, 25, 24, 43, 33, 24, 32, 46], [29, 47, 44, 11, 60, 60, 60, 31, 18, 15, 60, 60, 60, 60, 60], ['和了', [-6000, 12000, -3000, -3000], [1, 1, 1, '跳満3000-6000点', '門前清自摸和(1飜)', '三色同順(2飜)', 'ドラ(1飜)', '赤ドラ(2飜)']]], [[5, 0, 0], [14900, 26700, 31300, 27100], [45], [42], [11, 12, 16, 18, 23, 27, 33, 33, 41, 43, 44, 44, 46], [26, 43, 17, 43, 34, 17, 29, 27, 46, 33, 22, 25, 16, 18], [43, 60, 41, 60, 23, 60, 34, 12, 11, 44, 44, 'r22', 60, 60], [51, 16, 19, 22, 28, 29, 36, 37, 39, 41, 42, 46, 47], [13, 41, 28, 'p414141', 17, 37, 32, 24, 24, 11, 23, 44, 16, 34, 21], [39, 19, 42, 46, 47, 22, 60, 13, 37, 60, 60, 60, 17, 16, 34], [12, 17, 19, 19, 21, 23, 25, 32, 34, 36, 37, 38, 43], [23, 24, 13, 32, 19, 53, 21, 47, 52, 42, 26, 29, 14, 33, 21], [43, 12, 60, 17, 34, 38, 'r23', 60, 60, 60, 60, 60, 60, 60], [13, 14, 15, 15, 18, 26, 31, 31, 35, 35, 37, 39, 45], [12, 22, 45, 22, 31, 41, 38, 11, 27, 28, 13, 12, 14, 28], [45, 18, 60, 39, 26, 60, 60, 15, 37, 12, 11, 13, 22, 22], ['和了', [-1300, -2600, 7200, -1300], [2, 2, 2, '40符3飜1300-2600点', '立直(1飜)', '門前清自摸和(1飜)', '赤ドラ(1飜)']]], [[6, 0, 0], [12600, 24100, 37500, 25800], [14], [21], [13, 15, 16, 17, 18, 23, 23, 25, 28, 31, 39, 41, 44], [16, 44, 37, 14, 24, 23, 18, 12, 11], [44, 60, 31, 41, 28, 39, 37, 'r18'], [12, 17, 18, 21, 24, 34, 53, 36, 37, 39, 45, 47, 47], [23, 35, 35, 42, 13, 25, 39, 11], [21, 12, 45, 60, 60, 39, 60, 37], [16, 19, 22, 24, 26, 29, 32, 34, 38, 42, 44, 46, 47], [11, 36, 33, 27, 31, 47, 14, 27, 29], [38, 29, 19, 11, 42, 46, 44, 36, 31], [15, 17, 19, 22, 22, 25, 26, 31, 37, 38, 39, 43, 46], [33, 46, 12, 42, 31, 'p464646', 19, 41, 32], [43, 31, 60, 33, 60, 42, 60, 60, 37], ['和了', [9000, -2000, -4000, -2000], [0, 0, 0, '満貫2000-4000点', '立直(1飜)', '一発(1飜)', '門前清自摸和(1飜)', '平和(1飜)', 'ドラ(1飜)']]], [[7, 0, 0], [20600, 22100, 33500, 23800], [25], [], [11, 11, 14, 14, 16, 21, 22, 25, 26, 31, 36, 38, 41], [29, 28, 19, 39, 35, 'c242526', 18, 24, 12, 24, 22, 14], [41, 31, 60, 60, 38, 14, 60, 21, 28, 29, 60, 11], [11, 51, 16, 22, 25, 28, 29, 31, 33, 42, 42, 43, 46], [24, 33, 38, 43, 34, 'c145116', 45, 53, 23, 44, 44, 42], [11, 46, 22, 38, 31, 28, 60, 33, 29, 60, 60], [13, 16, 18, 19, 22, 26, 31, 32, 33, 34, 37, 39, 39], [32, 41, 14, 17, 35, 34, 15, 15, 33, 36], [19, 22, 32, 41, 37, 60, 31, 39, 39, 32], [17, 18, 19, 23, 27, 29, 36, 39, 41, 45, 45, 46, 47], [12, 47, 11, 21, 36, 24, 38, '45p4545', 37, 27, 23, 18], [39, 23, 36, 60, 41, 60, 36, 38, 60, 46, 29, 60], ['和了', [-1300, 5200, -1300, -2600], [1, 1, 1, '40符3飜1300-2600点', '場風 南(1飜)', '赤ドラ(2飜)']]]], 'ratingc': 'PF4', 'rule': {'disp': '鳳南喰赤', 'aka53': 1, 'aka52': 1, 'aka51': 1}, 'lobby': 0, 'dan': ['天鳳', '八段', '七段', '七段'], 'rate': [2198.17, 2177.6, 2077.91, 2089.3], 'sx': ['M', 'F', 'M', 'F'], 'sc': [19300, -31, 27300, 7, 32200, 43, 21200, -19], 'name': ['おかもと', 'KOTA**', '爆弾五郎２', 'trail']}
{'ver': 2.3, 'ref': '2017062223gm-00a9-0000-3e858893', 'log': [[[0, 0, 0], [25000, 25000, 25000, 25000], [34], [29], [11, 14, 18, 19, 26, 31, 31, 34, 39, 41, 42, 43, 45], [16, 42, 32, 39, 47, 43, 23, 12, 29, 24, 25, 16, 21, 36], [11, 43, 41, 19, 60, 45, 43, 26, 23, 16, 18, 14, 16, 12], [12, 17, 23, 33, 34, 35, 35, 36, 38, 39, 46, 46, 47], [13, 53, 13, 37, 45, 47, 39, 32, 38, 14, 26, 31, 44, 25], [47, 17, 23, 13, 60, 60, 13, 'r12', 60, 60, 60, 60, 60, 60], [11, 12, 16, 19, 22, 24, 27, 29, 33, 34, 36, 37, 41], [37, 23, 27, 18, 32, 45, 46, 36, 24, 18, 37, 32, 17, 27], [19, 41, 11, 12, 29, 60, 37, 23, 16, 60, 18, 22, 60, 46], [11, 14, 14, 15, 16, 17, 19, 52, 26, 28, 41, 43, 47], [12, 33, 27, 28, 22, 21, 44, 44, 15, 17, 51, 28, 25], [43, 47, 41, 14, 60, 60, 60, 60, 12, 60, 11, 19, 14], ['和了', [0, 17000, -16000, 0], [1, 2, 1, '倍満16000点', '立直(1飜)', '役牌 發(1飜)', '混一色(3飜)', 'ドラ(3飜)', '赤ドラ(1飜)']]], [[1, 0, 0], [25000, 41000, 9000, 25000], [44], [28], [11, 14, 14, 18, 18, 18, 26, 32, 35, 41, 41, 43, 44], [46, 21, 29, 27, 19, 42, 24, 44], [11, 44, 43, 46, 60, 60, 35, 41], [12, 16, 16, 19, 21, 31, 32, 33, 36, 38, 42, 45, 45], [13, 19, 34, '4545p45', 51, 33, 12, 37, 'c353334', 19], [60, 42, 21, 12, 16, 36, 38, 60, 12, 60], [15, 17, 21, 22, 23, 24, 25, 25, 26, 29, 36, 44, 45], [11, 36, 36, 27, 39, 16, 17, 35, 46, 26], [60, 44, 45, 25, 60, 'r36', 60, 60, 60, 60], [13, 15, 22, 22, 23, 23, 27, 32, 33, 34, 38, 42, 47], [17, 34, 31, 28, 24, 39, 16, 41], [42, 47, 38, 23, 22, 60, 'r13', 60], ['和了', [0, 0, -2000, 4000], [3, 2, 3, '30符2飜2000点', '立直(1飜)', '平和(1飜)']]], [[2, 0, 0], [25000, 41000, 6000, 28000], [29], [12], [51, 16, 22, 23, 52, 26, 31, 32, 33, 34, 36, 42, 47], [24, 24, 38, 37, 28, 13, 31, 37, 47, 38, 14], [47, 42, 60, 60, 60, 60, 'r36', 60, 60, 60], [14, 15, 17, 26, 26, 27, 29, 35, 37, 38, 44, 45, 46], [18, 37, 14, 44, 'c282729', 'c131415', 33, 29, 17, 12], [45, 46, 44, 60, 35, 37, 60, 60, 37, 38], [11, 11, 12, 13, 16, 19, 22, 28, 42, 44, 45, 46, 47], [21, 39, 24, 12, 18, 18, 39, 15, 19, 41, 43], [44, 42, 45, 39, 28, 24, 60, 47, 46, 60, 13], [13, 16, 16, 21, 22, 23, 24, 27, 29, 34, 36, 41, 45], [13, 42, 22, 43, 32, 25, 35, 19, 35, 28, 34], [41, 60, 45, 60, 22, 27, 32, 13, 13, 60, 29], ['和了', [9000, -2000, -4000, -2000], [0, 0, 0, '満貫2000-4000点', '立直(1飜)', '門前清自摸和(1飜)', '平和(1飜)', '赤ドラ(2飜)']]], [[3, 0, 0], [33000, 39000, 2000, 26000], [46], [14], [11, 12, 13, 14, 21, 22, 23, 31, 36, 41, 44, 45, 46], [29, 34, 47, 14, 37, 27, 21, 11, 37, 16], [44, 46, 45, 41, 29, 60, 60, 60, 37, 37], [16, 24, 24, 26, 26, 28, 32, 34, 35, 53, 39, 44, 44], [17, 13, 15, 45, 26, 32, 23, 35, 18, 17], [39, 44, 44, 60, 28, 13, 35, 60, 60, 17], [14, 15, 18, 18, 19, 22, 27, 28, 39, 39, 42, 47, 47], ['p393939', 34, 43, 42, 41, 46, 11, 43, 'p181818', 24], [22, 60, 60, 14, 15, 60, 60, 60, 19, 41], [13, 16, 17, 19, 27, 31, 32, 33, 37, 38, 42, 42, 44], [52, 51, 17, 23, 28, 11, 12, 22, 21, 33, 26], [44, 19, 60, 13, 37, 38, 60, 11, 'r52', 60], ['和了', [-4000, -4000, -4000, 13000], [3, 3, 3, '満貫4000点∀', '立直(1飜)', '門前清自摸和(1飜)', '平和(1飜)', '赤ドラ(1飜)', '裏ドラ(1飜)']]]], 'ratingc': 'PF4', 'rule': {'disp': '鳳南喰赤', 'aka53': 1, 'aka52': 1, 'aka51': 1}, 'lobby': 0, 'dan': ['七段', '八段', '天鳳', '七段'], 'rate': [2116.61, 2181.53, 2204.42, 2108.55], 'sx': ['M', 'M', 'M', 'M'], 'sc': [29000, -11, 35000, 15, -2000, -52, 38000, 48], 'name': ['知らんし', 'iq180真剣様', 'おかもと', 'うみキチ']}
{'ver': 2.3, 'ref': '2017062222gm-00a9-0000-ebe8b727', 'log': [[[0, 0, 0], [25000, 25000, 25000, 25000], [17], [32], [18, 18, 21, 22, 22, 24, 28, 31, 31, 34, 36, 41, 42], [44, 37, 34, 24, 16, 27, 39, 23, 47, 29, 32, 25, 45, 28, 19], [60, 21, 28, 37, 60, 60, 36, 60, 60, 60, 60, 60, 60, 41, 60], [15, 17, 26, 27, 29, 37, 37, 37, 39, 42, 44, 45, 47], [52, 22, 38, 38, 29, 27, 44, 46, 32, 34, 12, 41, 43, 28, 11], [44, 47, 45, 42, 22, 60, 60, 60, 60, 60, 37, 60, 60, 38, 38], [13, 14, 14, 16, 18, 24, 25, 31, 53, 39, 43, 46, 47], [11, 38, 26, 45, 35, 26, 25, 41, 16, 27, 44, 36, 15, 19, 34], [31, 11, 47, 60, 46, 43, 38, 39, 41, 14, 60, 18, 'r53', 60], [11, 19, 21, 21, 23, 23, 28, 36, 38, 39, 42, 43, 45], [12, 17, 35, 42, 35, 32, 41, 33, 26, 18, 13, 31, 14, 22], [36, 60, 28, 11, 12, 60, 45, 60, 60, 19, 41, 60, 60, 38], ['和了', [-2600, -1300, 6200, -1300], [2, 2, 2, '20符4飜1300-2600点', '立直(1飜)', '門前清自摸和(1飜)', '平和(1飜)', '断幺九(1飜)']]], [[1, 0, 0], [22400, 23700, 30200, 23700], [29], [], [14, 17, 27, 27, 32, 33, 33, 34, 34, 39, 44, 47, 47], [35, 35, 29, 24, 14], [39, 44, 60, 32, 17], [12, 17, 18, 18, 21, 52, 26, 31, 37, 37, 38, 43, 43], [38, 42, 15, 17, 41, 19], [31, 60, 12, 26, 60, 60], [13, 14, 51, 17, 21, 23, 23, 25, 31, 34, 44, 45, 46], [16, 43, 47, 46, 42, 18], [31, 44, 43, 45, 60, 47], [16, 18, 19, 21, 24, 24, 27, 34, 39, 41, 42, 44, 46], [47, 33, 32, 12, 23, 13], [34, 16, 27, 60, 24, 24], ['和了', [1600, 0, 0, -1600], [0, 3, 0, '25符2飜1600点', '七対子(2飜)']]], [[2, 0, 0], [24000, 23700, 30200, 22100], [22, 45], [39, 15], [12, 13, 14, 15, 22, 25, 29, 31, 32, 35, 36, 37, 47], [21, 31, 23, 45, 51, 35, 14, 26, 13], [29, 47, 32, 60, 'r25', 60, 60, 60, 60], [11, 14, 16, 17, 18, 21, 21, 26, 28, 53, 36, 41, 47], [34, 44, 27, 38, 36, 39, 43, 29, 18], [11, 47, 44, 41, 'r14', 60, 60, 60, 60], [11, 11, 16, 19, 23, 28, 33, 34, 42, 42, 45, 46, 47], [12, 19, 18, 41, 33, 27, 12, 17, 46, 43], [28, 33, 34, 23, 60, 47, 45, 11, 11, 19], [13, 15, 22, 22, 24, 24, 24, 28, 29, 29, 34, 35, 38], [18, 37, 14, 42, 46, 24, 19, 28, 32, 39, 33], [60, 28, 29, 60, 60, '242424a24', 29, 60, 60, 'r19'], ['和了', [-1300, -1300, -2600, 8200], [3, 3, 3, '40符3飜1300-2600点', '立直(1飜)', '一発(1飜)', '門前清自摸和(1飜)']]], [[3, 0, 0], [21700, 21400, 27600, 29300], [39, 24], [], [11, 12, 13, 17, 18, 18, 22, 22, 33, 35, 53, 42, 47], [23, 34, 12, 13], [47, 42, 60, 60], [11, 12, 15, 16, 16, 17, 18, 19, 27, 32, 35, 42, 47], [14, 39, 47], [32, 60, 42], [13, 51, 16, 19, 19, 21, 28, 33, 36, 37, 44, 44, 45], ['4444p44', 44, 38, 41, 13, '13p1313'], [21, '4444k4444', 45, 60, 28, 33], [11, 23, 25, 25, 26, 27, 29, 33, 33, 38, 42, 44, 45], [34, 27, 24, 14, 26, 'p333333'], [44, 42, 45, 11, 29, 14], ['和了', [0, 0, 2600, -2600], [2, 3, 2, '40符2飜2600点', '自風 北(1飜)', '赤ドラ(1飜)']]], [[4, 0, 0], [21700, 21400, 30200, 26700], [27, 18], [], [11, 11, 16, 24, 26, 27, 29, 38, 38, 39, 42, 42, 43], [17, 16, 22, 'p424242', 'c151617', 39, 23, 'c252627', 34, 21, 23, 19], [29, 24, 60, 39, 16, 60, 60, 43, 60, 60, 60, 60], [14, 14, 17, 21, 22, 23, 52, 29, 33, 36, 44, 45, 47], [45, 31, 22, 19, 16, 37, '45p4545', 45, 42, 13, 32, 39, 13, 11], [44, 29, 60, 47, 19, 33, 31, '45k454545', 60, 52, 13, 60, 23, 32], [12, 17, 18, 21, 25, 29, 34, 34, 37, 41, 44, 47, 47], [31, 16, 44, 'p474747', 41, 26, 12, 41, 32, 47, 27, 38, 46], [44, 31, 60, 41, 12, 41, 60, 60, 21, 29, 47, 32, 60], [12, 13, 15, 21, 22, 24, 34, 36, 37, 38, 41, 42, 46], [24, 33, 32, 25, 35, 45, 43, 39, 36, 14, 35, 28, 36], [41, 46, 42, 15, 21, 60, 60, 25, 22, 'r39', 60, 60, 60], ['和了', [0, 0, 2000, -1000], [2, 3, 2, '30符1飜1000点', '役牌 中(1飜)']]], [[5, 0, 0], [21700, 21400, 32200, 24700], [52], [], [15, 15, 51, 16, 18, 19, 25, 27, 28, 32, 34, 39, 45], [41, 11, 32, 12, 39, 24, 47, 27, '32p3232'], [39, 60, 41, 60, 60, 19, 34, 47, 45], [13, 14, 17, 23, 23, 24, 25, 28, 33, 34, 38, 38, 44], [44, 17, 16, 22, 46, 33, 35, 26, 29], [28, 44, 44, 60, 23, 60, 46, 23, 60], [14, 16, 18, 22, 25, 34, 36, 37, 38, 38, 41, 43, 46], [18, 28, 22, 12, 36, 42, 22, 26, 32], [41, 43, 28, 46, 25, 60, 34, 60, 60], [11, 21, 26, 27, 29, 34, 35, 53, 36, 37, 41, 41, 45], [45, 21, 42, 13, 'c252627', 36, 37, 19], [11, 29, 60, 60, 41, 41, 60, 60], ['和了', [-3900, 0, 0, 3900], [3, 0, 3, '30符3飜3900点', '役牌 白(1飜)', 'ドラ(1飜)', '赤ドラ(1飜)']]], [[6, 0, 0], [17800, 21400, 32200, 28600], [24], [], [14, 14, 15, 18, 18, 24, 24, 29, 31, 34, 45, 45, 47], [27, 28, 42, 21, 26, 38, 45, 37, 43, 'p181818', 13], [31, 47, 60, 60, 14, 60, 34, 60, 60, 29], [14, 15, 16, 17, 18, 24, 26, 31, 36, 36, 36, 39, 42], [28, 41, 32, 27, 11, 43, 22, 35, 15], [31, 39, 41, 32, 60, 60, 42, 22, 60], [17, 21, 22, 23, 26, 31, 34, 38, 39, 42, 43, 44, 44], [42, 35, 39, '42p4242', 33, 16, 37, 47, 33, 51, 11], [31, 38, 17, 43, 26, 60, 60, 60, 33, 60, 60], [11, 12, 12, 14, 16, 18, 22, 22, 27, 28, 38, 45, 46], [23, 46, 32, 12, 19, 31, 29, 25, 32, 'c511416', 47], [11, 45, 38, 32, 60, 60, 23, 29, 28, 18, 60], ['和了', [1500, -400, -700, -400], [0, 0, 0, '40符1飜400-700点', '役牌 白(1飜)']]], [[7, 0, 0], [19300, 21000, 31500, 28200], [32], [], [11, 17, 19, 21, 22, 27, 28, 33, 36, 36, 37, 39, 46], [46, 24, 32, '46p4646', 26, 12, 21, 37, 18, 26, 31], [11, 21, 39, 37, 24, 60, 60, 60, 22, 60], [11, 13, 18, 18, 21, 23, 34, 37, 39, 41, 45, 47, 47], [12, 39, 'p393939', 17, 17, 12, 38, 45, 'c222123', 14], [45, 41, 37, 34, 60, 60, 60, 60, 18, 60], [14, 51, 16, 23, 27, 29, 32, 34, 35, 38, 41, 45, 46], [53, 27, 31, 31, 43, 28, 43, 44, 43, 34], [41, 45, 46, 38, 60, 27, 60, 60, 60, 23], [11, 13, 15, 15, 16, 16, 19, 35, 41, 42, 44, 45, 47], [26, 25, 36, 46, 47, 'c272526', 21, 42, 26, 42], [44, 45, 19, 42, 11, 13, 60, 41, 46, 26], ['和了', [2000, -500, -500, -1000], [0, 0, 0, '30符2飜500-1000点', '役牌 發(1飜)', 'ドラ(1飜)']]]], 'ratingc': 'PF4', 'rule': {'disp': '鳳南喰赤', 'aka53': 1, 'aka52': 1, 'aka51': 1}, 'lobby': 0, 'dan': ['七段', '天鳳', '七段', '七段'], 'rate': [2148.92, 2212.94, 2161.58, 2114.2], 'sx': ['M', 'M', 'M', 'M'], 'sc': [21300, -19, 20500, -29, 31000, 41, 27200, 7], 'name': ['光りもの親方', 'おかもと', 'ディソール', 'ぴのこ']}
...
All 40 logs with players greater than level 19 are processed.
```

### <a name="printlog"></a>prt_log_format(log)
```python
glc = GameLogCrawler()
log_generator = glc.db_get_logs_where_players_lv_gr(19)
one_log = log_generator.__next__()
glc.prt_log_format(one_log)
```
```console
ver: 2.3
ref: 2017120321gm-00a9-0000-34e876fb
log:
    Round 0
        [0, 0, 0]
        [25000, 25000, 25000, 25000]
        [46]
        []
        [12, 13, 13, 17, 22, 22, 27, 34, 37, 42, 45, 46, 47]
        [32, 39, 44, 22, 47, 19, 11, 11, 35, 24, 11, 18, 16, 16, 24, 28, 21, 39]
        [42, 46, 60, 45, 27, 60, 13, 17, 32, 37, 39, 60, 60, 60, 35, 60, 34, 60]
        [15, 18, 19, 26, 28, 31, 31, 32, 33, 33, 53, 38, 38]
        [47, 21, 14, 13, 26, 17, 43, 36, 39, 27, 46, 41, 23, 26, 11, 12, 21, 22]
        [19, 60, 18, 47, 31, 60, 60, 28, 33, 39, 60, 60, 38, 38, 53, 31, 32, 36]
        [21, 23, 25, 25, 26, 27, 28, 29, 32, 36, 38, 42, 44]
        [27, 15, 38, 14, 45, 37, 37, 17, 33, 29, 43, 25, 46, 14, 35, 16, 37]
        [32, 44, 42, 36, 60, 60, 60, 60, 60, 60, 60, 38, 38, 46, 60, 29, 60]
        [12, 15, 17, 19, 23, 23, 52, 28, 29, 31, 36, 41, 43]
        [18, 13, 41, 24, 34, 14, 24, 34, 42, 12, 41, 19, 42, 18, 44, 39, 34]
        [43, 31, 15, 36, 60, 29, 'r28', 60, 60, 60, 60, 60, 60, 60, 60, 60, 60]
        ['流局', [-1000, -1000, -1000, 3000]]
    Round 1
        [1, 1, 1]
        [24000, 24000, 24000, 27000]
        [11]
        [42]
        [15, 22, 24, 24, 29, 32, 33, 34, 53, 43, 44, 45, 47]
        [35, 23, 22, 23, 12, 39, 34, 47, 46, 46, 25, 15, 32]
        [43, 29, 44, 47, 45, 60, 12, 60, 60, 60, 32, 25, 60]
        [14, 18, 19, 19, 21, 23, 52, 27, 28, 31, 38, 42, 46]
        [27, 17, 41, 38, 13, 42, 24, 43, 41, 22, 43, 47, 34, 33]
        [42, 31, 19, 46, 41, 60, 21, 60, 60, 13, 60, 60, 52, 28]
        [13, 16, 24, 27, 28, 31, 36, 37, 38, 39, 39, 44, 47]
        [32, 17, 25, 41, 26, 46, 21, 21, 23, 12, 36, 12, 37, 18]
        [44, 47, 31, 32, 13, 60, 60, 60, 'r41', 60, 60, 60, 60]
        [51, 16, 17, 19, 21, 25, 26, 28, 28, 29, 33, 38, 39]
        [37, 44, 44, 18, 26, 36, 18, 26, 31, 29, 35, 29, 15]
        [21, 60, 60, 33, 29, 39, 26, 60, 60, 60, 38, 60, 28]
        ['和了', [-2100, -4100, 10300, -2100], [2, 2, 2, '満貫2000-4000点', '立直(1飜)', '門前清自摸和(1飜)', '平和(1飜)', '三色同順(2飜)']]
    Round 2
        [2, 0, 0]
        [21900, 19900, 33300, 24900]
        [26]
        []
        [14, 17, 18, 19, 22, 25, 26, 33, 34, 34, 44, 45, 46]
        [37, 17, 11, 12, 28, 43, 13, 36, 32]
        [44, 46, 60, 45, 12, 17, 60, 14, 37]
        [12, 14, 22, 23, 26, 27, 28, 29, 36, 42, 45, 45, 46]
        [52, 29, 22, 'p454545', 32, 29, '29p2929', 47, 12, 18]
        [36, 12, 14, 46, 60, 42, 23, 60, 60, 60]
        [14, 15, 16, 19, 24, 24, 25, 31, 35, 37, 41, 42, 43]
        [16, 23, 32, 21, 38, 42, 38, 31, 36]
        [31, 19, 42, 60, 43, 60, 32, 35, 38]
        [17, 24, 27, 28, 29, 35, 39, 39, 41, 41, 46, 46, 47]
        [13, 41, 13, 17, '46p4646', 16, 26, 42, 44, 37]
        [60, 47, 60, 35, 24, 17, 29, 60, 60, 60]
        ['和了', [0, -5200, 0, 5200], [3, 1, 3, '40符3飜5200点', '場風 東(1飜)', '役牌 發(1飜)', 'ドラ(1飜)']]
    Round 3
        ...
ratingc: PF4
rule: {'disp': '鳳南喰赤', 'aka53': 1, 'aka52': 1, 'aka51': 1}
lobby: 0
dan: ['天鳳', '八段', '七段', '七段']
rate: [2198.17, 2177.6, 2077.91, 2089.3]
sx: ['M', 'F', 'M', 'F']
sc: [19300, -31, 27300, 7, 32200, 43, 21200, -19]
name: ['おかもと', 'KOTA**', '爆弾五郎２', 'trail']

```

# <a name="pre"></a>2. Game log preprocessor: PreProsessing

| function  | Description |
| --------- | ----------- |
| [process_one_log(log)](#onelog) | Preprocess a whole log, which contains several game rounds. Return a dict with key as round number and value as a list of state-action pair object |

### <a name="onelog"></a>process_one_log(log)
This function returns a dict with key as the round number and value as a list of state-action pair objects. A state-action pair object is an instance of class ***PlayerState***. This class wraps all the visible states of the Mahjong table and opponents. 
```python
glc = GameLogCrawler()
log_generator = glc.db_get_logs_where_players_lv_gr(19)
one_log = log_generator.__next__()
processed = PreProcessing.process_one_log(one_log)
for round_num, state_action_pair_objs in processed.items():
    print("Round {}:".format(round_num))
    for sa_pair in state_action_pair_objs:
        print(sa_pair)
```
```console
Round 1:
    Player: おかもと, 天鳳, 25000
    State:
        Hand:[1, 2, 2, 6, 10, 10, 15, 21, 24, 28, 31, 32, 33, 19]
        Discard: []
        Status: Reach False, Player wind 27, Round wind 27, Red fives [], Bonus tiles [33]
        Revealed: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
    Last action: None
    Action: {'type': 'drop', 'tile': 28}
    Result: None

    Player: KOTA**, 八段, 25000
    State:
        Hand:[4, 7, 8, 14, 16, 18, 18, 19, 20, 20, 22, 25, 25, 33]
        Discard: []
        Status: Reach False, Player wind 28, Round wind 27, Red fives [], Bonus tiles [33]
        Revealed: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0]
    Last action: None
    Action: {'type': 'drop', 'tile': 8}
    Result: None

    Player: 爆弾五郎２, 七段, 25000
    State:
        Hand:[9, 11, 13, 13, 14, 15, 16, 17, 19, 23, 25, 28, 30, 15]
        Discard: []
        Status: Reach False, Player wind 29, Round wind 27, Red fives [], Bonus tiles [33]
        Revealed: [0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0]
    Last action: None
    Action: {'type': 'drop', 'tile': 19}
    Result: None

    Player: trail, 七段, 25000
    State:
        Hand:[1, 4, 6, 8, 11, 11, 13, 16, 17, 18, 23, 27, 29, 7]
        Discard: []
        Status: Reach False, Player wind 30, Round wind 27, Red fives [], Bonus tiles [33]
        Revealed: [0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0]
    Last action: None
    Action: {'type': 'drop', 'tile': 29}
    Result: None
    ...
Round 2:
    ...
```

# <a name="deter"></a>3. Deterministic algorithms: WinWaitCal, Partition

## Partition

| function  | Description |
| --------- | ----------- |
| [partition(tiles34)](#parti) | Partition hand tiles into melds, pairs and single tiles |
| [shantin_normal(tiles34, called_melds)](#nmst) | Calculate the shantin of normal form |

### <a name="parti"></a>partition(tiles34)
```python
partitions = Partition.partition([0, 0, 1, 2, 3, 14, 14, 14, 15, 27, 27, 28, 28])
for p in partitions:
    print("{} {}".format(p, Tile.t34_to_grf(p)))
```
```console
[[0, 0], [1, 2, 3], [14, 14, 14], [15], [27, 27], [28, 28]] 🀇🀇 🀈🀉🀊 🀞🀞🀞 🀟 🀀🀀 🀁🀁 
[[0, 0], [1, 2, 3], [14, 14], [14, 15], [27, 27], [28, 28]] 🀇🀇 🀈🀉🀊 🀞🀞 🀞🀟 🀀🀀 🀁🀁 
```
### <a name="nmst"></a>shantin_normal(tiles34, called_melds)
```python
hand_tiles = [0, 0, 1, 2, 3, 14, 14, 14, 15, 27, 27, 28, 28]
called_melds = []
nm_st = Partition.shantin_normal(hand_tiles, called_melds)
print("{} has {} shantin(normal)".format(Tile.t34_to_grf(hand_tiles), nm_st))
```
```console
🀇🀇🀈🀉🀊🀞🀞🀞🀟🀀🀀🀁🀁 has 1 shantin(normal)
```

***

# To be continued
