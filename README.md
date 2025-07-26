# Online Course Management System (OOP)

## Overview
A basic object-oriented system for managing online courses, instructors, students, assignments, and grades.

##  UML Diagram
![Screenshot 2025-07-21 185805-modified (1)_page-0001](https://github.com/user-attachments/assets/dd3393a6-d0c1-4449-a68c-e3e0fa94b9ee)


##  Features
- User Roles: Instructor & Student
- Course creation and enrollment
- Assignment submission and grading
- Role-based dashboard

##  Tech
### JavaScript (ES6+):

```
class User {
    constructor(id, name, email) {
        this._id = id;
        this._name = name;
        this._email = email;
    }

    login() {
        console.log(`${this._name} logged in.`);
    }

    logout() {
        console.log(`${this._name} logged out.`);
    }

    viewDashboard() {
        console.log("Viewing general dashboard.");
    }

    
    get email() {
        return this._email;
    }

    set email(newEmail) {
        if (newEmail.includes("@")) {
            this._email = newEmail;
        } else {
            console.log("Invalid email.");
        }
    }
}


class Student extends User {
    constructor(id, name, email) {
        super(id, name, email);
        this.enrolledCourses = [];
    }

    enrollCourse(course) {
        this.enrolledCourses.push(course);
        course.addStudent(this);
    }

    submitAssignment(assignment, content) {
        assignment.submit(this, content);
    }

    viewDashboard() {
        console.log(`${this._name}'s Student Dashboard`);
    }
}


class Instructor extends User {
    constructor(id, name, email) {
        super(id, name, email);
        this.createdCourses = [];
    }

    createCourse(title) {
        const course = new Course(title, this);
        this.createdCourses.push(course);
        return course;
    }

    gradeAssignment(assignment, student, score) {
        assignment.grade(student, score);
    }

    viewDashboard() {
        console.log(`${this._name}'s Instructor Dashboard`);
    }
}


class Course {
    constructor(title, instructor) {
        this.title = title;
        this.instructor = instructor;
        this.students = [];
        this.assignments = [];
    }

    addStudent(student) {
        this.students.push(student);
    }

    addAssignment(title) {
        const assignment = new Assignment(title, this);
        this.assignments.push(assignment);
        return assignment;
    }
}


class Assignment {
    constructor(title, course) {
        this.title = title;
        this.course = course;
        this.submissions = new Map(); // student → content
        this.grades = new Map();      // student → Grade
    }

    submit(student, content) {
        this.submissions.set(student, content);
        console.log(`${student._name} submitted '${this.title}'`);
    }

    grade(student, score) {
        const grade = new Grade(student, this, score);
        this.grades.set(student, grade);
        console.log(`${student._name} graded with ${score} on '${this.title}'`);
    }
}


class Grade {
    constructor(student, assignment, score) {
        this.student = student;
        this.assignment = assignment;
        this._score = score;
    }

    get score() {
        return this._score;
    }

    set score(newScore) {
        if (newScore >= 0 && newScore <= 100) {
            this._score = newScore;
        } else {
            console.log("Invalid score");
        }
    }
}

```

##  OOP Concepts Used
### Abstraction
We abstracted real-world roles like Student, Instructor, Course, Assignment, and Grade into classes.

Each class exposes only what is necessary (e.g., submit() in Assignment, not how submissions are stored internally).

### Encapsulation
Private properties (_id, _email, _score) are accessed/modified via getter/setter methods.

This protects data from invalid access (e.g., setting invalid email or score).

### Inheritance
Student and Instructor inherit from the User class.

This promotes code reuse (e.g., login, logout, email management) and simplifies maintenance.

 ### Polymorphism
viewDashboard() is overridden in Student and Instructor to provide role-specific behavior.

This demonstrates runtime polymorphism.

##  SOLID Principles
- SRP, OCP, LSP are followed in class designs
-> Single Responsibility:

Each class has one job (e.g., Student handles enrollment, Course manages students/assignments).

-> Open/Closed Principle:

Classes like Assignment and Grade can be extended (e.g., add due dates or comments) without modifying existing code.

-> Liskov Substitution Principle:

Student and Instructor can be used wherever User is expected.

-> Interface Segregation Principle:

Not directly applicable in JavaScript, but methods are logically split per class roles.

-> Dependency Inversion:

While not complex enough here, we design Course to accept instructor/user objects — promoting loose coupling.


# Conclusion:
 
The UML class diagram and the oops concept followed by solid principles are executed successfully.
