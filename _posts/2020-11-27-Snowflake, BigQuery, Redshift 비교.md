---
layout: post
title:  "Snowflake, BigQuery, Redshift 비교"
comments: true
tags: Snowflake, BigQuery, Redshift
---
빅데이터와 분석은 오늘날 업계의 비즈니스를 발전시키는 근본적인 힘으로 작용되었습니다. 수년 동안 매초 생성되는 데이터의 양은 기하급수적으로 증가했으며, 이로 인해 데이터를 처리하는데 효율적인 클라우드 데이터웨어 하우스 기술이 등장했습니다.

데이터웨어 하우스는 데이터를 효율적으로 활용하여 심층적인 통찰력을 도출하는데 매우 중요합니다. 어떤 데이터웨어 하우스가 내 비즈니스에 적합한가?라는 고민이 생길 것 입니다.

데이타웨어 하우스 아키텍처는 빠르게 변화하고 있습니다. 기업은 기존 On-Premise대신 낮은 초기 비용, 향상된 확장성 및 성능을 갖춘 클라우드 기반 데이터웨어 하우스로 점차 이동하고 있습니다.

![image](https://user-images.githubusercontent.com/111643/116041048-b74da380-a6a7-11eb-9b78-ab3c9a4c1504.png)

# 클라우드기반 데이터웨어 하우스 선택시 고려할 점
클라우드상에서의 데이터웨어 하우스를 선택해야 할 경우 다음을 고려해야 합니다.

# 데이터웨어 하우스 지리적 위치
아래의 국가에서 해당 솔루션을 사용할 수 있습니다.

![image](https://user-images.githubusercontent.com/111643/116041083-c16fa200-a6a7-11eb-96bf-9f55b71a818f.png)

# 데이터 양
## 데이터 볼륨
* Postgres, MySQL, MSSQL 및 기타 여러 RDBMS는 최대 1TB 크기의 데이터를 지원하고 이 크기를 초과하면 성능이 저하될 가능성이 존재합니다.
* Redshift, BigQuery, Snowflake는 최적의 방식으로 최대 수 PB의 데이터 세트 크기를 지원합니다.

## 데이터 형식
![image](https://user-images.githubusercontent.com/111643/116041233-f11eaa00-a6a7-11eb-9d50-2f411a979573.png)

## 데이터 소스
기존 서비스가 클라우드를 사용하고 있지 않으면 VPN응 이용해 데이터를 전송해야 합니다. 따라서 데이터 파이프 라인 구축에 대한 비용도 고려해야 합니다. 각 서비스별 예제는 다음과 같습니다.

### 1. Redshift
![image](https://user-images.githubusercontent.com/111643/116041311-05fb3d80-a6a8-11eb-94df-5a4b79187e4d.png)

### 2. BigQuery
![image](https://user-images.githubusercontent.com/111643/116041351-13b0c300-a6a8-11eb-8f3b-21686a5baa1d.png)

### 3. Snowflake
![image](https://user-images.githubusercontent.com/111643/116041379-1b706780-a6a8-11eb-837f-97d6f5cd4868.png)

파이프라인은 환경에 따라 달라집니다.

# 보안
구매 결정에 영향을 미치는 주요 요소는 보안입니다. 데이터가 제3장에게 유출되지 않아야 합니다. 세가지 솔루션 모두 데이터를 보호하기 위한 보안 조치가 내장되어 있습니다.

![image](https://user-images.githubusercontent.com/111643/116041418-26c39300-a6a8-11eb-8fbc-4ef46dd1f131.png)

# 가격 모델
가성비를 가진 솔루션을 결정하는 것은 측정하기가 어려운 부분입니다. 사용 사례를 기반으로 설명하도록 하겠습니다. 아래는 각 솔루션별 가격 모델입니다.

![image](https://user-images.githubusercontent.com/111643/116041473-3347eb80-a6a8-11eb-8746-bb61d950f4ea.png)

Redshift는 리소스가 미리 결정되기 때문에 예측 가능성이 높습니다. Snowflake는 소요 시간에 따라 가격이 달라지기 때문에 쉽게 측정할 수 있지만, BigQuery는 필요한 쿼리 리소스가 다양하기에 예측하기가 어렵습니다.

가격 책정 모델을 기반으로 각 서비스에 가장 적합한 사례를 살펴보겠습니다.

## Redshift
일정한 계산이 필요한 시나리오에 적합합니다.

e.g.
* NASDAQ 일일 레포트: 데이터 보고를 위한 워크로드
* 자동 광고 입찰: 광고의 입찰은 실시간으로 예측 모델을 통해 조정
* 라이브 대시 보드: 새로 고침을 통한 지속적인 쿼리로 라이브 데이터 스트리밍

## BigQuery
워크로드가 급증하는 시나리오에 가장 적합합니다. (유휴 시간이 길고 가끔 많은양의 쿼리를 실행하는 경우)

e.g.
* 권장 모델: 전자상거래 애플리케이션을 위해 하루에 한번 실행
* 임시보고: 분기별 보고서에 복잡한 쿼리가 있는 경우
* 영업 인텔리전스: 영업 또는 마케팅 팀이 원하는 방식으로 데이터를 분석하여 임시로 수행할 경우
* 기계 학습: 데이터, 소비자 행동에서 새로운 패턴을 발견하는 경우

## Snowflake
안정적이고 지속적인 사용 패턴에 가장 적합합니다. 하지만 지속적으로 Up/Down 스케일링이 필요합니다.

e.g.
* 비즈니스 인텔리전스 회사: 데이터의 패턴을 찾기 위해 동시에 데이터를 쿼리하는 사용자가 많은 경우 (100명 ~ 1000명)
* 데이터를 서비스로 제공: 분석 사용자 인터페이스 또는 데이터 API의 형태로 분석 목적의 데이터를 수천개의 클라이언트에 제공하는 경우

# 클라우드 기반 데이터웨어 하우스 소개 및 비교
현재 시점에 고려해야 할 데이터웨어 하우스는 Amazon Redshift, Google BigQuery, Snowflake입니다.

본 글에서는 언급한 3가지 솔루션에 대해서 비교하려고 합니다.

## Amazon Redshift란?
Redshift는 비즈니스 인텔리전스(BI) 도구와 원할하게 통합 될 수 있는 Managed형 클라우드 데이터웨어 하우스 서비스라고 할 수 있습니다. 웨어하우스에 ETL(Extract, Transform, Load)만 있으면 현명한 비즈니스 의사결정을 내리기가 수월해집니다.

AWS를 사용하면 작은 사이즈의 데이터로 시작하고 요구에 따라 원할하게 확장/축소 할 수 있습니다. 이를 통해 기업은 데이터를 활용하여 자사 또는 고객에 대한 비즈니스 통찰력을 얻을 수 있습니다.

클라우드 데이터웨어 하우스를 사용하려면 Redshift 클러스터로 알려진 노드 그룹을 사용해야 합니다. 클러스터를 프로비저닝 한 후에 데이터 분석 쿼리를 실행하기 위해 데이터 세트를 업로드 해야 합니다.

데이터 세트의 크기에 관계없이 동일한 SQL 기반 도구 및 BI 솔루션을 사용하여 빠른 쿼리 성능을 이용할 수 있습니다.

## Google BigQuery란?
Google BigQuery는 Google의 자체 데이터웨어 하우징 솔루션입니다. 2010년에 처음 출시되었고 C-Store 및 MonetDB에 이어 일반적으로 사용할 수 있는 최초의 데이터웨어 하우스 솔루션 중 하나였습니다.

BigQuery는 Google Cloud Platform으로 알려진 Google의 전체 클라우드 컴퓨팅 생태계에서 중요한 부분입니다.

Dremel은 BigQuery에서 쿼리를 실행하는데 사용되는 Google에서 개발한 강력한 쿼리 엔진입니다. Google에 의하면 Dremel은 “매우 큰 데이터 세트에서 SQL과 같은 유사한 쿼리를 실행하고 단 몇 초만에 정확한 결과를 얻을 수 있는 쿼리서비스”입니다.

BugQuery 및 Dremel은 리소스를 할당하고 Dremel 작업에 데이터를 제공하는데 도움이 되는 Borg 및 Colossus와 같은 다른 Google 클라우드 기술을 활용할 수 있습니다.

## SnowFlake란?
Redshift와 Snowflake는 관계형 데이터베이스 관리 시스템입니다. SaaS(Software-as-a-Service) 모델로 제공되는 구조화된 데이터와 반 구조화된 데이터 모두를 지원하는 데이터웨어 하우스입니다.

이는 기존 데이터베이스 또는 빅 데이터 소프트웨어 플랫폼(e.g. Hadoop)위에 구축되지 않는 것을 의미합니다. 대신 Snowflake는 클라우드 용으로 설계된 고유한 아키텍처가 존재하는 SQL 데이터베이스 엔진을 사용합니다.

Snowflake는 빠르고 사용자 친화적이며 기존 데이터웨어 하우스보다 더 많은 유연성을 제공합니다.

Redshift ETL과 Snowflake ETL을 모두 사용하는 경우 두 솔루션간에 많은 유사점이 있음을 알고 있을 것입니다.

Snowflake는 Snowflake Elastic Data Warehouse의 형태로 클라우드 기반 데이터 스토리지 및 분석을 제공합니다. 사용자는 클라우드 기반 하드웨어 및 소프트웨어를 사용하여 데이터를 분석하고 저장 할 수 있습니다.

물리 데이터는 Amazon S3에 저장됩니다. Snowflake ETL을 사용하는 경우 Hadoop과 같은 기술을 사용하지 않고도 퍼블릭 클라우드 시스템을 활용할 수 있습니다.

이러한 클라우드웨어 하우스 시스템은 모두 강력하며 데이터 관리와 관련하여 몇 가지 고유한 기능을 제공합니다. 그러나 각 솔루션별 차이점은 존재합니다.

회사에 적합한 솔루션을 선택하려면 통합, 데이터베이스 기능, 유지 관리, 보안등의 비용도 비교해야 합니다.

## 기능 비교
### Computing Layer
* BigQuery: 분산 컴퓨팅에서 실행됩니다. Google이 제공하는 각 데이터센터의 Borg에서 실행됩니다.
* Redshift: AWS 가상 머신에서 실행되는 ParAccel의 독점 포크 (부분적으로 Postgres에서 포크됨), Postgres라고 하나 많이 다름
* SnowFlake: 클라우드(AWS, GCP, Azure)의 가상머신에서 실행되는 Intelligent Predicate Pushdown + Smart Caching이 포함된 독점 컴퓨팅 엔진이고 C-Store, MonetDB에서 영감을 얻은 하이브리드 컬럼 시스템

### Storage Layer
Redshift, BigQuery, Snowflake 모두 Hot/Warm/Cold 스토리지를 구현합니다.
* Big Query: 독점적이며 ColumnIO를 스토리지 형식으로 사용하고 Colossus 파일 시스템에 저장됩니다. 분산 컴퓨팅과 스토리지를 완전히 분리합니다.
* Redshift: 독점적이지만 일반적으로 SSD(dc1, dc2) / HDD (ds1, ds2) 또는 독점 컬럼 형식을 사용하는 S3기반 (RA3)을 포함하여 혼합됩니다 RA3는 컴퓨팅과 스토리지를 분리하는 반면에 다른 모든 노드 유형은 컴퓨팅과 스토리지를 함께 지역화합니다.
* Snowflake: 원하는 클라우드의 컴퓨팅/객체 스토리지에서 실행되는 메모리/SSD/객체 저장소의 독점적인 Row 형식으로 제공하며 메타 데이터 캐싱을 사용하여 PAX(하이브리드 컬럼 형식)로 저장됩니다.

### Compression
* BigQuery: ColumnIO 열 형식으로 처리되는 독점 압축입니다. BigQuery는 지속적으로 내부에서 데이터를 압축하지만 압축되지 않은 바이트를 스캔하는 것처럼 쿼리 요금이 청구됩니다.
* Redshift: Redshift는 LZO, ZStandard와 같은 개방형 알고리즘을 구현하여 투명한 압축을 달성합니다. 최근에 자체 독점 압축 알고리즘(AZ64)을 출시했지만 데이터 유형 선택이 제한적입니다. 열을 압축할 방법을 선택할 수 있습니다. Analyze Compression은 쿼리 패턴과 열에 저장하려는 데이터에 따라 압축 방식을 선택하는 것이 가장 좋습니다.
* SnowFlake: 자체 압축 레이어를 제공합니다. BigQuery와 달리 스캔한 바이트에 대해서는 요금이 청구되지 않지만 쿼리 플래너가 압축 및 테이블 통계를 활용하여 더 적은 데이터를 스캔하기에 컴퓨팅 비용을 줄일 수 있습니다.

### Deployments
* BigQuery, Redshift, Snowflake 모두 클라우드위에서 동작됩니다.
* BigQuery: 클라우드 전용이고 Google Cloud Platform내에서만 사용이 가능합니다.
* Redshift: 클라우드 전용이고 Amazon Web Services내에서만 사용이 가능합니다.
* Snowflake: 클라우드 전용이고 선호하는 클라우드 유형(AWS, GCP, Azure)에 배포가 가능합니다.

### 데이터 접근
데이터에 쉽게 접근할 수 없다면 데이터웨어 하우스는 많이 사용될 수 없습니다. 일반적으로 데이터베이스는 ODBC/JDBC에 의존해왔지만 시간이 지남에 따라 새로운 형태의 데이터 검색 및 핸들링이 필요합니다.
* BigQuery
** Simba 드라이버를 통한 ODBC/JDBC 접근
** BigQuery 사용자 인터페이스
** BigQuery 연결 API
** BigQuery Jobs API
** BigQuery Storage API
** BigQuery CLI
* Redshift
** AWS 제공 드라이버를 통한 ODBC/JDBC 접근
** Redshift 사용자 인터페이스
** 데이터 접근 API
** AWS CLI
** Postgres CLI
* Snowflake
** 드라이버를 통한 ODBC/JDBC 접근
** Spark 플러그인 (spark-snowflake)을 통한 접근
** Kafka를 통한 접근
** 특정 언어용 Python/Node.js/Go/.Net 드라이버
** SnowSQL
** Snowsight

### 암호화
세가지 솔루션은 저장 및 전송 중 일부 암호화 방법을 제공합니다.
* BigQuery: Google에서 제공하는 암호화 키 관리 서비스(KMS) 또는 CMEK를 사용하여 암호화
* Redshift: AWS 키관리 서비스를 사용하여 암호화
* Snowflake: 종단간 암호화, VPS(Virtual Private Snowflake)는 전용 서버의 메모리에 암호화 키를 저장하는 기능 제공

### 타사 도구 지원 (시각화, 데이터 모델링)
대부분의 인기 있는 도구는 모든 데이터베이스를 지원합니다.

AWS와 GCP는 모두 고유한 시각화/데이터 모델링 소프트웨어(각각 QuickSight, Looker/Data Studio)를 제공합니다.

모든 기능 앞에 “Snow”를 배치하는 전통에 따라 Snowflake는 몇 가지 기본 데이터 시각화를 수행할 수 있는 Snowsight를 제공합니다.

### Query Language
쿼리 언어 없이는 훌륭한 데이터베이스를 제공할 수 없습니다.
* BigQuery: BigQuery는 Legacy(BigQuery라고 함) SQL과 표준 SQL이라는 두 가지 주요 언어를 제공합니다.
* Redshift: ANSI와도 호환되며 Postgres에 대한 경험이 있다면 익숙한 언어를 제공합니다.
* Snowflake: ANSI와 호환되는 Snowflake 쿼리 언어는 작업하기가 매우 수월합니다. 언어는 처음부터 명확하게 설계되었으며 중첩된 데이터로 작업하는 경우 대부분 쉽게 작업이 가능합니다.

BigQuery와 Redshift는 모두 지리 공간 데이터 쿼리를 지원합니다. Snowflake는 현재 지리 공간 데이터에 대한 미리보기 기능을 제공합니다. 모든 구현은 네이티브 유형을 사용하거나 텍스트 표현(e.g. GeoJSON)간에 변환할 수 있습니다.

### 통합 쿼리
통합 쿼리는 기존 분석(OLAP) 스타일 데이터베이스와 일반적인 행기반 트랜잭션 스타일의 데이터베이스(Postgres, MySQL)사이의 격차를 해소하기 위해 설계된 기능입니다.
* BigQuery: Postgres 및 MySQL 데이터베이스 지원을 포함하는 CloudSQL을 통해 통합 쿼리를 지원합니다. 다른 소스로는 Google 스프레트시트 및 Google Cloud Storage가 있습니다.
* Redshift: Amazon RDS(Postgres, MySQL 및 Aurora)에 대한 통합 쿼리 지원을 합니다.
* Snowflake: 현재 통합 쿼리를 지원하지 않습니다.

### 사용자 정의 함수(UDF)
UDF는 데이터베이스에서 수행 할 수 있는 작업에 추가적인 유연성을 도입 할 수 있는 방법입니다. 데이터베이스에 로직을 배포할 수 있다는 점은 과거로 회귀할 수 있지만, 특정 상황에서는 필요할 수 있습니다.
* BigQuery: SQL 및 Java Script로 사용자 정의 함수 작성을 지원합니다. Java Script 함수의 경우 복잡한 UDF를 작성시 Google Cloud Storage의 외부 라이브러리를 포함할 수 있습니다.
* Redshift: SQL 또는 Pythob으로 UDF를 작성할 수 있습니다. BigQuery와 유사하게 외부 라이브러리 및 함수를 쉽게 패키징하고 S3에서 호스팅 할 수 있습니다. Python에 대한 지원은 없습니다. 반면 Lambda UDF를 도입했습니다.
* Snowflake: SQL 및 Java Script 함수를 지원합니다. 하지만 외부 라이브러리를 사용할 수 없습니다.

### 캐싱
* BigQuery: 쿼리를 캐시하고 인메모리 캐시를 제공하는 중간 캐시를 제공합니다. 캐시된 쿼리는 비용이 발생하지 않습니다. 캐싱 엔진은 데이터 스튜디오뿐만 아니라 BigQuery에서도 사용 될 수 있기에 쿼리 소스에 관계없이 스마트한 중간 캐싱이 가능합니다.
* Redshift: 쿼리 및 결과를 캐싱합니다. (노드 유형 및 메모리/디스크의 사용 가능한 스토리지에 따라 다름)
* Snowflake: 콜드 데이터 스토리지와 분리된 중간 스토리지에 Hot/Warm 쿼리 캐시를 제공합니다.

### 스트리밍
데이터를 데이터베이스에 빠르고 안정적으로 저장하는 것은 매우 어려운 문제입니다.
* BigQuery: 기본 스트리밍, 하나의 테이블처럼 보이도록 주어진 테이블에서 분할 된 데이터로 스트리밍 버퍼를 조정하는 몇 가지 방법이 존재합니다. 그러나 이것은 대부분 추상화됩니다.
* Redshift: Redshit로 마이크로 배치를 할 수 있지만, 기본 스트리밍 기능은 없습니다. 마이크로 배치를 시작하는 가장 쉬운 방법은 Kinesis Firehose를 통해 Redshift를 직접 연결하는 것입니다.
* Snowflake: Snowpipe를 통해 Amazon S3, Google Cloud Storage, Azure Blob Storage에서 마이크로 배치를 할 수 있지만, 기본 스트리밍 기능은 없습니다.

### 데이터 소스
일부 제품에서는 데이터베이스 자체에 데이터를 저장하지 않고도 데이터를 쿼리 할 수 있습니다. 이는 유용 할 수 있지만 데이터 현지화 및 최적화 된 데이터 구조를 활용할 수 없을만큼 성능이 현저히 떨어지는 경향이 있습니다.
* BigQuery: 통합 쿼리를 통해 Cloud Storage, Google Drive, Bigtable, Cloud SQL등을 비롯한 일부 외부 데이터 소스에 대한 연결을 설정 할 수 있습니다.
* Redshift: S3와 Redshift Cluster 사이의 중간 컴퓨팅 계층 역할을 하는 Redshift Spectrum을 통해 S3에 있는 데이터에 연결할 수 있습니다. 통합 쿼리 설정이 있는 경우에는 RDS(Postgres, MySQL 또는 Aurora)를 쿼리 할 수 있습니다.
* Snowflake: BigQuery 및 Redshift와 마찬가지로 최상의 성능을 위해서 Snowflake내에 있는 것이 이상적입니다. 그러나 Snowflake는 Snowflake로 데이터를 가져 오지 않고도 주요 클라우드 (Amazon S3, Google Cloud Storage, Azure Blog Storage)의 객체 스토리지에 연결할 수 있는 외부 테이블 기능을 제공합니다.

끝으로,

클라우드 기반의 데이터웨어 하우스를 선택하려는 분들께 도움이 되길 희망합니다.

References:
* www.xplenty.com/blog/redshift-vs-snowflake/
* www.xplenty.com/blog/snowflake-vs-bigquery/#bigquery
* fivetran.com/blog/warehouse-benchmark
