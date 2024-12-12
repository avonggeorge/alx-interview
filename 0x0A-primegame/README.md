# Prime Game
The **Prime Game** is a coding problem that revolves around prime numbers and game theory. Here's how it typically works and how to solve it:


### **Problem Description**

Two players, Maria and Ben, take turns playing a game. The rules are:

1.  They are given a number `n`.
2.  The players take turns picking numbers from `1` to `n` such that:
    -   Each chosen number must be prime.
    -   All multiples of the chosen number (including the number itself) are removed from the set.
3.  The player who cannot make a move loses the game.

The challenge is to determine the winner of the game given a range of `n` values.



### **Key Concepts**

1.  **Prime Numbers**:
    
    -   A prime number is greater than 1 and divisible only by 1 and itself.
    -   Examples: 2, 3, 5, 7, 11, etc.
    -   Non-prime examples: 4, 6, 8 (divisible by other numbers).
2.  **Prime Sieve**:
    
    -   Efficiently find all primes up to `n` using the **Sieve of Eratosthenes**.
3.  **Game Strategy**:
    
    -   The game's result depends on the number of available moves. If the total moves are odd, the first player (Maria) wins. If even, the second player (Ben) wins.


### **Algorithm**

1.  **Precompute Primes**:
    -   Use the Sieve of Eratosthenes to find all primes up to the maximum `n` in the input.
2.  **Simulate the Game**:
    -   Count the number of primes less than or equal to `n`.
    -   If this count is odd, Maria wins; otherwise, Ben wins.
3.  **Process Multiple Rounds**:
    -   For each number in the list, determine the winner.


### **Python Implementation**

Here’s a Python solution:
```
def is_prime_sieve(max_num):
    """Generate a list indicating whether numbers <= max_num are prime."""
    sieve = [True] * (max_num + 1)
    sieve[0] = sieve[1] = False  # 0 and 1 are not prime

    for i in range(2, int(max_num**0.5) + 1):
        if sieve[i]:
            for j in range(i * i, max_num + 1, i):
                sieve[j] = False

    return sieve

def prime_game(nums):
    """Determine the winner of the prime game."""
    if not nums:
        return None

    max_num = max(nums)
    prime_sieve = is_prime_sieve(max_num)

    # Precompute prime counts up to each number
    prime_counts = [0] * (max_num + 1)
    for i in range(1, max_num + 1):
        prime_counts[i] = prime_counts[i - 1] + (1 if prime_sieve[i] else 0)

    maria_wins = 0
    ben_wins = 0

    for n in nums:
        # Determine winner for this round
        if prime_counts[n] % 2 == 1:  # Odd count -> Maria wins
            maria_wins += 1
        else:  # Even count -> Ben wins
            ben_wins += 1

    # Determine overall winner
    if maria_wins > ben_wins:
        return "Maria"
    elif ben_wins > maria_wins:
        return "Ben"
    else:
        return None

# Test Cases
nums = [4, 5, 10]
print(prime_game(nums))  # Output: "Maria"
```

### **Explanation of the Code**

1.  **Prime Sieve**:
    
    -   Generate a boolean list where `True` indicates a number is prime.
    -   Mark multiples of each prime as non-prime.
2.  **Prime Count**:
    
    -   Use a cumulative count of primes up to each number.
3.  **Game Logic**:
    
    -   For each number `n`, determine if the count of primes is odd (Maria wins) or even (Ben wins).
4.  **Final Result**:
    
    -   Compare the total wins of Maria and Ben to determine the overall winner.


### **Complexity**

-   **Time Complexity**:
    -   Prime sieve: O(nlog⁡log⁡n)O(n \log \log n)O(nloglogn), where nnn is the largest number in `nums`.
    -   Simulating the game: O(m)O(m)O(m), where mmm is the length of `nums`.
-   **Space Complexity**:
    -   O(n)O(n)O(n) for the sieve and prime counts.


### **Edge Cases**

1.  **Empty Input**: If `nums` is empty, return `None`.
2.  **All Non-Prime Numbers**: Handle cases where no primes exist below `n`.
3.  **Single Round**: Handle a single number in `nums`.

This approach ensures efficiency and correctness for the "Prime Game" problem.
