
#include <iostream>
#include <string>
using namespace std;

// Base class
class Person
{
protected:
    string name;
    int id;
    string phone;

public:
    Person(string n = "", int i = 0, string p = "")
        : name(n), id(i), phone(p)
    {
    }

    void displayPerson() const
    {
        cout << "Name: " << name << endl;
        cout << "ID: " << id << endl;
        cout << "Phone: " << phone << endl;
    }
};

// Student inherits Person virtually
class Student : virtual public Person
{
protected:
    string programme;
    double cgpa;

public:
    Student(string n = "", int i = 0, string p = "",
            string prog = "", double c = 0.0)
        : Person(n, i, p), programme(prog), cgpa(c)
    {
    }

    void displayStudent() const
    {
        cout << "Programme: " << programme << endl;
        cout << "CGPA: " << cgpa << endl;
    }
};

// Staff inherits Person virtually
class Staff : virtual public Person
{
protected:
    string department;
    double salary;

public:
    Staff(string n = "", int i = 0, string p = "",
          string dept = "", double s = 0.0)
        : Person(n, i, p), department(dept), salary(s)
    {
    }

    void displayStaff() const
    {
        cout << "Department: " << department << endl;
        cout << "Salary: " << salary << endl;
    }
};

// TeachingAssistant inherits Student and Staff
class TeachingAssistant : public Student, public Staff
{
public:
    TeachingAssistant(string n, int i, string p,
                      string prog, double c,
                      string dept, double s)
        : Person(n, i, p),
          Student(n, i, p, prog, c),
          Staff(n, i, p, dept, s)
    {
    }

    void display() const
    {
        cout << "TEACHING ASSISTANT DETAILS" << endl;
        displayPerson();
        displayStudent();
        displayStaff();
    }
};

// ResearchScholar inherits Student
class ResearchScholar : public Student
{
private:
    double fellowship;

public:
    ResearchScholar(string n, int i, string p,
                    string prog, double c, double f)
        : Person(n, i, p),
          Student(n, i, p, prog, c),
          fellowship(f)
    {
    }

    void displayScholar() const
    {
        cout << "RESEARCH SCHOLAR DETAILS" << endl;
        displayPerson();
        displayStudent();
        cout << "Fellowship: " << fellowship << endl;
    }
};

int main()
{
    TeachingAssistant ta(
        "Arun", 101, "9876543210",
        "Computer Science", 8.5,
        "AI and ML", 35000
    );

    ResearchScholar rs(
        "Priya", 102, "9876501234",
        "Data Science", 9.1, 15000
    );

    ta.display();

    cout << endl;

    rs.displayScholar();

    return 0;
}
