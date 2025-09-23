# OTIF 향상·리드타임 분석/예측 기반 공급망 최적화
- DataCo Smart Supply Chain 데이터 활용

## 1) 개요

    - 목표:
        주문→출하 구간의 OTD/OTIF 현황을 수치화하고, 지연 요인(배송모드·지역/국가·품목)을 규명.
        간단 MRP 시뮬레이션과 **수요 예측(Prophet)**을 결합해 실행 가능한 개선안을 제시.

    - 핵심 수치(요약)
        표본: 180,519건
        OTD 42.7% / 지연 57.3%
        평균 실제 리드타임 3.50일, P95 6일

    - 핵심 해석: 
        계획 리드타임이 현실보다 과소하게 잡힌 구간이 광범위.
        배송모드×국가별 P85~P90 실적 리드타임을 약속 기준으로 재설정하면 OTD 즉시 개선 기대.

## 2) 데이터
- 출처: Kaggle — DataCo Smart Supply Chain for Big Data Analysis
- 사용 파일
        DataCoSupplyChainDataset.csv (주요 트랜잭션)
        DescriptionDataCoSupplyChain.csv (컬럼 설명)
        tokenized_access_logs.csv (선택)

- 표준화 컬럼(일부):
        order_date, shipping_date, lt_real_days, lt_sched_days, lt_gap_days, late_flag, otd_flag, shipping_mode, order_region, order_country, category_name, order_qty, unit_price

## 3) 분석 질문
      - 현재 OTD/OTIF 수준과 월·주·일 추이는 어떤가?
      - 실제 vs 계획 리드타임 분포 차이는 무엇이며, 괴리가 큰 구간은 어디인가?
      - 지연이 많은 조합(배송모드·지역/국가·카테고리)은 무엇인가?
      - 간단 규칙 기반 MRP 시뮬레이션으로 부족 시점(발주 신호)을 포착할 수 있는가?
      - 수요 예측(Prophet)을 운영 계획(MPS/MRP)에 어떻게 연결할 수 있는가?

## 4) 분석 절차
      - 진단: 결측·이상치 점검, OTD/OTIF, 평균·P95 리드타임, 분포/추이
      - 차원 분석: groupby/피벗으로 배송모드·지역/국가·카테고리 Top N 도출
      - 파레토: 국가별 지연건수 누적 비중(80% 룰)
      - MRP(간단): 주차 수요 합계 → 안전재고/조달LT 가정 → 재주문 신호
      - 수요 예측: Prophet(주/월 집계, 최근 미완성 구간 제외, 필요 시 로지스틱 cap/floor, 변곡점 민감도 조절)

## 5) 결과 요약

    6-1. 진단
      OTD 42.7% / Late 57.3%, 평균 실제 LT 3.50일, P95 6일
      계획 LT는 0~2·4일에 집중, 실제 LT는 2~6일 중심 → 현실-계획 괴리 뚜렷
          즉시 조치: 
          배송모드×국가별 P85~P90 실적 LT를 약속 기준으로 재설정(현실 기반 SLA).
          컷오프/휴일 캘린더 반영(D+2/D+3 등)으로 고객 기대관리.

    6-2. 지연이 많은 조합
      - 배송모드: First/Second Class 지연율 높음 → 약속일 과소 신호
      - 지역/국가: Central Africa, Western Europe, Ecuador 등 고지연
      - 카테고리: 카메라/스포츠 등 취급·포장 시간 영향 추정
      - 조치: 모드×국가×카테고리 조합 기준 약속 재설정 + 버퍼 표준화.

    6-3. 파레토(국가별)
      - 지연건수 Top N 국가를 집중 개선하면 전체 지연 대부분을 빠르게 절감 가능
      - (미국은 절대 모수 큼 → 지연율이 보통이어도 효과 큼)

    6-4. MRP(간단)
      - 주차 수요 누적 대비 리드타임+안전재고 고려 시 재주문 시점 포착 가능
      - 실제 적용 시 품목/BOM, 공급처별 조달LT, 우선순위 규칙으로 확장

    6-5. 수요 예측
      - 후반 급락치로 기본 예측이 하향 편향 가능
      - 최근 6~8주 제외, 월간 집계, 로지스틱(cap/floor) 적용 시 안정적
