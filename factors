#include <stdio.h>
#include <gmp.h>
#include <string.h>

void gnfs(mpz_t n, mpz_t p, mpz_t q) {
    mpz_t x, y, d, t, c, a, b, g;
    mpz_inits(x, y, d, t, c, a, b, g, NULL);
    mpz_set_ui(x, 2);
    mpz_set_ui(y, 2);
    mpz_set_ui(c, 1);
    mpz_set(d, n);
    int k = 1;
    while (mpz_cmp_ui(d, 1) != 0) {
        mpz_set(x, y);
        for (int i = 1; i <= k; i++) {
            mpz_mul(y, y, y);
            mpz_add(y, y, c);
            mpz_mod(y, y, n);
        }
        mpz_set(t, d);
        int m = 0;
        while (mpz_cmp_ui(t, 1) != 0) {
            mpz_set(a, x);
            for (int i = 1; i <= m && mpz_cmp(t, d) <= 0; i++) {
                mpz_mul(a, a, a);
                mpz_add(a, a, c);
                mpz_mod(a, a, n);
                mpz_sub(t, a, x);
                mpz_gcd(t, t, n);
            }
            m = k;
            k *= 2;
        }
        mpz_gcd(g, d, t);
        mpz_set(d, g);
    }
    mpz_div(p, n, d);
    mpz_set(q, d);
    mpz_clears(x, y, d, t, c, a, b, g, NULL);
}

int main(int argc, char *argv[]) {
    if (argc != 2) {
        printf("Usage: %s <file>\n", argv[0]);
        return 1;
    }
    FILE *file = fopen(argv[1], "r");
    if (file == NULL) {
        printf("Error opening file\n");
        return 1;
    }
    char buffer[1024];
    mpz_t n, p, q;
    mpz_inits(n, p, q, NULL);
    while (fgets(buffer, sizeof(buffer), file) != NULL) {
        mpz_set_str(n, buffer, 10);
        gnfs(n, p, q);
        gmp_printf("%Zd=%Zd*%Zd\n", n, p, q);
    }
    mpz_clears(n, p, q, NULL);
    fclose(file);
    return 0;
}
