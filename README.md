# Interview problems
Unfortunately I can't tell which company was it due to their rules.

These are the problems that I had to solve in tech interview. I was interviewed for Senior Node.js developer position.
If you are interested in more details you can check out a [Video](https://youtu.be/QczD-QKiD-E 'Tech interview') I made about it. Including problems and explanations of the solutions.


## Step 2

### Problem 1 - phonebook

---

When you start typing phone number on your phone, contact names for that number show up.
Implement the same feature, given 2 arrays `A` and `B`, and a string `S`.

`A` contains names of the contacts.
`B` contains numbers of the contacts.
Indexes of names and numbers match, so contact at `A[i]` has the phone number of `B[i]`.

Our goal is to return name of the contact whose phone number contains string `S`.

For example:

```js
A = ['abby', 'sam', 'joel'];
B = ['2345', '1111', '445555'];
S = '445';
return 'joel';
```

If there are more than 1 matches, return alphabetically smaller one.
For example:

```js
A = ['abby', 'sam', 'joel'];
B = ['23455', '1111', '445555'];
S = '55';
return 'abby';
```

If there are no matches return `'NOT FOUND'`

_performance is NOT measured_

#### **Javascript**

---

```js
/**
 *
 * @param {Array} A Names of contacts
 * @param {Array} B Phone numbers of contacts
 * @param {String} S Search string
 * @returns {String} Matching contact name
 */
const solution = (A, B, S) => {
    let match = 'NOT FOUND';

    for (let i = 0; i < B.length; i++) {
        if (B[i].includes(S)) {
            if (match === 'NOT FOUND' || A[i] < match) {
                match = A[i];
            }
        }
    }

    return match;
};

// Test cases
console.log(solution(['adam', 'meggie'], ['12345', '7777999'], '779')); // meggie
console.log(
    solution(
        ['samuel', 'aaron', 'michelle', 'frank'],
        ['2345', '22233', '11111', '1233576'],
        '2'
    )
); // aaron
console.log(
    solution(['theodore', 'leon', 'kara'], ['5555', '22222', '33333'], '778')
); // NOT FOUND
```

#### **Python**

---

```python
def solution(A, B, S):
    """Returns a matching contact's name

    Arguments:
    A -- List of contact names
    B -- List of contact phone numbers
    S -- Search string
    """

    match = 'NOT FOUND'

    for i, val in enumerate(B):
        if S in val:
            if match == 'NOT FOUND' or A[i] < match:
                match = A[i]

    return match


# Test cases
print(solution(['adam', 'meggie'], ['12345', '7777999'], '779'))  # meggie
print(solution(['samuel', 'aaron', 'michelle', 'frank'], ['2345', '22233', '11111', '1233576'], '2'))  # aaron
print(solution(['theodore', 'leon', 'kara'], ['5555', '22222', '33333'], '778'))  # NOT FOUND
```

### Problem 2 - Format Numbers

---

You are given a string of random characters and digits.
You have to extract digits and split them into chunks of length 3, and split them with dashes.

for example if you are given a string:
`dlkj \_ 23-44, 45x6rl 39 ` <br>
you have to return
`234-445-639`

If the final block can not be of length 3, you can return chunks of length 2, but not 1.

For example:
`45 - 6543 23/ 456’,` should return `456-543-234-56` <br>
`45 - 6543 23/ 45` should return `456-543-23-45` <br>
and NOT `456-543-234-5`

_performance is NOT measured_

#### **Javascript**

---

```js
/**
 *
 * @param {String} S Random characters and digits
 * @returns {String} Formated string of digits
 */
const solution = (S) => {
    const res = [];
    let counter = 0;

    for (let i = 0; i < S.length; i++) {
        const c = S[i];

        if (counter === 3) {
            res.push('-');
            counter = 0;
        }

        if (!isNaN(c) && c !== ' ') {
            res.push(c);
            counter++;
        }
    }

    if (counter === 1) {
        res[res.length - 2] = res[res.length - 3];
        res[res.length - 3] = '-';
    }

    return res.join('');
};

// Test cases
console.log(solution('45 - 6543 23/ 456')); // 456-543-234-56
console.log(solution('45 - 6543 23/ 45')); // 456-543-23-45
console.log(solution('567834587')); // 567-834-587
```

#### **Python**

---

```python
def solution(S):
    """Returns formated string of digits

    Arguments:
    S -- String of random characters and digits
    """

    res = ''
    counter = 0

    for c in S:
        if counter == 3:
            res += '-'
            counter = 0
        if c.isdigit():
            res += c
            counter += 1

    if counter == 1 and len(res) >= 3:
        return res[:-3] + '-' + res[-3] + res[-1]
    return res

# Test cases
print(solution('45 - 6543 23/ 456'))  # 456-543-234-56
print(solution('45 - 6543 23/ 45'))  # 456-543-23-45
print(solution('567834587'))  # 567-834-587
```

### Problem 3 - Vacation

---

Jack is going on a vacation to Brazil.

The problem is that flights to Brazil leave only on `Mondays`
and return only on `Sundays`.
Meaning Jack can only spend whole weeks in Brazil.

Jack knows a month and a year when his vacation starts and a month when it ends.
And we know that his vacation `starts at the 1st of starting month`, and `ends with last day of ending month`.

We have to take into account leap years, a year is a leap year if it is divisible by 4.

We know weekday names, month names and days in months.

_performance is NOT measured_

```js
WEEKDAYS = [
    'Sunday',
    'Monday',
    'Tuesday',
    'Wednesday',
    'Thursday',
    'Friday',
    'Saturday',
];
MONTHS = [
    'January',
    'February',
    'March',
    'April',
    'May',
    'June',
    'July',
    'August',
    'September',
    'October',
    'November',
    'December',
];
DAYS = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
```

Given a `starting year`, `starting month`, `ending month` and `weekday of the 1st of january of the given year`,
we have to calculate how many weeks will Jack spend in Brazil.

e.g <br>
`2014, April, May, Wednesday` <br>
`return 7`

#### **Javascript**

---

```js
const MONTHS = [
    'January',
    'February',
    'March',
    'April',
    'May',
    'June',
    'July',
    'August',
    'September',
    'October',
    'November',
    'December',
];
const WEEKS = [
    'Sunday',
    'Monday',
    'Tuesday',
    'Wednesday',
    'Thursday',
    'Friday',
    'Saturday',
];
const DAYS = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];

/**
 *
 * @param {Number} Y Starting year of the vacation
 * @param {String} A Starting month of the vacation
 * @param {String} B Ending month of the vacation
 * @param {String} W Weekday of 1st of January of year Y
 * @returns {Number} Number of weeks spent in Brazil
 */
const solution = (Y, A, B, W) => {
    // If Y is a leap year
    let isLeap = !(Y % 4);
    if (isLeap) DAYS[1] = 29;

    let sumDays = 0;
    let i = 0; // Index for months
    let hasStarted = false; // Vacation has started

    while (true) {
        if (MONTHS[i] === A) {
            // Vacation started
            hasStarted = true;
            const remainderDays = sumDays % 7;
            const januaryWeekday = WEEKS.indexOf(W);
            const currentWeekday = (januaryWeekday + remainderDays) % 7;
            const weekdayToMonday = (7 - currentWeekday + 1) % 7;

            sumDays = DAYS[i] - weekdayToMonday;
        } else if (MONTHS[i] === B && hasStarted) {
            // Vacation ended
            sumDays += DAYS[i];
            break;
        } else {
            sumDays += DAYS[i];
        }

        i++;
        if (i >= 12) {
            // A year passes
            i = 0;
            Y++;
            isLeap = !(Y % 4);
            if (isLeap) DAYS[1] = 29;
            else DAYS[1] = 28;
        }
    }

    return Math.trunc(sumDays / 7);
};

// Test cases
console.log(solution(2014, 'April', 'May', 'Wednesday')); // 7
console.log(solution(2014, 'April', 'June', 'Wednesday')); // 12
console.log(solution(2014, 'December', 'January', 'Wednesday')); // 8
console.log(solution(2016, 'February', 'April', 'Friday')); // 12
```

#### **Python**

---

```Python
from datetime import date, timedelta

DAYS = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
WEEKS = [ 'Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
MONTHS = ['January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December']

def solution(Y, A, B, W):
    start_date = date(Y, MONTHS.index(A) + 1, 1)

    # A year passes
    if MONTHS.index(B) < MONTHS.index(A):
        Y += 1

    end_date = date(Y, MONTHS.index(B) + 2, 1) - timedelta(days=1)

    # Make start_date a Monday
    if start_date.weekday() != 0:
        start_date += timedelta(days=7-start_date.weekday())

    return (end_date-start_date).days//7


# Test cases
print(solution(2014, 'April', 'May', 'Wednesday')) # 7
print(solution(2014, 'December', 'January', 'Wednesday')) # 8
print(solution(2016, 'February', 'April', 'Friday')) # 12
```

### Problem 4 - Undirected Graphs

---

You are given an undirected graph of `N` vertices, with values from `1` to `N`, and `M` edges.

Connections between vertices are described by two arrays `A` and `B`,
both of length `M`.

So `A[i]` and `B[i]` are connected to each other.

e.g:

```js
N = 4;
A = [1, 2, 4, 4, 3];
B = [2, 3, 1, 3, 1][(1, 1, 1)];
```

You have to return `true`, if there is a path from vertex `1` to `N` in increasing order(without skipping a number).
so if there is a path `1 -> 2 -> 3 -> 4`
otherwise return `false`

_Performance IS measured!_

#### **Javascript**

---

```js
/**
 *
 * @param {Number} N number of vertices
 * @param {Array} A vertices
 * @param {Array} B vertices
 * @returns {Boolean}
 */
const solution = (N, A, B) => {
    const conns = Array(N - 1).fill(0);

    for (let i = 0; i < A.length; i++) {
        const diff = Math.abs(A[i] - B[i]);

        if (diff === 1) {
            conns[Math.min(A[i], B[i]) - 1] = 1;
        }
    }

    return conns.reduce((a, b) => a + b, 0) === N - 1;
};

// Test cases
console.log(solution(4, [1, 2, 4, 4, 3], [2, 3, 1, 3, 1])); // true
console.log(solution(6, [2, 4, 5, 3], [3, 5, 6, 4])); // false
console.log(solution(3, [1, 3], [2, 2])); // true
```

#### **Python**

---

```python
def solution(N, A, B):
    """Returns a Boolean value

    Arguments:
    N -- Number of vertices
    A -- List of vertices
    B -- List of vertices
    """

    conns = [0] * (N-1)

    for i in range(len(A)):
        diff = abs(A[i] - B[i])

        if diff == 1:
            conns[min(A[i], B[i]) - 1] = 1

    return sum(conns) == (N-1)

# Test cases
print(solution(4, [1, 2, 4, 4, 3], [2, 3, 1, 3, 1])) # True
print(solution(6, [2, 4, 5, 3], [3, 5, 6, 4])) # False
print(solution(3, [1,3], [2, 2])) # True
```

## Step 3

### Problem 1 - getChange (JS only)

---

You are given some amount of money(`m`) and price(`p`) of a product.
Calculate the change to return in the least amount of coins possible and return an array of coins.
Coins you have are `1, 5, 10, 25, 50 cents`, and `1 dollar`, each corresponding to an index in an array.

example

```js
getChange(5, 0.99); // should return [1,0,0,0,0,4] 4 1-dollar coins and 1 1-cent coin
```

#### **Solution**

---

```js
/**
 *
 * @param {Number} m Current amount of money
 * @param {Number} p Price of the product
 * @returns {Array} Amounts of coins
 */
const getChange = (m, p) => {
    let rem = m * 100 - p * 100;
    const coins = [1, 5, 10, 25, 50, 100];
    const res = Array(coins.length).fill(0);
    for (let i = res.length - 1; i >= 0; i--) {
        res[i] = Math.floor(rem / coins[i]);
        rem = rem - coins[i] * res[i];
        if (rem === 0) break;
    }
    return res;
};
// Test cases
console.log(getChange(5, 0.99)); // should return [1,0,0,0,0,4]
console.log(getChange(3.14, 1.99)); // should return [0,1,1,0,0,1]
console.log(getChange(3, 0.01)); // should return [4,0,2,1,1,2]
console.log(getChange(4, 3.14)); // should return [1,0,1,1,1,0]
console.log(getChange(0.45, 0.34)); // should return [1,0,1,0,0,0]
```

### Problem 2 - findWord (JS only)

---

You have sequences of characters denoted with `>`.
`P>E` means character `P` is directly followed by character `E`, there are no characters in between.
you are given an array of this sequences, your goal is to return a word.

example

```js
findWord(['P>E', 'E>R', 'R>U']); // PERU
```

#### **Solution**

---

```js
/**
 *
 * @param {Array} arr Character sequences
 * @returns {String} Word
 */
const findWord = (arr) => {
    let res = arr[0][0] + arr[0][2];
    const pre = {};
    const post = {};

    for (let i = 1; i < arr.length; i++) {
        const start = arr[i][0];
        const end = arr[i][2];

        pre[start] = end;
        post[end] = start;
    }

    while (res[0] in post || res[res.length - 1] in pre) {
        const first = res[0];
        const last = res[res.length - 1];

        if (res[0] in post) {
            res = post[first] + res;
            delete post[first];
        }
        if (res[res.length - 1] in pre) {
            res += pre[last];
            delete pre[last];
        }
    }

    return res;
};

// Test cases
console.log(findWord(['P>E', 'E>R', 'R>U'])); // PERU
console.log(findWord(['I>N', 'A>I', 'P>A', 'S>P'])); // SPAIN
console.log(findWord(['U>N', 'G>A', 'R>Y', 'H>U', 'N>G', 'A>R'])); // HUNGARY
console.log(findWord(['I>F', 'W>I', 'S>W', 'F>T'])); // SWIFT
console.log(findWord(['R>T', 'A>L', 'P>O', 'O>R', 'G>A', 'T>U', 'U>G'])); // PORTUGAL
console.log(
    findWord([
        'W>I',
        'R>L',
        'T>Z',
        'Z>E',
        'S>W',
        'E>R',
        'L>A',
        'A>N',
        'N>D',
        'I>T',
    ])
); // SWITZERLAND
```
