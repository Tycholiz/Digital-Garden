
A greedy algorithm always takes the best/cheapest option available at that moment, even if that might not be the best way to go from an overall point of view.

A greedy algorithm is one that picks the best option at the moment, even if doing so means missing better choices later on.

A greedy algorithm is an algorithm that tries to maximize its gain each step of the way, instead of seeking out the overall best solution.

The opposite of a greedy algorithm is a dynamic algorithm:
- The *greedy algorithm* would say "I need to make my way from point A to point Z, so at every step in-between I look for the adjacent vertex closest to Z and move there." It's easy to program and will probably get the job done but it doesn't guarantee that you've found the shortest path.
- *Dynamic algorithm* says "I go to every node and make a note of the distance to each adjacent node and see if it's shorter than any previously known shortest path to that node. Once I've figured out the shortest path to every node then I can easily lookup the shortest path from A to Z." So, for example we see that A-B-C-D-E gets us to E in five steps, and we know that A-F gets us to F in one step, and then we see that E connects to F so we update our records to say the shortest path from A to E is A-F-E instead of A-B-C-D-E. Continue doing this until you have found the shortest paths from any node to any other node.

### Example: Packing boxes in vans
if your algorithm was about packing differently sized boxes into a van, the algorithm would always choose to put the biggest box that could fit in the van— even if we could achieve a higher cumulative volume by using a combination of smaller boxes.

### Example: Highest altitude
Say you're dropped off in the the woods and want to find the highest point in the area so you can have a look around. A greedy algorithm would be to look at your immediate surroundings and pick the direction that has the highest slope, then travel that direction for a bit and repeat. This will usually get you to the top of a hill, but you can't be sure it's the highest hill. Mount Everest could be on the other side of the ravine you started next to, but you'll never find it because your algorithm never chooses a direction that descends.
