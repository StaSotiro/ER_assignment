# ER_assignment

### Q1: Create the blocks
For this solution we are using SckitLearn's [Vectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html), which allows us to extract features from text like inputs. <br>
In order to do that we are transforming the data to strings, and concatenating it to a new column, later passed for extraction. By default, Vectorizer data is altered to lowerCase, unless instructed otherwise.<br>
The process generates a 2D array, with each row referencing an entity from the original Data passed, and each column having 1 if a respective feature belongs to this entity, 0 otherwise. <br>
In order to get the required results (Blocks of entities with each block referencing a feature), we created a process that iterates through the generated Entity Table and creates a secondary table with each row referencing a Feature, which contains an array of the matching entities' IDs.<br><br>

### Q2: Calculate comparisons

In this example we know the entities' structure. So each comparison would be comparing EntityA FieldA w/ EntityB FieldA, etc.  This let's us assume that a comparison between two entities would cost us 4 field comparisons. 
If we did not know the schema, we would need to to compare all fields between each other. This brings up the number to #NumberOfEntityAKeys x #NumberOfEntityBKeys. E.g. Entity A has 4 fields and Entity B has 5 -> A comparison would be 4x5=20.

### Q3: Meta Building Graph
In order to build a graph, we are creating a series of edges in the form of 
[NodeFrom, NodeTo, Weight] <br>

A target Node is a node that exists in the same block as the original Node <br>
In order to build this graph we will:<br>
* Iterate over the dictionary we built earlier
* For each of its elements add edges and weights between the Entities (0 if it's the first occurence, +1 if it already exists)
<!-- end of the list -->
This allows us to create a graph faster than a double for loop<br>
There is one issue here: When in one block contains nodes 123 and 321 and another one contains 321 and 123 then with this implementation there will be an arc created for 123 -> 321 and when the second block is checked, the new arc 321->123 will be created, instead of updating the first one. <br>
How do we easily solve this? - By sorting the blocks' arrays / Thankfully, since the package worked iteratively and the initial Entities were sorted, the blocks had sorted values appended. <br>
Unfortunately, the process took serious amount of time, and not even finished. I did however implement the following steps (pruning & Comparison) theoretically. 


### Q4: Jaccard Similarity

To calculate the Jaccard Similarity between two strings, we split the strings on the space(_) character and then calculated the interchange/union metric.
