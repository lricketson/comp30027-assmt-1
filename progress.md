I used to have it so that when I'm classifying a test instance, i would create the crosstab df. Which means if I'm classifying 10,000 test instances, I'm creating 10,000 crosstabs. Instead, I decided to create the crosstab at the start for every possible value of an attribute and just look up that same number every time i needed to.

I just:

- identified the category value most strongly predictive of each class. I.e. the value that makes it most likely for a given instance to be class 0, and the value that makes it most liekly for it to be class 1.
- also listed the top 5 most strongly predictive attribute values for each class, so the values for any category such that knowing them makes it most likely an instance is class 0 or 1.
