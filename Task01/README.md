TASK 1 – CLASS DESIGN REVIE

1. AIM
To analyse the design deficiencies in the given payroll classes and redesign them using inheritance, encapsulation, function overriding and runtime polymorphism to create a reusable and extensible class hierarchy.
2. Task 1.1 – Diagnose the Design (2 Marks)
The given payroll system contains three separate classes: Professor, LabAssistant and GuestFaculty.

Identified design deficiencies:
1. Duplicate data members such as name, id and basic salary are present in multiple classes.
2. Repeated salary calculation logic exists in Professor and LabAssistant.
3. Public data members allow direct and uncontrolled modification.
4. Inconsistent variable names are used, such as name and nm, and id and code.
5. The da data member is declared but never used.
6. The magic number 0.42 is directly used in the salary calculation.
7. Adding a new staff category requires writing another independent class.
8. All staff members cannot be processed uniformly using a common base-class pointer.
Conclusion: The original design contains duplication, poor encapsulation and limited extensibility.
3. Task 1.2 – Repair the Design (3 Marks)
Proposed class hierarchy
A common abstract base class named Staff is created. Professor, LabAssistant and GuestFaculty inherit from Staff.
- Staff stores common details such as name and ID.
- pay() is declared as a pure virtual function.
- Each derived class implements its own salary calculation.
- Protected data members allow derived classes to access common information.
- A virtual destructor ensures safe destruction through base-class pointers.
UML class diagram
                 +----------------------+
                 |   <<Abstract>>       |
                 |        Staff         |
                 +----------------------+
                 | # name : string      |
                 | # id : int           |
                 +----------------------+
                 | + pay() : double     |
                 | + display() : void   |
                 +----------------------+
                           |
             +-------------+-------------+
             |             |             |
             v             v             v
      +------------+ +-------------+ +--------------+
      | Professor  | |LabAssistant | | GuestFaculty |
      +------------+ +-------------+ +--------------+
      | basic      | | basic       | | hours        |
      | research   | |             | | rate         |
      +------------+ +-------------+ +--------------+
      | pay()      | | pay()       | | pay()        |
      +------------+ +-------------+ +--------------+


File 1: Task01.cpp
Copy this complete program into Task01.cpp:

#include <iostream>
#include <memory>
#include <string>
#include <utility>
#include <vector>
using namespace std;

// Abstract Base Class
class Staff {
protected:
    string name;
    int id;

public:
    Staff(string n, int i) : name(move(n)), id(i) {}

    virtual double pay() const = 0;

    virtual void display() const {
        cout << "Name: " << name << endl;
        cout << "ID: " << id << endl;
        cout << "Salary: " << pay() << endl;
        cout << "----------------------" << endl;
    }

    virtual ~Staff() = default;
};

// Derived Class 1
class Professor : public Staff {
private:
    double basic;
    double research;
    static constexpr double DA_RATE = 0.42;

public:
    Professor(string n, int i, double b, double r)
        : Staff(move(n), i), basic(b), research(r) {}

    double pay() const override {
        return basic + basic * DA_RATE + research;
    }
};

// Derived Class 2
class LabAssistant : public Staff {
private:
    double basic;
    static constexpr double DA_RATE = 0.42;

public:
    LabAssistant(string n, int i, double b)
        : Staff(move(n), i), basic(b) {}

    double pay() const override {
        return basic + basic * DA_RATE;
    }
};

// Derived Class 3
class GuestFaculty : public Staff {
private:
    double hours;
    double rate;

public:
    GuestFaculty(string n, int i, double h, double r)
        : Staff(move(n), i), hours(h), rate(r) {}

    double pay() const override {
        return hours * rate;
    }
};

// Main Function
int main() {

    vector<unique_ptr<Staff>> staff;

    staff.push_back(
        make_unique<Professor>("Anita", 101, 50000, 5000));

    staff.push_back(
        make_unique<LabAssistant>("Ravi", 102, 30000));

    staff.push_back(
        make_unique<GuestFaculty>("Meera", 103, 20, 1500));

    cout << "COLLEGE PAYROLL SYSTEM" << endl;
    cout << "======================" << endl;

    for (const auto& person : staff) {
        person->display();
    }

    return 0;
}



4. Explanation of the Program
1. Staff class: Acts as the abstract base class and stores common employee details.
2. Professor class: Calculates salary using basic salary, DA and research allowance.
3. LabAssistant class: Calculates salary using basic salary and DA.
4. GuestFaculty class: Calculates salary using working hours multiplied by hourly rate.
5. Function overriding: Each derived class provides its own implementation of pay().
6. Runtime polymorphism: A common Staff pointer accesses the appropriate derived-class implementation.
7. Encapsulation: Salary-related data members are private, while common details are protected.
8. Smart pointers: unique_ptr automatically manages dynamically allocated objects.
5. Compilation and Execution
Compile:
g++ -std=c++17 -Wall -Wextra Task01.cpp -o Task01


Run:
./Task01


6. Expected Output
COLLEGE PAYROLL SYSTEM
======================
Name: Anita
ID: 101
Salary: 76000
----------------------
Name: Ravi
ID: 102
Salary: 42600
----------------------
Name: Meera
ID: 103
Salary: 30000
----------------------


Salary calculations:
Staff category	Calculation	Salary
Professor	50000 + 21000 + 5000	76000
LabAssistant	30000 + 12600	42600
GuestFaculty	20 × 1500	30000

7. Task 1.3 – Design Defence (1 Mark)
The proposed hierarchy improves reusability by placing common employee information in the Staff base class. Each derived class implements its own salary calculation using function overriding. When a new staff category is introduced, a new derived class can be created without modifying the existing classes. Runtime polymorphism allows all staff categories to be processed through a common base-class pointer. This reduces code duplication and improves maintainability, flexibility and extensibility.
8. Alternative Concept and Complexity Analysis
Alternative approach: Single Staff class with a type field and switch statement

Instead of inheritance, all staff members could be represented using one Staff class containing a type field.

Example:
double pay() {
    switch (type) {
        case 1:
            return basic + basic * 0.42 + research;

        case 2:
            return basic + basic * 0.42;

        case 3:
            return hours * rate;

        default:
            return 0;
    }
}


Comparison:
Criteria	Inheritance	Single Staff class
Salary calculation	Virtual function overriding	Switch statement
Time per pay() call	O(1)	O(1)
Space per object	Stores relevant class data	May contain unused fields
Adding new staff	Add a derived class	Modify switch and related code
Maintainability	High	Lower as categories increase
Extensibility	Easy	Requires existing code changes

Complexity Analysis:
- Time complexity: O(1) for each salary calculation in both approaches.
- Space complexity: O(1) per object in both approaches, although the single-class design may waste space on unused fields.
- Adding a new staff category requires modifying multiple parts of the single-class implementation, whereas inheritance allows a new derived class to be added independently.
Conclusion: Inheritance is a better approach for designing reusable and extensible payroll applications.
9. RESULT
Thus, the payroll system was successfully redesigned using inheritance, encapsulation, abstract classes, function overriding and runtime polymorphism. The program was implemented using C++ and demonstrates how a common base class can manage different employee categories efficiently.
