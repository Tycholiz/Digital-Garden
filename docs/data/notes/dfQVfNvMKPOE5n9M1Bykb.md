
Stands for *Start of Authority*

The SoA record indicates that "this nameserver is the best source of information for the data within this zone".
- therefore, the nameserver that this record exists on is authoritative *because* of the SoA.

There can only be one SoA per zone.

This is the most important address record, and must be specified.
- Contains the authoritative master nameserver for the zone or domain, as well as an admin email that we specify.
- Also contains administrative information about the zone
- *MNAME* - the primary nameserver for the zone
