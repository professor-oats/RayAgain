A Simple Monte Carlo Program

Let’s start with one of the simplest Monte Carlo programs. If you're not familiar with Monte Carlo programs, then it'll be good to pause and catch you up. There are two kinds of randomized algorithms: Monte Carlo and Las Vegas. Randomized algorithms can be found everywhere in computer graphics, so getting a decent foundation isn't a bad idea. A randomized algorithm uses some amount of randomness in its computation. A Las Vegas random algorithm always produces the correct result, whereas a Monte Carlo algorithm may produce a correct result—and frequently gets it wrong! But for especially complicated problems such as ray tracing, we may not place as huge a priority on being perfectly exact as on getting an answer in a reasonable amount of time. Las Vegas algorithms will eventually arrive at the correct result, but we can't make too many guarantees on how long it will take to get there. The classic example of a Las Vegas algorithm is the quicksort sorting algorithm. The quicksort algorithm will always complete with a fully sorted list, but, the time it takes to complete is random. Another good example of a Las Vegas algorithm is the code that we use to pick a random point in a unit disk:

inline vec3 random_in_unit_disk() {
    while (true) {
        auto p = vec3(random_double(-1,1), random_double(-1,1), 0);
        if (p.length_squared() < 1)
            return p;
    }
}

Listing 1: [vec3.h] A Las Vegas algorithm

This code will always eventually arrive at a random point in the unit disk, but we can't say beforehand how long it'll take. It may take only 1 iteration, it may take 2, 3, 4, or even longer. Whereas, a Monte Carlo program will give a statistical estimate of an answer, and this estimate will get more and more accurate the longer you run it. Which means that at a certain point, we can just decide that the answer is accurate enough and call it quits. This basic characteristic of simple programs producing noisy but ever-better answers is what Monte Carlo is all about, and is especially good for applications like graphics where great accuracy is not needed. 

The canonical example of a Monte Carlo algorithm is estimating π, so let's do that. There are many ways to estimate π, with Buffon's needle problem being a classic case study. In Buffon's needle problem, one is presented with a floor made of parallel strips of floor board, each of the same width. If a needle is randomly dropped onto the floor, what is the probability that the needle will lie across two boards? (You can find more information on this problem with a simple Internet search.)

We’ll do a variation inspired by this method. Suppose you have a circle inscribed inside a square:

We get very quickly near π, and then more slowly zero in on it. This is an example of the Law of Diminishing Returns, where each sample helps less than the last. This is the worst part of Monte Carlo. We can mitigate this diminishing return by stratifying the samples (often called jittering), where instead of taking random samples, we take a grid and take one sample within each: 

This changes the sample generation, but we need to know how many samples we are taking in advance because we need to know the grid. Let’s take a million and try it both ways: 

 Interestingly, the stratified method is not only better, it converges with a better asymptotic rate! Unfortunately, this advantage decreases with the dimension of the problem (so for example, with the 3D sphere volume version the gap would be less). This is called the Curse of Dimensionality. Ray tracing is a very high-dimensional algorithm, where each reflection adds two new dimensions: ϕo and θo. We won't be stratifying the output reflection angle in this book, simply because it is a little bit too complicated to cover, but there is a lot of interesting research currently happening in this space.

As an intermediary, we'll be stratifying the locations of the sampling positions around each pixel location. Let's start with our familiar Cornell box scene. 
