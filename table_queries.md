Final Exam Sit Status

CREATE TABLE final_exam_sit_status(
    Eligibility_ID CHAR(6) Primary Key,
    Course_Code CHAR(6),
    Status BOOLEAN,
    Attendance_Record_ID CHAR(6),
    Mark_Record_ID,
    FOREIGN KEY (Attendance_Record_ID) REFERENCES AS attendance(Attendance_Record_ID),
    FOREIGN KEY (Mark_Record_ID) REFERENCES AS mark(Mark_Record_ID) ON UPDATE CASCADE ON DELETE CASCADE,
)

student

CREATE TABLE student(
    Reg_No CHAR(6) PRIMARY KEY,
    Level CHAR(2), 
    Department VARCHAR(50),
    Academic_Status BOOLEAN,
    First_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    DOB DATE,
    Address VARCHAR(100)
)

technical officer

CREATE TABLE Technical_Officer(
	Staff_ID CHAR(6) PRIMARY KEY,
	Enrollement_Date DATE,
	First_Name VARCHAR(50),
	Last_Name VARCHAR(50),
	DOB DATE,
	Address VARCHAR(100)

)

admin

CREATE TABLE Admin(
	Admin_ID CHAR(6) PRIMARY KEY,
	First_Name VARCHAR(50),
	Last_Name VARCHAR(50),
	DOB DATE,
	Address VARCHAR(100)

)

course

CREATE TABLE course
(
	Course_Code CHAR(6) PRIMARY KEY,
	Course_Name VARCHAR(50),
	Theory BOOLEAN,
	Practical BOOLEAN,
	Credit INT,
	
	FOREIGN KEY(Lec_ID) REFERENCES Lecture(Lec_ID)ON UPDATE CASCADE ON DELETE CASCADE
);


lecturer

CREATE TABLE lecturer
(
	Lec_ID CHAR(6) PRIMARY KEY,
	First_Name VARCHAR(50),
	Last_Name VARCHAR(50),
	DOM DATE,
	Address VARCHAR(100),
	Position VARCHAR(50),
	Department VARCHAR(50),
	Enrolment_Date DATE,
);

mark

CREATE TABLE mark
(
	Mark_Recode_ID CHAR(5) PRIMARY KEY,
	Course_Code CHAR(6),
	Grade VARCHAR(1),
	Quiz VARCHAR(3),
	Credit VARCHAR(3),
	CA VARCHAR(4),
	M_Theory VARCHAR(10),
	M_Patrical VARCHAR(10),
	F_Theory VARCHAR(10),
	F_Patrical VARCHAR(10),
	Reg_No CHAR(6),

	FOREIGN KEY(Course_Code) REFERENCES course(Course_Code) ON UPDATE CASCADE ON DELETE CASCADE,
	FOREIGN KEY(Reg_No) REFERENCES student(Reg_No) ON UPDATE CASCADE ON DELETE CASCADE


);



Attendence

CREATE TABLE Attendence
(
	Attendence_Recode_ID CHAR(5) PRIMARY KEY,
	Course_Code CHAR(6),
	Theory BOOLEAN,
	Patrical BOOLEAN,
	Medical_Status BOOLEAN,
	Session_No char(4),
	Date DATE,
	Reg_No CHAR(4),

	FOREIGN KEY(Course_Code) REFERENCES course(Course_Code)ON UPDATE CASCADE ON DELETE CASCADE,
	FOREIGN KEY(Reg_No) REFERENCES student(Reg_No)ON UPDATE CASCADE ON DELETE CASCADE
);