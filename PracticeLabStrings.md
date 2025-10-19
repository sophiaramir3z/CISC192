# Practice Lab - Strings, Substrings & Text Analysis

## Task 1: Extract Domain from Email
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string email;
    cout << "Enter an email address: ";
    getline(cin, email);

    int pos = email.find('@');
    if (pos != string::npos) {
        string user = email.substr(0, pos);
        string domain = email.substr(pos + 1);
        cout << "Username: " << user << endl;
        cout << "Domain: " << domain << endl;
    } else {
        cout << "Invalid email format.\n";
    }
    return 0;
}
```
## Task 2: Replace a Word
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string text;
    cout << "Enter a sentence: ";
    getline(cin, text);

    size_t pos = 0;
    while ((pos = text.find("Miramar", pos)) != string::npos) {
        text.replace(pos, 7, "San Diego Miramar College");
        pos += 24; // move past replacement
    }

    cout << "Result: " << text << endl;
    return 0;
}
```
## Task 3: Palindrome Checker (Ignore Spaces)
```cpp
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

bool isPalindrome(string s) {
    s.erase(remove(s.begin(), s.end(), ' '), s.end());
    transform(s.begin(), s.end(), s.begin(), ::tolower);
    string reversed = s;
    reverse(reversed.begin(), reversed.end());
    return s == reversed;
}

int main() {
    string phrase;
    cout << "Enter a phrase: ";
    getline(cin, phrase);

    if (isPalindrome(phrase))
        cout << "It’s a palindrome.\n";
    else
        cout << "Not a palindrome.\n";
    return 0;
}
```
## Task 4: Find the Longest Word
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string sentence;
    cout << "Enter a sentence: ";
    getline(cin, sentence);

    string word, longest;
    int maxLen = 0;
    for (size_t i = 0; i <= sentence.length(); i++) {
        if (i == sentence.length() || sentence[i] == ' ') {
            if (word.length() > maxLen) {
                longest = word;
                maxLen = word.length();
            }
            word = "";
        } else {
            word += sentence[i];
        }
    }

    cout << "Longest word: " << longest << endl;
    cout << "Length: " << maxLen << endl;
    return 0;
}
```
## Task 5: Character Histogram
```cpp
#include <iostream>
#include <string>
#include <map>
#include <algorithm>
#include <cctype>
using namespace std;

int main() {
    string s;
    cout << "Enter text: ";
    getline(cin, s);

    transform(s.begin(), s.end(), s.begin(), ::toupper);
    map<char, int> freq;

    for (char c : s)
        if (isalpha(c))
            freq[c]++;

    for (auto &pair : freq)
        cout << pair.first << " -> " << pair.second << endl;

    return 0;
}
```
## Task 6: Mask Email Address
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string email;
    cout << "Enter an email: ";
    getline(cin, email);

    int atPos = email.find('@');
    if (atPos != string::npos && atPos > 2) {
        for (int i = 1; i < atPos - 1; i++)
            email[i] = '*';
        cout << "Masked: " << email << endl;
    } else {
        cout << "Invalid or too short username.\n";
    }
    return 0;
}
```
## Extra Credit (Sort All Words Alphabetically)
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <sstream>
#include <algorithm>
using namespace std;

int main() {
    string sentence;
    cout << "Enter a sentence: ";
    getline(cin, sentence);

    stringstream ss(sentence);
    vector<string> words;
    string temp;

    while (ss >> temp)
        words.push_back(temp);

    sort(words.begin(), words.end());

    cout << "Sorted words:\n";
    for (string &w : words)
        cout << w << endl;

    return 0;
}
```
#Screenshots of successful execution
<img width="1919" height="1199" alt="image" src="https://github.com/user-attachments/assets/d2059588-e714-4b58-9d05-8807a66117f9" />
<img width="1919" height="1135" alt="image" src="https://github.com/user-attachments/assets/b937f261-ef63-4381-9d52-d637964c09fe" />

