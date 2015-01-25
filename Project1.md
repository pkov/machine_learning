---
title: "Practical Machine Learning - Course Project"
output: html_document
---



## Loading and processing the data
The training data were obtained from  https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv


Loading the data


```r
pml<-read.csv("pml-training.csv")
```

```
## Warning: cannot open file 'pml-training.csv': No such file or directory
```

```
## Error: cannot open the connection
```

A quick summary of the data set shows us that there are lot of variables
containing more then 90% of NA values.

```r
knitr::kable(summary(pml))
```



|   |      X         |   user_name    |raw_timestamp_part_1 |raw_timestamp_part_2 |         cvtd_timestamp  |new_window  |  num_window  |  roll_belt     |  pitch_belt     |   yaw_belt      |total_accel_belt |kurtosis_roll_belt |kurtosis_picth_belt |kurtosis_yaw_belt |skewness_roll_belt |skewness_roll_belt.1 |skewness_yaw_belt |max_roll_belt   |max_picth_belt  | max_yaw_belt   |min_roll_belt   |min_pitch_belt  | min_yaw_belt   |amplitude_roll_belt |amplitude_pitch_belt |amplitude_yaw_belt |var_total_accel_belt |avg_roll_belt   |stddev_roll_belt |var_roll_belt   |avg_pitch_belt  |stddev_pitch_belt |var_pitch_belt  | avg_yaw_belt   |stddev_yaw_belt | var_yaw_belt   | gyros_belt_x     | gyros_belt_y     | gyros_belt_z    | accel_belt_x     | accel_belt_y   | accel_belt_z    |magnet_belt_x   |magnet_belt_y |magnet_belt_z  |   roll_arm      |  pitch_arm      |   yaw_arm        |total_accel_arm |var_accel_arm   | avg_roll_arm   |stddev_roll_arm | var_roll_arm   |avg_pitch_arm   |stddev_pitch_arm |var_pitch_arm   | avg_yaw_arm    |stddev_yaw_arm  | var_yaw_arm    | gyros_arm_x     | gyros_arm_y     | gyros_arm_z    | accel_arm_x     | accel_arm_y     | accel_arm_z     | magnet_arm_x  | magnet_arm_y  | magnet_arm_z  |kurtosis_roll_arm |kurtosis_picth_arm |kurtosis_yaw_arm |skewness_roll_arm |skewness_pitch_arm |skewness_yaw_arm | max_roll_arm   |max_picth_arm   | max_yaw_arm    | min_roll_arm   |min_pitch_arm   | min_yaw_arm    |amplitude_roll_arm |amplitude_pitch_arm |amplitude_yaw_arm |roll_dumbbell    |pitch_dumbbell   | yaw_dumbbell     |kurtosis_roll_dumbbell |kurtosis_picth_dumbbell |kurtosis_yaw_dumbbell |skewness_roll_dumbbell |skewness_pitch_dumbbell |skewness_yaw_dumbbell |max_roll_dumbbell |max_picth_dumbbell |max_yaw_dumbbell |min_roll_dumbbell |min_pitch_dumbbell |min_yaw_dumbbell |amplitude_roll_dumbbell |amplitude_pitch_dumbbell |amplitude_yaw_dumbbell |total_accel_dumbbell |var_accel_dumbbell |avg_roll_dumbbell |stddev_roll_dumbbell |var_roll_dumbbell |avg_pitch_dumbbell |stddev_pitch_dumbbell |var_pitch_dumbbell |avg_yaw_dumbbell |stddev_yaw_dumbbell |var_yaw_dumbbell |gyros_dumbbell_x  |gyros_dumbbell_y |gyros_dumbbell_z |accel_dumbbell_x |accel_dumbbell_y |accel_dumbbell_z |magnet_dumbbell_x |magnet_dumbbell_y |magnet_dumbbell_z | roll_forearm     |pitch_forearm    | yaw_forearm     |kurtosis_roll_forearm |kurtosis_picth_forearm |kurtosis_yaw_forearm |skewness_roll_forearm |skewness_pitch_forearm |skewness_yaw_forearm |max_roll_forearm |max_picth_forearm |max_yaw_forearm |min_roll_forearm |min_pitch_forearm |min_yaw_forearm |amplitude_roll_forearm |amplitude_pitch_forearm |amplitude_yaw_forearm |total_accel_forearm |var_accel_forearm |avg_roll_forearm |stddev_roll_forearm |var_roll_forearm |avg_pitch_forearm |stddev_pitch_forearm |var_pitch_forearm |avg_yaw_forearm |stddev_yaw_forearm |var_yaw_forearm |gyros_forearm_x   |gyros_forearm_y  |gyros_forearm_z  |accel_forearm_x  |accel_forearm_y |accel_forearm_z  |magnet_forearm_x |magnet_forearm_y |magnet_forearm_z |classe   |
|:--|:---------------|:---------------|:--------------------|:--------------------|:------------------------|:-----------|:-------------|:---------------|:----------------|:----------------|:----------------|:------------------|:-------------------|:-----------------|:------------------|:--------------------|:-----------------|:---------------|:---------------|:---------------|:---------------|:---------------|:---------------|:-------------------|:--------------------|:------------------|:--------------------|:---------------|:----------------|:---------------|:---------------|:-----------------|:---------------|:---------------|:---------------|:---------------|:-----------------|:-----------------|:----------------|:-----------------|:---------------|:----------------|:---------------|:-------------|:--------------|:----------------|:----------------|:-----------------|:---------------|:---------------|:---------------|:---------------|:---------------|:---------------|:----------------|:---------------|:---------------|:---------------|:---------------|:----------------|:----------------|:---------------|:----------------|:----------------|:----------------|:--------------|:--------------|:--------------|:-----------------|:------------------|:----------------|:-----------------|:------------------|:----------------|:---------------|:---------------|:---------------|:---------------|:---------------|:---------------|:------------------|:-------------------|:-----------------|:----------------|:----------------|:-----------------|:----------------------|:-----------------------|:---------------------|:----------------------|:-----------------------|:---------------------|:-----------------|:------------------|:----------------|:-----------------|:------------------|:----------------|:-----------------------|:------------------------|:----------------------|:--------------------|:------------------|:-----------------|:--------------------|:-----------------|:------------------|:---------------------|:------------------|:----------------|:-------------------|:----------------|:-----------------|:----------------|:----------------|:----------------|:----------------|:----------------|:-----------------|:-----------------|:-----------------|:-----------------|:----------------|:----------------|:---------------------|:----------------------|:--------------------|:---------------------|:----------------------|:--------------------|:----------------|:-----------------|:---------------|:----------------|:-----------------|:---------------|:----------------------|:-----------------------|:---------------------|:-------------------|:-----------------|:----------------|:-------------------|:----------------|:-----------------|:--------------------|:-----------------|:---------------|:------------------|:---------------|:-----------------|:----------------|:----------------|:----------------|:---------------|:----------------|:----------------|:----------------|:----------------|:--------|
|   |Min.   :    1   |adelmo  :3892   |Min.   :1.32e+09     |Min.   :   294       |28/11/2011 14:14: 1498   |no :19216   |Min.   :  1   |Min.   :-28.9   |Min.   :-55.80   |Min.   :-180.0   |Min.   : 0.0     |         :19216    |         :19216     |       :19216     |         :19216    |         :19216      |       :19216     |Min.   :-94     |Min.   : 3      |       :19216   |Min.   :-180    |Min.   : 0      |       :19216   |Min.   :  0         |Min.   : 0           |       :19216      |Min.   : 0           |Min.   :-27     |Min.   : 0       |Min.   :  0     |Min.   :-51     |Min.   :0         |Min.   : 0      |Min.   :-138    |Min.   :  0     |Min.   :    0   |Min.   :-1.0400   |Min.   :-0.6400   |Min.   :-1.460   |Min.   :-120.00   |Min.   :-69.0   |Min.   :-275.0   |Min.   :-52.0   |Min.   :354   |Min.   :-623   |Min.   :-180.0   |Min.   :-88.80   |Min.   :-180.00   |Min.   : 1.0    |Min.   :  0     |Min.   :-167    |Min.   :  0     |Min.   :    0   |Min.   :-82     |Min.   : 0       |Min.   :   0    |Min.   :-173    |Min.   :  0     |Min.   :    0   |Min.   :-6.370   |Min.   :-3.440   |Min.   :-2.33   |Min.   :-404.0   |Min.   :-318.0   |Min.   :-636.0   |Min.   :-584   |Min.   :-392   |Min.   :-597   |        :19216    |        :19216     |        :19216   |        :19216    |        :19216     |        :19216   |Min.   :-73     |Min.   :-173    |Min.   : 4      |Min.   :-89     |Min.   :-180    |Min.   : 1      |Min.   :  0        |Min.   :  0         |Min.   : 0        |Min.   :-153.7   |Min.   :-149.6   |Min.   :-150.87   |       :19216          |       :19216           |       :19216         |       :19216          |       :19216           |       :19216         |Min.   :-70       |Min.   :-113       |       :19216    |Min.   :-150      |Min.   :-147       |       :19216    |Min.   :  0             |Min.   :  0              |       :19216          |Min.   : 0.0         |Min.   :  0        |Min.   :-129      |Min.   :  0          |Min.   :    0     |Min.   :-71        |Min.   : 0            |Min.   :   0       |Min.   :-118     |Min.   :  0         |Min.   :    0    |Min.   :-204.00   |Min.   :-2.10    |Min.   : -2.4    |Min.   :-419.0   |Min.   :-189.0   |Min.   :-334.0   |Min.   :-643      |Min.   :-3600     |Min.   :-262.0    |Min.   :-180.00   |Min.   :-72.50   |Min.   :-180.0   |       :19216         |       :19216          |       :19216        |       :19216         |       :19216          |       :19216        |Min.   :-67      |Min.   :-151      |       :19216   |Min.   :-72      |Min.   :-180      |       :19216   |Min.   :  0            |Min.   :  0             |       :19216         |Min.   :  0.0       |Min.   :  0       |Min.   :-177     |Min.   :  0         |Min.   :    0    |Min.   :-68       |Min.   : 0           |Min.   :   0      |Min.   :-155    |Min.   :  0        |Min.   :    0   |Min.   :-22.000   |Min.   : -7.02   |Min.   : -8.09   |Min.   :-498.0   |Min.   :-632    |Min.   :-446.0   |Min.   :-1280    |Min.   :-896     |Min.   :-973     |A:5580   |
|   |1st Qu.: 4906   |carlitos:3112   |1st Qu.:1.32e+09     |1st Qu.:252912       |05/12/2011 11:24: 1497   |yes:  406   |1st Qu.:222   |1st Qu.:  1.1   |1st Qu.:  1.76   |1st Qu.: -88.3   |1st Qu.: 3.0     |#DIV/0!  :   10    |#DIV/0!  :   32     |#DIV/0!:  406     |#DIV/0!  :    9    |#DIV/0!  :   32      |#DIV/0!:  406     |1st Qu.:-88     |1st Qu.: 5      |-1.1   :   30   |1st Qu.: -88    |1st Qu.: 3      |-1.1   :   30   |1st Qu.:  0         |1st Qu.: 1           |#DIV/0!:   10      |1st Qu.: 0           |1st Qu.:  1     |1st Qu.: 0       |1st Qu.:  0     |1st Qu.:  2     |1st Qu.:0         |1st Qu.: 0      |1st Qu.: -88    |1st Qu.:  0     |1st Qu.:    0   |1st Qu.:-0.0300   |1st Qu.: 0.0000   |1st Qu.:-0.200   |1st Qu.: -21.00   |1st Qu.:  3.0   |1st Qu.:-162.0   |1st Qu.:  9.0   |1st Qu.:581   |1st Qu.:-375   |1st Qu.: -31.8   |1st Qu.:-25.90   |1st Qu.: -43.10   |1st Qu.:17.0    |1st Qu.:  9     |1st Qu.: -38    |1st Qu.:  1     |1st Qu.:    2   |1st Qu.:-23     |1st Qu.: 2       |1st Qu.:   3    |1st Qu.: -29    |1st Qu.:  3     |1st Qu.:    7   |1st Qu.:-1.330   |1st Qu.:-0.800   |1st Qu.:-0.07   |1st Qu.:-242.0   |1st Qu.: -54.0   |1st Qu.:-143.0   |1st Qu.:-300   |1st Qu.:  -9   |1st Qu.: 131   |#DIV/0! :   78    |#DIV/0! :   80     |#DIV/0! :   11   |#DIV/0! :   77    |#DIV/0! :   80     |#DIV/0! :   11   |1st Qu.:  0     |1st Qu.:  -2    |1st Qu.:29      |1st Qu.:-42     |1st Qu.: -73    |1st Qu.: 8      |1st Qu.:  5        |1st Qu.: 10         |1st Qu.:13        |1st Qu.: -18.5   |1st Qu.: -40.9   |1st Qu.: -77.64   |#DIV/0!:    5          |-0.5464:    2           |#DIV/0!:  406         |#DIV/0!:    4          |-0.2328:    2           |#DIV/0!:  406         |1st Qu.:-27       |1st Qu.: -67       |-0.6   :   20    |1st Qu.: -60      |1st Qu.: -92       |-0.6   :   20    |1st Qu.: 15             |1st Qu.: 17              |#DIV/0!:    5          |1st Qu.: 4.0         |1st Qu.:  0        |1st Qu.: -12      |1st Qu.:  5          |1st Qu.:   22     |1st Qu.:-42        |1st Qu.: 3            |1st Qu.:  12       |1st Qu.: -77     |1st Qu.:  4         |1st Qu.:   15    |1st Qu.:  -0.03   |1st Qu.:-0.14    |1st Qu.: -0.3    |1st Qu.: -50.0   |1st Qu.:  -8.0   |1st Qu.:-142.0   |1st Qu.:-535      |1st Qu.:  231     |1st Qu.: -45.0    |1st Qu.:  -0.74   |1st Qu.:  0.00   |1st Qu.: -68.6   |#DIV/0!:   84         |#DIV/0!:   85          |#DIV/0!:  406        |#DIV/0!:   83         |#DIV/0!:   85          |#DIV/0!:  406        |1st Qu.:  0      |1st Qu.:   0      |#DIV/0!:   84   |1st Qu.: -6      |1st Qu.:-175      |#DIV/0!:   84   |1st Qu.:  1            |1st Qu.:  2             |#DIV/0!:   84         |1st Qu.: 29.0       |1st Qu.:  7       |1st Qu.:  -1     |1st Qu.:  0         |1st Qu.:    0    |1st Qu.:  0       |1st Qu.: 0           |1st Qu.:   0      |1st Qu.: -26    |1st Qu.:  1        |1st Qu.:    0   |1st Qu.: -0.220   |1st Qu.: -1.46   |1st Qu.: -0.18   |1st Qu.:-178.0   |1st Qu.:  57    |1st Qu.:-182.0   |1st Qu.: -616    |1st Qu.:   2     |1st Qu.: 191     |B:3797   |
|   |Median : 9812   |eurico  :3070   |Median :1.32e+09     |Median :496380       |30/11/2011 17:11: 1440   |NA          |Median :424   |Median :113.0   |Median :  5.28   |Median : -13.0   |Median :17.0     |-1.908453:    2    |47.000000:    4     |NA                |0.000000 :    4    |0.000000 :    4      |NA                |Median : -5     |Median :18      |-1.4   :   29   |Median :  -8    |Median :16      |-1.4   :   29   |Median :  1         |Median : 1           |0.00   :   12      |Median : 0           |Median :116     |Median : 0       |Median :  0     |Median :  5     |Median :0         |Median : 0      |Median :  -7    |Median :  0     |Median :    0   |Median : 0.0300   |Median : 0.0200   |Median :-0.100   |Median : -15.00   |Median : 35.0   |Median :-152.0   |Median : 35.0   |Median :601   |Median :-320   |Median :   0.0   |Median :  0.00   |Median :   0.00   |Median :27.0    |Median : 41     |Median :   0    |Median :  6     |Median :   33   |Median :  0     |Median : 8       |Median :  66    |Median :   0    |Median : 17     |Median :  278   |Median : 0.080   |Median :-0.240   |Median : 0.23   |Median : -44.0   |Median :  14.0   |Median : -47.0   |Median : 289   |Median : 202   |Median : 444   |-0.02438:    1    |-0.00484:    1     |0.55844 :    2   |-0.00051:    1    |-0.00184:    1     |-1.62032:    2   |Median :  5     |Median :  23    |Median :34      |Median :-22     |Median : -34    |Median :13      |Median : 28        |Median : 55         |Median :22        |Median :  48.2   |Median : -21.0   |Median :  -3.32   |-0.2583:    2          |-0.9334:    2           |NA                    |-0.9324:    2          |-0.3521:    2           |NA                    |Median : 15       |Median :  40       |0.2    :   19    |Median : -44      |Median : -66       |0.2    :   19    |Median : 35             |Median : 42              |0.00   :  401          |Median :10.0         |Median :  1        |Median :  48      |Median : 12          |Median :  149     |Median :-20        |Median : 8            |Median :  65       |Median :  -5     |Median : 10         |Median :  105    |Median :   0.13   |Median : 0.03    |Median : -0.1    |Median :  -8.0   |Median :  41.5   |Median :  -1.0   |Median :-479      |Median :  311     |Median :  13.0    |Median :  21.70   |Median :  9.24   |Median :   0.0   |-0.8079:    2         |-0.0073:    1          |NA                   |-0.1912:    2         |0.0000 :    4          |NA                   |Median : 27      |Median : 113      |-1.2   :   32   |Median :  0      |Median : -61      |-1.2   :   32   |Median : 18            |Median : 84             |0.00   :  322         |Median : 36.0       |Median : 21       |Median :  11     |Median :  8         |Median :   64    |Median : 12       |Median : 6           |Median :  30      |Median :   0    |Median : 25        |Median :  612   |Median :  0.050   |Median :  0.03   |Median :  0.08   |Median : -57.0   |Median : 201    |Median : -39.0   |Median : -378    |Median : 591     |Median : 511     |C:3422   |
|   |Mean   : 9812   |charles :3536   |Mean   :1.32e+09     |Mean   :500656       |05/12/2011 11:25: 1425   |NA          |Mean   :431   |Mean   : 64.4   |Mean   :  0.31   |Mean   : -11.2   |Mean   :11.3     |-0.016850:    1    |-0.150950:    3     |NA                |0.422463 :    2    |-2.156553:    3      |NA                |Mean   : -7     |Mean   :13      |-1.2   :   26   |Mean   : -10    |Mean   :11      |-1.2   :   26   |Mean   :  4         |Mean   : 2           |0.0000 :  384      |Mean   : 1           |Mean   : 68     |Mean   : 1       |Mean   :  8     |Mean   :  1     |Mean   :1         |Mean   : 1      |Mean   :  -9    |Mean   :  1     |Mean   :  107   |Mean   :-0.0056   |Mean   : 0.0396   |Mean   :-0.130   |Mean   :  -5.59   |Mean   : 30.1   |Mean   : -72.6   |Mean   : 55.6   |Mean   :594   |Mean   :-346   |Mean   :  17.8   |Mean   : -4.61   |Mean   :  -0.62   |Mean   :25.5    |Mean   : 53     |Mean   :  13    |Mean   : 11     |Mean   :  417   |Mean   : -5     |Mean   :10       |Mean   : 196    |Mean   :   2    |Mean   : 22     |Mean   : 1056   |Mean   : 0.043   |Mean   :-0.257   |Mean   : 0.27   |Mean   : -60.2   |Mean   :  32.6   |Mean   : -71.2   |Mean   : 192   |Mean   : 157   |Mean   : 306   |-0.04190:    1    |-0.01311:    1     |0.65132 :    2   |-0.00696:    1    |-0.01185:    1     |0.55053 :    2   |Mean   : 11     |Mean   :  36    |Mean   :35      |Mean   :-21     |Mean   : -34    |Mean   :15      |Mean   : 32        |Mean   : 70         |Mean   :21        |Mean   :  23.8   |Mean   : -10.8   |Mean   :   1.67   |-0.3705:    2          |-2.0833:    2           |NA                    |0.1110 :    2          |-0.7036:    2           |NA                    |Mean   : 14       |Mean   :  33       |-0.8   :   18    |Mean   : -41      |Mean   : -33       |-0.8   :   18    |Mean   : 55             |Mean   : 66              |NA                     |Mean   :13.7         |Mean   :  4        |Mean   :  24      |Mean   : 21          |Mean   : 1020     |Mean   :-12        |Mean   :13            |Mean   : 350       |Mean   :   0     |Mean   : 17         |Mean   :  590    |Mean   :   0.16   |Mean   : 0.05    |Mean   : -0.1    |Mean   : -28.6   |Mean   :  52.6   |Mean   : -38.3   |Mean   :-328      |Mean   :  221     |Mean   :  46.1    |Mean   :  33.83   |Mean   : 10.71   |Mean   :  19.2   |-0.9169:    2         |-0.0442:    1          |NA                   |-0.4126:    2         |-0.6992:    2          |NA                   |Mean   : 24      |Mean   :  81      |-1.3   :   31   |Mean   :  0      |Mean   : -58      |-1.3   :   31   |Mean   : 25            |Mean   :139             |NA                    |Mean   : 34.7       |Mean   : 34       |Mean   :  33     |Mean   : 42         |Mean   : 5274    |Mean   : 12       |Mean   : 8           |Mean   : 140      |Mean   :  18    |Mean   : 45        |Mean   : 4640   |Mean   :  0.158   |Mean   :  0.08   |Mean   :  0.15   |Mean   : -61.7   |Mean   : 164    |Mean   : -55.3   |Mean   : -313    |Mean   : 380     |Mean   : 394     |D:3216   |
|   |3rd Qu.:14717   |jeremy  :3402   |3rd Qu.:1.32e+09     |3rd Qu.:751891       |02/12/2011 14:57: 1380   |NA          |3rd Qu.:644   |3rd Qu.:123.0   |3rd Qu.: 14.90   |3rd Qu.:  12.9   |3rd Qu.:18.0     |-0.021024:    1    |-0.684748:    3     |NA                |-0.003095:    1    |-3.072669:    3      |NA                |3rd Qu.: 18     |3rd Qu.:19      |-0.9   :   24   |3rd Qu.:   9    |3rd Qu.:17      |-0.9   :   24   |3rd Qu.:  2         |3rd Qu.: 2           |NA                 |3rd Qu.: 0           |3rd Qu.:123     |3rd Qu.: 1       |3rd Qu.:  0     |3rd Qu.: 16     |3rd Qu.:1         |3rd Qu.: 0      |3rd Qu.:  14    |3rd Qu.:  1     |3rd Qu.:    0   |3rd Qu.: 0.1100   |3rd Qu.: 0.1100   |3rd Qu.:-0.020   |3rd Qu.:  -5.00   |3rd Qu.: 61.0   |3rd Qu.:  27.0   |3rd Qu.: 59.0   |3rd Qu.:610   |3rd Qu.:-306   |3rd Qu.:  77.3   |3rd Qu.: 11.20   |3rd Qu.:  45.88   |3rd Qu.:33.0    |3rd Qu.: 76     |3rd Qu.:  76    |3rd Qu.: 15     |3rd Qu.:  223   |3rd Qu.:  8     |3rd Qu.:16       |3rd Qu.: 267    |3rd Qu.:  38    |3rd Qu.: 36     |3rd Qu.: 1295   |3rd Qu.: 1.570   |3rd Qu.: 0.140   |3rd Qu.: 0.72   |3rd Qu.:  84.0   |3rd Qu.: 139.0   |3rd Qu.:  23.0   |3rd Qu.: 637   |3rd Qu.: 323   |3rd Qu.: 545   |-0.05051:    1    |-0.02967:    1     |-0.01548:    1   |-0.01884:    1    |-0.01247:    1     |-0.00311:    1   |3rd Qu.: 27     |3rd Qu.:  96    |3rd Qu.:41      |3rd Qu.:  0     |3rd Qu.:   0    |3rd Qu.:19      |3rd Qu.: 51        |3rd Qu.:115         |3rd Qu.:29        |3rd Qu.:  67.6   |3rd Qu.:  17.5   |3rd Qu.:  79.64   |-0.5855:    2          |-2.0851:    2           |NA                    |1.0312 :    2          |0.1090 :    2           |NA                    |3rd Qu.: 51       |3rd Qu.: 133       |-0.3   :   16    |3rd Qu.: -25      |3rd Qu.:  21       |-0.3   :   16    |3rd Qu.: 81             |3rd Qu.:100              |NA                     |3rd Qu.:19.0         |3rd Qu.:  3        |3rd Qu.:  64      |3rd Qu.: 26          |3rd Qu.:  695     |3rd Qu.: 13        |3rd Qu.:19            |3rd Qu.: 370       |3rd Qu.:  71     |3rd Qu.: 25         |3rd Qu.:  609    |3rd Qu.:   0.35   |3rd Qu.: 0.21    |3rd Qu.:  0.0    |3rd Qu.:  11.0   |3rd Qu.: 111.0   |3rd Qu.:  38.0   |3rd Qu.:-304      |3rd Qu.:  390     |3rd Qu.:  95.0    |3rd Qu.: 140.00   |3rd Qu.: 28.40   |3rd Qu.: 110.0   |-0.0227:    1         |-0.0489:    1          |NA                   |-0.0004:    1         |-0.0113:    1          |NA                   |3rd Qu.: 46      |3rd Qu.: 175      |-1.4   :   24   |3rd Qu.: 12      |3rd Qu.:   0      |-1.4   :   24   |3rd Qu.: 40            |3rd Qu.:350             |NA                    |3rd Qu.: 41.0       |3rd Qu.: 51       |3rd Qu.: 107     |3rd Qu.: 85         |3rd Qu.: 7289    |3rd Qu.: 28       |3rd Qu.:13           |3rd Qu.: 166      |3rd Qu.:  86    |3rd Qu.: 86        |3rd Qu.: 7368   |3rd Qu.:  0.560   |3rd Qu.:  1.62   |3rd Qu.:  0.49   |3rd Qu.:  76.0   |3rd Qu.: 312    |3rd Qu.:  26.0   |3rd Qu.:  -73    |3rd Qu.: 737     |3rd Qu.: 653     |E:3607   |
|   |Max.   :19622   |pedro   :2610   |Max.   :1.32e+09     |Max.   :998801       |02/12/2011 13:34: 1375   |NA          |Max.   :864   |Max.   :162.0   |Max.   : 60.30   |Max.   : 179.0   |Max.   :29.0     |-0.025513:    1    |-1.750749:    3     |NA                |-0.010002:    1    |-6.324555:    3      |NA                |Max.   :180     |Max.   :30      |-1.3   :   22   |Max.   : 173    |Max.   :23      |-1.3   :   22   |Max.   :360         |Max.   :12           |NA                 |Max.   :16           |Max.   :157     |Max.   :14       |Max.   :201     |Max.   : 60     |Max.   :4         |Max.   :16      |Max.   : 174    |Max.   :177     |Max.   :31183   |Max.   : 2.2200   |Max.   : 0.6400   |Max.   : 1.620   |Max.   :  85.00   |Max.   :164.0   |Max.   : 105.0   |Max.   :485.0   |Max.   :673   |Max.   : 293   |Max.   : 180.0   |Max.   : 88.50   |Max.   : 180.00   |Max.   :66.0    |Max.   :332     |Max.   : 163    |Max.   :162     |Max.   :26232   |Max.   : 76     |Max.   :43       |Max.   :1885    |Max.   : 152    |Max.   :177     |Max.   :31345   |Max.   : 4.870   |Max.   : 2.840   |Max.   : 3.02   |Max.   : 437.0   |Max.   : 308.0   |Max.   : 292.0   |Max.   : 782   |Max.   : 583   |Max.   : 694   |-0.05695:    1    |-0.07394:    1     |-0.01749:    1   |-0.03359:    1    |-0.02063:    1     |-0.00562:    1   |Max.   : 86     |Max.   : 180    |Max.   :65      |Max.   : 66     |Max.   : 152    |Max.   :38      |Max.   :120        |Max.   :360         |Max.   :52        |Max.   : 153.6   |Max.   : 149.4   |Max.   : 154.95   |-2.0851:    2          |-2.0889:    2           |NA                    |-0.0082:    1          |1.0326 :    2           |NA                    |Max.   :137       |Max.   : 155       |-0.2   :   15    |Max.   :  73      |Max.   : 121       |-0.2   :   15    |Max.   :256             |Max.   :274              |NA                     |Max.   :58.0         |Max.   :230        |Max.   : 126      |Max.   :124          |Max.   :15321     |Max.   : 94        |Max.   :83            |Max.   :6836       |Max.   : 135     |Max.   :107         |Max.   :11468    |Max.   :   2.22   |Max.   :52.00    |Max.   :317.0    |Max.   : 235.0   |Max.   : 315.0   |Max.   : 318.0   |Max.   : 592      |Max.   :  633     |Max.   : 452.0    |Max.   : 180.00   |Max.   : 89.80   |Max.   : 180.0   |-0.0359:    1         |-0.0523:    1          |NA                   |-0.0013:    1         |-0.0131:    1          |NA                   |Max.   : 90      |Max.   : 180      |-1.5   :   24   |Max.   : 62      |Max.   : 167      |-1.5   :   24   |Max.   :126            |Max.   :360             |NA                    |Max.   :108.0       |Max.   :173       |Max.   : 177     |Max.   :179         |Max.   :32102    |Max.   : 72       |Max.   :48           |Max.   :2280      |Max.   : 169    |Max.   :198        |Max.   :39009   |Max.   :  3.970   |Max.   :311.00   |Max.   :231.00   |Max.   : 477.0   |Max.   : 923    |Max.   : 291.0   |Max.   :  672    |Max.   :1480     |Max.   :1090     |NA       |
|   |NA              |NA              |NA                   |NA                   |(Other)         :11007   |NA          |NA            |NA              |NA               |NA               |NA               |(Other)  :  391    |(Other)  :  361     |NA                |(Other)  :  389    |(Other)  :  361      |NA                |NA's   :19216   |NA's   :19216   |(Other):  275   |NA's   :19216   |NA's   :19216   |(Other):  275   |NA's   :19216       |NA's   :19216        |NA                 |NA's   :19216        |NA's   :19216   |NA's   :19216    |NA's   :19216   |NA's   :19216   |NA's   :19216     |NA's   :19216   |NA's   :19216   |NA's   :19216   |NA's   :19216   |NA                |NA                |NA               |NA                |NA              |NA               |NA              |NA            |NA             |NA               |NA               |NA                |NA              |NA's   :19216   |NA's   :19216   |NA's   :19216   |NA's   :19216   |NA's   :19216   |NA's   :19216    |NA's   :19216   |NA's   :19216   |NA's   :19216   |NA's   :19216   |NA               |NA               |NA              |NA               |NA               |NA               |NA             |NA             |NA             |(Other) :  324    |(Other) :  322     |(Other) :  389   |(Other) :  325    |(Other) :  322     |(Other) :  389   |NA's   :19216   |NA's   :19216   |NA's   :19216   |NA's   :19216   |NA's   :19216   |NA's   :19216   |NA's   :19216      |NA's   :19216       |NA's   :19216     |NA               |NA               |NA                |(Other):  393          |(Other):  396           |NA                    |(Other):  395          |(Other):  396           |NA                    |NA's   :19216     |NA's   :19216      |(Other):  318    |NA's   :19216     |NA's   :19216      |(Other):  318    |NA's   :19216           |NA's   :19216            |NA                     |NA                   |NA's   :19216      |NA's   :19216     |NA's   :19216        |NA's   :19216     |NA's   :19216      |NA's   :19216         |NA's   :19216      |NA's   :19216    |NA's   :19216       |NA's   :19216    |NA                |NA               |NA               |NA               |NA               |NA               |NA                |NA                |NA                |NA                |NA               |NA               |(Other):  316         |(Other):  317          |NA                   |(Other):  317         |(Other):  313          |NA                   |NA's   :19216    |NA's   :19216     |(Other):  211   |NA's   :19216    |NA's   :19216     |(Other):  211   |NA's   :19216          |NA's   :19216           |NA                    |NA                  |NA's   :19216     |NA's   :19216    |NA's   :19216       |NA's   :19216    |NA's   :19216     |NA's   :19216        |NA's   :19216     |NA's   :19216   |NA's   :19216      |NA's   :19216   |NA                |NA               |NA               |NA               |NA              |NA               |NA               |NA               |NA               |NA       |


