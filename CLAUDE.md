# Simulation-Informed Gaussian Process Interpolation for Rainfall

## Project Overview

This project implements a novel **two-stage Gaussian process interpolation method** for rainfall data that leverages WRF (Weather Research and Forecasting) model simulations to inform spatial covariance structures. Unlike traditional kriging methods, this approach uses physics-based simulations as priors while handling systematic model biases through residual interpolation.

## Key Innovation

**Two-Stage Method:**
1. **Stage 1 (WRF-GP)**: Uses WRF-derived covariance matrices for GP interpolation of station observations
2. **Stage 2 (Residual Kriging)**: Applies ordinary kriging to interpolate residuals between observations and Stage 1 predictions

**Advantages:**
- Incorporates physically-consistent spatial relationships from WRF
- Honors station observations exactly
- Handles systematic model biases through residual correction
- Provides uncertainty quantification for both stages

## Methodological Contributions

### Climate Change Impact Assessment
Developed a **counterfactual analysis framework** to assess monitoring network adequacy under climate change:
- Uses **historical means** but **future covariances** to isolate pure spatial correlation effects
- Avoids contamination from mean rainfall changes under climate scenarios
- Answers: *"Will current station networks remain adequate as spatial patterns shift?"*

### Noise Parameter Analysis
Station-specific noise parameters (`sn`) from GP calibration serve dual purposes:
- **Methodological**: Optimize interpolation by accounting for measurement/model uncertainties
- **Practical**: Guide monitoring network optimization by identifying problematic stations

**Key Finding**: Spatial patterns dominate temporal patterns, suggesting noise reflects genuine station-specific uncertainties rather than seasonal model biases.

## Scripts and Workflow

### Core Analysis Scripts
1. **`two_stage_validation.ipynb`**: Leave-one-out cross-validation comparing methods
2. **`two_stage_interpolation_hist.ipynb`**: Creates historical rainfall climatology maps
3. **`his_vs_fut_twostage.ipynb`**: Counterfactual climate change impact assessment
4. **`sn_map.ipynb`**: Station noise analysis and network optimization guidance
5. **`ordinary_kriging.ipynb`**: Baseline comparison using traditional kriging

### Utility Functions
- **`utils.py`**: Core GP interpolation class, evaluation metrics, mapping tools
- **`ipcc_colormap.py`**: IPCC-compliant color schemes for precipitation visualization

## Data Structure

**Station Network**: 14 rainfall stations across Singapore
**Time Period**: 40 years (1979-2018) of monthly data
**WRF Simulations**: 
- Historical: ERA5-forced simulations
- Future: CMIP6-forced projections
- Grid: 120×160 points covering Singapore

## Key Results

### Methodological Validation
- **Improved Performance**: Two-stage method outperforms traditional kriging, especially with bias correction
- **Seasonal Variability**: Method shows consistent improvements across all months
- **Network Adequacy**: KS statistic predicts where method provides most benefit

### Climate Change Assessment
- **Spatial Pattern Changes**: Future correlations differ from historical patterns
- **Network Vulnerabilities**: Some months show reduced representativeness under changed climate
- **Clean Counterfactual**: Methodology isolates correlation effects from mean changes

### Network Optimization
- **Station Ranking**: Noise parameters identify consistently problematic locations
- **Spatial Consistency**: Same stations show issues across multiple months
- **Resource Prioritization**: Clear guidance for maintenance/upgrade investments

## Technical Implementation Notes

### GP Implementation
- Custom `gp_interpolator` class with noise parameter optimization
- Supports manual override of means and covariances for counterfactual analysis
- Leave-one-out cross-validation for noise calibration

### Visualization Standards
- Consistent black diamond markers for station locations
- IPCC color schemes for precipitation variables
- 4×3 monthly panel layouts for seasonal analysis
- Terrain background maps for geographic context

### Performance Considerations
- Efficient masking approach for leave-one-out validation
- Vectorized operations where possible
- Modular function design for reusability

## Future Extensions

1. **Multi-variable Extension**: Extend to temperature, humidity, wind
2. **Uncertainty Propagation**: Full uncertainty quantification through prediction chains
3. **Real-time Applications**: Operational implementation for nowcasting
4. **Network Design**: Optimal station placement using GP-derived uncertainty fields

## Reference Implementation

Run analysis in sequence:
1. `two_stage_validation.ipynb` - Method validation
2. `two_stage_interpolation_hist.ipynb` - Historical climatology  
3. `his_vs_fut_twostage.ipynb` - Climate change assessment
4. `sn_map.ipynb` - Network optimization analysis

Each script includes comprehensive documentation, error handling, and standardized visualization outputs.