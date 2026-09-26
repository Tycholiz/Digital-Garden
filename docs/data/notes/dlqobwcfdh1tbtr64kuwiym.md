
### Archive
An archive is any object, such as a photo, video, or document, that you store in a vault. It is a base unit of storage in Amazon S3 Glacier
- Each archive has a unique ID

### Inventory
An inventory is a detailed list of the archives stored in a glacier vault.
- each vault can have multiple inventories, and an inventory is essentially a point-in-time list of the archives within a specific vault.

after you upload your first archive to your vault, glacier automatically creates a vault inventory and then updates it approximately once a day

### Vault
A vault is a container for storing archives

### deleting vault
[guide](https://gist.github.com/veuncent/ac21ae8131f24d3971a621fac0d95be5)