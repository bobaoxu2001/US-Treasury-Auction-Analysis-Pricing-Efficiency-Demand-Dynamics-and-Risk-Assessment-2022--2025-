# US Treasury Auction Analysis

Quantitative analysis of 1,561 US Treasury auctions (2022-2025), examining pricing efficiency and demand dynamics across Bills, Notes, and Bonds.

## Overview

This project analyzes the relationship between Treasury auction yields and secondary market yields, quantifying the "auction premium" and identifying anomalies in pricing and demand.

**Key Findings:**
- Auction yields correlate at ρ = 0.9903 (R² = 98.1%) with market yields
- Mean auction-market spread: -2.14 bps (systematic auction premium)
- Zero weak-demand auctions (all BTC > 2.0×) despite Fed tightening
- 4.5% anomaly rate, exclusively spread-driven (no demand failures)

## Data Sources

**Treasury Auction Data:**
- API: `https://api.fiscaldata.treasury.gov/services/api/fiscal_service/v1/accounting/od/auctions_query`
- Fields: auction date, security type/term, yields, bid-to-cover ratios

**Market Yields (FRED):**
- Bills: DGS1MO, DGS3MO, DGS6MO, DTB4WK, DTB13WK, DTB26WK, DTB52WK
- Notes/Bonds: DGS1, DGS2, DGS3, DGS5, DGS7, DGS10, DGS20, DGS30
- TIPS: DFII5, DFII7, DFII10, DFII20, DFII30

## Repository Structure

```
├── US_Treasury_Auction_Analysis_Improved.ipynb    # Main analysis notebook
├── US_Treasury_Auction_Analysis_FINAL.tex         # LaTeX report (40+ pages)
├── auction_data_improved.csv                      # Full dataset (1,561 auctions)
├── auction_analysis_improved.png                  # Summary visualization
├── table1_overall_statistics.csv                  # Aggregate statistics
├── table2_type_statistics.csv                     # By security type
├── table3_anomalies_report.csv                    # Outlier detection results
├── theme1_demand_analysis.png                     # Demand dynamics charts
├── theme2_pricing_efficiency.png                  # Pricing accuracy analysis
├── theme3_temporal_trends.png                     # Time series patterns
├── theme4_term_structure.png                      # Maturity effects
├── theme5_risk_anomalies.png                      # Risk identification
└── theme6_statistical_properties.png              # Distribution diagnostics
```

## Installation

```bash
pip install pandas numpy matplotlib seaborn scipy fredapi requests
```

## Quick Start

### 1. Get FRED API Key (Required)
Register for free: https://fred.stlouisfed.org/docs/api/api_key.html

### 2. Run Analysis
```python
# Open US_Treasury_Auction_Analysis_Improved.ipynb
# Set your FRED_API_KEY in the notebook
# Run all cells
```

### 3. Generate Report
```bash
pdflatex US_Treasury_Auction_Analysis_FINAL.tex
```

## Methodology

**Data Engineering:**
- Automated API integration with pagination
- Flexible lookback window (0-5 days) for yield matching
- Linear interpolation for non-standard maturities
- 97.0% match rate (1,561 of 1,610 auctions)

**Analysis Framework:**
- Spread calculation: (Auction Yield - Market Yield) × 10,000 bps
- Anomaly detection: |Spread| > 2σ or BTC < 2.0×
- Statistical testing: Shapiro-Wilk, Kruskal-Wallis, autocorrelation
- Distribution analysis: Heavy-tailed (kurtosis = 21.4)

## Key Results

### Pricing Efficiency
- Correlation: ρ = 0.9903, R² = 98.1%
- Mean spread: -2.14 bps (auction premium)
- Std deviation: 20.12 bps
- 63.5% of auctions have negative spreads

### Demand Stability
- Mean bid-to-cover: 2.82×
- Min observed: 2.02× (no weak auctions)
- Bills: 2.91×, Notes: 2.52×, Bonds: 2.50×

