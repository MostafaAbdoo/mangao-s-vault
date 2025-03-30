```cpp
struct str {
    ll gcd(ll a, ll b) {
        if (b == 0)
            return a;
        return gcd(b, a % b);
    }
    ll lcm(int a, int b) {
        return a / gcd(a, b) * b;
    }
    bool prime[N];
    void Sieve(ll n) {
        memset(prime, true, sizeof(prime));
        prime[0] = 0;
        prime[1] = 0;
        for (ll p = 2; p * p <= n; p++) {
            if (prime[p]) {
                for (ll i = p * p; i <= n; i += p) {
                    prime[i] = false;
                }
            }
        }
    }
    ll spf[N];
    void Spf() {
        for (ll i = 1; i < N; i++) {
            spf[i] = i;
        }
        for (ll i = 2; i * i < N; i++) {
            if (spf[i] == i) {
                for (ll k = i * i; k < N; k += i) {
                    if (spf[k] == k) {
                        spf[k] = min(i, spf[k]);
                    }
                }
            }
        }
    }
    ll multi(ll a, ll b) { return ((a % MOD) * (b % MOD)) % MOD; }
    ll subt(ll a, ll b) { return ((a % MOD) - (b % MOD) + MOD) % MOD; }
    ll add(ll a, ll b) { return ((a % MOD) + (b % MOD)) % MOD; }
    ll fp(ll b, ll exp) {
        if (exp == 0) return 1;
        ll res = 1;
        while (exp) {
            if (exp % 2 == 1)
                res = multi(res, b);
            b = multi(b, b), exp /= 2;
        }
        return res;
    }
    ll INV(ll a) { return fp(a, MOD - 2); }
    ll divi(ll a, ll b) {
        return multi(a, INV(b));
    }
    void calcFactorial() {
        spf[0] = 1;
        for (ll i = 1; i < 200005; i++)
            spf[i] = multi(spf[i - 1], i);
    }
    ll nCr(ll n, ll r) {
        if (r > n || r < 0 || n < 0)
            return 0;
        if (r == n || r == 0)
            return 1;
        return divi(spf[n], multi(spf[r], spf[n - r]));
    }
};
ll power(ll b, ll e) {
    ll ans = 1;
    while (e) {
        if (e % 2) ans *= b;
        b *= b;
        e /= 2;
    }
    return ans;
}
void print_128(__int128 n) {
    
    if (n == 0) {
        cout << "0";
        return;
    }
    string result = "";
    bool negative = n < 0;
    if (negative) n = -n;
    while (n > 0) {
        result += (n % 10) + '0';
        n /= 10;
    }
    if (negative) result += '-';
    reverse(result.begin(), result.end());
    cout << result;
}
```