We will remove such variables

```r
pml2<-pml[,-grep("avg_|max_|min_|var_|stddev_|skewness_|kurtosis_|amplitude_",names(pml))]
colSums(is.na(pml2))
```

```
##                    X            user_name raw_timestamp_part_1 
##                    0                    0                    0 
## raw_timestamp_part_2       cvtd_timestamp           new_window 
##                    0                    0                    0 
##           num_window            roll_belt           pitch_belt 
##                    0                    0                    0 
##             yaw_belt     total_accel_belt         gyros_belt_x 
##                    0                    0                    0 
##         gyros_belt_y         gyros_belt_z         accel_belt_x 
##                    0                    0                    0 
##         accel_belt_y         accel_belt_z        magnet_belt_x 
##                    0                    0                    0 
##        magnet_belt_y        magnet_belt_z             roll_arm 
##                    0                    0                    0 
##            pitch_arm              yaw_arm      total_accel_arm 
##                    0                    0                    0 
##          gyros_arm_x          gyros_arm_y          gyros_arm_z 
##                    0                    0                    0 
##          accel_arm_x          accel_arm_y          accel_arm_z 
##                    0                    0                    0 
##         magnet_arm_x         magnet_arm_y         magnet_arm_z 
##                    0                    0                    0 
##        roll_dumbbell       pitch_dumbbell         yaw_dumbbell 
##                    0                    0                    0 
## total_accel_dumbbell     gyros_dumbbell_x     gyros_dumbbell_y 
##                    0                    0                    0 
##     gyros_dumbbell_z     accel_dumbbell_x     accel_dumbbell_y 
##                    0                    0                    0 
##     accel_dumbbell_z    magnet_dumbbell_x    magnet_dumbbell_y 
##                    0                    0                    0 
##    magnet_dumbbell_z         roll_forearm        pitch_forearm 
##                    0                    0                    0 
##          yaw_forearm  total_accel_forearm      gyros_forearm_x 
##                    0                    0                    0 
##      gyros_forearm_y      gyros_forearm_z      accel_forearm_x 
##                    0                    0                    0 
##      accel_forearm_y      accel_forearm_z     magnet_forearm_x 
##                    0                    0                    0 
##     magnet_forearm_y     magnet_forearm_z               classe 
##                    0                    0                    0
```