### Security Type Patterns
| Type  | Count | Mean Spread | Std Dev | Mean BTC |
|-------|-------|-------------|---------|----------|
| Bill  | 1,197 | -3.34 bps   | 8.82    | 2.91×    |
| Note  | 264   | -1.90 bps   | 31.77   | 2.52×    |
| Bond  | 100   | +11.49 bps  | 50.52   | 2.50×    |

### Anomalies
- 70 outliers detected (4.5% of auctions)
- All are spread-driven (no concurrent demand failures)
- Temporal clustering: 2022 H1 (Fed tightening), 2025 Q3 (recent volatility)

## Visualizations

The analysis produces 25 charts organized into 6 themes:

1. **Demand Dynamics** - BTC evolution, distribution by type, demand classification
2. **Pricing Efficiency** - Spread distribution, auction vs market scatter, pricing accuracy
3. **Temporal Patterns** - Monthly spread evolution, quarterly heatmap, YoY comparison
4. **Term Structure** - Spread by maturity, yield distribution, term vs spread scatter
5. **Risk & Anomalies** - Anomaly timeline, volatility regimes, risk score distribution
6. **Statistical Properties** - Q-Q plot, distribution fit, autocorrelation function

## Technical Details

**Statistical Properties:**
- Distribution: Non-normal (Shapiro-Wilk p < 0.001)
- Skewness: +0.37 (slight right tail)
- Kurtosis: 21.41 (heavy tails)
- Autocorrelation: Weak (ACF lag-1 = 0.10)

**Temporal Regimes:**
- Phase 1 (2022): High volatility (σ ≈ 28 bps) - Fed tightening
- Phase 2 (2023-2024): Stabilization (σ ≈ 16 bps) - Policy pause
- Phase 3 (2025): Renewed volatility (σ ≈ 31 bps) - Bond stress

## Applications

**For Debt Managers:**
- Auction timing optimization
- Supply management (Bond spread widening flagged)
- Benchmark alignment improvements

**For Investors:**
- Bid-shading strategy (systematic -2.14 bps premium)
- Bills offer tightest pricing (σ = 8.82 bps)
- Bond volatility requires larger cushions

**For Risk Managers:**
- 2σ spread alerts for operational risk
- Demand floor validated (zero weak auctions)
- Heavy-tail models required (t-distribution)

## Limitations

- Benchmark timing: FRED end-of-day vs intraday auction
- Interpolation artifacts for non-standard maturities
- TIPS lower liquidity affects spread estimates
- Sample period specific (2022-2025 tightening cycle)

## Future Work

- Microstructure analysis with TRACE intraday data
- Dealer position integration (FR 2004 data)
- Foreign demand decomposition (TIC data)
- Machine learning for spread prediction
- Causal inference on supply/demand shocks
- International comparison (Gilts, Bunds, JGBs)

## Citation

If using this analysis, please cite:

```
US Treasury Auction Analysis: Pricing Efficiency, Demand Dynamics, and Risk Assessment (2022-2025)
Data Analysis Portfolio Project
October 2025
```

## Dependencies

- Python 3.11+
- pandas 2.1+
- numpy 1.26+
- matplotlib 3.8+
- seaborn 0.13+
- scipy 1.11+
- fredapi 0.5+
- requests 2.31+

## LaTeX Requirements (for report)

- pdflatex
- Packages: geometry, amsmath, graphicx, booktabs, hyperref, float, caption

## License

Data sources are public (US Treasury, Federal Reserve). Analysis code and visualizations are available for educational and research purposes.

## Contact

For questions about methodology or data access:
- Treasury API: https://fiscaldata.treasury.gov/api-documentation/
- FRED API: https://fred.stlouisfed.org/docs/api/

## Changelog

**v4.2 (October 2025)**
- Final corrected edition
- Fixed data source links in LaTeX
- Updated Appendix A with verified URLs
- Removed non-existent web interfaces

**v3.0 (October 2025)**
- Complete visual analysis (25 charts, 6 themes)
- Enhanced data engineering pipeline
- Comprehensive LaTeX report

**v2.0 (October 2025)**
- Improved yield curve interpolation
- Added anomaly detection framework

**v1.0 (October 2025)**
- Initial analysis (1,561 auctions, 97% match rate)
