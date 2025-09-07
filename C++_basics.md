# Lab 1 - C++ Basics

## Part A - Expressions & Assignments

## A1: Precedence check
```cpp
#include <iostream>
int main() {
    int a, b, c;
    std::cin >> a >> b >> c;

    int r1 = a + b * c;          // multiplication before addition
    int r2 = (a + b) * c;        // explicit grouping
    int r3 = a / b * c;          // left to right with integer truncation
    int r4 = a / (b * c);        // multiply first, then divide

    std::cout << "a + b * c = " << r1 << "\n";
    std::cout << "(a + b) * c = " << r2 << "\n";
    std::cout << "a / b * c = " << r3 << "\n";
    std::cout << "a / (b * c) = " << r4 << "\n";
    return 0;
}
```
## A2: Compound assignment
```cpp
#include <iostream>
#include <iomanip>
int main() {
    double balance;
    int deposits, withdrawals;
    std::cin >> balance >> deposits >> withdrawals;

    const double depositAmt = 25.50;
    const double withdrawalAmt = 12.75;

    double start = balance;
    balance += deposits * depositAmt;     // compound addition
    balance -= withdrawals * withdrawalAmt; // compound subtraction

    std::cout << std::fixed << std::setprecision(2);
    std::cout << "Final balance = " << start
              << " + " << deposits << "*" << depositAmt
              << " - " << withdrawals << "*" << withdrawalAmt
              << " = " << balance << "\n";
    return 0;
}
```
## Part B - Implicit vs Explicit Conversion

## B1: The average trap
```cpp
#include <iostream>
#include <iomanip>
int main() {
    int s1, s2, s3;
    std::cin >> s1 >> s2 >> s3;

    int sum = s1 + s2 + s3;

    int implicitAvg = sum / 3; // integer division truncates
    double explicitAvg = static_cast<double>(sum) / 3.0;

    std::cout << "Raw average (implicit): " << implicitAvg << "\n";
    std::cout << std::fixed << std::setprecision(1);
    std::cout << "Correct average (explicit cast): " << explicitAvg << "\n";
    return 0;
}
```

## B2: Percentage with safe casting
```cpp
#include <iostream>
#include <iomanip>
int main() {
    int wins, games;
    std::cin >> wins >> games;

    if (games == 0) {
        std::cout << "No games played.\n";
        return 0;
    }

    double pct = static_cast<double>(wins) / static_cast<double>(games) * 100.0;

    std::cout << std::fixed << std::setprecision(1);
    std::cout << "Win% = " << pct << "%\n";
    return 0;
}
```

## Part C - Paycheck Calculator
```cpp
#include <iostream>
#include <iomanip>
#include <algorithm>
int main() {
    double hours, rate;
    std::cin >> hours >> rate;

    double regHours = std::min(hours, 40.0);
    double otHours  = std::max(hours - 40.0, 0.0);
    double gross    = regHours * rate + otHours * rate * 1.5;
    double tax      = 0.18 * gross;
    double benefits = 35.0;
    double net      = gross - tax - benefits;

    std::cout << std::fixed << std::setprecision(2);
    std::cout << "Regular: " << regHours << ", Overtime: " << otHours << "\n";
    std::cout << "Gross: $" << gross << "\n";
    std::cout << "Tax (18%): $" << tax << "\n";
    std::cout << "Benefits: $" << benefits << "\n";
    std::cout << "Net: $" << net << "\n";
    return 0;
}
```
## Part D - Debugging
```cpp
#include <iostream>
#include <iomanip>
int main() {
    int items; 
    double price;
    std::cout << "Enter items and price: ";
    std::cin >> items >> price;

    double total = 0.0;                 // initialize before use
    total = items * price;

    int discountPercent = 15;
    double discount = total * (static_cast<double>(discountPercent) / 100.0);

    double shipping = 5 + 2 * items;    // precedence is fine here
    double afterDiscount = total - discount;
    if (afterDiscount >= 100.0) shipping = 0.0;

    std::cout << std::fixed << std::setprecision(2);
    std::cout << "Total: $" << total << "\n";
    std::cout << "Discount: $" << discount << "\n";
    std::cout << "Shipping: $" << shipping << "\n";
    std::cout << "Grand Total: $" << (afterDiscount + shipping) << "\n";
    return 0;
}
```
## README
One lesson about implicit and explicit conversion:
- Using integer types in division silently truncates toward zero. Casting to double before dividing preserves the fractional component, for example `sum / 3` loses decimals while `static_cast<double>(sum) / 3.0` keeps them.

One bug fixed and how it was found:
- The discount was computed with an integer percentage, which can cause unintended integer arithmetic if not cast. By inspecting the expression and types, the fix is to promote the percentage to double with `static_cast<double>(discountPercent) / 100.0`, which produces the correct fractional discount and matches expected totals.
