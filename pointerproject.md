# C++ Pointer Project

## Part A - Raw Pointers to Stack Objects
### Task.h
```cpp
#ifndef TASK_H
#define TASK_H

#include <string>

class Task {
private:
    int id;
    std::string description;
    bool completed;

public:
    Task();
    Task(int id, const std::string& desc);

    void markCompleted();
    bool isCompleted() const;
    int getId() const;
    std::string getDescription() const;
};

#endif
```

## Part B - Dynamic Memory Using Raw Pointers (new / delete)
### Task.cpp
```cpp
#include "Task.h"

Task::Task() : id(0), description(""), completed(false) {}

Task::Task(int id, const std::string& desc) : id(id), description(desc), completed(false) {}

void Task::markCompleted() {
    completed = true;
}

bool Task::isCompleted() const {
    return completed;
}

int Task::getId() const {
    return id;
}

std::string Task::getDescription() const {
    return description;
}
```
## Part C - Smart Pointer Version (std::unique_ptr)
### TaskManager.cpp
```cpp
#include "TaskManager.h"
#include <iostream>

TaskManager::TaskManager(int initialCapacity)
    : tasks(std::make_unique<Task[]>(initialCapacity)),
      size(0),
      capacity(initialCapacity),
      nextId(1) {}

void TaskManager::resize(int newCapacity) {
    if (newCapacity <= capacity) {
        return;
    }

    std::unique_ptr<Task[]> newTasks = std::make_unique<Task[]>(newCapacity);
    for (int i = 0; i < size; i++) {
        newTasks[i] = tasks[i];
    }

    tasks = std::move(newTasks);
    capacity = newCapacity;
}

void TaskManager::addTask(const std::string& desc) {
    if (size >= capacity) {
        int newCapacity = capacity * 2;
        if (newCapacity < 1) {
            newCapacity = 1;
        }
        resize(newCapacity);
    }

    tasks[size] = Task(nextId, desc);
    size++;
    nextId++;
}

void TaskManager::removeTask(int id) {
    int index = -1;
    for (int i = 0; i < size; i++) {
        if (tasks[i].getId() == id) {
            index = i;
            break;
        }
    }

    if (index == -1) {
        std::cout << "Task not found.\n";
        return;
    }

    for (int i = index; i < size - 1; i++) {
        tasks[i] = tasks[i + 1];
    }
    size--;

    std::cout << "Task removed.\n";
}

void TaskManager::markTaskCompleted(int id) {
    for (int i = 0; i < size; i++) {
        if (tasks[i].getId() == id) {
            tasks[i].markCompleted();
            std::cout << "Task marked completed.\n";
            return;
        }
    }
    std::cout << "Task not found.\n";
}

void TaskManager::listTasks() const {
    if (size == 0) {
        std::cout << "No tasks.\n";
        return;
    }

    for (int i = 0; i < size; i++) {
        std::cout << "ID: " << tasks[i].getId() << " | ";
        std::cout << (tasks[i].isCompleted() ? "Completed" : "Not completed") << " | ";
        std::cout << tasks[i].getDescription() << "\n";
    }
}

```
### main.cpp
```cpp
#include <iostream>
#include <string>
#include <limits>
#include "Task.h"
#include "TaskManager.h"

void completeTask(Task* t) {
    if (t == nullptr) {
        return;
    }
    t->markCompleted();
}

void listTasks(Task* tasks, int size) {
    if (size == 0) {
        std::cout << "No tasks.\n";
        return;
    }

    for (int i = 0; i < size; i++) {
        Task* current = tasks + i;
        std::cout << "ID: " << current->getId() << " | ";
        std::cout << (current->isCompleted() ? "Completed" : "Not completed") << " | ";
        std::cout << current->getDescription() << "\n";
    }
}

void resizeTasks(Task*& tasks, int& capacity, int size, int newCapacity) {
    if (newCapacity <= capacity) {
        return;
    }

    Task* newArray = new Task[newCapacity];
    for (int i = 0; i < size; i++) {
        newArray[i] = tasks[i];
    }

    delete[] tasks;
    tasks = newArray;
    capacity = newCapacity;
}

void addTask(Task* tasks, int& size, int capacity, const std::string& desc) {
    if (size >= capacity) {
        std::cout << "Task list is full.\n";
        return;
    }

    int newId = 1;
    if (size > 0) {
        newId = tasks[size - 1].getId() + 1;
    }

    tasks[size] = Task(newId, desc);
    size++;
}

void addTaskWithResize(Task*& tasks, int& size, int& capacity, const std::string& desc) {
    if (size >= capacity) {
        int newCapacity = capacity * 2;
        if (newCapacity < 1) {
            newCapacity = 1;
        }
        resizeTasks(tasks, capacity, size, newCapacity);
    }

    int newId = 1;
    if (size > 0) {
        newId = tasks[size - 1].getId() + 1;
    }

    tasks[size] = Task(newId, desc);
    size++;
}

void removeTask(Task* tasks, int& size, int id) {
    int index = -1;
    for (int i = 0; i < size; i++) {
        if (tasks[i].getId() == id) {
            index = i;
            break;
        }
    }

    if (index == -1) {
        std::cout << "Task not found.\n";
        return;
    }

    for (int i = index; i < size - 1; i++) {
        tasks[i] = tasks[i + 1];
    }
    size--;

    std::cout << "Task removed.\n";
}

void markTaskCompleted(Task* tasks, int size, int id) {
    for (int i = 0; i < size; i++) {
        if (tasks[i].getId() == id) {
            tasks[i].markCompleted();
            std::cout << "Task marked completed.\n";
            return;
        }
    }
    std::cout << "Task not found.\n";
}

int readInt() {
    int value = 0;
    while (true) {
        if (std::cin >> value) {
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
            return value;
        }
        std::cin.clear();
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        std::cout << "Enter a valid integer: ";
    }
}

std::string readLine() {
    std::string s;
    std::getline(std::cin, s);
    return s;
}

void runPartA() {
    std::cout << "Part A stack objects with raw pointers\n";

    Task t1(1, "Finish project");
    Task t2(2, "Study for exam");

    Task* p1 = &t1;
    Task* p2 = &t2;

    std::cout << "Before completion\n";
    std::cout << "ID: " << (*p1).getId() << " | " << p1->getDescription() << " | " << (p1->isCompleted() ? "Completed" : "Not completed") << "\n";
    std::cout << "ID: " << p2->getId() << " | " << (*p2).getDescription() << " | " << (p2->isCompleted() ? "Completed" : "Not completed") << "\n";

    completeTask(p1);

    std::cout << "After completing task 1 via pointer\n";
    std::cout << "ID: " << p1->getId() << " | " << p1->getDescription() << " | " << (p1->isCompleted() ? "Completed" : "Not completed") << "\n";
    std::cout << "ID: " << p2->getId() << " | " << p2->getDescription() << " | " << (p2->isCompleted() ? "Completed" : "Not completed") << "\n\n";
}

void runPartB() {
    int capacity = 5;
    int size = 0;

    Task* tasks = new Task[capacity];

    while (true) {
        std::cout << "\nPart B menu\n";
        std::cout << "1 Add task\n";
        std::cout << "2 Remove task\n";
        std::cout << "3 Mark task completed\n";
        std::cout << "4 List tasks\n";
        std::cout << "5 Quit Part B\n";
        std::cout << "Choice: ";

        int choice = readInt();

        if (choice == 1) {
            std::cout << "Description: ";
            std::string desc = readLine();
            addTaskWithResize(tasks, size, capacity, desc);
        } else if (choice == 2) {
            std::cout << "Task id: ";
            int id = readInt();
            removeTask(tasks, size, id);
        } else if (choice == 3) {
            std::cout << "Task id: ";
            int id = readInt();
            markTaskCompleted(tasks, size, id);
        } else if (choice == 4) {
            std::cout << "Capacity: " << capacity << " Size: " << size << "\n";
            listTasks(tasks, size);
        } else if (choice == 5) {
            break;
        } else {
            std::cout << "Invalid choice.\n";
        }
    }

    delete[] tasks;
    tasks = nullptr;
}

void runPartC() {
    TaskManager manager(5);

    while (true) {
        std::cout << "\nPart C menu\n";
        std::cout << "1 Add task\n";
        std::cout << "2 Remove task\n";
        std::cout << "3 Mark task completed\n";
        std::cout << "4 List tasks\n";
        std::cout << "5 Quit Part C\n";
        std::cout << "Choice: ";

        int choice = readInt();

        if (choice == 1) {
            std::cout << "Description: ";
            std::string desc = readLine();
            manager.addTask(desc);
        } else if (choice == 2) {
            std::cout << "Task id: ";
            int id = readInt();
            manager.removeTask(id);
        } else if (choice == 3) {
            std::cout << "Task id: ";
            int id = readInt();
            manager.markTaskCompleted(id);
        } else if (choice == 4) {
            manager.listTasks();
        } else if (choice == 5) {
            break;
        } else {
            std::cout << "Invalid choice.\n";
        }
    }
}

int main() {
    runPartA();

    while (true) {
        std::cout << "Select version\n";
        std::cout << "1 Part B raw pointers heap array\n";
        std::cout << "2 Part C unique_ptr TaskManager\n";
        std::cout << "3 Exit\n";
        std::cout << "Choice: ";

        int choice = readInt();

        if (choice == 1) {
            runPartB();
        } else if (choice == 2) {
            runPartC();
        } else if (choice == 3) {
            break;
        } else {
            std::cout << "Invalid choice.\n";
        }

        std::cout << "\n";
    }

    return 0;
}

```
