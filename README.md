# Bot Detection Framework for Incentivized Online Surveys

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository contains the implementation of a systematic multi-filter approach for detecting and removing bot responses from incentivized online surveys, as described in our paper:

**"Explainable Data-Driven Multi-Filter Framework for Bot Detection in Incentivized Online Surveys"**  
Andrew Ruger¹†, Mohammed Abdalazeem²*†, Eleni Christofa², Jimi Oke²

¹Department of Economics, Grinnell College  
²Department of Civil and Environmental Engineering, University of Massachusetts Amherst  
†These authors contributed equally to this work

## Overview

Online surveys with monetary incentives are increasingly vulnerable to automated bots that compromise data quality and exhaust research budgets. This framework provides a transparent, generalizable, and parameterized approach to identify and filter bot responses using commonly available survey platform metadata.

### Key Features

- **Multi-layered detection**: 11 independent filters organized into 4 categories
- **Platform-agnostic**: Uses standard survey metadata (no specialized tools required)
- **Transparent methodology**: Explicit, tunable thresholds with sensitivity analysis
- **Machine learning validation**: XGBoost classifier achieving 99.5% accuracy
- **Explainable AI**: SHAP analysis for feature importance interpretation

### Performance Highlights

- Successfully filtered 94.8% of submissions (11,400 of 12,020 responses)
- Identified 73% of platform-approved responses as invalid through behavioral analysis
- XGBoost validation: 0.95 F1-score for human class, 0.9971 for bot class

## Repository Structure
```
survey-bot-detection/
│
├── .gitattributes
├── .gitignore
├── README.md                        # Revised (as per commit history)
│
└── bin/                            # Directory containing the notebook
    └── jupyter/
        └── bot_detection_pipeline.ipynb    # Main analysis notebook
```


### Areas for Contribution

- Extensions to additional survey platforms (SurveyMonkey, Google Forms, etc.)
- Alternative machine learning models (Random Forest, Neural Networks)
- Additional visualization methods
- Performance optimizations for large datasets
- Integration with real-time survey monitoring

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Funding

This research was funded by:
- US Department of Transportation via the New England University Transportation Center (Grant Number: 69A35523483)
- Armstrong Fund for Science

## Contact

For questions or inquiries:

- **Mohammed Abdalazeem**: mamohammed@umass.edu
- **Andrew Ruger**: rugerand@grinnell.edu
- **Eleni Christofa**: echristofa@umass.edu
- **Jimi Oke**: jimi@umass.edu


## Related Publications

- [Link to published paper when available]
- [Link to preprint if available]

---

**Keywords**: survey bots, data quality, fraud detection, online surveys, reCAPTCHA, multi-filter pipeline, explainable machine learning, SHAP, XGBoost
```
