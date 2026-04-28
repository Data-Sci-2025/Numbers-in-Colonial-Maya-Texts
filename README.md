# Numbers-in-Colonial-Maya-Texts

**Author:** Alex LaPrevotte
**Email:** alaprevo@gmail.com
**Date:** 27 April 2026

## Project Overview

This project analyzes the usage of numbers in colonial-era Maya texts. The primary goals are to identify how frequently Spanish numbers, Maya numbers, and numerals were used in colonial-era Maya-language texts; whether they were used interchangeably; and whether numeral classifiers were used the same way in earlier days of Maya-Spanish contact as they are in modern Maya.

### Data Source

The data used as the basis of this analysis come from *Los títulos de ebtún: transcripción, traducción y análisis histórico*, the PhD dissertation of Dr. Julien Machault, which can be found online [here](https://ru.dgb.unam.mx/items/62ecc542-2ca8-46cc-84a3-b055179249b4). Cite as:

Machault, J. (2025). Los títulos de ebtún: transcripción, traducción y análisis histórico [Doctoral dissertation, Universidad Nacional Autónoma de México]. Repositorio Universitario UNAM. https://hdl.handle.net/20.500.14330/TES01000869324

This dissertation contains digitized and analyzed colonial-era Maya and Spanish documents (1561–1869). For this project, a 200-page subset (pp. 301–500) was extracted and processed. The data included in this repository are derived from this subset and used with permission.

## Repository Structure

* **[final_report.md](final_report.md)**
  Final paper, presenting research questions, methodology, results, and interpretation
  
* [presentation_slides.pdf](presentation_slides.pdf)
  Presentation of analysis and findings

* [README.md](README.md)  
  Project overview and guide to repository contents

* [project_plan.md](project_plan.md)  
  Initial project proposal outlining goals and planned methods

* [progress_report.md](progress_report.md)  
  Mid-project report documenting development and preliminary work

* [LICENSE.md](LICENSE.md)  
  License information for this repository
  
* [PERMISSION.md](PERMISSION.md)  
  Documentation confirming permission to use data from Julien Machault’s dissertation.

## Code and Analysis

* [Numbers-in-Colonial-Maya-Texts.qmd](Numbers-in-Colonial-Maya-Texts.qmd)  
  Main Quarto document containing the full data processing and analysis pipeline

* [Numbers-in-Colonial-Maya-Texts.md](Numbers-in-Colonial-Maya-Texts.md)  
  Rendered GitHub Flavored Markdown version of the Quarto document for easy viewing

## Data

* [data/](data/)  
  Contains input files used in the analysis, including:
  * PDF excerpt of the source text
  * Lists of Maya numbers, Spanish numbers, and classifiers used in analysis

## Figures

* [figures/](figures/)  
  Contains visualizations generated during analysis and used in the final report

## Notes

Some data cleaning steps (particularly removal of non-Maya content and formatting inconsistencies) were performed manually due to the complexity of the source PDF. As a result, portions of the pipeline are tailored to this specific dataset and may require adjustment for use with other datasets or sources.
