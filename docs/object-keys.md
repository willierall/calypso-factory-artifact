# Object keys (all types)

Natural key the Workbench uses to match each object type across environments, from `default-identifiers-config.xml`, `aliases.json` and `groups.json` in the 5.8.2 release. Of 351 configured types, 255 use a pure business key, 31 have keys containing ID-named fields, 62 use an internal database ID and 3 accept either. Another 35 use custom translators whose key logic sits in code.

| Key basis | Meaning |
| --- | --- |
| Business key | Fields such as name or code |
| ID fields | The key includes fields named ...Id; these may be converted to names on export (to confirm) |
| Internal ID | A database ID |
| (GD) | The type also carries global deployment identifiers, the tool's cross-environment ID mapping (inferred) |
| Custom translator | Key built in code, for example curves, products and quotes |

Group is the export dependency group from the alias table; blank means none is assigned.

| Class | Workbench name | Natural key | Key basis | Group |
| --- | --- | --- | --- | --- |
| AccExternalName | Account External Name | accountName | Business key | Configuration |
| Account | Accounts | id | Internal ID (GD) | Static Data |
| AccountAdapter | Accounts | poShortName # name # currency | Business key | Core |
| AccountingBook | Accounting Books | name | Business key | Configuration |
| AccountingBookRuleLink | Book Rule Links | filterSet # productType # book # rule | Business key | Configuration |
| AccountingEventConfig | Events | eventType # productType | Business key | Configuration |
| AccountingPLConfig | AccountingPL Config | name | Business key | Configuration |
| AccountingRule | Rules | name | Business key | Configuration |
| AccountingRuleWrapper |  | name | Business key | Configuration |
| AccountInterestConfig | Interests Config | name | Business key | Configuration |
| AccountSweepingConfig | Accounting Sweeping Config | id | Internal ID (GD) | Configuration |
| AdviceConfig | Message Setup | id | Internal ID | Configuration |
| AdviceConfigAdapter | Message Setup | externalRef | Business key | Configuration |
| AnalysisViewerConfig | Analysis Viewer Config | readViewerConfigB | Business key | Configuration |
| AnalysisViewerConfigSearch | Analysis Viewer Config | readViewerConfigB | Business key | Configuration |
| AttributeConfigurationAdapter | Attribute Configuration | eventClass # attLevel # name | Business key | Configuration |
| AuditFilter | Audit Filters | name | Business key | Core |
| AuditFilterFieldMapping | Audit Filter Field Mapping | fieldName # productFamily # productType | Business key | Configuration |
| AuthorizationConfig | Authorization | name | Business key | Configuration |
| AuthorizationConfigAdapter | Authorization | classNames | Business key | Configuration |
| BasketTemplate | Basket Template | templateName | Business key | Product Data |
| BetaValue | Beta Value | referenceQuoteName # assetQuoteName # date # notNullableDataSource | Business key | Market Data |
| BillingGrid | Billing Grid | processingOrgId # legalEntityId # legalEntityRole # staticDataFilter # effectiveDateFrom # effectiveDateTo # feeTypeName # feeAmount # description # exchangeLeId # minimumAmount # maximumAmount # calculationType # accountId # currencyList | ID fields | Configuration |
| BondAdapter | Bond | name # benchmark # cusip # isin # bbticker # productClass | Business key | Product Data |
| BondAssetBacked |  | id or name | ID or business key | Product Data |
| BondAssetBackedSearch | Bond Asset Backed | name # cusip # isin | Business key |  |
| BondBenchmark | Benchmarks | name # effectiveDatetime | Business key | Configuration |
| BondDefault | Default | name | Business key | Product Data |
| BondSpreadAdapter | Spread | currency # maturity # dateRoll # frequency # holiday # dayCount # rateIndex # yearTenor | Business key |  |
| Book | Trading Books | name | Business key | Static Data |
| BookCurrencyAccessPermission | Book Currency Access Permission | bookId # bookBundleName # tradeCurrency | ID fields | Configuration |
| BookCurrencyPairAccessPermission | Book Currency Pair Access | bookId # bookBundleName # currencyCode1 # currencyCode2 | ID fields | Configuration |
| BookHierarchy | Book Hierarchy | name | Business key | Configuration |
| BookProductAccessPermission | Book Product Access Permission | bookId # bookBundleName # productFamily | ID fields | Configuration |
| BrokerageFeeDiscount | FX Brokerage Discount | id | Internal ID (GD) | Configuration |
| CAElectionDeadlineRule |  | id | Internal ID (GD) |  |
| CAElectionInstruction |  | id | Internal ID (GD) |  |
| CalypsoImage | Images | name | Business key | Configuration |
| CalypsoRiskAggregationNodeDb | CalypsoRiskAggregationNodeDb | treeNodeId | Internal ID (GD) |  |
| CalypsoTreeDb | CalypsoTreeDb | treeId | Internal ID (GD) |  |
| CalypsoTreeViewNodeDb | CalypsoTreeViewNodeDb | treeNodeId | Internal ID (GD) |  |
| CalypsoVisokioNodeDb | CalypsoVisokioNodeDb | treeNodeId | Internal ID (GD) |  |
| CalypsoWorkspaceNodeDb |  | treeNodeId | Internal ID (GD) |  |
| CASwiftEventCodeAdapter |  | swiftCode | Business key |  |
| CDSIndexDefinitionAdapter | CDS Index Definition | referencePortfolio # currency # maturityDate # indexName | Business key |  |
| CDSSettlementMatrix | CDS Settlement Matrix | id # name | Internal ID (GD) | Product Data |
| CDSSettlementMatrixConfig | CDS Settlement Matrix Configuration | username | Business key | Reference Data |
| CFDContractDefinition | CFD Contracts | id | Internal ID (GD) | Product Data |
| CFDCountryGrid | CFD Country Grid | id | Internal ID (GD) | Product Data |
| ChasingConfig | Chasing Config | messageType # productType # status # formatType # senderId # receiverId # sdFilter | ID fields | Configuration |
| CommodityAdapter | Commodity Adapter | currency # name # location | Business key |  |
| CommodityCertificateStockParam |  | name | Business key | Configuration |
| CommodityFwdPointGenerator | Commodity Forward Point Generator | name | Business key | Market Data |
| CommodityOptionVolType | Commodity Option Volatility Types | optionType # delta | Business key | Product Data |
| CommodityQuoteNameDescriptor | Commodity Quote Creator | quoteName # commodityId | ID fields | Product Data |
| CommodityReset | Commodity Reset | name | Business key | Product Data |
| CommodityRiskWeight | Commodity Risk Weight | productId # rank | ID fields | Product Data |
| CommodityUnitConversion | Commodity Unit Conversion | fromUnit # toUnit # commodityName | Business key | Product Data |
| CommodityVolPointGenerator | Commodity Volatility Point Generator | name | Business key | Market Data |
| ComparatorDynamic | Liquidation Info Comparator Dynamic | name | Business key | Core |
| ConfigurableField | Configurable Field | uniqueKey | Business key | Configuration |
| CorrelationFormula | Correlation Formula | type # currency # name | Business key | Market Data |
| CorrelationMatrix | Correlation Matrix | In code | Custom translator | Market Data |
| CorrelationSurface | Basket Correlation | In code | Custom translator | Market Data |
| CorrSurfUnderlyingCDSIndexTranche | CorrSurfUnderlyingCDSIndexTranche | id | Internal ID (GD) | Market Data |
| Country | Countries | name | Business key | Reference Data |
| CreditFacility |  | name | Business key |  |
| CreditRating | Credit Rating | legalEntityName # asOfDate # debtSeniority # agencyName # ratingType | Business key | Reference Data |
| CrossAssetPLParam | Cross Asset PL Parameters | name | Business key | Configuration |
| CurrencyDefault | Currency Definitions | code | Business key | Reference Data |
| CurrencyPair | Currency Pairs | primaryCode # quotingCode | Business key | Reference Data |
| CurRollover | Position Rollover | curPair # rolloverType # bookId | ID fields | Configuration |
| Curve | Curve | In code | Custom translator |  |
| CurveBasis | Curve Basis | name # currency # datetime # type | Business key | Market Data |
| CurveBasisAdapter | Curve Basis | In code | Custom translator | Market Data |
| CurveBondSpread | CurveBondSpread | name # currency # datetime # type | Business key | Market Data |
| CurveBorrowAdapter | Curve Borrow | In code | Custom translator | Market Data |
| CurveCDSBasisAdjustment | Basis Adjustment | In code | Custom translator | Market Data |
| CurveCommodity | Curve Commodity | In code | Custom translator | Market Data |
| CurveCommoditySeasonality | Curve Commodity Seasonality | In code | Custom translator | Market Data |
| CurveCommoditySpread | Spread Curve | In code | Custom translator | Market Data |
| CurveDefault | Curve Default | In code | Custom translator | Market Data |
| CurveDelinquency | Curve Delinquency | In code | Custom translator | Market Data |
| CurveDividendAdapter | Curve Dividend | In code | Custom translator | Market Data |
| CurveFX | Curve FX | In code | Custom translator | Market Data |
| CurveInflationAdapter | Curve Inflation | In code | Custom translator | Market Data |
| CurvePrepay | Curve Prepayment | In code | Custom translator | Market Data |
| CurveProbability | Curve Probability | In code | Custom translator | Market Data |
| CurveProbabilityAdapter | Curve Probability | In code | Custom translator | Market Data |
| CurveRecovery | Curve Recovery | In code | Custom translator | Market Data |
| CurveRepo | Curve Repo | In code | Custom translator | Market Data |
| CurveUnderlying | Curve Underlying | id | Internal ID (GD) | Market Data |
| CurveUnderlyingBasisSwap | Curve Underlying Basis Swap | currency # maturity # baseRateIndex # basisRateIndex # basisCouponFrequency | Business key | Market Data |
| CurveUnderlyingBasisTwoSwap | Curve Underlying Basis Two Swap | currency # maturity # baseRateIndex # basisRateIndex # basisCouponFrequency # payFixedBasisB | Business key | Market Data |
| CurveUnderlyingBond | Curve Underlying Bond | id | Internal ID (GD) | Market Data |
| CurveUnderlyingBondAdapter | Curve Underlying Bond | benchmarkName # relative # maturityTenor # bondName # bondBenchmark # bondCusip # bondIsin # bondBbticker | Business key | Market Data |
| CurveUnderlyingBondFuture | Curve Underlying Bond Future | currency # exchange # contractName # rank | Business key | Market Data |
| CurveUnderlyingBondFutureAdapter | Curve Underlying Bond Future | currency # exchange # contractName # rank | Business key | Market Data |
| CurveUnderlyingCDS | Curve Underlying CDS | id | Internal ID (GD) | Market Data |
| CurveUnderlyingCDSAdapter | Curve Underlying CDS | tenor # currency # ticker # referenceIssuer # referenceSeniority # restructuringType | Business key | Market Data |
| CurveUnderlyingCDSIndex | Curve Underlying CDS Index | id | Internal ID (GD) | Market Data |
| CurveUnderlyingCDSIndexAdapter | Curve Underlying CDS Index | currency # indexName | Business key | Market Data |
| CurveUnderlyingCommoditySpot | Curve Underlying Commodity Spot | id | Internal ID (GD) | Market Data |
| CurveUnderlyingCommoditySpreadGenerator | CurveUnderlyingCommoditySpreadGenerator | id | Internal ID (GD) | Market Data |
| CurveUnderlyingCommoditySwapGenerator | CurveUnderlyingCommoditySwapGenerator | id | Internal ID (GD) |  |
| CurveUnderlyingContango |  | id | Internal ID (GD) |  |
| CurveUnderlyingEquityIndexAdapter | Curve Underlying Equity Index | currency # name | Business key | Market Data |
| CurveUnderlyingETO | CurveUnderlyingETO | id | Internal ID (GD) | Market Data |
| CurveUnderlyingETOEquity |  | id | Internal ID (GD) |  |
| CurveUnderlyingETOEquityIndex |  | id | Internal ID (GD) |  |
| CurveUnderlyingFRA | Curve Underlying FRA | currency # rateIndex # startTenor # maturityTenor # rank # underlyingSeriesName # specificStartDate # specificEndDate # isSpecific | Business key (GD) | Market Data |
| CurveUnderlyingFuture | Curve Underlying Future | currency # exchange # contractName # rank # _isSerial | Business key | Market Data |
| CurveUnderlyingFutureAdapter | Curve Underlying Future | currency # exchange # contractName # rank # _isSerial | Business key | Market Data |
| CurveUnderlyingFutureCommodity | CurveUnderlyingFutureCommodity | currency # exchange # contractName # rank | Business key | Market Data |
| CurveUnderlyingFutureCommodityAdapter | Curve Underlying Future Commodity | currency # exchange # contractName # rank | Business key | Market Data |
| CurveUnderlyingFXForward | Curve Underlying FX Forward | id | Internal ID (GD) | Market Data |
| CurveUnderlyingFXForwardAdapter | Curve Underlying FX Forward | currency # quotingCurrency # tenor # contango | Business key | Market Data |
| CurveUnderlyingFXForwardFixed | Curve Underlying FX Forward Fixed | currency # quotingCurrency # endDate | Business key | Market Data |
| CurveUnderlyingFXForwardMonthEnd |  | currency # quotingCurrency # month # yearOffset | Business key |  |
| CurveUnderlyingGenericCDS |  | id | Internal ID (GD) |  |
| CurveUnderlyingGenericCDSAdapter |  | tenor # currency # agency # sector # rating # region | Business key |  |
| CurveUnderlyingListedFRA |  | currency # exchange # contractName # rank | Business key |  |
| CurveUnderlyingListedFRAAdapter | Curve Underlying ListedFRA | currency # exchange # contractName # rank | Business key | Market Data |
| CurveUnderlyingMoneyMarketAdapter | Curve Underlying Money Market | currency # instrumentType # moneyMarketType # tenor # settleDays # rateIndexSource # specificStartDate # specificEndDate # isSpecific | Business key | Market Data |
| CurveUnderlyingSearch |  | id | Internal ID (GD) | Market Data |
| CurveUnderlyingSpecificMMkt | Curve Underlying Turn Rate | In code | Custom translator | Market Data |
| CurveUnderlyingSpread | Curve Underlying Spread | currency # bondSpreadCurrency # maturity # dateRoll # frequency # holiday # dayCount # rateIndex # yearTenor | Business key | Market Data |
| CurveUnderlyingSpreadAdapter | Curve Underlying Spread | currency # bondSpreadCurrency # maturity # dateRoll # frequency # holiday # dayCount # rateIndex # yearTenor | Business key | Market Data |
| CurveUnderlyingSpreadOnInstrumentAdapter | Curve Underlying Spread On Instrument | currency # spreadName # type | Business key | Market Data |
| CurveUnderlyingSwap | Curve Underlying Swap | currency # fixedCouponFrequency # fixedLegCurrency # isSpecificStartEndDate # maturity # rateIndex | Business key | Market Data |
| CurveUnderlyingSwapAdapter | Curve Underlying Swap | currency # fixedCouponFrequency # fixedLegCurrency # isSpecificStartEndDate # maturity # rateIndex | Business key | Market Data |
| CurveYield | Par Yield Curve | In code | Custom translator | Market Data |
| CurveZero | Zero Yield Curve | name # currency # datetime # type | Business key | Market Data |
| CurveZeroAdapter | Zero Yield Curve | In code | Custom translator | Market Data |
| CurveZeroFXDerived | FX Derived Curve | name # currency # datetime # type | Business key | Market Data |
| CurveZeroFXDerivedAdapter | FX Derived Curve | In code | Custom translator | Market Data |
| CustodyB2BConfigAdapter |  | bookName # productName # sdFilter # effectiveFrom # effectiveTo | Business key |  |
| CustomRule | Custom Rule | name | Business key | Configuration |
| CustomRuleAdapter | CustomRuleAdapter | name | Business key | Configuration |
| CustomTenorShortcut | Custom Tenors | shortCutChar | Business key | Configuration |
| CWSUserConfigDocumentsAdapter | CWS Documents by user | userName | Business key | Configuration |
| DataSegregationAccessPermission | Data Segregation | userName # legalEntityId # groupName # versionNumber | ID fields | Configuration |
| DateRule | Date Rules | name | Business key | Core |
| DealEntryRateToleranceConfig | Deal Entry Rate Tolerance | curPair | Business key | Configuration |
| DispatcherConfig | Dispatcher Config | name | Business key | Configuration |
| DomainAdapter |  | name | Business key |  |
| DomainValueAdapter |  | domainName # value | Business key | Core |
| DomainValues$DomainValuesRow | Domain Values | key # value | Business key | Core |
| DomainValuesAdapter |  | name | Business key | Core |
| EconomicPLParam | Economic PL Parameters | name | Business key | Configuration |
| EngineConfigAdapter | All Engine Configuration | In code | Custom translator | Configuration |
| EngineConfigurationAdapter | Engine Configuration | engineName | Business key | Configuration |
| EngineEventFilter |  | eventConfigName # eventFilter # engineName | Business key | Configuration |
| EngineParamAdapter |  | engineName | Business key | Configuration |
| Equity | Equity | In code | Custom translator | Product Data |
| EquityIndex | Equity Index | In code | Custom translator | Product Data |
| EquityReset | EquityReset | id # source # type # underlyingEquityId | Internal ID | Market Data |
| ETOContract | ETO Contract | id # name # exchange # settlementType # currency # underlyingType # exerciseType # contractSize # quoteType # autoExercise # tradingEndTime # lastExerciseTime # minimumMoveInTicks # noOfContracts # tickSize # tickValue | Internal ID | Product Data |
| EventConfig | Event Config | name | Business key | Configuration |
| EventConfiguration |  | eventConfigName # eventClass # engineName | Business key | Configuration |
| ExoticConfigurableTypeI | Exotic Type Creator | configurableTypeName | Business key | Market Data |
| FeeBillingRuleAdapter | Fee Billing Rule | poShortName # leShortName # leRole # currency # staticDataFilter # entryType # effectiveDateFrom # effectiveDateTo | Business key | Configuration |
| FeeConfig | Fee Config | name # activeFrom # activeTo | Business key | Configuration |
| FeedAddress | Feed Address | quoteName # feedName # quoteType # feedAddress | Business key | Market Data |
| FeeDefinition | Fee Definition | type | Business key | Configuration |
| FeedParamsAdapter | Feed Config | feedName | Business key | Market Data |
| FeeGrid |  | processingOrgId # legalEntityId # legalEntityRole # staticDataFilter # effectiveDateFrom # effectiveDateTo # feeTypeName # feeAmount # description # exchangeLeId # minimumAmount # maximumAmount # calculationType # accountId # currencyList | ID fields | Configuration |
| FeeGridAdapter2 | Fee Grid | processingOrgId # legalEntityId # legalEntityRole # staticDataFilter # effectiveDateFrom # effectiveDateTo # feeType # feeAmount # description # exchangeLeId # minimumAmount # maximumAmount # calculationType # accountId # currencyList # baseCurrency # quotingCurrency # productName | ID fields | Configuration |
| FileDocument | FileDocument | name # version | Business key | Configuration |
| FilterSet | Filter Set | name | Business key | Core |
| FixingDatePolicy |  | name | Business key |  |
| FolderDb | FolderDb | treeNodeId | Internal ID (GD) |  |
| FOLivePLParam | FOLivePLParam | name | Business key | Configuration |
| FundingRateAdapter | Funding Rate Definitions | processingOrg # productType # bookName # sdFilter # currency # daycount # financingSpread # bondName # benchmark # cusip # isin # bbticker # rateIndex # rateIndexSpread | Business key | Reference Data |
| FutureContract | Future Contracts | currency # exchange # name | Business key | Product Data |
| FutureContractAdapter | Future Contracts | futureType # exchange # currency # name | Business key |  |
| FutureOptionContract | Future Option Contracts | currency # exchange # name | Business key | Product Data |
| FwdRiskTransfer | Fwd Risk Transfer | bookId # product # curPair | ID fields | Configuration |
| FX | FX | In code | Custom translator | Reference Data |
| FXLinkedBookSubstitution | Book Substitution | id | Internal ID (GD) | Configuration |
| FXLinkedBookSubstitutionSearch | Book Substitution | id | Internal ID (GD) | Configuration |
| FXOptExpTZ | FX Option Expiry Time Zone | name | Business key | Reference Data |
| FXQVar | Exotic FX Variables | name | Business key | Market Data |
| FXRateResetReportTemplateAdapter | FX Rate Reset Report Template | reportType # reportName # user # isPrivate | Business key | Configuration |
| FXReset | FX Rate Reset | primary # quoting # name | Business key | Market Data |
| FXResetPair | FX Reset Pairs | name | Business key | Reference Data |
| FXVolatilitySurface | FX Volatility Surface | name # currency # datetime # volType | Business key | Market Data |
| FXVolatilitySurfaceAdapter | FX Volatility Surface | In code | Custom translator | Market Data |
| Group | Groups | name | Business key | Configuration |
| GroupAccessAdapter |  | groupName # permName # accessValue | Business key | Configuration |
| GroupAdapter | Groups | groupName | Business key | Configuration |
| Haircut | Haircut | name # secFilter | Business key | Static Data |
| HaircutRuleAdapter | HairCut Rule | name | Business key | Static Data |
| HedgeParam | Hedge Parameters | name | Business key | Configuration |
| Holiday | Holidays | In code | Custom translator | Reference Data |
| HolidayAdapter | Holiday Calender Definition | code | Business key |  |
| HolidayCode | Holiday Codes | code | Business key | Reference Data |
| HolidayRule | Holiday Rules | name | Business key | Reference Data |
| IFMFeeTypeFormulaMapping |  | name | Business key |  |
| IFMRowFeeConfigMapping |  | name | Business key |  |
| IncomingConfigAdapter | Incoming Message Config | objectType # receiverShortName # incomingMessageType # addressMethod # roleType # matchingConfigB | Business key | Configuration |
| InMemoryRiskServerConfig | InMemoryRiskServerConfig | configName # analysisType # tradeFilterName # paramName # serverName | Business key | Configuration |
| InterestRateQVar | Exotic Interest Rate Variables | name | Business key | Market Data |
| IntradayConfiguration | Intraday Configuration | id | Internal ID (GD) | Configuration |
| InventoryParam |  | name | Business key |  |
| InvestmentPerformanceParam |  | name | Business key |  |
| JumpToDefaultParam | Jump To Default Parameters | name | Business key | Configuration |
| KickOffCutOffConfigAdapter | Kick Off Cut Off Configuration | processingOrg # eventClass # subtype # product # origStatus # actionType # resultStatus # receiver # currency # method # sdFilter | Business key | Configuration |
| LadderLivePLParam | LadderLivePLParam | name | Business key | Configuration |
| LEContactAdapter | LE Contacts | leName # leRole # contactType # productType # poName # sdFilter | Business key | Static Data |
| LegalAgreement | LE Agreements | In code | Custom translator | Static Data |
| LegalAgreementAdapter | LE Agreements | externalReference | Business key | Static Data |
| LegalEntity | Legal Entities | code | Business key | Static Data |
| LegalEntityAdapter | Legal Entities | code | Business key | Static Data |
| LegalEntityAttributeAdapter | LegalEntityAttribute | entityName # processingOrgName # type # role | Business key | Static Data |
| LegalEntityRegistrationAdapter | LE Registration | legalEntityName # agentLegalEntityName # legalEntityRole | Business key | Static Data |
| LegalEntityRelation | LE Relations | legalEntityId # legalEntityRole # POId | ID fields | Static Data |
| LegalEntityRelationAdapter | LE Relations | poName | Business key | Static Data |
| LERegistration | LERegistration | id | Internal ID (GD) | Static Data |
| LifeCycleProcessorRule | LifeCycle Processor Rule | event # productType # payoff | Business key | Configuration |
| LifeCycleTriggerRule | LifeCycle Trigger Rule | event # productType # payoff | Business key | Configuration |
| LiquidationConfig | Liquidation Config | name | Business key | Core |
| LiquidationConfigAdapter | Liquidation Config | name | Business key | Configuration |
| LiquidationInfoAdapter | Liquidation Info Configuration | bookName # productType # liquidationConfigName | Business key | Configuration |
| ListedFRA |  | id | Internal ID |  |
| ListedFRAContract | Listed FRAContracts | currency # exchange # name | Business key | Product Data |
| MainEntryProperties | User Configuration | userName | Business key | Configuration |
| ManualSDI | Manual SDI | id or reference | ID or business key (GD) | Static Data |
| MappingStatus | Mapping Status | In code | Custom translator | Configuration |
| MappingStatusAdapter | Mapping Status | processingOrg # eventClass # subType # product # origStatus # actionType # resultStatus # incomingMsgType # incomingStatus | Business key | Configuration |
| MarginCallConfig | Margin Call | name | Business key (GD) | Static Data |
| MarketDataGroup | MarketDataGroup | name | Business key | Market Data |
| MarketMeasureDefinition | Market Measure Definition | name | Business key | Market Data |
| MasterConfirmationAdapter | MasterConfirmation | poName # cptyName # productName # confirmName | Business key | Reference Data |
| MessageEditing | Message Editing | format # template | Business key | Configuration |
| MessageGroup | Message Group | name | Business key | Configuration |
| MessageGroupingAdapter | Message Grouping | categoryName | Business key | Configuration |
| MessageRule | Message Rule | id | Internal ID (GD) | Static Data |
| MessageRuleAdapter | Message Rule | processingOrgName # contactType # messageGroup # productTypeList # excludeProductTypeList # method # language # isSent | Business key | Static Data |
| MktDataConfigItemNameAdapter | Market Data Config Item | type # currency # name | Business key | Market Data |
| MktDataConfigSet | Market Data Generation Policy | name | Business key | Market Data |
| MultiCurvePackage | MultiCurve Curves | name | Business key | Market Data |
| MultiCurvePackageAdapter | MultiCurve Curves | name # currency # curveDateTime # curveType # instanceAsString | Business key | Market Data |
| MultiQuoteMapping | MultiQuoteMapping | productName # productType # source # currency # settleDays | Business key | Market Data |
| MultiSimulationParam |  | name | Business key | Configuration |
| Mutation | Mutation | id | Internal ID (GD) |  |
| NatClearingData | NatClearingData | natCode # systemCode | Business key | Static Data |
| NAVParam |  | name | Business key |  |
| NettingConfig$NettingConfigRow | Netting Config | type | Business key | Configuration |
| NettingConfigAdapter | NettingConfigAdapter | nettingType | Business key | Configuration |
| NettingMethodAdapter | Netting Method | legalEntityName # processingOrg # currency # effectiveFrom # role # productTypeList # settleMethod # effectiveTo # sdFilter | Business key | Configuration |
| ObservedData | ObservedData | id | Internal ID (GD) |  |
| ODAUserConfigDocumentsAdapter | ODA Documents by user | userName | Business key | Configuration |
| OfficialPLConfig | PL Configuration | name | Business key | Configuration |
| OfficialPLParam | OfficialPL Parameter | name | Business key | Configuration |
| OptionLifecycleParameters | Option Lifecycle Parameters | name | Business key | Configuration |
| ParRatesSet | Par Rates Set | name | Business key | Configuration |
| PartialCWSUserConfigDocumentsAdapter | CWS Partial Report | userName # uuid | Business key | Configuration / Market Data |
| PartialRiskConfig |  | name # tradeFilterName # analysisName # pricingEnvName # paramName | Business key |  |
| PartialRiskPresenterConfig | Partial Risk Presenter Config | name # tradeFilterName # analysisName # pricingEnvName # paramName # presentationName # riskOnDemandRunnerName | Business key | Configuration |
| PaymentSetup | Payment Setup | poId # type # leid # leRole # currency # productType # nettingType # fixed # messageType # eventType # messageStatus # transferType # linkedId # allMessages # sdfilter # linkedType # configuration # checkUnderlying # configGroup # validationGroup # referenceCurrency | ID fields | Configuration |
| PercentAllocRule | Allocation Rule | id | Internal ID (GD) | Configuration |
| PeriodDistribution | Period Distribution | id | Internal ID (GD) | Market Data |
| PLGreeksParam |  | name | Business key | Configuration |
| PLMethodologyConfig | PL Methodology | name | Business key | Configuration |
| PLPosition | PLPosition | positionId | Internal ID (GD) |  |
| PLTaxRule | PL Tax Rule | id | Internal ID (GD) | Configuration |
| PortfolioParam |  | name | Business key |  |
| PortfolioSwapContract | PortfolioSwapContract | id | Internal ID (GD) | Configuration |
| Position | Position | positionLongId | Internal ID (GD) |  |
| PositionAggregation | PositionAggregation | id | Internal ID (GD) |  |
| PositionAggregationConfig | Position/Liquidation Key Configuration | name | Business key | Configuration |
| PositionInfoAdapter | Position Configuration | bookName # productType | Business key | Configuration |
| PositionSpec | Position Specification | name | Business key | Configuration |
| PresentationServerConfig | Presentation Server Config | name | Business key | Configuration |
| PricerConfig | Pricer Configuration (Full) | name | Business key | Market Data |
| PricerConfigItemABS | Pricer Config Item - ABS | name # currency # issuerId # absSeries # absClass # marketDataUsage # marketDataType # marketDataId # absGroup # absCollateralName # curveName | ID fields | Market Data |
| PricerConfigItemCalibrationModel | Pricer Config Item - CalibrationModel | name # productPricerContext # model | Business key | Market Data |
| PricerConfigItemCalibrator | Pricer Config Item - Calibrator | name # model # calibrator | Business key | Market Data |
| PricerConfigItemCommodity | Pricer Config Item - Commodity | pricerConfigName # productType # mdiId # mdiType # mdiName | ID fields | Market Data |
| PricerConfigItemCorrelation | Pricer Config Item - Correlation | pricerConfigName # firstAxisType # secondAxisType # matrixId # mdiType # matrixName | ID fields | Market Data |
| PricerConfigItemCredit | Pricer Config Item - Credit | name # keyType # keyId # marketDataType # marketDataUsage # marketDataId # curveName | ID fields | Market Data |
| PricerConfigItemDefinition | Pricer Config Item - Definition | name | Business key | Market Data |
| PricerConfigItemDiscount | Pricer Config Item - Discount Curves | name # currency # currency2 # domiciliation # productType # subType # extType # rateIndexName # rateIndexTenor # collateralCurrency # zeroCurveId # zeroCurveName # marketDataItemType | ID fields | Market Data |
| PricerConfigItemForecast | Pricer Config Item - Forecast Curves | name # currency # rateIndexCode # zeroCurveId # zeroCurveName # marketDataItemType | ID fields | Market Data |
| PricerConfigItemFX | Pricer Config Item - FX | name # currency1 # currency2 # domiciliation # productType # subType # extType # marketDataItemType # marketDataUsage # marketDataId # curveName | ID fields | Market Data |
| PricerConfigItemModelParameter | Pricer Config Item - Model Parameters | pricerConfigName # pricer # parameterName # value # valuetype | Business key | Market Data |
| PricerConfigItemPricer | Pricer Config Item - Pricers | name # productType # productPricer | Business key | Market Data |
| PricerConfigItemProductSpecific | Pricer Config Item - Product Specific | name # description # marketDataType # marketDataId # curveName | ID fields | Market Data |
| PricerConfigItemRepo | Pricer Config Item - Repo | name # usage # currency # matcher # priority # curveId # curveName | ID fields | Market Data |
| PricerConfigItemSurface | Pricer Config Item - Surfaces | pricerConfigName # currency # surfaceDesc # surfaceId # surfaceName # mdiType | ID fields | Market Data |
| PricerConfigItemTradeLevelMDIOverride | Pricer Config Item - Trade Level MDI Override | name # keyName # pricerName # usage # marketDataItemId # marketDataType # marketDataName | ID fields | Market Data |
| PricerConfigItemTradeLevelPricerOverride | Pricer Config Item - Trade Level Pricer Override | name # keyName # productName # pricerName | Business key | Market Data |
| PricerMeasure$PricerMeasureRow | Pricer Measure | id # measure | Internal ID | Market Data |
| PricingEnv | Pricing Environment | name | Business key | Market Data |
| PricingParam | Pricing Parameter | name | Business key | Configuration |
| PricingParameters | Pricing Parameter Set | name | Business key | Market Data |
| PricingParamSearch |  | param_name | Business key | Market Data |
| PricingParamType | Pricing Parameter Type | name | Business key | Market Data |
| Product | Product | In code | Custom translator |  |
| ProductCode | Product Code | code | Business key | Product Data |
| ProductQVarI | Exotic Product Variables | name | Business key | Market Data |
| QuoteNameItem | Default Quote Type | quoteName | Business key | Market Data |
| QuoteSet | Quote Set | name | Business key | Market Data |
| QuoteValue | Quote Value | In code | Custom translator | Market Data |
| RateIndex | Rate Index Tenors | currency # name # tenor # source | Business key | Reference Data |
| RateIndexDefaults | Rate Index Definitions | currency # name | Business key | Reference Data |
| RatingValues$RatingValuesAdapter | Rating Values | agencyName # type # value | Business key | Product Data |
| RatingValuesAdapter |  | agency # type | Business key | Static Data |
| ReadAnalysisViewerConfigSearch | Read Analysis Viewer Config | readViewerConfigB | Business key | Configuration |
| ReferenceEntityBasket | Reference Entity Basket | name | Business key | Product Data |
| ReferenceEntityNthDefault | ReferenceEntityNthDefault | id | Internal ID (GD) |  |
| ReferenceEntitySingle | ReferenceEntitySingle | id | Internal ID (GD) | Static Data |
| ReferenceEntityTranche | ReferenceEntityTranche | id | Internal ID (GD) |  |
| ReportBrowserConfig | Report Browser Config | id | Internal ID (GD) | Configuration |
| ReportBrowserConfigTreeNode | Report Browser reporting node | nodeId | Internal ID (GD) | Configuration |
| ReportTemplate | Report Template | reportType # templateName # user # isPrivate | Business key | Configuration |
| ReportView | Report View | id | Internal ID | Configuration |
| ReportWrapperParam | ReportWrapperParam | name | Business key | Configuration |
| ResetRiskParam | Reset Risk Parameter | name | Business key | Configuration |
| RiskConfig | Risk Config | name | Business key | Configuration |
| RiskPresenterConfig | Presentation Server | name | Business key | Configuration |
| SalesB2B | Back to Back Trades | bookId # product # curPair # staticDataFilter | ID fields | Configuration |
| SalesMarginAnalysisParam |  | name | Business key | Configuration |
| ScenarioCommodityParam | Scenario Commodity Parameter | name | Business key | Configuration |
| ScenarioGreeksParam | ScenarioGreeksParam | name | Business key | Configuration |
| ScenarioMarketData | Scenario Market Data | scenarioName | Business key | Configuration |
| ScenarioParam | Scenario Parameter | name | Business key | Configuration |
| ScenarioParamSearch |  | param_name | Business key | Configuration |
| ScenarioRiskPositionParam | Scenario Risk Position Parameter | name | Business key | Configuration |
| ScenarioRule | Scenario Rule | scenarioName | Business key | Configuration |
| SenderConfig | Message Sender | status # productType # adviceType # addressMethod # gateway | Business key | Configuration |
| SenderCopyConfig | Sender Copy Config | senderConfigId # senderContactId # senderRole # senderContactType # receiverContactId # receiverRole # receiverContactType # adviceType # addressMethod # gateway # language | ID fields | Configuration |
| SensitivityParam | Sensitivity Parameter | name | Business key | Configuration |
| SettleDeliveryInstruction | Settlement Instructions | id or reference | ID or business key (GD) | Static Data |
| SettlementMessageSending | Message Sending | id | Internal ID (GD) | Configuration |
| SettlementMethod | Method | id | Internal ID (GD) | Configuration |
| SimulationParam | Simulation Paramameter | name | Business key |  |
| SimulationParamSearch | Simulation Parameter | param_name | Business key | Configuration |
| SpeedButton | Speed Button | id | Internal ID (GD) | Configuration |
| SpotRiskTransfer | Spot Risk Transfer | bookId # product # curPair | ID fields | Configuration |
| StatementConfig | Statement Config | name # billingType | Business key (GD) | Configuration |
| StaticDataFilter | Static Data Filters | name | Business key | Core |
| SwiftBICData | SwiftBICData | bic # bicBranch # institution | Business key | Static Data |
| TaskConfigAdapter | Task Access | groupName | Business key | Configuration |
| TaskInternalRefAdapter | Task Internal Reference | eventClass # level # sequence # sdFilter | Business key | Configuration |
| TaskPriority | Task Priority | id | Internal ID | Configuration |
| TaskStationConfigAdapter | Task Station Configuration | userName # configName | Business key | Configuration |
| TaskWorkflowConfigAdapter | Workflows | processingOrg # eventClass # subtype # product # origStatus # action # resultStatus | Business key | Configuration |
| TemplateInfo | Product Templates | user # productType # name # additionalIdentifier # comment | Business key | Configuration |
| Ticker | Ticker | name | Business key |  |
| Trade | Trade | longId | Internal ID (GD) | Transactional |
| TradeBlotterConfigAdapter | Blotter Users | userName | Business key | Configuration |
| TradeBundle | Trade Bundle | type # name | Business key |  |
| TradeByProductType | Trades | id | Internal ID (GD) | Market Data |
| TradeFilter | Trade Filters | name | Business key | Core |
| TradeKeywordConfig | Trade Keyword Config | name | Business key | Configuration |
| TradeWinClassConfig | Trade Window Configuration | configName | Business key | Configuration |
| TriangulationCcyRuleSet | Triangulation Ccy Rule | name | Business key | Reference Data |
| TTMRate | TTM Rate | currencyPair.primaryCode # currencyPair.quotingCode # type # date | Business key | Reference Data |
| User | Users | name | Business key | Configuration |
| UserAccessPermission | User Access Permission | version | Business key | Configuration |
| UserConfigDocumentAdapter | All Documents | user # name # uri # saveDate | Business key | Configuration |
| UserConfigDocumentsAdapter | All Documents by user | userName | Business key | Configuration |
| UserConfiguration | Users Configuration | userName | Business key | Configuration |
| UserDefaults | UserDefaults | userName | Business key | Configuration |
| UserSettingsAdapter | Position Keeper | user # property | Business key |  |
| UserWorkflowConfig | Task Station Defaults | user # name | Business key | Configuration |
| VolatilityIndex |  | In code | Custom translator |  |
| VolatilityIndexAdapter |  | name | Business key |  |
| VolatilitySurface3D | Volatility Surface | currency # name | Business key | Market Data |
| VolatilitySurface3DAdapter | Volatility Surface | In code | Custom translator | Market Data |
| VolSurfaceUnderlying | Volatility Surface Underlying | id | Internal ID (GD) | Market Data |
| VolSurfaceUnderlyingCap | Volatility Surface Underlying Cap | currency # rateIndex # startTenor # maturity # couponFrequency # optionType # strikeRelativeB # strike | Business key | Market Data |
| VolSurfaceUnderlyingCommodityOption | VolSurfaceUnderlyingCommodityOption | volPointGeneratorName # underlyingType # currency # volatilityType # expiry # pillarDate # rank | Business key | Market Data |
| VolSurfaceUnderlyingCommodityOptionAdapter | Volatility Surface Underlying Commodity Option | volPointGeneratorName # underlyingType # commodityName # currency # volatilityType # expiryDate # pillarDate # expiryTenor # rank | Business key | Market Data |
| VolSurfaceUnderlyingETOAdapter | VolSurfaceUnderlyingETO | optionType # rank # currency # kickedOutB # strike # strikeRelative # optionContract | Business key |  |
| VolSurfaceUnderlyingFutureOptionAdapter | Volatility Surface Underlying Future Option | underlyingType # optionType # strike # currency # exchange # contract # rank | Business key | Market Data |
| VolSurfaceUnderlyingFXOptAdapter | Volatility Surface Underlying FX Option | currencyPair # optionType # maturity # delta | Business key | Market Data |
| VolSurfaceUnderlyingOTCEquityOptionAdapter | Volatility Surface Underlying OTC Equity Option | underlying # optionType # strike # expiryTenor # exerciseType # currency # dateRule # rank | Business key | Configuration |
| VolSurfaceUnderlyingSwaption | Volatility Surface Underlying Swaption | currency # rateIndex # swapTenor # expiration # fixedRate # fixedRateRelativeB # payFixedB | Business key (GD) | Market Data |
| WithholdingTaxAttributeAdapter | WithholdingTaxAttribute | legalEntityId # processingOrgId # legalEntityRole # entityType # entityId # attributeType # attributeValue | ID fields | Static Data |
| WithholdingTaxConfig |  | id | Internal ID (GD) |  |
| WorkflowAccessPermission | Workflow Access | groupName # productFamily # status # tradeAction # workflowType # messageType | Business key | Configuration |
| XCcySplit | Currency Splits | bookId # product # curPair | ID fields | Reference Data |
| XCcySpotMismatch | Spot Mismatch | curPair # bookId # swapBookId | ID fields | Configuration |
