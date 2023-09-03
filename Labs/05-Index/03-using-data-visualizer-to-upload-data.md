# Uploading Data with the Data Visualizer in Elasticsearch
- The Elastic Stack really excels at continuously streaming data from countless sources into Elasticsearch
- But sometimes, you just want to quickly ingest some data to analyze it ad hoc without having to set up a data processing pipeline for it
- For this use case, we can use the data visualizer

Log in to Kibana
Click the hamburger menu icon in the upper left corner
Click Machine Learning
Click Data Visualizer
Under Import data, click Select file
Either drag and drop the malicious_urls.csv file, or upload it by using the File menu
Observe how it's already started analyzing the data
Click Import
Configure and Import the malicious_urls Dataset
Click the Advanced tab

In the Index settings box, add a comma after 1 and enter a new line of "number of replicas": 0:

{
  "number_of_shards": 1,
  "number_of_replicas": 0
}
For Index name, enter malicious_urls.

Leave Create index pattern checked.

Click Import.

Monitor its progress along the bottom of the screen.

Once it's complete, click View index in Discover.

You should then see a long list of URLs.