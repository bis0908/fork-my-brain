---
Create at: 2025-01-04
tags:
  - design
---
---

![[법원 경매 수집.png]]

---
 
| as-is    | description                                                                |
| -------- | -------------------------------------------------------------------------- |
| 매각 기일    | sale_date                                                                  |
| 물건 비고    | item_remark                                                                |
| 목록 소재지   | list_location                                                              |
| 담당       | incharge                                                                   |
| 사건 접수    | case_receiot                                                               |
| 경매 개시일   | 경매 시작일                                                                     |
| 배당요구종기일  | 경매사건의 이해관계자 중 채권자가 낙찰대금에서 본인의 몫을 변제받기 위해 본인의 권리를 신고하고 배당을 요구하는 절차의 마감기일    |
| 청구 금액    | 경매신청 채권자가 채무를받을금액을 청구한날짜를기준으로 청구한 금액                                       |
| 기일       | 정해진 날짜                                                                     |
| 기일 종류    | 어떤 종류의 경매가 정해졌는지에 대한 여부                                                    |
| 기일 장소    | 법원 내부 법정                                                                   |
| 최저 매각 가격 | 그 경매사건의 매각기일에서 당해 부동산을 그 가격보다 저가로는 매각 할 수 없으며, 그 금액 이상으로 입찰할 것을 요하는 기준경매가격 |
| 기일 결과    | 이 날짜에 지정된 기일 종류에 따른 결과                                                     |
| 기간       | 어느 시점에서 다른 시점까지 계속하는 시간의 구분                                                |
| 매각 건수    | 매각된 건수                                                                     |
| 평균 감정가   | “감정가” 란 “토지 등” 의 경제적 가치를 판정하여 그 결과를 가액으로 표시하는 것                            |
| 평균 매각가   | 평균적으로 판매되는 가격                                                              |
| 매각 가율    | 경매에서 감정가에 대해 실제 낙찰된 금액의 비율                                                 |
| 평균 유찰 횟수 | 입찰 결과 낙찰(落札)이 결정되지 아니하고 무효로 돌아가는 일의 평균 횟수                                  |
| 매각 월     | 매각된 월                                                                      |
| 매각 대금    | 매각된 금액                                                                     |

---


| Korean Name (as-is) | Description (to-be)                                                                                                        |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 매각 기일               | sale_date                                                                                                                  |
| 물건 비고               | item_remark                                                                                                                |
| 목록 소재지              | list_location                                                                                                              |
| 담당                  | incharge                                                                                                                   |
| 사건 접수               | case_receiot                                                                                                               |
| 경매 개시일              | 경매 시작일 (auction_start_date)                                                                                                |
| 배당요구종기일             | (dividend_request_deadline) - Date by which a creditor in an auction case must request their share of the auction proceeds |
| 청구 금액               | claim_amount - Amount claimed by the auction applicant creditor based on the date of claim                                 |
| 기일                  | scheduled_date                                                                                                             |
| 기일 종류               | auction_type - Type of auction scheduled                                                                                   |
| 기일 장소               | court_location - Location within the court                                                                                 |
| 최저 매각 가격            | minimum_sale_price - Minimum price below which the property cannot be sold at the auction                                  |
| 기일 결과               | auction_result - Result according to the type of auction scheduled on this date                                            |
| 기간                  | duration - Period of time from one point to another                                                                        |
| 매각 건수               | number_of_sales - Number of sales                                                                                          |
| 평균 감정가              | average_appraisal_value - Economic value of the property                                                                   |
| 평균 매각가              | average_sale_price - Average price at which the property is sold                                                           |
| 매각 가율               | sale_ratio - Ratio of the actual auction price to the appraisal value                                                      |
| 평균 유찰 횟수            | average_fail_to_sell_count - Average number of times an auction fails to result in a sale                                  |
| 매각 월                | sale_month - Month in which the sale took place                                                                            |
| 매각 대금               | sale_amount - Amount for which the property was sold                                                                       |