C++ Programming Tasks & Instructions — CodeAlpha

Task 1:
TASK 1: CGPA Calculator 
● Take input for the number of courses taken by the student.
 ● For each course, input the grade and the credit hours. 
● Calculate the total credits and total grade points (grade × credit hours). 
● Compute the GPA for the semester and then the overall CGPA. 
● Display individual course grades and the final CGPA to the user

Source Code:
#include <iostream>
#include <vector>
#include <string>

using namespace std;

struct Course {
    string grade;
    int creditHours;
    double gradePoints;
};

double getGradePoints(string grade) {
    if (grade == "A") return 4.0;
    else if (grade == "B") return 3.0;
    else if (grade == "C") return 2.0;
    else if (grade == "D") return 1.0;
    else if (grade == "F") return 0.0;
    else return 0.0; // In case of invalid grade input
}

int main() {
    int numCourses;
    double totalGradePoints = 0.0, totalCredits = 0.0, semesterGradePoints = 0.0, semesterCredits = 0.0;

    // Input the number of courses taken by the student
    cout << "Enter the number of courses: ";
    cin >> numCourses;

    vector<Course> courses(numCourses);

    // Input grades and credit hours for each course
    for (int i = 0; i < numCourses; i++) {
        cout << "Enter grade and credit hours for course " << i + 1 << ": ";
        cin >> courses[i].grade >> courses[i].creditHours;

        // Calculate grade points for the course
        courses[i].gradePoints = getGradePoints(courses[i].grade);
        
        // Update total credits and grade points for GPA calculation
        semesterGradePoints += courses[i].gradePoints * courses[i].creditHours;
        semesterCredits += courses[i].creditHours;
        
        // Display individual course details
        cout << "Course " << i + 1 << " - Grade: " << courses[i].grade
             << ", Credit Hours: " << courses[i].creditHours
             << ", Grade Points: " << courses[i].gradePoints
             << ", Total Grade Points: " << courses[i].gradePoints * courses[i].creditHours << endl;
    }

    // Calculate Semester GPA
    double semesterGPA = semesterGradePoints / semesterCredits;

    // Display the semester GPA
    cout << "\nSemester GPA: " << semesterGPA << endl;

    // Ask for total number of semesters to calculate CGPA
    int numSemesters;
    cout << "Enter the number of semesters: ";
    cin >> numSemesters;

    // Calculate CGPA over multiple semesters (totalGradePoints and totalCredits across semesters)
    double totalGradePointsOverall = semesterGradePoints;
    double totalCreditsOverall = semesterCredits;
    for (int i = 1; i < numSemesters; i++) {
        double semesterGradePointsTemp = 0.0, semesterCreditsTemp = 0.0;
        cout << "\nEnter number of courses for semester " << i + 1 << ": ";
        int numCoursesInSemester;
        cin >> numCoursesInSemester;
        
        for (int j = 0; j < numCoursesInSemester; j++) {
            string grade;
            int creditHours;
            cout << "Enter grade and credit hours for course " << j + 1 << ": ";
            cin >> grade >> creditHours;

            double gradePoints = getGradePoints(grade);

            semesterGradePointsTemp += gradePoints * creditHours;
            semesterCreditsTemp += creditHours;

            // Display individual course details for each semester
            cout << "Course " << j + 1 << " - Grade: " << grade
                 << ", Credit Hours: " << creditHours
                 << ", Grade Points: " << gradePoints
                 << ", Total Grade Points: " << gradePoints * creditHours << endl;
        }

        totalGradePointsOverall += semesterGradePointsTemp;
        totalCreditsOverall += semesterCreditsTemp;
    }

    // Calculate overall CGPA
    double cgpa = totalGradePointsOverall / totalCreditsOverall;

    // Display the final CGPA
    cout << "\nOverall CGPA: " << cgpa << endl;

    return 0;
}
Explanation:
Input number of courses: The program first takes input for the number of courses taken by the student in a semester.

Input grade and credit hours for each course: For each course, the program asks the user to input the grade (e.g., A, B, C, etc.) and the number of credit hours.

