
## Approaches
### Compaction
Compaction means throwing away duplicate keys in the log, and keeping only the most recent update for each key.
- This is where we'd keep an append-only log of our database, and remove every key-value pair that is not the latest one (that is, removing the history of each key), thereby reducing the size of the dataset.

![](/assets/images/2022-03-07-20-36-56.png)

### Segment merging
![](/assets/images/2022-03-07-20-44-25.png)