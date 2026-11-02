# EX-11-Elliptic-Curve-Cryptography-ECC
Implementation of Elliptic Curve Cryptography (ECC) in C using elliptic curve point addition and point multiplication for public and private key generation.

## Name: Joshna.M
## Reference Number: 212225230118

# Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)

# Algorithm:

    1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

    2. Initialization:
        Select an elliptic curve equation ( y^2 = x^3 + ax + b ) with parameters ( a ) and ( b ), along with a large prime ( p ) (defining the finite field).
        Choose a base point ( G ) on the curve, which will be used for generating public keys.

    3. Key Generation:
        Each party selects a private key ( d ) (a random integer).
        Calculate the public key as ( Q = d \times G ) (using elliptic curve point multiplication).

    4. Encryption and Decryption:
        Encryption: The sender uses the recipient’s public key and the base point ( G ) to encode the message.
        Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

    5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

# Program:
```

#include <stdio.h>

#define P 17
#define A 2
#define B 2

typedef struct {
    int x, y;
} Point;

int mod(int n) {
    return (n % P + P) % P;
}

int inverse(int n) {
    for (int i = 1; i < P; i++)
        if (mod(n * i) == 1)
            return i;
    return -1;
}

Point add(Point p, Point q) {
    int m, x, y;

    if (p.x == -1) return q;
    if (q.x == -1) return p;

    if (p.x == q.x && mod(p.y + q.y) == 0)
        return (Point){-1, -1};

    if (p.x == q.x && p.y == q.y)
        m = mod((3 * p.x * p.x + A) * inverse(2 * p.y));
    else
        m = mod((q.y - p.y) * inverse(q.x - p.x));

    x = mod(m * m - p.x - q.x);
    y = mod(m * (p.x - x) - p.y);

    return (Point){x, y};
}

Point multiply(int k, Point g) {
    Point r = {-1, -1};

    while (k > 0) {
        if (k % 2)
            r = add(r, g);
        g = add(g, g);
        k /= 2;
    }
    return r;
}

int main() {
    Point G = {5, 1}, Q;
    int d;

    printf("Elliptic Curve: y^2 = x^3 + 2x + 2 mod 17\n");
    printf("Base Point G = (%d, %d)\n", G.x, G.y);

    printf("Enter private key: ");
    scanf("%d", &d);

    Q = multiply(d, G);

    printf("Private Key: %d\n", d);
    printf("Public Key: (%d, %d)\n", Q.x, Q.y);

    return 0;
}

```

# Output:

<img width="937" height="237" alt="image" src="https://github.com/user-attachments/assets/674d87c5-0a28-4499-a7a4-de3ad9ab6f81" />

# Result: 
The Elliptic Curve Cryptography (ECC) algorithm was successfully implemented in C. The program performed elliptic curve point addition and scalar multiplication to generate a public key from the given private key.
