 If we had more items in our data source, the number of bins would stay the same, but each bin would have a higher frequency of each item. If we divided the data into more bins, we'd have more bins, but each bin would have a lower frequency of each item. If we took the number of bins and raised it to infinity, we'd have an infinite number of zero-frequency bins. To solve for this, we'll replace our histogram, which is a discrete function, with a discrete density function. A discrete density function differs from a discrete function in that it normalizes the y-axis to a fraction or percentage of the total, i.e its density, instead of a total count for each bin. Converting from a discrete function to a discrete density function is trivial:

Density of Bin i=Number of items in Bin iNumber of items total

Once we have a discrete density function, we can then convert it into a density function by changing our discrete values into continuous values.

Bin Density=(Fraction of trees between height H and H′)(H−H′)

So a density function is a continuous histogram where all of the values are normalized against a total. If we had a specific tree we wanted to know the height of, we could create a probability function that would tell us how likely it is for our tree to fall within a specific bin.

Probability of Bin i=Number of items in Bin iNumber of items total

If we combined our probability function and our (continuous) density function, we could interpret that as a statistical predictor of a tree’s height:

Probability a random tree is between H and H′=Bin Density⋅(H−H′)

Indeed, with this continuous probability function, we can now say the likelihood that any given tree has a height that places it within any arbitrary span of multiple bins. This is a probability density function (henceforth PDF). In short, a PDF is a continuous function that can be integrated over to determine how likely a result is over an integral.

