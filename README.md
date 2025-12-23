# BI-Data-Analysis
공공데이터 수집 활용 및 시각화

# BI 데이터 파이프라인 포트폴리오 프로젝트  
**공공 API → 자동 ETL → AWS 네이티브 분석 & 대시보드**

서울시 실시간 교통·날씨·미세먼지 데이터를 활용한  
**현대적 서버리스 BI 파이프라인** (2025년 기준 취업/이직용 강력 추천 구성)

## 프로젝트 개요

공공데이터 API를 매일 자동 수집 → 전처리 → 변환 → 저장 → 카탈로그화 → 분석 → 시각화하는  
**완전 자동화된 end-to-end BI 파이프라인**을 구축합니다.

### 핵심 기술 스택
- **오케스트레이션**      : Apache Airflow (로컬 Docker)
- **데이터 수집/기본 처리** : Python (requests, pandas)
- **ETL (본격 변환)**      : **AWS Glue Studio** (Visual ETL, Spark 기반)
- **저장소**               : Amazon S3 (raw → staging → processed, Parquet + 파티션)
- **메타데이터 관리**      : AWS Glue Crawler + Data Catalog
- **분석 엔진**            : Amazon Athena (Serverless SQL)
- **BI 대시보드**          : Amazon QuickSight (인터랙티브 대시보드)



### 전체 아키텍처 다이어그램

```mermaid
flowchart LR
    A[공공 API\n서울시 교통/날씨/미세먼지] -->|매일/매시간| B[Airflow DAG\n로컬 Docker]
    
    B --> C[PythonOperator\nAPI 호출 → Raw JSON → S3 raw]
    B --> D[PythonOperator\n기본 cleansing → Staging]
    
    D --> E[AWS Glue Studio\nVisual ETL Job\nSpark 변환 + 파티션]
    E --> F[S3 Processed Zone\nParquet + year/month/day 파티션]
    
    F --> G[Glue Crawler\n스키마 자동 등록]
    G --> H[AWS Glue Data Catalog]
    
    H --> I[Amazon Athena\nServerless SQL 쿼리]
    I --> J[Amazon QuickSight\nKPI · 지도 · 시계열 대시보드]
    
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style J fill:#bbf,stroke:#333,stroke-width:2px
```


### 상세 데이터 흐름 (단순)

```mermaid
flowchart LR
    API[공공 API] -->|requests| Airflow[Airflow DAG\n로컬]
    
    Airflow --> Raw[S3 Raw\nJSON]
    Airflow --> Staging[S3 Staging\n전처리]
    
    Staging --> GlueETL[AWS Glue Studio\nVisual ETL + Partition]
    GlueETL --> Processed[S3 Processed\nParquet]
    
    Processed --> Crawler[Glue Crawler]
    Crawler --> Catalog[Glue Data Catalog]
    
    Catalog --> Athena[Athena]
    Athena --> QuickSight[QuickSight\n대시보드]

    classDef aws fill:#232f3e,stroke:#ff9900,color:#fff
    classDef airflow fill:#0073e6,stroke:#0056b3,color:#fff
    classDef s3 fill:#569a31,stroke:#3c6e22,color:#fff
    
    class API,Raw,Staging,Processed,Catalog,Athena,QuickSight aws
    class Airflow airflow
    class GlueETL,Crawler aws
```
