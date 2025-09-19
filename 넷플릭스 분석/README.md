🎬 Netflix 콘텐츠 분석 & 추천 시스템
- 기간: 2024.06
- 기술 스택: Python (Pandas, Numpy, Scikit-learn, Matplotlib, Seaborn), WordCloud
- 데이터: Kaggle 공개 데이터셋 netflix_titles.csv
    - 주요 컬럼: title, type, country, date_added, release_year, rating, listed_in, description 등

1️⃣ 프로젝트 개요

- Netflix 콘텐츠 카탈로그 데이터를 분석하여 작품 유형·국가·연도·장르별 특징을 탐색
- 텍스트 기반 간단한 추천 시스템(TF-IDF + 코사인 유사도) 구현
- 시각화와 추천 함수를 통해 Netflix 콘텐츠 소비 구조를 이해


2️⃣ 데이터 및 분석 과정

    1. 데이터 전처리
        - date_added → datetime 변환 및 year_added, month_added 파생
        - 결측치 처리 (country, director, cast 등)
        - rating을 연령대 그룹(Kids/Teens/Adults 등)으로 재분류

    2. 탐색적 분석 & 시각화
        - Movie vs TV Show 비율
        - 연도·월별 콘텐츠 증가 추이
        - 국가별 콘텐츠 생산 추이 및 연령 타깃
        - 장르/세부 장르 트렌드 변화
        - 워드클라우드로 설명(description) 핵심 키워드 도출

    3. 추천 시스템 구현
        - listed_in(장르) ×2 가중 + description 텍스트 결합 후 TF-IDF 벡터화
        - 코사인 유사도를 기반으로 유사 작품 추천


3️⃣ 주요 성과 및 인사이트

    1. 콘텐츠 공급 구조 변화
        - 연도별 공급 추세
        - 2015~2020년 사이 Movie보다 TV Show가 급격히 증가 → 오리지널 시리즈 투자 확대와 글로벌 시장 공략 의도와 연결 가능.
        - 코로나 시기(2020 전후) 공급 패턴 변화
        - 특정 장르(Thriller, Documentary)가 급증했는지 확인하면, 팬데믹 영향과 콘텐츠 소비 행태 변화 인사이트로 연결 가능.

    2. 국가별 차별화

        - 미국·인도·한국 등 주요 국가 비교
        - 미국: 전체 카탈로그의 큰 비중 차지 → 글로벌 중심
        - 인도: Movie 비중이 높음, Bollywood 영향 반영
        - 한국: TV Show 성장 → K-Drama의 글로벌 확산 흐름과 연결
                    → “한국 콘텐츠는 드라마 중심으로 글로벌 시장에 성공적으로 안착”이라는 서술 가능.

    3. 연령 타깃 전략

        - 국가별 청소년/성인 타깃 비중 비교
        - 미국·유럽 → 가족·키즈용 비중 일정
        - 아시아 → 15세 이상/성인 타깃 드라마·스릴러 강세
                    → “지역별 연령 타깃 전략 차이가 존재”라는 해석 가능.

    4. 장르 트렌드

        - 연도별 장르 변화
        - 드라마·코미디는 안정적 상위권 유지
        - Documentary·Stand-up Comedy는 최근 증가 추세
                    → “Netflix는 특정 시기 사회적 이슈·대중 취향 변화에 맞춰 장르를 강화하는 전략을 취함”이라고 정리 가능.

    5. 추천 시스템과 연결

        - TF-IDF 기반 추천에서 ‘Squid Game’과 유사 추천 시,
        - 비슷한 장르(Thriller, Survival, Korean Drama)가 클러스터링됨 → 모델이 콘텐츠 속성을 잘 반영했다는 근거 제공.
        - 단순 모델이지만, “텍스트 기반 추천만으로도 사용자 경험을 개선할 여지가 있다”는 실무적 메시지 전달.


4️⃣ 활용 기술

        - Python: Pandas, Numpy, Scikit-learn (TF-IDF, cosine similarity)
        - 시각화: Matplotlib, Seaborn, WordCloud
        - 기타: 텍스트 처리, 추천 함수 구현
