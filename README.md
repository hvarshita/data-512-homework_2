## DATA 512 - HOMEWORK 2 : Considering bias in Data

The goal of this assignment is to explore the concept of bias in data using Wikipedia articles. This assignment will consider articles about cities in different US states. For this assignment, you will combine a dataset of Wikipedia articles with a dataset of state populations, and use a machine learning service called ORES to estimate the quality of the articles about the cities.
An analysis is performed on how the coverage of US cities on Wikipedia and how the quality of articles about cities varies among states. The analysis consists of a series of tables that show:
* The states with the greatest and least coverage of cities on Wikipedia compared to their population.
* The states with the highest and lowest proportion of high quality articles about cities.
* A ranking of US geographic regions by articles-per-person and proportion of high quality articles.



### Data Sources:

* Data File 1: ["us_cities_by_state_SEPT.2023.csv"](https://drive.google.com/file/d/1XAydF2Cqjr5u1zs-B9p09JVliqtFYv15/view?usp=drive_link)
Description: Contains a list of Wikipedia article pages about US cities categorized by state. This data was generated by crawling the Wikipedia Category: Lists of cities in the United States by state.

* Data File 2: ["State Population Totals and Components of Change: 2020-2022"](https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-total.html)
Description: This Excel file, available on the US Census Bureau website, provides estimated populations of all US states for the year 2022.

* Data File 3: ["Regional Division Spreadsheet"](https://drive.google.com/file/d/1uG6Pj5m3NjBbx9Xkzdtfo_Ewo0zCNK8F/view?usp=drive_link)
Description: This spreadsheet, included in the homework folder, lists the states grouped into regional and divisional agglomerations as defined by the US Census Bureau for the purpose of this analysis.

### API Information:


In the process of obtaining article quality predictions, we have utilized ORES, the Objective Revision Evaluation Service, which relies on machine learning models trained on Wikipedia articles that have undergone peer review using Wikipedia's content assessment procedures. To access ORES for making page quality predictions, we have made use of the API:Info. We referred to sample code snippets provided under the Creative Commons CC-BY license for various tasks, such as making page info requests and ORES requests. Additional resources that have proven to be helpful include the ORES Wiki, Wikipedia's Content Assessment guidelines, and Wikimedia API Documentation. In our Python notebooks, we've demonstrated how to call the Wikipedia API to retrieve article metadata and access ORES's machine learning model for article quality predictions. These resources collectively guide us in the process of evaluating and enhancing the quality of Wikipedia articles.

1. [ORES](https://www.mediawiki.org/wiki/ORES)
2. [Wikimedia]( https://www.mediawiki.org/wiki/API:Info)
3. [Notebook for making Pageinfo request]( https://drive.google.com/file/d/15UoE16s-IccCTOXREjU3xDIz07tlpyrl/view?usp=sharing)
4. [Notebook for making ORES Predictions](https://drive.google.com/file/d/17C9xsmR9U3lJeD52UTbAedlHDetwYsxs/view?usp=sharing)


### **Repository Structure**

The structure of this repository is organized as follows:

- **Input Files:**
  - `NST-EST2022-POP (3).xlsx`: A file containing population estimates for the year 2022.
  - `US States by Region - US Census Bureau - Sheet1.csv`: Provides data about US states categorized by regions as per the US Census Bureau.
  - `us_cities_by_state_SEPT.2023.csv`: Contains data related to US cities by state as of September 2023.

- **Intermediate Data Files:**
  - `title_revid.json`: A file containing JSON data of titles and revision IDs.
  - `wiki_predictions.csv`: CSV file that stores Wikipedia article quality predictions.

- **Output Files:**
  - `wp_scored_city_articles_by_state.csv`: The final output dataset containing scored city articles by state.
  - `wp_areas-no_match.txt`: A text file listing areas with no matches during data merging.

- **Source Code:**
  - The Jupyter Notebook `hcds_a2.ipynb` is used for data acquisition and analysis.

- **License:**
  - A file named `LICENSE` with an MIT License for the repository.

- **README:**
  - The `README.md` file contains instructions for reproducing the analysis, data descriptions, attributions, provenance information, and other relevant details. It also provides insights into the project's goals and learning reflections.

Feel free to navigate through the repository's content for further details on each component.

## Overview 

In the "hcds_a2.ipynb" notebook, the process of making API calls and merging datasets was a fundamental part of the data acquisition and preparation. Initially, the notebook involved making API calls to access Wikipedia data. These calls included the retrieval of information about Wikipedia articles, encompassing their titles and revision IDs, as well as predicting their quality through the ORES model. It is worth noting that the API calls required a significant amount of time, approximately 5-6 hours, and encountered occasional timeouts, which necessitated resuming from the last successfully processed point.

Subsequently, the dataset was aligned with a predefined schema structure through the merging of datasets. This schema aimed to provide a comprehensive dataset with specific fields, including state, regional division, population, article title, revision ID, and article quality. The merging process encompassed the following key steps:

* Data Splitting: The dataset was initially split to segregate article information, separating it into distinct fields for city, state, and other pertinent data. Care was taken to handle instances where state information was absent.

* Region Mapping: To introduce regional information, an external dataset, "US States by Region - US Census Bureau," was imported. The region and division fields were amalgamated into a new field termed "regional_division," which was associated with each state.

* Dataset Merging: The dataset containing article predictions was merged with the regional and state-level information, unifying regional and article data.

* Population Data Integration: To create a comprehensive dataset for per capita analysis, population data from "NST-EST2022.xlsx" was imported. This population data was subsequently merged with the pre-existing dataset.

* Final Data Integration: The notebook concluded by reading the intermediate "city.json" file to acquire the last revision IDs, which were then integrated into the combined dataset.

Following this extensive merging process, the dataset was refined by eliminating redundant fields, renaming specific columns, and reordering the data to ensure clarity and consistency. The final structured dataset was saved as "wp_scored_city_articles_by_state.csv" to conform with the requisite schema structure. This dataset served as the foundation for subsequent data analysis, addressing questions regarding article counts, population, and article quality at both the state and regional levels.






## Reflections 

Before initiating our data analysis, it was reasonable to anticipate certain biases in the dataset. The common assumption was that states with larger populations, such as California or Washington, would naturally produce higher-quality articles due to a larger pool of potential contributors. This preconceived notion was overturned when the analysis demonstrated that states with substantial populations like California, Arizona, Florida, and Massachusetts actually had lower-quality articles per capita. This unexpected outcome underscored the complexity of factors influencing article quality, including linguistic diversity. A prominent bias emerged as we recognized that our dataset exclusively considered English articles, disregarding the non-English-speaking population. Consequently, relying solely on English articles as a data source appeared to introduce a notable bias. Additionally, while working on the dataset and  trying to categorize regions, it became evident that biases might arise based on the choice of regional division conventions.

I believe relying solely on English Wikipedia as a data source introduces a language-based bias, which might be problematic in various data science research scenarios, particularly those involving linguistically diverse regions. 

It is also possible that using these data might create biased or misleading results. Think of a situation where a company is using this data to teach a computer program to help with their marketing. They want the program to figure out what topics are important in different places. But here's the problem: the data is only from English Wikipedia, so it doesn't include stuff that's important to people who speak other languages. This means the program might not understand what really matters to those communities. So, the company's marketing decisions could be way off, leading to wrong choices and mistakes. 

In conclusion, it's essential to recognize the limitations and biases inherent in the data when applying it to data science research. Biased and misleading results can arise when the dataset doesn't fully represent diverse linguistic and cultural aspects. However, in situations where research questions align with the dataset's strengths, it can still be valuable. Careful consideration and appropriate adjustments are crucial to make the most of the dataset's potential while acknowledging its constraints.

