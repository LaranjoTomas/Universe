---
tags:
  - Project
  - SRC
---
<img src="src_2_img1.png" style="display: block; margin: auto;" />
direita é o com erros
ip 13.107.42.30 possui elevado valor de Upload. 

## O que pode significar??



# Information from the dataset (data2.parquet)

## Top Private Networks in Use (/24):

| **IP**           | **count** |
| ---------------- | --------- |
| 192.168.102.0/24 | 1225920   |
## Most Active Internal Services (Top 5 IP + Port):

| **dst_ip**      | **port** | **flows** |
| --------------- | -------- | --------- |
| 192.168.102.229 | 443      | 56701     |
| 192.168.102.234 | 53       | 56676     |
| 192.168.102.237 | 443      | 56126     |
| 192.168.102.230 | 53       | 55934     |
| 192.168.102.240 | 443      | 54501     |

## Internal Device Traffic Summary (Top 10 by Upload):

| **src_ip**      | **up_bytes** | **down_bytes** | **up/down_ratio** |
| --------------- | ------------ | -------------- | ----------------- |
| 192.168.102.102 | 114160123    | 1069426437     | 0.106749          |
| 192.168.102.16  | 112530525    | 1023368389     | 0.109961          |
| 192.168.102.199 | 103296621    | 934491338      | 0.110538          |
| 192.168.102.142 | 100830944    | 921286221      | 0.109446          |
| 192.168.102.144 | 96737151     | 905527873      | 0.106830          |
| 192.168.102.157 | 95283788     | 877557255      | 0.108578          |
| 192.168.102.115 | 94105301     | 878641302      | 0.107103          |
| 192.168.102.127 | 93213588     | 834596132      | 0.111687          |
| 192.168.102.152 | 91659073     | 849963477      | 0.107839          |
| 192.168.102.208 | 90271930     | 822009039      | 0.109819          |

##  Average Bytes Per Flow (Top 5 Download):
| **src_ip**      | **up_bytes** | **down_bytes** |
| --------------- | ------------ | -------------- |
| 192.168.102.136 | 10405.030948 | 99423.887814   |
| 192.168.102.120 | 10207.344986 | 99319.816056   |
| 192.168.102.40  | 10440.511478 | 98833.722681   |
| 192.168.102.113 | 10191.551807 | 98268.275754   |
| 192.168.102.189 | 10015.186620 | 97628.113556   |

## External Destinations contacted by internal devices:
$$ Ratio = Upload / Download $$

| **dst_ip**      | **up_bytes** | **down_bytes** | **up/down_ratio** |
| --------------- | ------------ | -------------- | ----------------- |
| 142.250.185.4   | 644461191    | 5988528264     | 0.107616          |
| 142.250.184.14  | 560427914    | 5119556071     | 0.109468          |
| 157.240.212.35  | 521051551    | 4812155109     | 0.108278          |
| 213.13.146.142  | 465365118    | 4296378903     | 0.108316          |
| 157.240.212.174 | 426048262    | 3903721939     | 0.109139          |
| 142.250.200.131 | 390221751    | 3601431012     | 0.108352          |
| 88.157.217.145  | 343620468    | 3191630288     | 0.107663          |
| 172.66.0.227    | 318346257    | 2938216367     | 0.108347          |
| 193.126.240.146 | 284577274    | 2581851944     | 0.110222          |
| 204.79.197.212  | 251629307    | 2335264497     | 0.107752          |

## Top Ports used by External Destinations:

| **port** | **count** |
| -------- | --------- |
| 443      | 6660044   |

##  Most Used Internal Ports:
| **port** | **count** |
| -------- | --------- |
| 443      | 167328    |
| 53       | 112610    |

## Average interval between flows from same source IP (for each source IP)

| **src_ip**      | **diff_timestamp** |
| --------------- | ------------------ |
| 192.168.102.136 | 2869.909091        |
| 192.168.102.140 | 2099.967860        |
| 192.168.102.207 | 1546.062249        |
| 192.168.102.36  | 1438.863636        |
| 192.168.102.198 | 1361.097998        |
| ------------    | ------------       |
| 192.168.102.209 | 406.873195         |
| 192.168.102.84  | 398.564129         |
| 192.168.102.64  | 397.879579         |
| 192.168.102.96  | 393.409704         |
| 192.168.102.194 | 240.555556         |


# Project Todo List
## Data Analysis

- [ ] Analyze data2.parquet to identify private IPv4 network(s)

- [ ] Identify internal server/services

- [ ] Describe and quantify traffic exchanges from internal users with internal and external servers

- [ ] Describe and quantify traffic exchanges from external users with corporation public servers

  

## SIEM Rules Definition

- [ ] Define rules for detecting internal BotNet activities

- [ ] Define rules for detecting data exfiltration using HTTPS and/or DNS

- [ ] Define rules for detecting C&C activities using DNS

- [ ] Define rules for detecting anomalous external destinations

- [ ] Define rules for detecting anomalous usage of corporate public services by external users

  

## Testing and Identification

- [ ] Test SIEM rules on test2.parquet

- [ ] Test SIEM rules on servers2.parquet

- [ ] Identify and list devices (IP addresses) with anomalous behaviors

  

## Report Preparation

- [ ] Compile analysis of non-anomalous behaviors

- [ ] Document SIEM rules with justifications

- [ ] Document test results and identified anomalous devices

- [ ] Format and finalize report