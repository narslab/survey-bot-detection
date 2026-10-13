# Multi-filter bot detection for incentivized online surveys

Code for the paper

> Ruger, A., Abdalazeem, M., Christofa, E., & Oke, J. (2026). **Explainable Data-Driven Multi-filter Framework for Bot Detection in Incentivized Online Surveys.** *Data Science for Transportation*, 8, 21. https://doi.org/10.1007/s42421-026-00158-4

Free full-text (view only): https://rdcu.be/fn7TW

Paying people to take an online survey attracts bots. In the study behind this repository, a survey on cycling preferences received 12,020 responses in 94 days, 3,951 of them on a single day. The pipeline in this repository removed 94.8% of them, leaving 620 responses. The survey platform's own checks (reCAPTCHA, fraud score, duplicate flags) would have kept 2,379 responses; 1,759 of those (73%) were still removed by checks on how the survey was actually filled in and on what device.

## What the pipeline does

Eleven filters are applied in sequence. A response is removed if it fails any one of them. Each filter uses metadata that survey platforms record automatically, so no extra instrumentation is needed.

| Category | Filter | Removes responses that |
|---|---|---|
| Platform risk signals | Low reCAPTCHA score | score below 0.6 |
| | High fraud score | platform fraud score above 30 |
| | Response ambiguity | the platform flagged for filler or nonsensical free text |
| Identity | Duplicate identity | the platform flagged as duplicates, or that share an IP address *and* identical browser, version, OS, and screen resolution |
| Interaction patterns | Zero clicks | recorded no clicks in the whole session |
| | Duplicate temporal pattern | have page-advance timing identical to another session, or paused only on the first page |
| | Extreme page duration | fall outside the 5th to 95th percentile of time on any page (lower bound floored at 15 s, except the reCAPTCHA page) |
| | Extreme survey duration | fall outside the 5th to 95th percentile of total completion time |
| Device consistency | Invalid resolution | report an unparsable, zero, or implausible screen size for the device type |
| | Software incompatibility | report browser, OS, and device combinations that cannot occur (Safari on Android, a desktop running iOS) |
| | User agent inconsistency | have a missing browser version, or a viewport width inconsistent with the device type |

Results on the study data (12,020 responses):

| Stage | Removed | Remaining |
|---|---:|---:|
| Low reCAPTCHA score | 6,082 | 5,938 |
| High fraud score | 2,658 | 3,280 |
| Response ambiguity | 42 | 3,238 |
| Duplicate identity | 859 | 2,379 |
| Zero clicks | 568 | 1,811 |
| Duplicate temporal pattern | 46 | 1,765 |
| Extreme page duration | 1,056 | 709 |
| Extreme survey duration | 72 | 637 |
| Invalid resolution | 3 | 634 |
| Software incompatibility | 12 | 622 |
| User agent inconsistency | 2 | 620 |

The paper also reports a blind expert review of 100 responses (88.9% agreement with the pipeline on the 90 responses the reviewer could label) and an XGBoost classifier trained to recover the pipeline's decisions (F1 of 0.94 for the human class). Those validation steps are not part of this repository.

## Repository contents

```
notebooks/bot_detection_pipeline.ipynb   the pipeline, with outputs from the study data
examples/synthetic_survey_export.csv     a small made-up export in Qualtrics format, for trying the notebook
requirements.txt
LICENSE
```

## Running the notebook

```bash
git clone https://github.com/narslab/survey-bot-detection.git
cd survey-bot-detection
pip install -r requirements.txt
jupyter lab notebooks/bot_detection_pipeline.ipynb
```

Run all cells. By default the notebook reads `examples/synthetic_survey_export.csv` and writes two files to `outputs/`: the responses that passed every filter, and a summary table of what each filter removed.

To run it on your own survey:

1. Export your responses as CSV with the embedded metadata fields. In Qualtrics these are the *RelevantID* and *reCAPTCHA* fields, the *Response Quality* flags, a *Timing* question on each page, and the *Browser Meta Info* question.
2. Set `DATA_PATH` in the configuration cell to your file, or set the environment variable `SURVEY_EXPORT` before starting Jupyter.
3. If your column names differ from the Qualtrics defaults, edit the `COLUMNS` dictionary. A filter whose columns are not in the export is skipped and reported, so the notebook also works on partial exports.
4. Thresholds are in the `THRESHOLDS` dictionary. The defaults are the values used in the paper; the paper's sensitivity and ablation analyses discuss how the results change with them.

## Data

The survey data analyzed in the paper are not public, because of privacy restrictions covering human participants (University of Massachusetts Amherst IRB protocol 5917). The notebook's saved outputs come from that data, but no individual response is displayed. The example file in `examples/` is synthetic and contains no real responses.

## Citation

```bibtex
@article{ruger2026bot,
  title   = {Explainable Data-Driven Multi-filter Framework for Bot Detection in Incentivized Online Surveys},
  author  = {Ruger, Andrew and Abdalazeem, Mohammed and Christofa, Eleni and Oke, Jimi},
  journal = {Data Science for Transportation},
  volume  = {8},
  number  = {21},
  year    = {2026},
  doi     = {10.1007/s42421-026-00158-4}
}
```

Andrew Ruger and Mohammed Abdalazeem contributed equally to this work.

## Funding

US Department of Transportation through the New England University Transportation Center (grant 69A35523483), and the Armstrong Fund for Science.

## Contact

Mohammed Abdalazeem, University of Massachusetts Amherst, mamohammed@umass.edu

## License

MIT. See [LICENSE](LICENSE).
