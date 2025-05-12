# Mathematical Algorithms

This section covers essential mathematical algorithms frequently used in competitive programming and software engineering.

## Number Theory

### GCD and LCM

```cpp
// Greatest Common Divisor using Euclidean algorithm
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}

// Least Common Multiple
int lcm(int a, int b) {
    return (a / gcd(a, b)) * b;  // Avoid overflow: a * b / gcd(a, b)
}
```

### Prime Numbers

#### Sieve of Eratosthenes

```cpp
vector<bool> sieveOfEratosthenes(int n) {
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    
    for (int i = 2; i * i <= n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i) {
                isPrime[j] = false;
            }
        }
    }
    
    return isPrime;
}

// Get all primes up to n
vector<int> getPrimes(int n) {
    vector<bool> isPrime = sieveOfEratosthenes(n);
    vector<int> primes;
    
    for (int i = 2; i <= n; i++) {
        if (isPrime[i]) {
            primes.push_back(i);
        }
    }
    
    return primes;
}
```

Time Complexity: O(n log log n)

#### Primality Test

```cpp
bool isPrime(int n) {
    if (n <= 1) return false;
    if (n <= 3) return true;
    if (n % 2 == 0 || n % 3 == 0) return false;
    
    for (int i = 5; i * i <= n; i += 6) {
        if (n % i == 0 || n % (i + 2) == 0) {
            return false;
        }
    }
    
    return true;
}
```

Time Complexity: O(√n)

#### Prime Factorization

```cpp
vector<int> primeFactorization(int n) {
    vector<int> factors;
    
    // Handle 2 separately
    while (n % 2 == 0) {
        factors.push_back(2);
        n /= 2;
    }
    
    // Check odd factors
    for (int i = 3; i * i <= n; i += 2) {
        while (n % i == 0) {
            factors.push_back(i);
            n /= i;
        }
    }
    
    // If n is a prime number greater than 2
    if (n > 2) {
        factors.push_back(n);
    }
    
    return factors;
}
```

Time Complexity: O(√n)

### Modular Arithmetic

```cpp
// Addition under modulo
int modAdd(int a, int b, int mod) {
    return (a % mod + b % mod) % mod;
}

// Subtraction under modulo
int modSub(int a, int b, int mod) {
    return ((a % mod - b % mod) % mod + mod) % mod;  // Handle negative result
}

// Multiplication under modulo
int modMul(int a, int b, int mod) {
    return ((long long)a % mod * b % mod) % mod;
}

// Modular exponentiation: a^b % mod
int modPow(int a, int b, int mod) {
    if (b == 0) return 1;
    
    long long res = modPow(a, b / 2, mod);
    res = (res * res) % mod;
    
    return (b % 2 == 0) ? res : (res * a) % mod;
}

// Modular inverse using Fermat's Little Theorem (works when mod is prime)
int modInverse(int a, int mod) {
    return modPow(a, mod - 2, mod);
}
```

### Combinatorics

#### Factorial

```cpp
// Calculate n! (factorial)
long long factorial(int n) {
    long long result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}

// Factorial with modulo
long long modFactorial(int n, int mod) {
    long long result = 1;
    for (int i = 2; i <= n; i++) {
        result = (result * i) % mod;
    }
    return result;
}
```

#### Binomial Coefficients (nCr)

```cpp
// nCr = n! / (r! * (n-r)!)
long long binomialCoefficient(int n, int r) {
    if (r > n - r) r = n - r;  // nCr = nC(n-r)
    
    long long result = 1;
    for (int i = 0; i < r; i++) {
        result *= (n - i);
        result /= (i + 1);
    }
    
    return result;
}

// Binomial coefficient with modulo
long long modBinomialCoefficient(int n, int r, int mod) {
    if (r > n - r) r = n - r;
    
    long long result = 1;
    for (int i = 0; i < r; i++) {
        result = (result * (n - i)) % mod;
        result = (result * modInverse(i + 1, mod)) % mod;
    }
    
    return result;
}
```

#### Permutations (nPr)

```cpp
// nPr = n! / (n-r)!
long long permutation(int n, int r) {
    long long result = 1;
    for (int i = n - r + 1; i <= n; i++) {
        result *= i;
    }
    return result;
}
```

## Matrix Operations

### Matrix Multiplication

```cpp
vector<vector<int>> matrixMultiply(const vector<vector<int>>& A, const vector<vector<int>>& B) {
    int n = A.size();
    int m = A[0].size();
    int p = B[0].size();
    
    vector<vector<int>> C(n, vector<int>(p, 0));
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < p; j++) {
            for (int k = 0; k < m; k++) {
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }
    
    return C;
}
```

Time Complexity: O(n³)

### Matrix Exponentiation

```cpp
vector<vector<int>> matrixPow(vector<vector<int>> A, int power) {
    int n = A.size();
    
    // Initialize result to identity matrix
    vector<vector<int>> result(n, vector<int>(n, 0));
    for (int i = 0; i < n; i++) {
        result[i][i] = 1;
    }
    
    while (power > 0) {
        if (power & 1) {
            result = matrixMultiply(result, A);
        }
        A = matrixMultiply(A, A);
        power >>= 1;
    }
    
    return result;
}
```

Time Complexity: O(n³ log power)

## Geometric Algorithms

### Distance between Points

```cpp
double distance(double x1, double y1, double x2, double y2) {
    return sqrt(pow(x2 - x1, 2) + pow(y2 - y1, 2));
}
```

### Area of Triangle

```cpp
// Using coordinates
double triangleArea(double x1, double y1, double x2, double y2, double x3, double y3) {
    return 0.5 * abs(x1 * (y2 - y3) + x2 * (y3 - y1) + x3 * (y1 - y2));
}

// Using sides (Heron's formula)
double triangleArea(double a, double b, double c) {
    double s = (a + b + c) / 2;
    return sqrt(s * (s - a) * (s - b) * (s - c));
}
```