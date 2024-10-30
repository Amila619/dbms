-- Final Exam Sit Status Table


CREATE TABLE Final_Exam_Sit_Status (
    Eligibility_ID CHAR(6) PRIMARY KEY,
    Course_Code CHAR(6),
    Status BOOLEAN,
    Attendance_Record_ID CHAR(6),
    Mark_Record_ID CHAR(6),
    Stu_Reg_No CHAR(6),
    FOREIGN KEY (Attendance_Record_ID) REFERENCES Attendance(Attendance_Record_ID),
    FOREIGN KEY (Mark_Record_ID) REFERENCES Mark(Mark_Record_ID) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY (Stu_Reg_No) REFERENCES Student(Stu_Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);

-- Student Table



CREATE TABLE Student (
    Stu_Reg_No CHAR(6) PRIMARY KEY,
    Level CHAR(2), 
    Department VARCHAR(50),
    Academic_Status BOOLEAN,
    First_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    DOB DATE,
    Address VARCHAR(100)
);



-- Technical Officer Table

CREATE TABLE Technical_Officer (
    Staff_ID CHAR(6) PRIMARY KEY,
    Enrollment_Date DATE,
    First_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    DOB DATE,
    Address VARCHAR(100)
);



-- Course Table

CREATE TABLE Course (
    Course_Code CHAR(6) PRIMARY KEY,
    Course_Name VARCHAR(50),
    Theory BOOLEAN,
    Practical BOOLEAN,
    Credit INT,
    Lec_ID CHAR(6),
    FOREIGN KEY(Lec_ID) REFERENCES Lecturer(Lec_ID) ON UPDATE CASCADE ON DELETE CASCADE
);



-- Lecturer Table


CREATE TABLE Lecturer (
    Lec_ID CHAR(6) PRIMARY KEY,
    First_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    DOB DATE,
    Address VARCHAR(100),
    Position VARCHAR(50),
    Department VARCHAR(50),
    Enrollment_Date DATE
);



-- Mark Table


CREATE TABLE Mark (
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
    Stu_Reg_No CHAR(6),
    FOREIGN KEY(Course_Code) REFERENCES Course(Course_Code) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY(Stu_Reg_No) REFERENCES Student(Stu_Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);




-- Attendance Table




CREATE TABLE Attendance (
    Attendance_Record_ID CHAR(6) PRIMARY KEY,
    Course_Code CHAR(6),
    Theory BOOLEAN,
    Practical BOOLEAN,
    Medical_Status BOOLEAN,
    Session_No CHAR(3),
    Date DATE,
    Stu_Reg_No CHAR(6),
    FOREIGN KEY(Course_Code) REFERENCES Course(Course_Code) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY(Stu_Reg_No) REFERENCES Student(Stu_Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);



-- Student Telephone Table




CREATE TABLE Stu_Telephone (
    Stu_Reg_No CHAR(6),
    Telephone VARCHAR(10),
    FOREIGN KEY(Stu_Reg_No) REFERENCES Student(Stu_Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);



-- Lecturer Telephone Table



CREATE TABLE Lec_Telephone (
    Lec_ID CHAR(6),
    Telephone VARCHAR(10),
    FOREIGN KEY(Lec_ID) REFERENCES Lecturer(Lec_ID) ON UPDATE CASCADE ON DELETE CASCADE
);



-- Technical Officer Telephone Table


CREATE TABLE Technical_Telephone (
    Staff_ID CHAR(6),
    Telephone VARCHAR(10),
    FOREIGN KEY(Staff_ID) REFERENCES TechnicalOfficer(Staff_ID) ON UPDATE CASCADE ON DELETE CASCADE
);



-- Student Course Table


CREATE TABLE Std_Course (
    Stu_Reg_No CHAR(6),
    Course_Code CHAR(6),
    PRIMARY KEY (Stu_Reg_No, Course_Code),
    FOREIGN KEY(Course_Code) REFERENCES Course(Course_Code) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY(Stu_Reg_No) REFERENCES Student(Stu_Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);



-- Technical Attendance Table



CREATE TABLE Tech_Attendance (
    Staff_ID CHAR(6),
    Attendance_Record_ID CHAR(6),
    FOREIGN KEY(Attendance_Record_ID) REFERENCES Attendance(Attendance_Record_ID) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY(Staff_ID) REFERENCES TechnicalOfficer(Staff_ID) ON UPDATE CASCADE ON DELETE CASCADE
);



-- GPA Table



CREATE TABLE GPA (
    Record_ID CHAR(6) PRIMARY KEY,
    Semester VARCHAR(20),
    Stu_Reg_No CHAR(6),
    FOREIGN KEY(Stu_Reg_No) REFERENCES Student(Stu_Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);
