# TASK 2 – FROM REQUIREMENT TO HIERARCHY

## Aim
To design a university people management system using inheritance and understand the diamond problem, virtual base classes and access specifiers in C++.

## Task 2.1 – Identify the Relationships

| Relationship | Relationship Type | Inheritance Type |
|---|---|---|
| Student is a Person | Is-a | Single inheritance |
| Staff is a Person | Is-a | Single inheritance |
| TeachingAssistant is a Student | Is-a | Multiple inheritance when combined with Staff |
| TeachingAssistant is a Staff | Is-a | Multiple inheritance |
| ResearchScholar is a Student | Is-a | Multilevel inheritance |
| Student and Staff inherit Person | Is-a | Hierarchical inheritance |
| Complete hierarchy | Is-a | Hybrid inheritance |

A has-a relationship represents composition, where one class contains an object of another class. The given requirement does not specify a separate member class.

## Task 2.2 – Resolve the Diamond

The diamond problem occurs when a derived class inherits from two classes that share the same base class.

In this system, Student and Staff both inherit from Person. TeachingAssistant inherits from both Student and Staff. Without virtual inheritance, TeachingAssistant contains two Person sub-objects.

Virtual inheritance solves this problem by allowing both Student and Staff to share one Person sub-object.

## Task 2.3 – Design the Classes

The following classes are implemented:

1. Person – Stores name, ID and phone number.
2. Student – Stores programme and CGPA.
3. Staff – Stores department and salary.
4. TeachingAssistant – Inherits from Student and Staff.
5. ResearchScholar – Inherits from Student and stores fellowship details.

Public inheritance is used because the derived classes represent specialised forms of Person, Student and Staff.

Virtual inheritance is used for Student and Staff to prevent duplicate Person sub-objects.

## Task 2.4 – Access Table

| Person Member | Public Inheritance | Protected Inheritance | Private Inheritance |
|---|---|---|---|
| Public | Public | Protected | Private |
| Protected | Protected | Protected | Private |
| Private | Not accessible | Not accessible | Not accessible |

Private members of the base class cannot be accessed directly by the derived class.

## Predict the Output

Expected output:

Person Student Staff TA

Person is printed only once because it is a virtual base class shared by Student and Staff.

Without virtual inheritance, the expected output is:

Person Student Person Staff TA

This happens because two separate Person sub-objects are created.

## Alternative Concept and Complexity Analysis

Composition can be used by storing Student and Staff as member objects inside TeachingAssistant.

Virtual inheritance creates one shared Person sub-object, whereas composition creates separate Person sub-objects inside Student and Staff.

| Feature | Virtual Inheritance | Composition |
|---|---|---|
| Relationship | Is-a | Has-a |
| Person objects | One shared object | Two separate objects |
| Access | Direct base access | Through member objects |
| Ambiguity | Avoided using virtual inheritance | Avoided using explicit member access |
| Time complexity | O(1) member access | O(1) member access |
| Space | One shared Person sub-object plus implementation overhead | Two Person sub-objects plus implementation overhead |

## Compilation

Compile:

```bash
g++ -std=c++17 -Wall -Wextra Task02.cpp -o Task02
```

Run:

```bash
./Task02
```

Compile the diamond program:

```bash
g++ -std=c++17 -Wall -Wextra DiamondPrediction.cpp -o DiamondPrediction
```

Run:

```bash
./DiamondPrediction
```

## Result

The university people management hierarchy was designed and implemented using C++ inheritance. The diamond problem was resolved using virtual base classes. Access specifiers and composition were also analysed.
