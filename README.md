# StreetnameLinks

This project is dedicated to linking street names to the person they are named after.
The street information is taken from [OpenStreetMap](https://www.openstreetmap.org/) and linked to person data that was generated using [Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page) (as at May 17th 2022).

Our method was tested on streets from the city of Bremen and the state of North Rhine-Westfalia in Germany. It was able to detect _896_ new person links in Bremen and _31.363_ new links in North Rhine-Westfalia respectively. Overall, we found _183.022_ possible street-person links in all of Germany.

## Approach
In an effort to correctly predict the namesake for a given street, we extracted street data from OpenStreetMap and generated possible candidates for each street.
The candidates are taken from an index database that was created by extracting data from a [Wikidata dump](https://dumps.wikimedia.org/wikidatawiki/). For each candidate we further get properties such as occupations, birth place, place of death etc. The candidates are than classified by a machine learning algorithm, picking the candidate with the highest score above a certain threshold as the final result for each street.

## Results
The machine learning algorithm was trained on known streets in Wikidata that had the property "named after". On the test data, it achieved a precision of 95,2% with a recall of 90,6%. When applied on streets in OSM, our method was able to detect _896_ new person links in Bremen and _31.363_ new links in North Rhine-Westfalia respectively.
You can take a look at the findings of this project in the following two files:
```
best_candidates_bremen.csv
best_candidates_nrw.csv
```
## Dependencies
- Python >= 3.8
- [Pandas](https://pandas.pydata.org/)
- [Geopandas](https://geopandas.org/en/stable/)
- [Pickledb](https://pypi.org/project/pickleDB/)
- [Scikit-Learn](https://pypi.org/project/scikit-learn/)
- [qwikidata](https://pypi.org/project/qwikidata/)

## Usage
To train the ML model and evaluate it, simply run the following command:
```
ml/ml_training.py
```
If you want to calculate the results of the approach on OSM, you can generate a file with all candidates for streets in Bremen or North Rhine-Westfalia using the following command. Alternatively, you can use your own file from a different region.
```
street_candidates.py [ bremen | nrw | file ]
```
You can then predict the most likely person from the aforementioned candidates for each street.
```
predict.py [bremen | nrw | file]
```
The code will return a csv-file containing the candidate with the highest score above a certain threshold for each street.
