# Benchmark environment

* Commit f6a1a97befde4d926e2cbf99a8e6f8399035532e
* Intel i5-7600 @ 3.5 GHz
* Linux 5.11.8
* `criterion 0.3.3`
* `critcmp 0.1.7`

These benchmarks were run after updating the `regex` and `bstr` crates to use
the implementation of `memmem` in this crate. These benchmarks are useful for
checking that overall runtimes should be roughly similar. Indeed, looking at
the benchmarks, the main differences appears to be latency, especially when
comparing against `regex`.

# memchr benchmarks

```
group               2021-05-01/memchr1/fallback/           2021-05-01/memchr1/krate/              2021-05-01/memchr1/libc/               2021-05-01/memchr1/naive/
-----               ----------------------------           -------------------------              ------------------------               -------------------------
empty/never         9.58      2.3±0.04ns        0 B/sec    1.00      0.2±0.00ns        0 B/sec    10.65     2.6±0.02ns        0 B/sec    2.00      0.5±0.00ns        0 B/sec
huge/common         2.92    643.8±1.21µs   881.3 MB/sec    1.00    220.1±0.38µs     2.5 GB/sec    1.29    283.4±0.74µs  2002.3 MB/sec    2.07    456.0±0.85µs  1244.4 MB/sec
huge/never          3.84     32.0±0.03µs    17.3 GB/sec    1.07      8.9±0.01µs    62.2 GB/sec    1.00      8.3±0.01µs    66.6 GB/sec    17.50   145.7±0.21µs     3.8 GB/sec
huge/rare           3.50     32.7±0.07µs    17.0 GB/sec    1.00      9.3±0.01µs    59.3 GB/sec    1.07     10.0±0.02µs    55.6 GB/sec    15.73   146.9±0.29µs     3.8 GB/sec
huge/uncommon       2.07    152.3±0.17µs     3.6 GB/sec    1.04     76.7±0.15µs     7.2 GB/sec    1.00     73.6±0.07µs     7.5 GB/sec    2.69    198.2±0.30µs     2.8 GB/sec
huge/verycommon     2.48   1141.9±4.65µs   496.9 MB/sec    1.00    459.8±1.19µs  1233.9 MB/sec    1.29    591.7±0.55µs   959.0 MB/sec    1.99    914.0±1.41µs   620.8 MB/sec
small/common        1.30    267.0±2.81ns     2.3 GB/sec    1.00    204.7±0.22ns     3.0 GB/sec    1.07    219.5±0.18ns     2.8 GB/sec    1.24    253.0±5.57ns     2.4 GB/sec
small/never         5.38     39.8±0.35ns    15.5 GB/sec    1.02      7.6±0.03ns    81.8 GB/sec    1.00      7.4±0.03ns    83.5 GB/sec    23.26   172.2±0.22ns     3.6 GB/sec
small/rare          4.29     48.1±0.51ns    12.9 GB/sec    1.09     12.2±0.04ns    50.8 GB/sec    1.00     11.2±0.02ns    55.2 GB/sec    16.08   180.3±0.31ns     3.4 GB/sec
small/uncommon      1.73     79.4±0.13ns     7.8 GB/sec    1.00     46.0±0.08ns    13.4 GB/sec    1.03     47.2±0.05ns    13.1 GB/sec    4.49    206.4±5.48ns     3.0 GB/sec
small/verycommon    1.50    497.7±0.45ns  1272.3 MB/sec    1.50    495.5±0.38ns  1278.0 MB/sec    1.62    535.9±0.57ns  1181.5 MB/sec    1.00    331.0±5.06ns  1912.9 MB/sec
tiny/common         1.25     50.0±0.05ns  1317.3 MB/sec    1.30     51.9±0.07ns  1267.4 MB/sec    1.23     49.4±0.06ns  1331.9 MB/sec    1.00     40.0±0.06ns  1644.3 MB/sec
tiny/never          1.78      6.1±0.00ns    10.5 GB/sec    1.07      3.7±0.00ns    17.5 GB/sec    1.00      3.4±0.01ns    18.7 GB/sec    7.97     27.3±0.20ns     2.4 GB/sec
tiny/rare           1.58      9.3±0.01ns     6.9 GB/sec    1.00      5.9±0.05ns    10.9 GB/sec    1.04      6.1±0.01ns    10.5 GB/sec    4.67     27.5±0.15ns     2.3 GB/sec
tiny/uncommon       1.97     35.3±0.07ns  1866.2 MB/sec    1.00     17.9±0.01ns     3.6 GB/sec    1.04     18.7±0.02ns     3.4 GB/sec    1.51     27.0±0.25ns     2.4 GB/sec
```

