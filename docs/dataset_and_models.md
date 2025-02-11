---
layout: default
title: Dataset and Models
nav_order: 3
---

# Dataset and Models
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Dataset

### About

|Training Dataset | Description                                             |
|-----------------|---------------------------------------------------------|
|ACT              | ACT is a cohort of community dwelling older adults age 65+. At time of accelerometer wear, the sample had a mean age of 76.7 years and was approximately 59% female and 90% non-Hispanic White.|
|AUSDIAB          | AUSDIAB is a cohort of older adults. At time of accelerometer wear, the sample had a mean age of 58.3 years and was approximately 56% female.| 
|PHASE            | PHASE is a cohort of children. At time of accelerometer wear, the sample had a mean age of x years and was approximately x female.| 



### Data format
- **Accelerometer Data**: We assume the input data is obtained from ActiGraph GT3X device and converted into single .csv files. The files should be named as **<subject_id>.csv** and files for all subjects should be put in the same directory. First few lines of a sample csv file are as follows:
    ~~~
    ------------ Data File Created By ActiGraph GT3X+ ActiLife v6.13.3 Firmware v3.2.1 date format M/d/yyyy at 30 Hz  Filter Normal -----------
    Serial Number: NEO1F18120387
    Start Time 00:00:00
    Start Date 5/7/2014
    Epoch Period (hh:mm:ss) 00:00:00
    Download Time 10:31:05
    Download Date 5/20/2014
    Current Memory Address: 0
    Current Battery Voltage: 4.07     Mode = 12
    --------------------------------------------------
    Accelerometer X,Accelerometer Y,Accelerometer Z
    -0.182,-0.182,0.962
    -0.182,-0.176,0.959
    -0.179,-0.182,0.959
    -0.179,-0.182,0.959
    ~~~

    **Note:** The pre-processing function expects, by default, an Actigraph RAW file at 30hz with normal filter. If you have Actigraph RAW data at a different Hz level, please specify in the pre_process_data function using the gt3x-frequency parameter.

- **(Optional) Valid Days File**: A .csv file indicating which dates are valid (concurrent wear days) for all subjects. Each row is subject id, date pair. The header should be of the from `ID,Date.Valid.Day`.  Date values should be formatted in `%m/%d/%Y` format. A sample valid days file is shown below.

    ~~~
    ID,Date.Valid.Day
    156976,1/19/2018
    156976,1/20/2018
    156976,1/21/2018
    156976,1/22/2018
    156976,1/23/2018
    156976,1/24/2018
    156976,1/25/2018
    ~~~

- **(Optional) Sleep Logs File**: A .csv file indicating sleep records for all subjects. Each row is tuple of subject id, date went to bed, time went to bed, data came out of bed, and time went out of bed. The header should be of the form `ID,Date.In.Bed,Time.In.Bed,Date.Out.Bed,Time.Out.Bed`. Date values should be formatted in `%m/%d/%Y` format and time values should be formatted in `%H:%M` format. A sample sleep logs file is shown below.


    ~~~
    ID,Date.In.Bed,Time.In.Bed,Date.Out.Bed,Time.Out.Bed
    33333,4/28/2016,22:00,4/29/2016,10:00
    33333,4/29/2016,22:00,4/30/2016,9:00
    33333,4/30/2016,21:00,5/1/2016,8:00
    ~~~

- **(Optional) Non-wear Times File**: A .csv file indicating non-wear bouts for all subjects. Non-wear bouts can be obtained using CHOI method or something else. Each row is a tuple of subject id, non-wear bout start date, start time, end date, and end time. The header should be of the form `ID,Date.Nw.Start,Time.Nw.Start,Date.Nw.End,Time.Nw.End`. Date values should be formatted in `%m/%d/%Y` format and time values should be formatted in `%H:%M` format. A sample sleep logs file is shown below.
  
    ~~~
    ID,Date.Nw.Start,Time.Nw.Start,Date.Nw.End,Time.Nw.End
    33333,4/27/2016,21:30,4/28/2016,6:15
    33333,4/28/2016,20:40,4/29/2016,5:58
    33333,4/29/2016,22:00,4/30/2016,6:00
    ~~~

- **(Optional) Events Data**: Optionally, you can also provide ActivPal events data-especially if you wish to train your own models-for each subject as a single .csv file. These files should also be named in the **<subject_id>.csv** format and files for all subjects should be put in the same directory. The first few lines of a sample csv file are as follows:
    ~~~
    "Time","DataCount (samples)","Interval (s)","ActivityCode (0=sedentary, 1= standing, 2=stepping)","CumulativeStepCount","Activity Score (MET.h)","Abs(sumDiff)"
    42633.4165162037,0,1205.6,0,0,.4186111,171
    42633.4304699074,12056,3.5,1,0,1.361111E-03,405
    42633.4305104167,12091,1.4,2,1,1.266667E-03,495
    42633.4305266204,12105,.9,2,2,1.072222E-03,340
    42633.430537037,12114,1,2,3,1.111111E-03,305
    42633.4305486111,12124,1,2,4,1.111111E-03,338
    42633.4305601852,12134,1,2,5,1.111111E-03,297
    ~~~

## Pre-trained Models

We currently support several [pre-trained models](https://github.com/ADALabUCSD/DeepPostures/tree/master/MSSE-2021/pre-trained-models) that can be used to generate predictions. They have been trained on different training datasets, which have different demographics. The recommended and default model is the `CHAP_ALL_ADULTS` model. However, users can change the pre-trained model to better match their needs using the `--model` option. Below we provide a summary of the available pre-trained models and the characteristics of the datasets that they were trained on.

| Model                                               | Training Dataset    |
|-----------------------------------------------------|---------------------|
|CHAP_ALL_ADULTS  (default and recommended)           | ACT + AUSDIAB       |
|CHAP_A                                               | ACT                 |
|CHAP_B                                               | ACT                 |
|CHAP_C                                               | ACT                 |
|CHAP (ensemble of A, B, and C)                       | ACT                 |
|CHAP_CHILDREN                                        | PHASE               |


