I used to have it so that when I'm classifying a test instance, i would create the crosstab df. Which means if I'm classifying 10,000 test instances, I'm creating 10,000 crosstabs. Instead, I decided to create the crosstab at the start for every possible value of an attribute and just look up that same number every time i needed to.

I just:

- identified the category value most strongly predictive of each class. I.e. the value that makes it most likely for a given instance to be class 0, and the value that makes it most liekly for it to be class 1.
- also listed the top 5 most strongly predictive attribute values for each class, so the values for any category such that knowing them makes it most likely an instance is class 0 or 1.

The EM works like this:

- i initialise the model by training it on the supervised train csv. It has 15k rows. For continuous variables I get baseline means and std devs, and for categorical variables I get baseline probabilities of an instance being class c given its attribute x has value v.
- I then apply those parameters to the unlabelled dataset, and get probabilities of each instance being of each class c.
- I then integrate that dataset back into the training process, treating the probabilities as expected values and including them in calculations for priors and likelihoods. E.g. we use a new mean and std dev for calculating likelihoods for continuous vars, and new (fractional) counts for calculating likelihoods for categorical vars.
- then reapply the new parameters to the unlabelled dataset, get new probabilities of each instance being of class c
- rinse and repeat until the probabilities converge, and then classify based on the probabilities
- The likelihood of seeing an instance with attributes xi = vi for all attributes changes per iteration, because mean/std are changing, and also since class counts changes per iteration since we're recalculating expected class counts, categorical attribute probabilities change too.

Log likelihood is this: it's the sum over all instances of the logs of the probability that the class is 0 + the probability that its attributes x1 = v1 given class is 0, ... xn = vn given class is 0, + the probability that the class is 1 + the probability that its attributes x1 = v1 given class is 1, ... xn = vn given class is 1