# memmem benchmarks

```
group                                                                2021-05-01/memmem/krate/               2021-05-01/memmem/regex/               2021-05-01/memmem/sliceslice/          2021-05-01/memmem/stud/                2021-05-01/memmem/twoway/
-----                                                                ------------------------               ------------------------               -----------------------------          -----------------------                -------------------------
oneshot/code-rust-library/never-fn-quux                              1.00     41.0±0.12µs    37.4 GB/sec                                           1.15     47.0±0.06µs    32.7 GB/sec    29.97  1229.8±2.66µs  1278.0 MB/sec    3.88    159.3±0.18µs     9.6 GB/sec
oneshot/code-rust-library/never-fn-strength                          1.00     48.7±0.10µs    31.5 GB/sec                                           1.01     49.3±0.04µs    31.1 GB/sec    28.21  1372.5±1.63µs  1145.2 MB/sec    4.62    224.6±0.33µs     6.8 GB/sec
oneshot/code-rust-library/never-fn-strength-paren                    1.00     48.9±0.11µs    31.4 GB/sec                                           1.07     52.1±0.06µs    29.5 GB/sec    26.04  1272.5±2.12µs  1235.2 MB/sec    4.60    224.7±0.36µs     6.8 GB/sec
oneshot/code-rust-library/rare-fn-from-str                           1.00     14.0±0.01µs   109.3 GB/sec                                           1.42     20.0±0.05µs    76.7 GB/sec    23.72   333.2±0.58µs     4.6 GB/sec    9.64    135.4±0.46µs    11.3 GB/sec
oneshot/huge-en/never-all-common-bytes                               1.11     25.7±0.05µs    22.3 GB/sec                                           1.00     23.1±0.03µs    24.8 GB/sec    11.13   256.7±0.41µs     2.2 GB/sec    4.17     96.1±0.14µs     5.9 GB/sec
oneshot/huge-en/never-john-watson                                    1.00     17.6±0.01µs    32.4 GB/sec                                           1.00     17.7±0.01µs    32.3 GB/sec    25.41   447.7±0.30µs  1306.4 MB/sec    4.78     84.3±0.16µs     6.8 GB/sec
oneshot/huge-en/never-some-rare-bytes                                1.01     17.6±0.03µs    32.4 GB/sec                                           1.00     17.5±0.02µs    32.6 GB/sec    13.50   236.4±0.32µs     2.4 GB/sec    3.78     66.2±0.04µs     8.6 GB/sec
oneshot/huge-en/never-two-space                                      1.02     17.6±0.03µs    32.4 GB/sec                                           1.00     17.3±0.02µs    33.0 GB/sec    37.00   640.7±1.45µs   912.9 MB/sec    7.64    132.3±0.26µs     4.3 GB/sec
oneshot/huge-en/rare-huge-needle                                     1.54     39.5±0.05µs    14.5 GB/sec                                           1.00     25.6±0.05µs    22.3 GB/sec    4.61    118.0±0.13µs     4.8 GB/sec    3.07     78.5±0.14µs     7.3 GB/sec
oneshot/huge-en/rare-long-needle                                     1.00     18.8±0.02µs    30.4 GB/sec                                           1.41     26.4±0.04µs    21.6 GB/sec    6.62    124.3±0.19µs     4.6 GB/sec    4.15     78.0±0.08µs     7.3 GB/sec
oneshot/huge-en/rare-medium-needle                                   1.00     18.8±0.02µs    30.4 GB/sec                                           2.20     41.4±0.06µs    13.8 GB/sec    10.67   200.8±0.30µs     2.8 GB/sec    4.44     83.5±0.09µs     6.8 GB/sec
oneshot/huge-en/rare-sherlock                                        1.00     17.5±0.02µs    32.6 GB/sec                                           1.01     17.6±0.01µs    32.4 GB/sec    15.56   272.3±0.17µs     2.1 GB/sec    4.21     73.8±0.10µs     7.7 GB/sec
oneshot/huge-en/rare-sherlock-holmes                                 1.00     17.4±0.02µs    32.8 GB/sec                                           1.47     25.5±0.04µs    22.4 GB/sec    17.43   303.2±0.63µs  1929.3 MB/sec    13.43   233.6±0.27µs     2.4 GB/sec
oneshot/huge-ru/never-john-watson                                    1.00     17.4±0.02µs    32.9 GB/sec                                           3.97     68.9±0.12µs     8.3 GB/sec    18.66   324.1±1.00µs  1805.1 MB/sec    8.80    152.9±0.21µs     3.7 GB/sec
oneshot/huge-ru/rare-sherlock                                        1.00     17.6±0.02µs    32.4 GB/sec                                           2.11     37.2±0.06µs    15.4 GB/sec    23.10   407.0±0.57µs  1437.3 MB/sec    4.16     73.4±0.14µs     7.8 GB/sec
oneshot/huge-ru/rare-sherlock-holmes                                 1.00     15.3±0.01µs    37.4 GB/sec                                           3.90     59.6±0.09µs     9.6 GB/sec    18.15   277.4±0.35µs     2.1 GB/sec    14.97   228.8±0.59µs     2.5 GB/sec
oneshot/huge-zh/never-john-watson                                    1.00     16.4±0.02µs    34.9 GB/sec                                           1.60     26.1±0.02µs    21.9 GB/sec    7.46    122.1±0.19µs     4.7 GB/sec    3.64     59.6±0.08µs     9.6 GB/sec
oneshot/huge-zh/rare-sherlock                                        1.00     18.0±0.03µs    31.8 GB/sec                                           1.23     22.2±0.02µs    25.8 GB/sec    11.46   206.0±0.36µs     2.8 GB/sec    3.66     65.9±0.12µs     8.7 GB/sec
oneshot/huge-zh/rare-sherlock-holmes                                 1.00     18.5±0.02µs    31.0 GB/sec                                           1.56     28.8±0.02µs    19.8 GB/sec    8.27    152.7±0.18µs     3.7 GB/sec    3.79     69.9±0.12µs     8.2 GB/sec
oneshot/pathological-defeat-simple-vector-freq/rare-alphabet         1.44     68.7±0.06µs     9.8 GB/sec                                           8.68    413.9±0.96µs  1658.9 MB/sec    1.00     47.7±0.07µs    14.1 GB/sec    1.99     95.0±0.16µs     7.1 GB/sec
oneshot/pathological-defeat-simple-vector-repeated/rare-alphabet     18.48  1229.7±2.07µs   558.4 MB/sec                                           40.15     2.7±0.00ms   257.0 MB/sec    17.23  1146.3±1.08µs   599.1 MB/sec    1.00     66.5±0.09µs    10.1 GB/sec
oneshot/pathological-defeat-simple-vector/rare-alphabet              2.20    208.6±0.72µs     2.5 GB/sec                                           1.24    117.8±0.19µs     4.3 GB/sec    1.10    104.5±0.15µs     4.9 GB/sec    1.00     94.9±0.11µs     5.4 GB/sec
oneshot/pathological-md5-huge/never-no-hash                          1.00      5.7±0.02µs    24.9 GB/sec                                           1.62      9.2±0.02µs    15.4 GB/sec    4.69     26.5±0.03µs     5.3 GB/sec    3.47     19.6±0.03µs     7.2 GB/sec
oneshot/pathological-md5-huge/rare-last-hash                         1.00      6.0±0.02µs    23.5 GB/sec                                           1.49      8.9±0.02µs    15.8 GB/sec    4.33     25.9±0.06µs     5.4 GB/sec    3.31     19.8±0.02µs     7.1 GB/sec
oneshot/pathological-repeated-rare-huge/never-tricky                 1.01     14.3±0.02µs    32.5 GB/sec                                           1.00     14.2±0.02µs    32.9 GB/sec    38.92   551.0±0.76µs   865.6 MB/sec    19.04   269.5±0.51µs  1769.7 MB/sec
oneshot/pathological-repeated-rare-small/never-tricky                1.21     62.5±0.16ns    14.9 GB/sec                                           1.00     51.7±0.27ns    18.0 GB/sec    21.82  1128.6±1.09ns   845.9 MB/sec    11.07   572.6±0.63ns  1667.2 MB/sec
oneshot/teeny-en/never-all-common-bytes                              1.00     24.1±0.04ns  1110.3 MB/sec                                           1.19     28.7±0.03ns   929.5 MB/sec    1.44     34.5±0.05ns   773.3 MB/sec    1.84     44.2±0.07ns   604.6 MB/sec
oneshot/teeny-en/never-john-watson                                   1.00     24.5±0.05ns  1089.4 MB/sec                                           1.18     29.0±0.03ns   921.7 MB/sec    1.55     37.9±0.03ns   704.5 MB/sec    1.93     47.4±0.06ns   563.4 MB/sec
oneshot/teeny-en/never-some-rare-bytes                               1.00     24.3±0.03ns  1100.1 MB/sec                                           1.14     27.7±0.02ns   965.0 MB/sec    1.09     26.6±0.02ns  1005.4 MB/sec    1.13     27.4±0.04ns   975.2 MB/sec
oneshot/teeny-en/never-two-space                                     1.00     23.8±0.04ns  1121.8 MB/sec                                           1.17     27.8±0.04ns   961.9 MB/sec    1.38     32.8±0.03ns   813.4 MB/sec    1.15     27.4±0.02ns   975.6 MB/sec
oneshot/teeny-en/rare-sherlock                                       1.00     19.9±0.02ns  1340.9 MB/sec                                           1.41     28.1±0.04ns   951.8 MB/sec    2.04     40.6±0.05ns   657.0 MB/sec    1.85     36.8±0.06ns   724.9 MB/sec
oneshot/teeny-en/rare-sherlock-holmes                                1.00     26.1±0.04ns  1024.4 MB/sec                                           1.69     43.9±0.14ns   607.7 MB/sec    2.82     73.5±0.14ns   363.5 MB/sec    1.95     50.9±0.04ns   525.0 MB/sec
oneshot/teeny-ru/never-john-watson                                   1.00     33.1±0.03ns  1211.9 MB/sec                                           1.12     36.9±0.07ns  1085.1 MB/sec    2.00     66.1±0.10ns   606.2 MB/sec    2.08     68.6±0.06ns   583.5 MB/sec
oneshot/teeny-ru/rare-sherlock                                       1.00     27.3±0.05ns  1467.6 MB/sec                                           1.09     29.7±0.05ns  1348.2 MB/sec    2.08     56.7±0.06ns   705.8 MB/sec    1.66     45.2±0.04ns   886.6 MB/sec
oneshot/teeny-ru/rare-sherlock-holmes                                1.00     35.0±0.06ns  1144.9 MB/sec                                           1.13     39.4±0.06ns  1016.3 MB/sec    3.11    108.8±0.10ns   368.2 MB/sec    1.89     66.0±1.47ns   607.1 MB/sec
oneshot/teeny-zh/never-john-watson                                   1.00     26.9±0.06ns  1100.9 MB/sec                                           1.26     33.9±0.04ns   871.2 MB/sec    1.71     45.8±0.06ns   645.1 MB/sec    1.98     53.2±0.06ns   556.0 MB/sec
oneshot/teeny-zh/rare-sherlock                                       1.00     19.2±0.03ns  1540.4 MB/sec                                           1.46     27.9±0.08ns  1058.5 MB/sec    2.62     50.2±0.16ns   588.7 MB/sec    2.05     39.3±0.08ns   752.7 MB/sec
oneshot/teeny-zh/rare-sherlock-holmes                                1.00     28.8±0.03ns  1025.1 MB/sec                                           1.60     46.2±0.09ns   639.4 MB/sec    3.24     93.5±0.25ns   316.0 MB/sec    2.28     65.8±0.14ns   449.5 MB/sec
oneshotiter/code-rust-library/common-fn                              1.00     99.7±0.10µs    15.4 GB/sec                                                                                  10.91  1087.6±1.60µs  1445.2 MB/sec    2.53    252.3±0.34µs     6.1 GB/sec
oneshotiter/code-rust-library/common-fn-is-empty                     1.00     52.5±0.11µs    29.2 GB/sec                                                                                  26.00  1365.8±1.62µs  1150.8 MB/sec    3.24    170.4±0.16µs     9.0 GB/sec
oneshotiter/code-rust-library/common-let                             1.00    145.2±0.13µs    10.6 GB/sec                                                                                  9.74   1413.8±2.26µs  1111.7 MB/sec    2.22    322.3±0.31µs     4.8 GB/sec
oneshotiter/code-rust-library/common-paren                           1.13    402.0±0.46µs     3.8 GB/sec                                                                                  4.19   1494.4±2.46µs  1051.8 MB/sec    1.00    356.9±0.80µs     4.3 GB/sec
oneshotiter/huge-en/common-one-space                                 1.14    570.8±1.05µs  1024.7 MB/sec                                                                                  2.66   1326.3±2.30µs   441.0 MB/sec    1.00    499.4±0.62µs  1171.3 MB/sec
oneshotiter/huge-en/common-that                                      1.00     39.7±0.05µs    14.4 GB/sec                                                                                  9.69    385.1±0.41µs  1519.1 MB/sec    2.70    107.4±0.13µs     5.3 GB/sec
oneshotiter/huge-en/common-you                                       1.00     97.6±0.06µs     5.9 GB/sec                                                                                  4.68    456.8±0.39µs  1280.6 MB/sec    1.67    163.4±0.20µs     3.5 GB/sec
oneshotiter/huge-ru/common-not                                       1.00     71.1±0.15µs     8.0 GB/sec                                                                                  10.44   742.9±1.47µs   787.5 MB/sec    4.33    308.1±0.36µs  1898.8 MB/sec
oneshotiter/huge-ru/common-one-space                                 1.13    307.9±0.63µs  1900.0 MB/sec                                                                                  3.19    867.1±1.15µs   674.6 MB/sec    1.00    271.7±0.62µs     2.1 GB/sec
oneshotiter/huge-ru/common-that                                      1.00     35.4±0.03µs    16.2 GB/sec                                                                                  20.44   722.7±1.10µs   809.5 MB/sec    4.17    147.3±0.18µs     3.9 GB/sec
oneshotiter/huge-zh/common-do-not                                    1.00     64.6±0.06µs     8.9 GB/sec                                                                                  4.75    306.5±0.45µs  1908.9 MB/sec    2.44    157.6±0.25µs     3.6 GB/sec
oneshotiter/huge-zh/common-one-space                                 1.14    172.0±0.22µs     3.3 GB/sec                                                                                  4.12    619.9±1.06µs   943.7 MB/sec    1.00    150.6±0.09µs     3.8 GB/sec
oneshotiter/huge-zh/common-that                                      1.00     34.5±0.06µs    16.6 GB/sec                                                                                  5.34    184.0±0.36µs     3.1 GB/sec    2.65     91.4±0.13µs     6.3 GB/sec
oneshotiter/pathological-md5-huge/common-two-bytes                   1.00     11.3±0.02µs    12.5 GB/sec                                                                                  13.61   153.7±0.21µs   938.7 MB/sec    2.52     28.4±0.03µs     5.0 GB/sec
oneshotiter/pathological-repeated-rare-huge/common-match             1.19    500.9±0.87µs   952.1 MB/sec                                                                                  1.00    422.5±0.74µs  1128.8 MB/sec    4.71   1988.9±2.77µs   239.8 MB/sec
oneshotiter/pathological-repeated-rare-small/common-match            1.14   1010.6±0.68ns   944.6 MB/sec                                                                                  1.00    888.1±1.12ns  1074.9 MB/sec    4.49      4.0±0.00µs   239.5 MB/sec
prebuilt/code-rust-library/never-fn-quux                             1.00     41.3±0.04µs    37.2 GB/sec    1.00     41.1±0.51µs    37.3 GB/sec    1.13     46.5±0.07µs    33.0 GB/sec                                         
prebuilt/code-rust-library/never-fn-strength                         1.00     48.6±0.05µs    31.6 GB/sec    1.01     48.8±0.09µs    31.4 GB/sec    1.01     49.2±0.06µs    31.2 GB/sec                                         
prebuilt/code-rust-library/never-fn-strength-paren                   1.00     48.1±0.07µs    31.9 GB/sec    1.02     49.3±0.10µs    31.1 GB/sec    1.08     52.1±0.09µs    29.5 GB/sec                                         
prebuilt/code-rust-library/rare-fn-from-str                          1.00     14.0±0.02µs   109.6 GB/sec    1.00     14.0±0.03µs   109.6 GB/sec    1.43     20.0±0.02µs    76.9 GB/sec                                         
prebuilt/huge-en/never-all-common-bytes                              1.11     25.6±0.05µs    22.3 GB/sec    1.18     27.1±0.04µs    21.0 GB/sec    1.00     23.0±0.04µs    24.9 GB/sec                                         
prebuilt/huge-en/never-john-watson                                   1.00     17.6±0.01µs    32.5 GB/sec    1.00     17.6±0.03µs    32.4 GB/sec    1.00     17.6±0.02µs    32.4 GB/sec                                         
prebuilt/huge-en/never-some-rare-bytes                               1.00     17.3±0.04µs    32.9 GB/sec    1.00     17.3±0.03µs    32.9 GB/sec    1.01     17.5±0.02µs    32.7 GB/sec                                         
prebuilt/huge-en/never-two-space                                     1.01     17.6±0.02µs    32.5 GB/sec    1.00     17.3±0.01µs    32.9 GB/sec    1.00     17.3±0.03µs    33.0 GB/sec                                         
prebuilt/huge-en/rare-huge-needle                                    1.62     37.4±0.05µs    15.3 GB/sec    1.00     23.1±0.03µs    24.7 GB/sec    1.10     25.3±0.04µs    22.6 GB/sec                                         
prebuilt/huge-en/rare-long-needle                                    1.00     17.9±0.02µs    31.9 GB/sec    1.01     18.1±0.02µs    31.6 GB/sec    1.48     26.4±0.02µs    21.6 GB/sec                                         
prebuilt/huge-en/rare-medium-needle                                  1.01     18.7±0.02µs    30.5 GB/sec    1.00     18.5±0.02µs    30.9 GB/sec    2.23     41.2±0.08µs    13.9 GB/sec                                         
prebuilt/huge-en/rare-sherlock                                       1.02     17.7±0.03µs    32.2 GB/sec    1.01     17.5±0.03µs    32.7 GB/sec    1.00     17.4±0.02µs    32.8 GB/sec                                         
prebuilt/huge-en/rare-sherlock-holmes                                1.01     17.6±0.02µs    32.5 GB/sec    1.00     17.4±0.01µs    32.9 GB/sec    1.47     25.4±0.03µs    22.5 GB/sec                                         
prebuilt/huge-ru/never-john-watson                                   1.00     17.3±0.01µs    33.0 GB/sec    1.01     17.5±0.03µs    32.6 GB/sec    3.98     68.9±0.09µs     8.3 GB/sec                                         
prebuilt/huge-ru/rare-sherlock                                       1.01     17.6±0.03µs    32.5 GB/sec    1.00     17.4±0.03µs    32.9 GB/sec    2.13     37.0±0.04µs    15.4 GB/sec                                         
prebuilt/huge-ru/rare-sherlock-holmes                                1.01     15.3±0.02µs    37.2 GB/sec    1.00     15.2±0.02µs    37.5 GB/sec    3.90     59.4±0.08µs     9.6 GB/sec                                         
prebuilt/huge-zh/never-john-watson                                   1.00     16.3±0.02µs    35.0 GB/sec    1.00     16.3±0.03µs    35.1 GB/sec    1.61     26.1±0.02µs    21.9 GB/sec                                         
prebuilt/huge-zh/rare-sherlock                                       1.01     17.9±0.02µs    31.8 GB/sec    1.00     17.7±0.02µs    32.2 GB/sec    1.24     22.0±0.02µs    26.0 GB/sec                                         
prebuilt/huge-zh/rare-sherlock-holmes                                1.00     18.4±0.02µs    31.1 GB/sec    1.00     18.4±0.03µs    31.0 GB/sec    1.56     28.7±0.04µs    19.9 GB/sec                                         
prebuilt/pathological-defeat-simple-vector-freq/rare-alphabet        1.00     68.3±0.07µs     9.8 GB/sec    1.00     68.4±0.07µs     9.8 GB/sec    6.06    414.0±0.59µs  1658.5 MB/sec                                         
prebuilt/pathological-defeat-simple-vector-repeated/rare-alphabet    1.01   1227.6±2.00µs   559.4 MB/sec    1.00   1220.1±2.42µs   562.8 MB/sec    2.19      2.7±0.00ms   257.3 MB/sec                                         
prebuilt/pathological-defeat-simple-vector/rare-alphabet             1.77    207.9±0.39µs     2.5 GB/sec    1.74    205.3±0.37µs     2.5 GB/sec    1.00    117.7±0.17µs     4.4 GB/sec                                         
prebuilt/pathological-md5-huge/never-no-hash                         1.00      5.6±0.01µs    25.4 GB/sec    1.33      7.4±0.02µs    19.0 GB/sec    1.64      9.1±0.02µs    15.5 GB/sec                                         
prebuilt/pathological-md5-huge/rare-last-hash                        1.00      5.9±0.01µs    23.9 GB/sec    1.27      7.5±0.04µs    18.8 GB/sec    1.51      8.9±0.04µs    15.8 GB/sec                                         
prebuilt/pathological-repeated-rare-huge/never-tricky                1.00     14.1±0.02µs    33.1 GB/sec    1.01     14.3±0.01µs    32.6 GB/sec    1.00     14.1±0.01µs    33.0 GB/sec                                         
prebuilt/pathological-repeated-rare-small/never-tricky               1.20     36.0±0.04ns    25.9 GB/sec    1.44     43.3±0.21ns    21.6 GB/sec    1.00     30.1±0.23ns    31.0 GB/sec                                         
prebuilt/sliceslice-i386/words                                       1.00     23.2±0.04ms        0 B/sec                                           1.15     26.6±0.02ms        0 B/sec    13.03   302.2±0.30ms        0 B/sec  
prebuilt/sliceslice-words/words                                      1.06     81.1±0.06ms        0 B/sec                                           1.00     76.8±0.09ms        0 B/sec    3.47    266.3±0.25ms        0 B/sec  
prebuilt/teeny-en/never-all-common-bytes                             1.56      9.1±0.01ns     2.9 GB/sec    2.67     15.4±0.05ns  1729.8 MB/sec    1.00      5.8±0.07ns     4.5 GB/sec                                         
prebuilt/teeny-en/never-john-watson                                  1.77      9.1±0.02ns     2.9 GB/sec    2.98     15.3±0.03ns  1747.2 MB/sec    1.00      5.1±0.00ns     5.1 GB/sec                                         
prebuilt/teeny-en/never-some-rare-bytes                              1.68      9.0±0.01ns     2.9 GB/sec    2.84     15.3±0.02ns  1744.9 MB/sec    1.00      5.4±0.01ns     4.8 GB/sec                                         
prebuilt/teeny-en/never-two-space                                    1.67      8.8±0.01ns     3.0 GB/sec    2.86     15.0±0.02ns  1774.8 MB/sec    1.00      5.3±0.05ns     5.0 GB/sec                                         
prebuilt/teeny-en/rare-sherlock                                      2.06      9.5±0.01ns     2.7 GB/sec    3.63     16.7±0.04ns  1594.4 MB/sec    1.00      4.6±0.03ns     5.6 GB/sec                                         
prebuilt/teeny-en/rare-sherlock-holmes                               1.00     10.5±0.01ns     2.5 GB/sec    1.67     17.5±0.06ns  1529.6 MB/sec    1.77     18.5±0.05ns  1439.6 MB/sec                                         
prebuilt/teeny-ru/never-john-watson                                  1.10      8.1±0.01ns     4.8 GB/sec    2.03     15.0±0.11ns     2.6 GB/sec    1.00      7.4±0.01ns     5.3 GB/sec                                         
prebuilt/teeny-ru/rare-sherlock                                      1.56      9.5±0.02ns     4.1 GB/sec    2.84     17.4±0.11ns     2.3 GB/sec    1.00      6.1±0.01ns     6.4 GB/sec                                         
prebuilt/teeny-ru/rare-sherlock-holmes                               1.28     12.6±0.02ns     3.1 GB/sec    2.01     19.7±0.04ns  2036.8 MB/sec    1.00      9.8±0.01ns     4.0 GB/sec                                         
prebuilt/teeny-zh/never-john-watson                                  1.23      9.1±0.01ns     3.2 GB/sec    2.25     16.6±0.13ns  1786.0 MB/sec    1.00      7.3±0.01ns     3.9 GB/sec                                         
prebuilt/teeny-zh/rare-sherlock                                      2.38     10.2±0.03ns     2.8 GB/sec    4.57     19.7±0.05ns  1504.1 MB/sec    1.00      4.3±0.03ns     6.7 GB/sec                                         
prebuilt/teeny-zh/rare-sherlock-holmes                               1.25     19.5±0.04ns  1516.0 MB/sec    1.67     26.2±0.02ns  1128.6 MB/sec    1.00     15.7±0.02ns  1888.1 MB/sec                                         
prebuiltiter/code-rust-library/common-fn                             1.00     99.2±0.17µs    15.5 GB/sec    1.34    132.8±0.09µs    11.6 GB/sec                                                                                
prebuiltiter/code-rust-library/common-fn-is-empty                    1.00     52.4±0.10µs    29.3 GB/sec    1.02     53.4±0.08µs    28.7 GB/sec                                                                                
prebuiltiter/code-rust-library/common-let                            1.00    143.6±0.19µs    10.7 GB/sec    1.39    199.7±0.26µs     7.7 GB/sec                                                                                
prebuiltiter/code-rust-library/common-paren                          1.00    397.8±0.43µs     3.9 GB/sec    1.67    664.5±1.20µs     2.3 GB/sec                                                                                
prebuiltiter/huge-en/common-one-space                                1.00    573.7±0.77µs  1019.6 MB/sec    2.34   1340.5±3.62µs   436.3 MB/sec                                                                                
prebuiltiter/huge-en/common-that                                     1.00     40.0±0.09µs    14.3 GB/sec    1.33     53.3±0.07µs    10.7 GB/sec                                                                                
prebuiltiter/huge-en/common-you                                      1.00     98.5±0.13µs     5.8 GB/sec    1.66    163.6±0.56µs     3.5 GB/sec                                                                                
prebuiltiter/huge-ru/common-not                                      1.00     68.0±0.08µs     8.4 GB/sec    1.67    113.6±0.15µs     5.0 GB/sec                                                                                
prebuiltiter/huge-ru/common-one-space                                1.00    308.2±0.47µs  1898.1 MB/sec    2.19    674.4±2.24µs   867.5 MB/sec                                                                                
prebuiltiter/huge-ru/common-that                                     1.00     35.3±0.06µs    16.2 GB/sec    1.35     47.8±0.10µs    12.0 GB/sec                                                                                
prebuiltiter/huge-zh/common-do-not                                   1.00     65.1±0.12µs     8.8 GB/sec    1.53     99.8±0.54µs     5.7 GB/sec                                                                                
prebuiltiter/huge-zh/common-one-space                                1.00    170.1±0.19µs     3.4 GB/sec    1.86    315.8±2.57µs  1852.7 MB/sec                                                                                
prebuiltiter/huge-zh/common-that                                     1.00     33.9±0.03µs    16.9 GB/sec    1.42     48.2±0.22µs    11.9 GB/sec                                                                                
prebuiltiter/pathological-md5-huge/common-two-bytes                  1.00     11.2±0.02µs    12.5 GB/sec    1.67     18.8±0.10µs     7.5 GB/sec                                                                                
prebuiltiter/pathological-repeated-rare-huge/common-match            1.00    500.8±0.68µs   952.3 MB/sec    2.14   1071.6±1.46µs   445.1 MB/sec                                                                                
prebuiltiter/pathological-repeated-rare-small/common-match           1.00    973.9±1.58ns   980.2 MB/sec    2.17      2.1±0.00µs   452.6 MB/sec                                                                                
```