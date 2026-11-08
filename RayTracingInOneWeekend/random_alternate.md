C++ did not traditionally have a standard random number generator, but newer versions of C++ have addressed this issue with the <random> header (if imperfectly according to some experts). If you want to use this, you can obtain a random number with the conditions we need as follows:

...

#include <random>

...

inline double random_double() {
    static std::uniform_real_distribution<double> distribution(0.0, 1.0);
    static std::mt19937 generator;
    return distribution(generator);
}

inline double random_double(double min, double max) {
    // Returns a random real in [min,max).
    return min + (max-min)*random_double();
}

...

Listing 42: [rtweekend.h] random_double(), alternate implementation

