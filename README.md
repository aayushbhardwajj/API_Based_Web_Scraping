# NFT Scraper

```python
A Jupyter Notebooks that has two scrpit:
	1) collection_asset_.ipynb
		a) Scrpit for NFT collections
		b) Scrpit for Assets of NFT collections
```

## Disclaimer

NOTE: APIs aren't always reliable. Websites have control over what information they provide through their APIs, and they can change or revoke access at any time. To avoid data gathering problems, consider using web scraping instead.

[More details here](https://blog.diffbot.com/why-dont-all-websites-have-an-api-and-what-can-you-do-about-it/)

## Overview

### Collecting data of all NFT collections

Overview: This script scrapes NFT collections data by making a POST request to the API endpoint. Using the specific payload to search on the web page.

### to begin

Prerequisites: Python installed and established a virtual environment with required package.

1. To learn more about Rest API [click here](https://realpython.com/api-integration-in-python/#rest-and-python-consuming-apis).
2. Making a POST request to the API endpoint.
    
    ```python
    api_url = 'https://search2.raritysniper.com/multi_search?use_cache=true&x-typesense-api-key=KEY'
    
    response = requests.post(api_url, data=json.dumps(q), headers=headers)
    ```
    
3. Search query for a specific collection of items.
    
    ```python
    q ={
        "searches": [
            {
                "query_by": "name",
                "sort_by": "launchDate:desc",
                "highlight_full_fields": "name",
                "collection": "collections",
                "q": "*",
                "facet_by": "blockchain,oneDayVolume,supply,thirtyDayVolume,totalVolume,sevenDayVolume,floorPrice",
                "max_facet_values": 20,
                "page": 1,
                "per_page": 250
            }
        ]
    }
    ```
    
4. Created DataFrame
    
    ```python
    collection_df = pd.DataFrame(all_collections)
    collection_df.to_csv('all_collections.csv', index=False)
    ```
    
5. Added columns.
    
    ```python
    Add 'https://raritysniper.com/' before collection_name in 'collectionSlug' column
    Add 'assets_' before collection_name in 'collectionSlug' column
    ```
    
6. Run "All Cells" on .ipynb

### Collecting data of all Assets of NFT collections

Overview: This script scrapes assets of NFT collections data by making a POST request to the API endpoint. Using the specific payload to search on the web page.

IMPORTANT NOTE: The Scrpit was running parallel to reduce the time to get the data.

Few changes in the scrpit:

- Name of .csv file is getting change
    
    ```python
    file_exists = os.path.isfile('collection_assets_batch_1.csv')
    file_exists = os.path.isfile('collection_assets_batch_2.csv')
    file_exists = os.path.isfile('collection_assets_batch_3.csv')
    file_exists = os.path.isfile('collection_assets_batch_4.csv')
    ```
    
- created a loop by setting a condition for every .csv file.
    
    ```python
    1) 'collection_assets_batch_1.csv'
    				a) if idx > 500:
    							  break
    2) 'collection_assets_batch_2.csv'
    				a) if idx < 500:
    				        continue
    					 if idx > 1000:
    				        break
    3) 'collection_assets_batch_3.csv'
    				a) if idx < 1000:
    				        continue
    					 if idx > 1500:
    				        break
    4) 'collection_assets_batch_4.csv'
    				a) if idx < 1500:
    				        continue
    ```
    

Prerequisites: create file csv file [collection_assets_batch_(i).csv]. Take in a list of rows (nfts) and writes them to a CSV file named "collection_assets_batch_(i).csv".

1. Define function to get all nfts from each collections.
    
    ```python
    get_all_nfts(idx, name)
    ```
    
2. Payload contains a search query for a specific collection of items.
    
    ```python
    payload = {
            "searches": [
                {
                    "sort_by": "rank:asc,nftId:asc",
                    "collection": name,
                    "q": "*",
                    "max_facet_values": 83,
                    "page": page,
                    "per_page": 250
                }
            ]
        }
    ```
    
3. Condition for the loop.
    
    ```python
    r = requests.post(api_url, json=payload, headers=headers)
    while len(r.json()['results'][0]['hits']) > 0:
    			nfts = r.json()['results'][0]['hits']
    ```
    
4. Create variable nft_keys, where there are only required keys.
    
    ```python
    nft_key = nft['document']['collectionId'],
    					nft['document']['collectionName'],
    					nft['document']['nftId'],
    					nft['document']['rarityScore'],
    					nft['document']['rank']
    ```
    
5. Define function to dump csv.
    
    ```python
    dump_csv(nfts)
    ```
    
6. Run the cell to get the output.