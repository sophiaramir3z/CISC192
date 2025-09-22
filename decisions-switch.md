# Lab: Menu-driven calculator

## Challenges I encountered during the lab:
The biggest challenge was input validation. At first, entering letters instead of numbers would break the program, 
so I had to learn how to use cin.clear() and cin.ignore() with numeric_limits to handle errors correctly.
Another challenge was casting user input into an enum class so I could use it cleanly with a switch statement. 
This step was confusing at first but made the menu code more organized.
Finally, I had to remember to check for division by zero and keep the program running afterward. 
Overall, the lab helped me practice validation, edge cases, and building a more user-friendly calculator.

## My code:
```cpp
#include <iostream>
#include <limits>   // for numeric_limits
using namespace std;

// Enum class for menu choices
enum class Menu {
    Add = 1,
    Subtract,
    Multiply,
    Divide,
    Quit
};

// Function to get validated double input
double getValidatedDouble(const string& prompt) {
    double value;
    while (true) {
        cout << prompt;
        if (cin >> value) {
            return value;
        } else {
            cout << "Invalid input. Please enter a number." << endl;
            cin.clear(); // clear error flag
            cin.ignore(numeric_limits<streamsize>::max(), '\n'); // discard invalid input
        }
    }
}

int main() {
    while (true) {
        cout << "\n--- Menu-driven Calculator ---" << endl;
        cout << "1. Add" << endl;
        cout << "2. Subtract" << endl;
        cout << "3. Multiply" << endl;
        cout << "4. Divide" << endl;
        cout << "5. Quit" << endl;

        int choiceInt;
        cout << "Enter your choice (1-5): ";
        cin >> choiceInt;

        // Validate menu input
        if (cin.fail() || choiceInt < 1 || choiceInt > 5) {
            cout << "Invalid choice. Please select between 1 and 5." << endl;
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            continue;
        }

        Menu choice = static_cast<Menu>(choiceInt);

        if (choice == Menu::Quit) {
            cout << "Exiting calculator. Goodbye!" << endl;
            break;
        }

        double a = getValidatedDouble("Enter first number: ");
        double b = getValidatedDouble("Enter second number: ");

        switch (choice) {
            case Menu::Add:
                cout << "Result: " << (a + b) << endl;
                break;
            case Menu::Subtract:
                cout << "Result: " << (a - b) << endl;
                break;
            case Menu::Multiply:
                cout << "Result: " << (a * b) << endl;
                break;
            case Menu::Divide:
                if (b == 0) {
                    cout << "Error: Division by zero is not allowed." << endl;
                } else {
                    cout << "Result: " << (a / b) << endl;
                }
                break;
            default:
                cout << "Unexpected error." << endl;
        }
    }

    return 0;
}
```

## Explanation Video:
https://sdccd.us-west-2.instructuremedia.com/embed/e1fd6fed-102c-4295-9326-8efc7f8af1ea
