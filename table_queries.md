User Table

CREATE TABLE user
(
    User_ID CHAR(6) PRIMARY KEY,
    Full_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    DOB DATE,
    Address VARCHAR(100)
);

Final Exam Sit Status Table

CREATE TABLE final_exam_sit_status(
    Eligibility_ID CHAR(6) PRIMARY KEY,
    Course_Code CHAR(6),
    Status BOOLEAN,
    Attendance_Record_ID CHAR(6),
    Mark_Record_ID CHAR(6),
    FOREIGN KEY (Attendance_Record_ID) REFERENCES Attendance(Attendance_Record_ID),
    FOREIGN KEY (Mark_Record_ID) REFERENCES mark(Mark_Record_ID) ON UPDATE CASCADE ON DELETE CASCADE
);

Student Table

CREATE TABLE student(
    Reg_No CHAR(6) PRIMARY KEY,
    Level CHAR(2), 
    Department VARCHAR(50),
    Academic_Status BOOLEAN,
    First_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    DOB DATE,
    Address VARCHAR(100),
    FOREIGN KEY(Reg_No) REFERENCES user(User_ID) ON UPDATE CASCADE ON DELETE CASCADE
);

Technical Officer Table

CREATE TABLE Technical_Officer(
    Staff_ID CHAR(6) PRIMARY KEY,
    Enrollment_Date DATE,
    First_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    DOB DATE,
    Address VARCHAR(100),
    FOREIGN KEY(Staff_ID) REFERENCES user(User_ID) ON UPDATE CASCADE ON DELETE CASCADE
);

Admin Table

CREATE TABLE Admin(
    Admin_ID CHAR(6) PRIMARY KEY,
    First_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    DOB DATE,
    Address VARCHAR(100),
    FOREIGN KEY(Admin_ID) REFERENCES user(User_ID) ON UPDATE CASCADE ON DELETE CASCADE
);

Course Table

CREATE TABLE course
(
    Course_Code CHAR(6) PRIMARY KEY,
    Course_Name VARCHAR(50),
    Theory BOOLEAN,
    Practical BOOLEAN,
    Credit INT,
    Lec_ID CHAR(6),
    FOREIGN KEY(Lec_ID) REFERENCES lecturer(Lec_ID) ON UPDATE CASCADE ON DELETE CASCADE
);

Lecturer Table

CREATE TABLE lecturer
(
    Lec_ID CHAR(6) PRIMARY KEY,
    First_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    DOB DATE,
    Address VARCHAR(100),
    Position VARCHAR(50),
    Department VARCHAR(50),
    Enrollment_Date DATE,
    FOREIGN KEY(Lec_ID) REFERENCES user(User_ID) ON UPDATE CASCADE ON DELETE CASCADE
);

Mark Table

CREATE TABLE mark
(
    Mark_Record_ID CHAR(5) PRIMARY KEY,
    Course_Code CHAR(6),
    Grade CHAR(1),
    Quiz VARCHAR(3),
    Credit VARCHAR(3),
    CA VARCHAR(4),
    M_Theory VARCHAR(10),
    M_Practical VARCHAR(10),
    F_Theory VARCHAR(10),
    F_Practical VARCHAR(10),
    Reg_No CHAR(6),
    FOREIGN KEY(Course_Code) REFERENCES course(Course_Code) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY(Reg_No) REFERENCES student(Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);

Attendance Table

CREATE TABLE Attendance
(
    Attendance_Record_ID CHAR(6) PRIMARY KEY,
    Course_Code CHAR(6),
    Theory BOOLEAN,
    Practical BOOLEAN,
    Medical_Status BOOLEAN,
    Session_No CHAR(3),
    Date DATE,
    Reg_No CHAR(6),
    FOREIGN KEY(Course_Code) REFERENCES course(Course_Code) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY(Reg_No) REFERENCES student(Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);

User Telephone Table

CREATE TABLE user_telephone
(
    User_ID CHAR(6),
    Telephone VARCHAR(10),
    FOREIGN KEY(User_ID) REFERENCES user(User_ID) ON UPDATE CASCADE ON DELETE CASCADE
);

Student Course Table

CREATE TABLE std_course
(
    Reg_No CHAR(6),
    Course_Code CHAR(6),
    PRIMARY KEY (Reg_No, Course_Code),
    FOREIGN KEY(Course_Code) REFERENCES course(Course_Code) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY(Reg_No) REFERENCES student(Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);

Technical Attendance Table

CREATE TABLE tech_attendance
(
    Staff_ID CHAR(6),
    Attendance_Record_ID CHAR(6),
    FOREIGN KEY(Attendance_Record_ID) REFERENCES Attendance(Attendance_Record_ID) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY(Staff_ID) REFERENCES Technical_Officer(Staff_ID) ON UPDATE CASCADE ON DELETE CASCADE
);

GPA Table

CREATE TABLE gpa
(
    Record_ID CHAR(6) PRIMARY KEY,
    Semester VARCHAR(20),
    Reg_No CHAR(6),
    FOREIGN KEY(Reg_No) REFERENCES student(Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);
