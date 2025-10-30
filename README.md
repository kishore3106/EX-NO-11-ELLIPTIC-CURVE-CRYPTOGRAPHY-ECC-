# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## NAME: KISHORE B
## Reg.NO: 212224100032

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:
```
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int x;
    int y;
    int is_infinity;
} Point;

const int a = 2;
const int b = 3;
const int p = 17;

int mod(int value, int m) {
    int r = value % m;
    return (r < 0) ? r + m : r;
}

int mod_inverse(int k, int m) {
    k = mod(k, m);
    for (int x = 1; x < m; x++) {
        if ((k * x) % m == 1) return x;
    }
    return -1;
}

Point point_addition(Point P, Point Q) {
    Point R;
    R.is_infinity = 0;

    if (P.is_infinity) return Q;
    if (Q.is_infinity) return P;

    if (P.x == Q.x && mod(P.y + Q.y, p) == 0) {
        R.is_infinity = 1;
        return R;
    }

    int lambda;
    if (P.x == Q.x && P.y == Q.y) {
        int denom = mod_inverse(mod(2 * P.y, p), p);
        if (denom == -1) {
            R.is_infinity = 1;
            return R;
        }
        lambda = mod((3 * P.x * P.x + a) * denom, p);
    } else {
        int denom = mod_inverse(mod(Q.x - P.x, p), p);
        if (denom == -1) {
            R.is_infinity = 1;
            return R;
        }
        lambda = mod((Q.y - P.y) * denom, p);
    }

    R.x = mod(lambda * lambda - P.x - Q.x, p);
    R.y = mod(lambda * (P.x - R.x) - P.y, p);
    return R;
}

Point scalar_multiplication(Point P, int n) {
    Point result;
    result.is_infinity = 1;
    Point addend = P;

    if (n == 0) return result;
    if (n < 0) {
        result.is_infinity = 1;
        return result;
    }

    while (n > 0) {
        if (n & 1) result = point_addition(result, addend);
        addend = point_addition(addend, addend);
        n >>= 1;
    }
    return result;
}

int main() {
    Point G = {5, 1, 0};
    int n = 7;
    printf("Base point G: (%d, %d)\n", G.x, G.y);
    Point R = scalar_multiplication(G, n);
    if (R.is_infinity) {
        printf("Result of %d * G: Point at Infinity\n", n);
    } else {
        printf("Result of %d * G: (%d, %d)\n", n, R.x, R.y);
    }
    return 0;
}

```


## Output:


<img width="821" height="233" alt="image" src="https://github.com/user-attachments/assets/fe40a05c-2481-4781-9b7b-5fe19741ab1a" />


## Result:
The program is executed successfully