Grade Points Calculation: The grade points corresponding to each grade (A=4.0, B=3.0, C=2.0, etc.) are calculated using the getGradePoints function.

GPA Calculation: For each semester, it computes the GPA by dividing the total grade points by the total credit hours.

CGPA Calculation: If the student has multiple semesters, the program accumulates the total grade points and credits over the semesters and calculates the overall CGPA.

Display results: It prints the individual course details and the final semester GPA, followed by the overall CGPA.


Input:

Enter the number of courses: 3
Enter grade and credit hours for course 1: A 3
Course 1 - Grade: A, Credit Hours: 3, Grade Points: 4, Total Grade Points: 12
Enter grade and credit hours for course 2: B 4
Course 2 - Grade: B, Credit Hours: 4, Grade Points: 3, Total Grade Points: 12
Enter grade and credit hours for course 3: A 2
Course 3 - Grade: A, Credit Hours: 2, Grade Points: 4, Total Grade Points: 8
Semester GPA: 3.33333
Enter the number of semesters: 1
Overall CGPA: 3.33333
Output:

Overall CGPA: 3.33333


TASK 2: Login and Registration System 
● Create a registration function that takes username and password as input. 
● Validate the inputs and check for duplicate usernames if needed. 
● Store user credentials securely in a file (one file per user or a database file). 
● Implement a login function that reads credentials and verifies user identity. 
● Provide appropriate success or error messages for registration and login.

Source code:
#include <iostream>
#include <fstream>
#include <string>
#include <sstream>
#include <vector>

using namespace std;

struct User {
    string username;
    string password;
};

// Function to validate the username and password
bool isValidUsername(const string& username) {
    // Username must not be empty
    return !username.empty();
}

bool isValidPassword(const string& password) {
    // Password must be at least 6 characters long
    return password.length() >= 6;
}

// Function to check if the username already exists
bool isUsernameTaken(const string& username) {
    ifstream file("users.txt");
    string line;

    while (getline(file, line)) {
        stringstream ss(line);
        string storedUsername, storedPassword;
        ss >> storedUsername >> storedPassword;

        if (storedUsername == username) {
            return true;
        }
    }
    return false;
}

// Registration function to store credentials
void registerUser() {
    string username, password;

    cout << "Enter a username: ";
    cin >> username;

    // Check if username is valid
    if (!isValidUsername(username)) {
        cout << "Invalid username! It cannot be empty.\n";
        return;
    }

    // Check if username is already taken
    if (isUsernameTaken(username)) {
        cout << "Username already taken! Please try another one.\n";
        return;
    }

    cout << "Enter a password (at least 6 characters): ";
    cin >> password;

    // Validate password length
    if (!isValidPassword(password)) {
        cout << "Invalid password! It must be at least 6 characters long.\n";
        return;
    }

    // Store user credentials in the file
    ofstream file("users.txt", ios::app);
    file << username << " " << password << endl;

    cout << "Registration successful!\n";
}

// Login function to verify user credentials
void loginUser() {
    string username, password;

    cout << "Enter your username: ";
    cin >> username;

    cout << "Enter your password: ";
    cin >> password;

    ifstream file("users.txt");
    string line;

    bool loginSuccess = false;

    while (getline(file, line)) {
        stringstream ss(line);
        string storedUsername, storedPassword;
        ss >> storedUsername >> storedPassword;

        if (storedUsername == username && storedPassword == password) {
            loginSuccess = true;
            break;
        }
    }

    if (loginSuccess) {
        cout << "Login successful!\n";
    } else {
        cout << "Invalid username or password!\n";
    }
}

int main() {
    int choice;

    do {
        cout << "Welcome to the Login & Registration System!\n";
        cout << "1. Register\n";
        cout << "2. Login\n";
        cout << "3. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                registerUser();
                break;
            case 2:
                loginUser();
                break;
            case 3:
                cout << "Exiting...\n";
                break;
            default:
                cout << "Invalid choice! Please try again.\n";
        }
    } while (choice != 3);

    return 0;
}
