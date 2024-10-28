# Naijaweb Fabric Data Engineering Project
Loading dataset from huggingface into Microsoft Fabric using dataset created by Saheed Azeez as a case study.
 @dataset{naijaweb_2024,
  author    = {Saheed Azeez},
  title     = {Naijaweb: A Web Scraped Nigerian Context Dataset},
  year      = {2024},
  publisher = {Hugging Face Datasets},
  version   = {1.0.0},
  url       = {https://huggingface.co/datasets/saheedniyi/naijaweb},
} 

## Requirements
1. Microsoft Fabric Trail or Prolicense-https://app.fabric.microsoft.com/singleSignOn?ru=https%3A%2F%2Fapp.fabric.microsoft.com%2F%3FnoSignUpCheck%3D1
2. Hugging Face Account-https://huggingface.co/join

## Execution Steps
1. Sign into your microsoft Fabric account
2. Create a workspace with fabric capacity enabled (You can create a new workspace if you don't have a fabric dedicated workspace already)
3. Sign up for a free hugging face account
4. Create an access token
5. Add a new Notebook under Items
6. Add a new lakehouse to the notebook for the sake of the demo I called mine Naijaweb
7. Install hugging face library with the code pip install huggingface_hub
8. Login to hugging face with the code - from huggingface_hub import login login() and paste your token.
9. Run the following code to load the dataset from hugging face in a new cell - from datasets import load_dataset dataset = load_dataset("saheedniyi/naijaweb")
10. import pandas as pd # Convert to Pandas DataFrame (assuming the dataset has a 'train' split) df = dataset["train"].to_pandas()
11. Save as CSV or Parquet (Parquet is generally more efficient for larger datasets) df.to_parquet("naijaweb_data.parquet")
12. Display dataframe to be show of your output display(df)
13. Save to the default Files section of your Lakehouse df.to_parquet("/lakehouse/default/Files/naijaweb_data.parquet")
14. parquet_path = "Files/naijaweb_data.parquet"
15. Load the Parquet file from the Lakehouse Files section df_lakehouse = spark.read.parquet(parquet_path)
16. Replace 'naijaweb_data_table' with your desired table name df_lakehouse.write.format("delta").saveAsTable("naijaweb_data_table")
17. Once you have done that you can run your sqlendpoint to query the data
18. A simple query would be to see based on section the count of text that comes from each section Select Count(text) As Count, section
From naijaweb_data_table
GROUP BY section
ORDER BY Count(text) DESC 
