# Working with Arrays in C++

##
```cpp
// Name: Sophia Ramirez
// Course: CISC 192 - C++ Programming
// Date: 11/02/2025
// Assignment: Non-Duplicated Integer Array Operations

#include <iostream>
#include <array>
using namespace std;

int main() {
    const int N = 5;
    array<int, N> numbers;
    bool duplicate;
    int choice;

    cout << "Enter " << N << " unique integers:\n";

    // Input loop ensuring unique integers
    for (int i = 0; i < N; i++) {
        int num;
        do {
            duplicate = false;
            cout << "Element " << i + 1 << ": ";
            cin >> num;

            for (int j = 0; j < i; j++) {
                if (numbers[j] == num) {
                    duplicate = true;
                    cout << "Duplicate found! Enter a different number.\n";
                    break;
                }
            }
        } while (duplicate);
        numbers[i] = num;
    }

    // Menu
    cout << "\nChoose an operation:\n";
    cout << "1. Sort Ascending\n";
    cout << "2. Sort Descending\n";
    cout << "3. Find Maximum\n";
    cout << "Enter your choice: ";
    cin >> choice;

    switch (choice) {
        case 1: {
            // Ascending bubble sort
            for (int i = 0; i < N - 1; i++) {
                for (int j = 0; j < N - i - 1; j++) {
                    if (numbers[j] > numbers[j + 1]) {
                        int temp = numbers[j];
                        numbers[j] = numbers[j + 1];
                        numbers[j + 1] = temp;
                    }
                }
            }

            cout << "\nArray sorted in ascending order:\n";
            for (int num : numbers) cout << num << " ";
            cout << endl;
            break;
        }

        case 2: {
            // Descending bubble sort
            for (int i = 0; i < N - 1; i++) {
                for (int j = 0; j < N - i - 1; j++) {
                    if (numbers[j] < numbers[j + 1]) {
                        int temp = numbers[j];
                        numbers[j] = numbers[j + 1];
                        numbers[j + 1] = temp;
                    }
                }
            }

            cout << "\nArray sorted in descending order:\n";
            for (int num : numbers) cout << num << " ";
            cout << endl;
            break;
        }

        case 3: {
            int maxVal = numbers[0];
            for (int i = 1; i < N; i++) {
                if (numbers[i] > maxVal)
                    maxVal = numbers[i];
            }
            cout << "\nMaximum value: " << maxVal << endl;
            break;
        }

        default:
            cout << "\nInvalid choice.\n";
    }

    return 0;
}
```
