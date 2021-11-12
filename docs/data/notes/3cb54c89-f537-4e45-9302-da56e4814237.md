
A controller watches the API server for new events. When it detects that there is a new object (eg. a new ReplicaSet), it acts in accordance with the type of controller it is.
- ex. In the case of a ReplicaSet, the controller would create pods equal to the number found in the replica-set yaml file
