## WebScraping

- Get all NFT collections:

    - making a POST request to the API endpoint
    - payload contains a search query for a specific collection of items.
    - extracted data and stored in a list called 'all_collections'
    - created a DataFrame from the list of dictionaries for all nft collections
    
- Get all NFTs from the collection:

    - Define fuction **get_all_nfts()**
    - Iterate while function where hits > 0
    - Create variable nft_keys, where there are only required keys [collectionId, CollectionName, nftId, rarityScore, rank]
    - payload contains a search query for a specific collection of items.
    - extract values from dictionaries
    - define fucntion **dum_csv()**
    - create file csv file collection_assets_batch_1.csv
    - take in a list of rows (nfts) and writes them to a CSV file named "collection_assets_batch_1.csv".
    - code loops through the first 500 items of the 'assetsname' column in a DataFrame named 'collection_df'.
    - create a empty list named [all_nft]
    - returned nfts are appended to a list called all_nfts.