We will also remove the first 7 variables  as our model should be independent of them.

```r
pml2<-pml2[,-c(1:7)]
```


## Machine learning model
Now we will split the dataset into a training and testing part.
The trainig part will contain 60% of data and will be used for training of our model. The test part will be used for cross validation


```r
library(caret)
set.seed(1234)
InTrain<-createDataPartition(y=pml2$classe,p=0.6,list=FALSE)
training<-pml2[InTrain,]
testing<-pml2[-InTrain,]
```

As this is a classification problem we've choosen a model based on random forrest algorithm.


```r
rf<-train(classe~.,data=training,method="rf")
```


```r
> > > rf
Random Forest 

11776 samples
   52 predictor
    5 classes: 'A', 'B', 'C', 'D', 'E' 

No pre-processing
Resampling: Bootstrapped (25 reps) 

Summary of sample sizes: 11776, 11776, 11776, 11776, 11776, 11776, ... 

Resampling results across tuning parameters:

  mtry  Accuracy   Kappa      Accuracy SD  Kappa SD   
   2    0.9864051  0.9827977  0.002650521  0.003345575
  27    0.9875215  0.9842125  0.001741941  0.002189933
  52    0.9784516  0.9727337  0.004086763  0.005168504

Accuracy was used to select the optimal model using  the largest value.
The final value used for the model was mtry = 27. 
```

Now we will use our model on testing set and check for accuracy


```r
> pr<-predict(rf,testing)
> table(pr,testing$classe)
   
pr     A    B    C    D    E
  A 2219   10    0    0    0
  B    8 1499   10    0    0
  C    3    9 1348   22    0
  D    0    0   10 1263   10
  E    2    0    0    1 1432
```

Callculate the accuracy.


```r
sum(predict(rf,testing) == testing$classe)/NROW(testing$classe)
[1] 0.9891665
```
## Out of sample accuracy
Our expectation is that our out of sample model accuracy is <=98.9 %.





