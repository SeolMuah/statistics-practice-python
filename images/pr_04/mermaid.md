flowchart TD
    Start([통계 분석 기법 선택])
    Start --> DataType{데이터 유형?}
    
    %% 메인 분기 - 4개 카테고리
    DataType -->|Y: 연속형/범주형/비율<br/>X: 없음| SingleSample[📋 단일 표본 검정<br/>표본 통계량 vs 기준값]
    DataType -->|Y: 연속형<br/>X: 범주형| Comparison[📊 집단 비교<br/>집단 간 위치 차이]
    DataType -->|Y: 범주형<br/>X: 범주형| Categorical[🔢 범주형 분석<br/>빈도·연관성]
    DataType -->|Y: 연속형<br/>X: 연속형| Relationship[📈 관계 분석<br/>상관·예측]
    
    %% ========================================
    %% 단일 표본 검정 섹션
    %% ========================================
    SingleSample --> SingleYType{Y의 측정 수준?}
    
    SingleYType -->|연속형| SingleNorm{정규성?}
    SingleYType -->|비율/이항형| SinglePropSize{정규근사 가능?<br/>np≥5, n·1-p≥5}
    SingleYType -->|범주형| GOF_ExpFreq{기대빈도 조건?}
    
    GOF_ExpFreq -->|모든 셀 ≥ 5| ChiSqGOF[["Chi-square 적합도 검정<br/>관측빈도 vs 기대빈도<br/>예: 요일별 방문자가<br/>균등한지"]]
    GOF_ExpFreq -->|일부 셀 < 5| MonteCarlo_GOF[["Monte Carlo 시뮬레이션<br/>관측빈도 vs 기대빈도<br/>소표본 시뮬레이션 검정<br/>예: 소규모 응급환자의<br/>요일별 균등 여부"]]
    
    SingleNorm -->|YES| OneSampleT[["One-sample t-test<br/>표본평균 vs 기준값<br/>예: 제품 평균 중량이<br/>500g인지"]]
    SingleNorm -->|NO| OneSampleWilcox[["Wilcoxon Signed-Rank<br/>단일표본<br/>중앙값 vs 기준값<br/>예: 만족도 중앙값이<br/>3점인지"]]
    
    SinglePropSize -->|YES| OnePropZ[["One-sample proportion z-test<br/>표본비율 vs 기준비율<br/>예: 불량률이<br/>5% 이하인지"]]
    SinglePropSize -->|NO| BinomialTest[["Binomial test<br/>표본비율 vs 기준비율<br/>예: 소표본에서<br/>합격률이 90%인지"]]
    
    %% ========================================
    %% 집단 비교 섹션
    %% ========================================
    Comparison --> CompGroups{집단 수?}
    
    CompGroups -->|2개| TwoType{독립/대응?}
    CompGroups -->|3개 이상| MultiType{독립/대응?}
    
    TwoType -->|독립| TwoIndepNorm{정규성?}
    TwoIndepNorm -->|YES| TwoIndepVar{등분산성?}
    TwoIndepVar -->|YES| IndepT[["Student's t-test<br/>두 집단 평균 차이<br/>예: A/B 그룹<br/>평균 구매액 차이"]]
    TwoIndepVar -->|NO| WelchT[["Welch's t-test<br/>두 집단 평균 차이<br/>이분산 보정<br/>예: 남녀 평균 연봉 차이"]]
    TwoIndepNorm -->|NO| MannW[["Mann-Whitney U<br/>두 집단 분포 위치 차이<br/>예: 두 지점 고객<br/>만족도 순위 비교"]]
    
    TwoType -->|대응| TwoPairedNorm{차이값 정규성?}
    TwoPairedNorm -->|YES| PairedT[["Paired t-test<br/>처리 전후 평균 차이<br/>예: 교육 전후<br/>시험 점수 변화"]]
    TwoPairedNorm -->|NO| Wilcox[["Wilcoxon Signed-Rank<br/>대응표본<br/>처리 전후 분포 위치 차이<br/>예: 약물 투여 전후<br/>통증 점수 변화"]]
    
    MultiType -->|독립| MultiIndepNorm{정규성?}
    MultiIndepNorm -->|YES| MultiIndepVar{등분산성?}
    MultiIndepVar -->|YES| OneWayANOVA[["One-way ANOVA<br/>3개+ 집단 평균 차이<br/>예: 3개 매장별<br/>일 매출 차이"]]
    MultiIndepVar -->|NO| WelchANOVA[["Welch's ANOVA<br/>3개+ 집단 평균 차이<br/>이분산 보정<br/>예: 학력별 연봉 차이"]]
    MultiIndepNorm -->|NO| KruskalW[["Kruskal-Wallis<br/>3개+ 집단 분포 위치 차이<br/>예: 3개 브랜드<br/>만족도 순위 비교"]]
    
    MultiType -->|대응| MultiPairedNorm{정규성?}
    MultiPairedNorm -->|YES| Sphericity{구형성?}
    Sphericity -->|YES| RMANOVA[["RM ANOVA<br/>반복측정 평균 차이<br/>예: 1·3·6개월 후<br/>체중 변화 추적"]]
    Sphericity -->|NO| RMANOVA_GG[["RM ANOVA + G-G 보정<br/>반복측정 평균 차이<br/>예: 4주간 매주 측정한<br/>혈압 변화"]]
    MultiPairedNorm -->|NO| Friedman[["Friedman test<br/>반복측정 분포 위치 차이<br/>예: 동일 환자의 3가지<br/>진통제 효과 순위 비교"]]
    
    %% ========================================
    %% 범주형 분석 섹션
    %% ========================================
    Categorical --> CatType{독립/대응?}
    
    CatType -->|독립| CatTableSize{교차표 크기?}
    
    CatTableSize -->|2×2| CatIndep2x2Freq{기대빈도 조건?}
    CatIndep2x2Freq -->|모든 셀 ≥ 5| ChiSq2x2[["Chi-square 독립성 검정<br/>두 변수 간 연관성<br/>예: 성별과<br/>구매 여부 연관성"]]
    CatIndep2x2Freq -->|일부 셀 < 5| Fisher[["Fisher's Exact test<br/>두 변수 간 연관성<br/>소표본 정확 검정<br/>예: 희귀질환<br/>치료 반응 여부"]]
    
    CatTableSize -->|R×C| CatIndepRCFreq{기대빈도 조건?}
    CatIndepRCFreq -->|모든 셀 ≥ 5| ChiSqRC[["Chi-square 독립성 검정<br/>두 변수 간 연관성<br/>예: 연령대별<br/>선호 브랜드 연관성"]]
    CatIndepRCFreq -->|일부 셀 < 5| RCSmallExpected{기대빈도 < 5인<br/>셀 비율?}
    RCSmallExpected -->|≤ 20%<br/>유사 범주 병합 가능| CellMerge[["셀 병합 후 Chi-square<br/>두 변수 간 연관성<br/>예: 10대+20대를<br/>청년층으로 병합"]]
    RCSmallExpected -->|> 20%<br/>병합 시 의미 손실| FisherRC[["Fisher-Freeman-Halton test<br/>두 변수 간 연관성<br/>소표본 정확 검정<br/>예: 소규모 병원 3개<br/>치료법별 부작용 유형 비교"]]
    
    CatType -->|대응| CatPairedType{측정 조건?}
    CatPairedType -->|2시점 이분형| McNemar[["McNemar test<br/>처리 전후 비율 변화<br/>예: 캠페인 전후<br/>인지도 변화"]]
    CatPairedType -->|2시점 다범주| McNemarBowker[["McNemar-Bowker test<br/>처리 전후 분포 변화<br/>예: 교육 전후<br/>선호도 분포 변화"]]
    CatPairedType -->|3시점+ 이분형| Cochran[["Cochran's Q test<br/>반복측정 비율 변화<br/>예: 동일 고객의<br/>1·3·6개월 후<br/>구독 유지 여부 변화"]]
    
    %% ========================================
    %% 관계 분석 섹션
    %% ========================================
    Relationship --> RelType{분석 목적?}
    
    RelType -->|상관관계<br/>방향·강도 파악| CorrNorm{정규성 & 선형성?}
    RelType -->|예측/설명<br/>Y 예측| RegType{관계 유형?}
    
    CorrNorm -->|YES| Pearson[["Pearson r<br/>선형 상관의 방향과 강도<br/>예: 광고비와 매출 간<br/>선형 상관"]]
    CorrNorm -->|NO| CorrNonParam{데이터 특성?}
    CorrNonParam -->|순서형/비선형 단조| Spearman[["Spearman ρ<br/>단조 관계의 방향과 강도<br/>예: 학력 순위와<br/>소득 순위 상관"]]
    CorrNonParam -->|동점 많음/소표본| Kendall[["Kendall τ<br/>순위 상관 (동점에 강건)<br/>예: 두 심사위원의<br/>순위 평가 간 상관"]]
    
    RegType -->|선형| RegMulticollin{다중공선성<br/>VIF < 10?}
    RegMulticollin -->|YES| LinearReg[["Linear/Multiple Regression<br/>X로 Y 예측·설명<br/>예: 면적으로<br/>부동산 가격 예측"]]
    RegMulticollin -->|NO| RegMulticollinAction{{변수 제거 또는<br/>Ridge/Lasso}}
    RegType -->|비선형| PolyReg[["Polynomial Regression<br/>비선형 관계로 Y 예측<br/>예: 온도와 수율<br/>비선형 관계 예측"]]
    
    LinearReg -.-> ResidCheck1{{잔차 진단:<br/>정규성, 등분산성, 독립성}}
    PolyReg -.-> ResidCheck2{{잔차 진단:<br/>정규성, 등분산성, 독립성}}
    
    %% ========================================
    %% 사후검정 / 후속분석
    %% ========================================
    OneWayANOVA -.->|유의| PostHoc1{{Tukey HSD<br/>어떤 집단 쌍이 다른가}}
    WelchANOVA -.->|유의| PostHoc2{{Games-Howell<br/>어떤 집단 쌍이 다른가}}
    KruskalW -.->|유의| PostHoc3{{Dunn test<br/>어떤 집단 쌍이 다른가}}
    RMANOVA -.->|유의| PostHoc4{{Bonferroni 쌍별 비교<br/>어떤 시점 쌍이 다른가}}
    RMANOVA_GG -.->|유의| PostHoc5{{Bonferroni 쌍별 비교<br/>어떤 시점 쌍이 다른가}}
    Friedman -.->|유의| PostHoc6{{Wilcoxon + Bonferroni<br/>어떤 시점 쌍이 다른가}}
    ChiSqGOF -.->|유의| ResidualAnalGOF{{잔차 분석<br/>어떤 범주가 기대와 다른가}}
    MonteCarlo_GOF -.->|유의| ResidualAnalMC_GOF{{잔차 분석<br/>어떤 범주가 기대와 다른가}}
    ChiSq2x2 -.->|필요 시| OddsRatio2x2{{오즈비<br/>두 집단 간 사건 발생<br/>가능성의 비}}
    Fisher -.->|필요 시| OddsRatioFisher{{오즈비<br/>두 집단 간 사건 발생<br/>가능성의 비}}
    ChiSqRC -.->|유의| ResidualAnalRC{{잔차 분석<br/>어떤 셀이 기대와 다른가}}
    CellMerge -.->|유의| ResidualAnalMerge{{잔차 분석<br/>어떤 셀이 기대와 다른가}}
    FisherRC -.->|유의| ResidualAnalFRC{{잔차 분석<br/>어떤 셀이 기대와 다른가}}
    McNemarBowker -.->|유의| PostHoc7{{쌍별 McNemar<br/>어떤 범주 쌍이 변화했는가}}
    Cochran -.->|유의| PostHoc8{{쌍별 McNemar + Bonferroni<br/>어떤 시점 쌍이 다른가}}
    
    %% ========================================
    %% 스타일
    %% ========================================
    classDef startStyle fill:#e74c3c,stroke:#c0392b,stroke-width:3px,color:#fff,font-weight:bold
    classDef categoryStyle fill:#3498db,stroke:#2980b9,stroke-width:2px,color:#fff,font-weight:bold
    classDef decisionStyle fill:#f39c12,stroke:#e67e22,stroke-width:2px,color:#000,font-weight:bold
    classDef paramTest fill:#27ae60,stroke:#1e8449,stroke-width:2px,color:#fff,font-weight:bold
    classDef nonParamTest fill:#00acc1,stroke:#00838f,stroke-width:2px,color:#fff,font-weight:bold
    classDef postHocStyle fill:#9b59b6,stroke:#7d3c98,stroke-width:1px,color:#fff,font-style:italic
    classDef warningStyle fill:#e67e22,stroke:#d35400,stroke-width:2px,color:#fff,font-weight:bold
    
    class Start startStyle
    class SingleSample,Comparison,Relationship,Categorical categoryStyle
    class DataType,CompGroups,TwoType,MultiType,RelType,CatType decisionStyle
    class TwoIndepNorm,TwoIndepVar,TwoPairedNorm decisionStyle
    class MultiIndepNorm,MultiIndepVar,MultiPairedNorm,Sphericity decisionStyle
    class CorrNorm,CorrNonParam,RegType,RegMulticollin decisionStyle
    class CatIndep2x2Freq,CatIndepRCFreq,CatTableSize,CatPairedType decisionStyle
    class SingleYType,SingleNorm,SinglePropSize decisionStyle
    class RCSmallExpected,GOF_ExpFreq decisionStyle
    class IndepT,WelchT,PairedT,OneWayANOVA,WelchANOVA,RMANOVA,RMANOVA_GG paramTest
    class Pearson,LinearReg,PolyReg,ChiSq2x2,ChiSqRC,OneSampleT,OnePropZ,CellMerge paramTest
    class MannW,Wilcox,KruskalW,Friedman,Spearman,Kendall nonParamTest
    class Fisher,FisherRC,McNemar,McNemarBowker,Cochran,OneSampleWilcox,ChiSqGOF,BinomialTest,MonteCarlo_GOF nonParamTest
    class PostHoc1,PostHoc2,PostHoc3,PostHoc4,PostHoc5,PostHoc6,PostHoc7,PostHoc8 postHocStyle
    class ResidualAnalRC,ResidualAnalGOF,ResidualAnalMC_GOF,ResidualAnalFRC,ResidualAnalMerge postHocStyle
    class ResidCheck1,ResidCheck2 postHocStyle
    class OddsRatio2x2,OddsRatioFisher postHocStyle
    class RegMulticollinAction warningStyle