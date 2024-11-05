-- Granting And Creating Users

-- admin

CREATE USER 'Admin'@'localhost' IDENTIFIED BY 'admin123';


GRANT ALL PRIVILEGES ON lms.* TO 'Admin'@'localhost' WITH GRANT OPTION;

FLUSH PRIVILEGES;



-- dean
 
CREATE USER 'Dean'@'localhost' IDENTIFIED BY 'dean123';

GRANT ALL PRIVILEGES ON lms.* TO 'Dean'@'localhost';

FLUSH PRIVILEGES;

-- lecturer


CREATE USER 'Lecturer_user'@'localhost' IDENTIFIED BY 'lec123';

GRANT ALL PRIVILEGES ON lms.* TO 'Lecturer_user'@'localhost';
FLUSH PRIVILEGES;


-- technicalofficer

CREATE USER 'TechnicalOfficer'@'localhost' IDENTIFIED BY 'tech123';

GRANT SELECT, INSERT, UPDATE ON lms.Attendance TO 'TechnicalOfficer'@'localhost';
GRANT SELECT, INSERT, UPDATE ON lms.Tech_Attendance TO 'TechnicalOfficer'@'localhost';

FLUSH PRIVILEGES;



-- student

CREATE USER 'Student'@'localhost' IDENTIFIED BY 'stu123';

GRANT SELECT ON lms.Attendance TO 'Student'@'localhost';
GRANT SELECT ON lms.Mark TO 'Student'@'localhost';

FLUSH PRIVILEGES;



-- Create Database And Use Database
CREATE DATABASE lms;
USE lms;

-- Student Table
CREATE TABLE Student (
    Stu_Reg_No CHAR(10) PRIMARY KEY, 
    Level CHAR(2), 
    Department ENUM('ICT', 'ET', 'BST', 'MULTI-DISCIPLINARY'),
    Academic_Status ENUM('REPEAT', 'NON-REPEAT', 'SUSPENDED'),
    First_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    DOB DATE,
    Address VARCHAR(100)
);

-- Technical Officer Table
CREATE TABLE Technical_Officer (
    Staff_ID CHAR(10) PRIMARY KEY,
    Enrollment_Date DATE,
    First_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    DOB DATE,
    Address VARCHAR(100)
);

-- Lecturer Table
CREATE TABLE Lecturer (
    Lec_ID CHAR(10) PRIMARY KEY,
    First_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    DOB DATE,
    Address VARCHAR(100),
    Position VARCHAR(50),
    Department ENUM('ICT', 'ET', 'BST', 'MULTI-DISCIPLINARY'),
    Enrollment_Date DATE
);

-- Course Table
CREATE TABLE Course (
    Course_Code CHAR(10) PRIMARY KEY,
    Course_Name VARCHAR(50),
    Theory BOOLEAN,
    Practical BOOLEAN,
    Credit INT,
    Credit_Status ENUM('CREDIT', 'NON-CREDIT'),
    Lec_ID CHAR(10),
    FOREIGN KEY(Lec_ID) REFERENCES Lecturer(Lec_ID) ON UPDATE CASCADE ON DELETE CASCADE
);

-- Mark Table
CREATE TABLE Mark (
    Mark_Record_ID CHAR(10) PRIMARY KEY,
    Course_Code CHAR(10),
    Quiz FLOAT,
    Assignment FLOAT,
    M_Theory FLOAT,
    M_Practical FLOAT,
    F_Theory FLOAT,
    F_Practical FLOAT,
    Stu_Reg_No CHAR(10),
    FOREIGN KEY(Course_Code) REFERENCES Course(Course_Code) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY(Stu_Reg_No) REFERENCES Student(Stu_Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);


-- Attendance Table
CREATE TABLE Attendance (
    Attendance_Record_ID CHAR(10) PRIMARY KEY,
    Course_Code CHAR(10),
    Type ENUM('Theory', 'Practical'),
    Medical_Status ENUM('NOT-APPROVED', 'APPROVED', 'NOT-SUBMITTED'),
    Session_No CHAR(3),
    Date DATE,
    Stu_Reg_No CHAR(10),
    FOREIGN KEY(Course_Code) REFERENCES Course(Course_Code) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY(Stu_Reg_No) REFERENCES Student(Stu_Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);

-- use a CA with final_exam_sit_status as a view
-- Final Exam Sit Status Table
CREATE TABLE Final_Exam_Sit_Status (
    Eligibility_ID CHAR(10) PRIMARY KEY,
    Course_Code CHAR(10),
    Status BOOLEAN,
    Attendance_Record_ID CHAR(10),
    Mark_Record_ID CHAR(10),
    Stu_Reg_No CHAR(10),
    FOREIGN KEY (Attendance_Record_ID) REFERENCES Attendance(Attendance_Record_ID) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY (Mark_Record_ID) REFERENCES Mark(Mark_Record_ID) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY (Stu_Reg_No) REFERENCES Student(Stu_Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);

-- Student Telephone Table
CREATE TABLE Stu_Telephone (
    Stu_Reg_No CHAR(10),
    Telephone VARCHAR(10),
    FOREIGN KEY(Stu_Reg_No) REFERENCES Student(Stu_Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);

-- Lecturer Telephone Table
CREATE TABLE Lec_Telephone (
    Lec_ID CHAR(10),
    Telephone VARCHAR(10),
    FOREIGN KEY(Lec_ID) REFERENCES Lecturer(Lec_ID) ON UPDATE CASCADE ON DELETE CASCADE
);

-- Technical Officer Telephone Table
CREATE TABLE Technical_Telephone (
    Staff_ID CHAR(10),
    Telephone VARCHAR(10),
    FOREIGN KEY(Staff_ID) REFERENCES Technical_Officer(Staff_ID) ON UPDATE CASCADE ON DELETE CASCADE
);

-- Student Course Table
CREATE TABLE Std_Course (
    Stu_Reg_No CHAR(10),
    Course_Code CHAR(10),
    PRIMARY KEY (Stu_Reg_No, Course_Code),
    FOREIGN KEY(Course_Code) REFERENCES Course(Course_Code) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY(Stu_Reg_No) REFERENCES Student(Stu_Reg_No) ON UPDATE CASCADE ON DELETE CASCADE
);

-- Technical Attendance Table
CREATE TABLE Tech_Attendance (
    Staff_ID CHAR(10),
    Attendance_Record_ID CHAR(10),
    FOREIGN KEY(Attendance_Record_ID) REFERENCES Attendance(Attendance_Record_ID) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY(Staff_ID) REFERENCES Technical_Officer(Staff_ID) ON UPDATE CASCADE ON DELETE CASCADE
);

-- Insert Data

INSERT INTO Technical_Officer (Staff_ID, Enrollment_Date, First_Name, Last_Name, DOB, Address)
VALUES
    ('TO_3456', '2017-04-15', 'Anura', 'Perera', '1985-06-15', 'Rajagiriya, Buthgamuwa Road, Colombo'),
    ('TO_3457', '2018-01-20', 'Ruwan', 'Fernando', '1988-02-12', 'Manipay, Jaffna-Kandy Road, Jaffna'),
    ('TO_3458', '2019-03-25', 'Dilini', 'Ekanayake', '1990-11-07', 'Katugasthota, Kandy Road, Kandy'),
    ('TO_3459', '2020-05-30', 'Sajith', 'Jayawardena', '1987-10-10', 'Wellawaya, Monaragala Road, Monaragala'),
    ('TO_3460', '2016-08-18', 'Kasun', 'De Silva', '1985-09-20', 'Aluthgama, Galle Road, Kalutara'),
    ('TO_3461', '2021-07-11', 'Nisansala', 'Kumari', '1992-03-27', 'BandarawAPPROVED, Haputale Road, Badulla'),
    ('TO_3462', '2018-10-22', 'Chathura', 'Wickramasinghe', '1984-12-05', 'Panadura, Galle Road, Colombo'),
    ('TO_3463', '2019-12-15', 'Lakshan', 'Herath', '1986-04-18', 'Peradeniya, Mahakanda Road, Kandy'),
    ('TO_3464', '2022-02-28', 'Tharindu', 'Samarasinghe', '1989-07-01', 'Deniyaya, Kotapola Road, Matara'),
    ('TO_3465', '2017-11-05', 'Kumudu', 'Weerasinghe', '1983-05-13', 'Homagama, High Level Road, Colombo');

INSERT INTO Student (Stu_Reg_No, Level, Department, Academic_Status, First_Name, Last_Name, DOB, Address)
VALUES
    ('TG1486', 'I', 'ICT', 'NON-REPEAT', 'Sajith', 'Perera', '2002-05-18', 'Mawathagama, Alawwa Road, Kurunegala'),
    ('TG1381', 'I', 'ICT', 'REPEAT', 'Anjali', 'Fernando', '2001-11-23', 'Piliyandala, Horana Road, Colombo'),
    ('TG1511', 'I', 'ICT', 'NON-REPEAT', 'Nimal', 'Wijesinghe', '2003-01-15', 'KaduruwAPPROVED, Polonnaruwa Road, Polonnaruwa'),
    ('TG1002', 'I', 'ICT', 'NON-REPEAT', 'Ishara', 'Jayasinghe', '2000-07-30', 'KAPPROVEDniya, Pethiyagoda Road, Colombo'),
    ('TG1534', 'I', 'ICT', 'REPEAT', 'Kasuni', 'Senanayake', '1999-09-05', 'Ambalangoda, Galle Road, Galle'),
    ('TG1367', 'I', 'ICT', 'NON-REPEAT', 'Chamath', 'De Silva', '1998-03-17', 'Mawanella, Kegalle Road, Kegalle'),
    ('TG1548', 'I', 'ICT', 'REPEAT', 'Amaya', 'Dissanayake', '1999-06-25', 'Talawakele, Nuwara Eliya Road, Nuwara Eliya'),
    ('TG1342', 'I', 'ICT', 'NON-REPEAT', 'Pasan', 'Rathnayake', '2003-02-12', 'Nugegoda, Pagoda Road, Colombo'),
    ('TG1256', 'I', 'ICT', 'REPEAT', 'Supun', 'Gunawardena', '2001-12-22', 'Kalutara, Nagoda Road, Kalutara'),
    ('TG1490', 'I', 'ICT', 'NON-REPEAT', 'Nadeeka', 'Wickramasinghe', '1998-05-10', 'Matale, UkuwAPPROVED Road, Matale'),
    ('TG1682', 'I', 'ICT', 'NON-REPEAT', 'Harshana', 'Gunarathne', '2002-08-14', 'Hikkaduwa, Galle Road, Galle'),
    ('TG1745', 'I', 'ICT', 'NON-REPEAT', 'Chathura', 'Lakmal', '2001-02-19', 'Moratuwa, Station Road, Kalutara'),
    ('TG1573', 'I', 'ICT', 'NON-REPEAT', 'Dilshan', 'Ratnayake', '1999-04-09', 'Wattala, Kandy Road, Gampaha'),
    ('TG1821', 'I', 'ICT', 'NON-REPEAT', 'Sampath', 'Karunaratne', '2003-06-30', 'Gampaha, Colombo Road, Gampaha'),
    ('TG1995', 'I', 'ICT', 'NON-REPEAT', 'Kavisha', 'Fernando', '2000-11-12', 'Dehiwala, Galle Road, Colombo'),
	('TG1505', 'I', 'ICT', 'SUSPENDED', 'Nadeesha', 'Perera', '2001-04-15', 'Kandy Road, Kadawatha, Gampaha'),
	('TG1598', 'I', 'ICT', 'SUSPENDED', 'Ravindu', 'Wijesinghe', '2002-09-25', 'Negombo Road, Chilaw, Puttalam');


INSERT INTO Lecturer (Lec_ID, First_Name, Last_Name, DOB, Address, Position, Department, Enrollment_Date)
VALUES
    ('LEC_1001', 'Nuwan', 'Perera', '1985-04-15', 'Colombo 07, University Avenue, Colombo', 'Assistant', 'ET', '2015-03-01'),
    ('LEC_1002', 'Kumari', 'Jayasinghe', '1987-09-12', 'Mahawa, Kandy Road, Kandy', 'Lecturer', 'MULTI-DISCIPLINARY', '2016-08-15'),
    ('LEC_1003', 'Amila', 'Fernando', '1980-06-20', 'Matara, Dikwella Road, Matara', 'Lecturer', 'ICT', '2012-01-10'),
    ('LEC_1004', 'Sajith', 'Senanayake', '1982-11-14', 'Colombo 03, Marine Drive, Colombo', 'Senior Lecturer', 'BST', '2013-05-15');
    ('LEC_1005', 'Nilanka', 'Bandara', '1976-12-10', 'Nallur, KKS Road, Jaffna', 'Associate Officer', 'ICT', '2014-04-22'),
    ('LEC_1006', 'Pasan', 'Rathnayake', '1982-03-05', 'Wattala, Negombo Road, Gampaha', 'Lecturer', 'ICT', '2018-07-19'),
    ('LEC_1007', 'Sanduni', 'Dissanayake', '1990-05-30', 'PolgahawAPPROVED, Railway Avenue, Kurunegala', 'HOD', 'ICT', '2019-02-27'),
    ('LEC_1008', 'Kavindu', 'Herath', '1989-11-18', 'Kekirawa, Mihintale Road, Anuradhapura', 'Lecturer', 'ICT', '2015-11-08'),
    ('LEC_1009', 'Nadeeka', 'Rajapaksha', '1981-07-14', 'Diyathalawa, Colombo Road, Badulla', 'Dean', 'ICT', '2013-09-05'),
    ('LEC_1010', 'Tharindu', 'Silva', '1984-07-22', 'Mirigama, Kurunegala Road, Mirigama', 'Senior Lecturer', 'ET', '2014-05-20'),
    ('LEC_1011', 'Rashmi', 'Kumari', '1986-02-18', 'Katunayake, Airport Road, Gampaha', 'Lecturer', 'BST', '2015-09-10'),
    ('LEC_1012', 'Kasun', 'Weerasinghe', '1979-11-30', 'Tangalle, Beach Road, Hambantota', 'Lecturer', 'MULTI-DISCIPLINARY', '2016-03-22'),
    ('LEC_1013', 'Janith', 'Wickramasinghe', '1983-08-25', 'Koggala, Galle Road, Galle', 'Lecturer', 'ICT', '2017-12-01'),
    ('LEC_1014', 'Dilshan', 'Peiris', '1988-01-15', 'Kadawatha, Kandy Road, Gampaha', 'Assistant', 'BST', '2018-06-12');

INSERT INTO Course (Course_Code, Course_Name, Theory, Practical, Credit, Credit_Status, Lec_ID)
VALUES
    ('ICT1212', 'Database Management Systems', TRUE, FALSE, 2, 'CREDIT', 'LEC_1006'),
    ('ICT1222', 'Database Management Systems Practicum', TRUE, TRUE, 3, 'CREDIT', 'LEC_1007'),
    ('ICT1233', 'Server-Side Web Development', TRUE, TRUE, 3, 'CREDIT', 'LEC_1008'),
    ('ICT1242', 'Computer Architecture', TRUE, FALSE, 2, 'CREDIT', 'LEC_1005'),
    ('ICT1253', 'Computer Networks', TRUE, FALSE, 3, 'CREDIT', 'LEC_1009'),
    ('TMS1233', 'Discrete Mathematics', TRUE, FALSE, 3, 'CREDIT', 'LEC_1001'),
    ('TCS1212', 'Fundamentals of Management', TRUE, FALSE, 2, 'CREDIT', 'LEC_1002'),
    ('ENG1212', 'English II', TRUE, FALSE, 2, 'NON-CREDIT', 'LEC_1012');

INSERT INTO Stu_Telephone (Stu_Reg_No, Telephone)
VALUES
    ('TG1486', '0711234567'),
    ('TG1381', '0772345678'),
    ('TG1511', '0753456789'),
    ('TG1002', '0724567890'),
    ('TG1534', '0765678901'),
    ('TG1367', '0706789012'),
    ('TG1548', '0717890123'),
    ('TG1342', '0778901234'),
    ('TG1256', '0759012345'),
    ('TG1490', '0720123456'),
    ('TG1682', '0761234567'),
    ('TG1745', '0702345678'),
    ('TG1573', '0773456789'),
    ('TG1821', '0754567890'),
    ('TG1995', '0715678901'),
	('TG1505', '0773116789'),
	('TG1598', '0773356689');

INSERT INTO Lec_Telephone (Lec_ID, Telephone)
VALUES
    ('LEC_1001', '0712345678'),
    ('LEC_1002', '0773456789'),
    ('LEC_1003', '0754567890'),
    ('LEC_1004', '0725678901'),
    ('LEC_1005', '0766789012'),
    ('LEC_1006', '0707890123'),
    ('LEC_1007', '0718901234'),
    ('LEC_1008', '0779012345'),
    ('LEC_1009', '0750123456'),
    ('LEC_1010', '0721234567'),
    ('LEC_1011', '0762345678'),
	('LEC_1012', '0750123355'),
	('LEC_1013', '0776688012'),
	('LEC_1014', '0762335688');

INSERT INTO Technical_Telephone (Staff_ID, Telephone)
VALUES
    ('TO_3456', '0713456789'),
    ('TO_3457', '0774567890'),
    ('TO_3458', '0755678901'),
    ('TO_3459', '0726789012'),
    ('TO_3460', '0767890123'),
    ('TO_3461', '0708901234'),
    ('TO_3462', '0719012345'),
    ('TO_3463', '0770123456'),
    ('TO_3464', '0751234567'),
    ('TO_3465', '0722345678');


INSERT INTO Mark (Mark_Record_ID, Course_Code, Quiz, Assignment, M_Theory, M_Practical, F_Theory, F_Practical, Stu_Reg_No)
VALUES
    
	-- Marks for Student TG1486
	('MRK001', 'ICT1212', 12.0, 1.5, 28.0, 10.0, 40.0, 8.5, 'TG1486'),
    ('MRK002', 'ICT1222', 10.5, 2.0, 25.0, 8.0, 35.0, 19.5, 'TG1486'),
    ('MRK003', 'ICT1233', 11.0, 3.0, 27.0, 5.0, 40.0, 14.0, 'TG1486'),
    ('MRK004', 'ICT1242', 9.0, 4.0, 20.0, 6.0, 25.0, 28.0, 'TG1486'),
    ('MRK005', 'ICT1253', 8.5, 5.0, 30.0, 0.0, 50.0, 6.5, 'TG1486'),
    ('MRK006', 'TMS1233', 15.0, 3.0, 25.0, 0.0, 57.0, 0.0, 'TG1486'),
    ('MRK007', 'TCS1212', 13.0, 1.0, 29.0, 0.0, 57.0, 0.0, 'TG1486'),
    ('MRK008', 'ENG1212', 10.5, 2.0, 28.0, 0.0, 58.5, 0.0, 'TG1486'),

    ('MRK009', 'ICT1212', 11.5, 1.5, 30.0, 8.0, 35.0, 14.0, 'TG1511'),
    ('MRK010', 'ICT1222', 12.5, 2.0, 25.0, 0.0, 40.0, 20.5, 'TG1511'),
    ('MRK011', 'ICT1233', 9.0, 4.0, 20.0, 10.0, 30.0, 27.0, 'TG1511'),
    ('MRK012', 'ICT1242', 10.0, 3.5, 22.0, 5.0, 37.5, 17.0, 'TG1511'),
    ('MRK013', 'ICT1253', 7.0, 5.0, 25.0, 10.0, 38.0, 15.0, 'TG1511'),
    ('MRK014', 'TMS1233', 14.0, 3.0, 28.0, 0.0, 55.0, 0.0, 'TG1511'),
    ('MRK015', 'TCS1212', 12.0, 2.5, 27.0, 0.0, 58.5, 0.0, 'TG1511'),
    ('MRK016', 'ENG1212', 10.5, 3.0, 25.0, 2.0, 51.5, 0.0, 'TG1511'),

    ('MRK017', 'ICT1212', 10.0, 1.0, 29.0, 10.0, 35.0, 15.0, 'TG1002'),
    ('MRK018', 'ICT1222', 8.5, 2.0, 27.0, 12.0, 40.0, 10.5, 'TG1002'),
    ('MRK019', 'ICT1233', 9.5, 2.5, 26.0, 9.0, 30.0, 22.0, 'TG1002'),
    ('MRK020', 'ICT1242', 7.0, 5.0, 28.0, 0.0, 50.0, 10.0, 'TG1002'),
    ('MRK021', 'ICT1253', 11.0, 4.0, 30.0, 0.0, 50.0, 5.0, 'TG1002'),
    ('MRK022', 'TMS1233', 15.0, 3.0, 25.0, 0.0, 57.0, 0.0, 'TG1002'),
    ('MRK023', 'TCS1212', 12.0, 1.5, 28.0, 0.0, 58.5, 0.0, 'TG1002'),
    ('MRK024', 'ENG1212', 10.5, 2.0, 28.0, 0.0, 59.5, 0.0, 'TG1002'),

	('MRK025', 'ICT1212', 10.5, 3.0, 30.0, 5.0, 35.0, 15.5, 'TG1534'),
    ('MRK026', 'ICT1222', 8.0, 2.5, 27.0, 6.0, 40.0, 16.5, 'TG1534'),
    ('MRK027', 'ICT1233', 9.0, 1.0, 29.0, 0.0, 40.0, 20.0, 'TG1534'),
    ('MRK028', 'ICT1242', 11.0, 4.0, 28.0, 2.0, 40.0, 15.0, 'TG1534'),
    ('MRK029', 'ICT1253', 13.0, 2.0, 20.0, 0.0, 30.0, 35.0, 'TG1534'),
    ('MRK030', 'TMS1233', 12.0, 2.5, 25.0, 0.0, 50.5, 10.0, 'TG1534'),
    ('MRK031', 'TCS1212', 14.5, 1.5, 20.0, 0.0, 60.0, 3.0, 'TG1534'),
    ('MRK032', 'ENG1212', 10.0, 2.0, 25.0, 0.0, 55.0, 8.0, 'TG1534'),

    ('MRK033', 'ICT1212', 11.5, 2.0, 24.0, 5.0, 30.0, 27.5, 'TG1367'),
    ('MRK034', 'ICT1222', 9.5, 3.0, 28.0, 7.0, 32.0, 20.5, 'TG1367'),
    ('MRK035', 'ICT1233', 12.0, 4.0, 25.0, 3.0, 40.0, 16.0, 'TG1367'),
    ('MRK036', 'ICT1242', 10.0, 2.5, 22.0, 8.0, 35.5, 22.0, 'TG1367'),
    ('MRK037', 'ICT1253', 11.0, 5.0, 27.0, 0.0, 40.0, 17.0, 'TG1367'),
    ('MRK038', 'TMS1233', 13.5, 2.0, 30.0, 0.0, 50.0, 4.5, 'TG1367'),
    ('MRK039', 'TCS1212', 10.0, 1.5, 25.0, 0.0, 50.5, 13.0, 'TG1367'),
    ('MRK040', 'ENG1212', 12.0, 3.0, 20.0, 0.0, 50.0, 15.0, 'TG1367'),

    ('MRK041', 'ICT1212', 9.5, 4.0, 26.0, 10.0, 30.0, 20.5, 'TG1548'),
    ('MRK042', 'ICT1222', 10.0, 2.5, 28.0, 5.5, 30.0, 23.0, 'TG1548'),
    ('MRK043', 'ICT1233', 11.0, 3.0, 29.0, 2.0, 35.0, 20.0, 'TG1548'),
    ('MRK044', 'ICT1242', 12.0, 5.0, 25.0, 0.0, 30.0, 28.0, 'TG1548'),
    ('MRK045', 'ICT1253', 13.0, 1.5, 27.0, 0.0, 45.5, 13.0, 'TG1548'),
    ('MRK046', 'TMS1233', 10.5, 2.0, 30.0, 0.0, 52.5, 5.0, 'TG1548'),
    ('MRK047', 'TCS1212', 11.5, 3.0, 28.0, 0.0, 50.0, 7.5, 'TG1548'),
    ('MRK048', 'ENG1212', 10.0, 1.0, 25.0, 0.0, 45.0, 19.0, 'TG1548'),

    ('MRK049', 'ICT1212', 8.0, 2.5, 27.0, 6.0, 30.0, 26.5, 'TG1342'),
    ('MRK050', 'ICT1222', 10.5, 1.5, 28.0, 4.0, 36.0, 20.0, 'TG1342'),
    ('MRK051', 'ICT1233', 13.0, 2.0, 25.0, 0.0, 40.0, 20.0, 'TG1342'),
    ('MRK052', 'ICT1242', 9.5, 3.0, 24.0, 5.0, 27.5, 31.0, 'TG1342'),
    ('MRK053', 'ICT1253', 11.0, 2.5, 30.0, 0.0, 35.0, 21.5, 'TG1342'),
    ('MRK054', 'TMS1233', 14.0, 1.0, 26.0, 0.0, 51.0, 8.0, 'TG1342'),
    ('MRK055', 'TCS1212', 12.0, 2.0, 28.0, 0.0, 58.0, 0.0, 'TG1342'),
    ('MRK056', 'ENG1212', 11.5, 1.5, 24.0, 0.0, 52.0, 11.0, 'TG1342'),

    ('MRK057', 'ICT1212', 9.5, 3.5, 28.0, 0.0, 41.0, 18.0, 'TG1256'),
    ('MRK058', 'ICT1222', 10.5, 2.0, 25.0, 0.0, 35.0, 27.5, 'TG1256'),
    ('MRK059', 'ICT1233', 8.5, 2.5, 29.0, 5.0, 37.0, 18.0, 'TG1256'),
    ('MRK060', 'ICT1242', 11.0, 1.0, 30.0, 0.0, 41.0, 17.0, 'TG1256'),
    ('MRK061', 'ICT1253', 13.0, 3.0, 25.0, 0.0, 38.0, 21.0, 'TG1256'),
    ('MRK062', 'TMS1233', 12.0, 2.0, 27.0, 0.0, 55.0, 4.0, 'TG1256'),
    ('MRK063', 'TCS1212', 14.5, 1.5, 24.0, 0.0, 60.0, 0.0, 'TG1256'),
    ('MRK064', 'ENG1212', 11.5, 2.0, 28.0, 0.0, 50.5, 8.0, 'TG1256'),

	('MRK065', 'ICT1212', 8.0, 4.5, 27.5, 2.0, 36.0, 22.0, 'TG1490'),
	('MRK066', 'ICT1222', 10.5, 3.0, 25.0, 1.5, 39.0, 21.0, 'TG1490'),
	('MRK067', 'ICT1233', 7.0, 4.0, 24.5, 3.5, 35.0, 26.0, 'TG1490'),
	('MRK068', 'ICT1242', 12.0, 2.5, 28.0, 1.0, 40.5, 16.0, 'TG1490'),
	('MRK069', 'ICT1253', 11.0, 5.0, 23.0, 0.0, 37.0, 24.0, 'TG1490'),
	('MRK070', 'TMS1233', 9.5, 3.0, 26.0, 2.0, 34.5, 25.0, 'TG1490'),
	('MRK071', 'TCS1212', 13.0, 2.5, 20.0, 0.0, 42.5, 22.0, 'TG1490'),
	('MRK072', 'ENG1212', 10.0, 4.0, 22.5, 1.5, 38.0, 24.0, 'TG1490'),

	('MRK073', 'ICT1212', 11.0, 3.0, 22.5, 0.5, 40.0, 23.0, 'TG1682'),
	('MRK074', 'ICT1222', 9.5, 2.0, 24.0, 1.5, 36.0, 27.0, 'TG1682'),
	('MRK075', 'ICT1233', 12.5, 1.5, 26.5, 0.0, 38.0, 21.5, 'TG1682'),
	('MRK076', 'ICT1242', 10.0, 3.5, 25.0, 2.0, 39.5, 20.0, 'TG1682'),
	('MRK077', 'ICT1253', 8.5, 4.0, 27.0, 0.0, 35.5, 25.0, 'TG1682'),
	('MRK078', 'TMS1233', 13.0, 2.0, 28.0, 1.0, 41.0, 15.0, 'TG1682'),
	('MRK079', 'TCS1212', 10.5, 1.5, 29.5, 0.0, 34.5, 24.0, 'TG1682'),
	('MRK080', 'ENG1212', 9.0, 3.5, 23.0, 2.5, 42.0, 20.0, 'TG1682'),

	('MRK081', 'ICT1212', 10.0, 3.0, 24.5, 2.0, 35.0, 25.5, 'TG1745'),
	('MRK082', 'ICT1222', 12.0, 2.5, 23.0, 0.0, 40.0, 22.5, 'TG1745'),
	('MRK083', 'ICT1233', 9.5, 4.5, 28.0, 1.0, 37.0, 20.0, 'TG1745'),
	('MRK084', 'ICT1242', 11.0, 1.5, 26.0, 0.0, 41.5, 20.0, 'TG1745'),
	('MRK085', 'ICT1253', 8.5, 3.0, 25.0, 1.0, 36.0, 26.5, 'TG1745'),
	('MRK086', 'TMS1233', 13.0, 2.0, 27.5, 0.0, 39.0, 18.5, 'TG1745'),
	('MRK087', 'TCS1212', 10.0, 3.5, 21.0, 0.0, 42.5, 23.0, 'TG1745'),
	('MRK088', 'ENG1212', 9.5, 2.5, 22.0, 1.5, 35.5, 29.0, 'TG1745'),

	('MRK089', 'ICT1212', 9.0, 4.0, 26.5, 0.0, 35.0, 25.5, 'TG1381'),
	('MRK090', 'ICT1222', 11.0, 3.5, 24.0, 1.0, 37.0, 23.5, 'TG1381'),
	('MRK091', 'ICT1233', 8.5, 2.5, 27.0, 3.0, 35.0, 23.0, 'TG1381'),
	('MRK092', 'ICT1242', 10.5, 2.0, 28.0, 0.5, 36.0, 23.0, 'TG1381'),
	('MRK093', 'ICT1253', 12.0, 3.0, 20.5, 2.0, 39.5, 22.0, 'TG1381'),
	('MRK094', 'TMS1233', 10.0, 2.5, 26.0, 0.0, 34.5, 27.0, 'TG1381'),
	('MRK095', 'TCS1212', 13.5, 1.5, 24.0, 1.0, 38.0, 22.0, 'TG1381'),
	('MRK096', 'ENG1212', 9.0, 3.5, 22.5, 0.5, 36.0, 28.5, 'TG1381'),

	('MRK097', 'ICT1212', 10.5, 3.0, 25.0, 0.0, 36.0, 25.5, 'TG1573'),
	('MRK098', 'ICT1222', 11.5, 2.5, 27.0, 1.0, 34.0, 24.0, 'TG1573'),
	('MRK099', 'ICT1233', 9.0, 4.0, 28.5, 0.5, 35.0, 23.0, 'TG1573'),
	('MRK100', 'ICT1242', 12.0, 1.0, 24.0, 0.0, 37.5, 25.5, 'TG1573'),
	('MRK101', 'ICT1253', 10.0, 3.5, 22.5, 1.5, 39.0, 23.5, 'TG1573'),
	('MRK102', 'TMS1233', 13.0, 2.0, 26.0, 0.0, 38.0, 21.0, 'TG1573'),
	('MRK103', 'TCS1212', 10.0, 2.5, 25.0, 1.5, 36.5, 24.5, 'TG1573'),
	('MRK104', 'ENG1212', 11.0, 3.0, 23.0, 0.5, 40.0, 22.5, 'TG1573'),

	('MRK105', 'ICT1212', 10.0, 2.5, 24.0, 1.0, 35.5, 27.0, 'TG1821'),
	('MRK106', 'ICT1222', 12.0, 3.0, 22.5, 0.5, 34.0, 28.0, 'TG1821'),
	('MRK107', 'ICT1233', 11.5, 2.0, 25.0, 0.0, 36.5, 25.0, 'TG1821'),
	('MRK108', 'ICT1242', 9.0, 3.5, 28.0, 1.5, 37.0, 21.0, 'TG1821'),
	('MRK109', 'ICT1253', 13.0, 1.5, 23.0, 0.0, 39.0, 23.5, 'TG1821'),
	('MRK110', 'TMS1233', 10.5, 3.0, 27.0, 0.0, 35.0, 24.5, 'TG1821'),
	('MRK111', 'TCS1212', 14.0, 1.0, 24.5, 0.0, 36.0, 24.5, 'TG1821'),
	('MRK112', 'ENG1212', 11.0, 2.5, 26.0, 0.5, 37.5, 22.5, 'TG1821'),

	('MRK113', 'ICT1212', 9.0, 4.0, 25.0, 0.0, 38.0, 24.0, 'TG1995'),
	('MRK114', 'ICT1222', 12.5, 3.5, 27.0, 1.0, 35.0, 21.0, 'TG1995'),
	('MRK115', 'ICT1233', 10.0, 2.5, 28.0, 0.5, 36.0, 22.0, 'TG1995'),
	('MRK116', 'ICT1242', 11.0, 1.5, 23.0, 0.0, 40.0, 23.0, 'TG1995'),
	('MRK117', 'ICT1253', 8.5, 5.0, 25.0, 2.0, 34.0, 25.0, 'TG1995'),
	('MRK118', 'TMS1233', 13.0, 2.0, 26.0, 0.0, 39.0, 20.0, 'TG1995'),
	('MRK119', 'TCS1212', 10.0, 3.0, 29.0, 0.5, 37.0, 20.5, 'TG1995'),
	('MRK120', 'ENG1212', 11.5, 2.5, 24.0, 0.0, 39.5, 23.5, 'TG1995');



INSERT INTO Attendance (Attendance_Record_ID, Course_Code, Type, Medical_Status, Session_No, Date, Stu_Reg_No)
VALUES

    ('ATD001', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-11', 'TG1381'),
	('ATD002', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-12', 'TG1381'),
	('ATD003', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-13', 'TG1381'),
	('ATD004', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-14', 'TG1381'),
	('ATD005', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-15', 'TG1381'),
	('ATD006', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-16', 'TG1381'),
	('ATD007', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-17', 'TG1381'),
	('ATD008', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-18', 'TG1381'),

	('ATD009', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1511'),
	('ATD010', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1511'),
	('ATD011', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1511'),
	('ATD012', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1511'),
	('ATD013', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1511'),
	('ATD014', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1511'),
	('ATD015', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1511'),
	('ATD016', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1511'),

	('ATD017', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1002'),
	('ATD018', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1002'),
	('ATD019', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1002'),
	('ATD020', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1002'),
	('ATD021', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1002'),
	('ATD022', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1002'),
	('ATD023', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1002'),
	('ATD024', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1002'),

	('ATD025', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1534'),
	('ATD026', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1534'),
	('ATD027', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1534'),
	('ATD028', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1534'),
	('ATD029', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1534'),
	('ATD030', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1534'),
	('ATD031', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1534'),
	('ATD032', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1534'),

	('ATD033', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1367'),
	('ATD034', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1367'),
	('ATD035', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1367'),
	('ATD036', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1367'),
	('ATD037', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1367'),
	('ATD038', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1367'),
	('ATD039', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1367'),
	('ATD040', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1367'),

	('ATD041', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1548'),
	('ATD042', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1548'),
	('ATD043', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1548'),
	('ATD044', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1548'),
	('ATD045', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1548'),
	('ATD046', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1548'),
	('ATD047', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1548'),
	('ATD048', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1548'),

	('ATD049', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1342'),
	('ATD050', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1342'),
	('ATD051', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1342'),
	('ATD052', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1342'),
	('ATD053', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1342'),
	('ATD054', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1342'),
	('ATD055', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1342'),
	('ATD056', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1342'),

	('ATD057', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1256'),
	('ATD058', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1256'),
	('ATD059', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1256'),
	('ATD060', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1256'),
	('ATD061', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1256'),
	('ATD062', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1256'),
	('ATD063', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1256'),
	('ATD064', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1256'),

	('ATD065', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1490'),
	('ATD066', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1490'),
	('ATD067', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1490'),
	('ATD068', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1490'),
	('ATD069', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1490'),
	('ATD070', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1490'),
	('ATD071', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1490'),
	('ATD072', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1490'),

	('ATD073', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1682'),
	('ATD074', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1682'),
	('ATD075', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1682'),
	('ATD076', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1682'),
	('ATD077', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1682'),
	('ATD078', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1682'),
	('ATD079', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1682'),
	('ATD080', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1682'),

	('ATD081', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1745'),
	('ATD082', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1745'),
	('ATD083', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1745'),
	('ATD084', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1745'),
	('ATD085', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1745'),
	('ATD086', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1745'),
	('ATD087', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1745'),
	('ATD088', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1745'),

	('ATD089', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1573'),
	('ATD090', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1573'),
	('ATD091', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1573'),
	('ATD092', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1573'),
	('ATD093', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1573'),
	('ATD094', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1573'),
	('ATD095', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1573'),
	('ATD096', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1573'),

	('ATD097', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1821'),
	('ATD098', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1821'),
	('ATD099', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1821'),
	('ATD100', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1821'),
	('ATD101', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1821'),
	('ATD102', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1821'),
	('ATD103', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1821'),
	('ATD104', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1821'),

	('ATD105', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-01', 'TG1995'),
	('ATD106', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '01', '2024-01-02', 'TG1995'),
	('ATD107', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-03', 'TG1995'),
	('ATD108', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-04', 'TG1995'),
	('ATD109', 'ICT1253', 'Theory', 'APPROVED', '01', '2024-01-05', 'TG1995'),
	('ATD110', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-06', 'TG1995'),
	('ATD111', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-07', 'TG1995'),
	('ATD112', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '01', '2024-01-08', 'TG1995'),



    ('ATD113', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1381'),
	('ATD114', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1381'),
	('ATD115', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1381'),
	('ATD116', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1381'),
	('ATD117', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1381'),
	('ATD118', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1381'),
	('ATD119', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1381'),
	('ATD120', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1381'),

	('ATD121', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1511'),
	('ATD122', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1511'),
	('ATD123', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1511'),
	('ATD124', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1511'),
	('ATD125', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1511'),
	('ATD126', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1511'),
	('ATD127', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1511'),
	('ATD128', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1511'),

	('ATD129', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1002'),
	('ATD130', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1002'),
	('ATD131', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1002'),
	('ATD132', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1002'),
	('ATD133', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1002'),
	('ATD134', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1002'),
	('ATD135', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1002'),
	('ATD136', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1002'),

	('ATD137', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1534'),
	('ATD138', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1534'),
	('ATD139', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1534'),
	('ATD140', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1534'),
	('ATD141', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1534'),
	('ATD142', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1534'),
	('ATD143', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1534'),
	('ATD144', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1534'),

    ('ATD145', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1367'),
	('ATD146', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1367'),
	('ATD147', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1367'),
	('ATD148', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1367'),
	('ATD149', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1367'),
	('ATD150', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1367'),
	('ATD151', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1367'),
	('ATD152', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1367'),

	('ATD153', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1548'),
	('ATD154', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1548'),
	('ATD155', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1548'),
	('ATD156', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1548'),
	('ATD157', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1548'),
	('ATD158', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1548'),
	('ATD159', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1548'),
	('ATD160', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1548'),

	('ATD161', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1342'),
	('ATD162', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1342'),
	('ATD163', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1342'),
	('ATD164', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1342'),
	('ATD165', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1342'),
	('ATD166', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1342'),
	('ATD167', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1342'),
	('ATD168', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1342'),

	('ATD169', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1256'),
	('ATD170', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1256'),
	('ATD171', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1256'),
	('ATD172', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1256'),
	('ATD173', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1256'),
	('ATD174', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1256'),
	('ATD175', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1256'),
	('ATD176', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1256'),

    ('ATD177', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1490'),
	('ATD178', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1490'),
	('ATD179', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1490'),
	('ATD180', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1490'),
	('ATD181', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1490'),
	('ATD182', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1490'),
	('ATD183', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1490'),
	('ATD184', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1490'),

	('ATD185', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1682'),
	('ATD186', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1682'),
	('ATD187', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1682'),
	('ATD188', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1682'),
	('ATD189', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1682'),
	('ATD190', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1682'),
	('ATD191', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1682'),
	('ATD192', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1682'),

	('ATD193', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1745'),
	('ATD194', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1745'),
	('ATD195', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1745'),
	('ATD196', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1745'),
	('ATD197', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1745'),
	('ATD198', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1745'),
	('ATD199', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1745'),
	('ATD200', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1745'),

	('ATD201', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1573'),
	('ATD202', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1573'),
	('ATD203', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1573'),
	('ATD204', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1573'),
	('ATD205', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1573'),
	('ATD206', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1573'),
	('ATD207', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1573'),
	('ATD208', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1573'),

    ('ATD209', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1821'),
	('ATD210', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1821'),
	('ATD211', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1821'),
	('ATD212', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1821'),
	('ATD213', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1821'),
	('ATD214', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1821'),
	('ATD215', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1821'),
	('ATD216', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1821'),

	('ATD217', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-12', 'TG1995'),
	('ATD218', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '02', '2024-01-13', 'TG1995'),
	('ATD219', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-14', 'TG1995'),
	('ATD220', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-15', 'TG1995'),
	('ATD221', 'ICT1253', 'Theory', 'APPROVED', '02', '2024-01-16', 'TG1995'),
	('ATD222', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-17', 'TG1995'),
	('ATD223', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-18', 'TG1995'),
	('ATD224', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '02', '2024-01-19', 'TG1995'),




    ('ATD225', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1381'),
	('ATD226', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1381'),
	('ATD227', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1381'),
	('ATD228', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1381'),
	('ATD229', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1381'),
	('ATD230', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1381'),
	('ATD231', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1381'),
	('ATD232', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1381'),

	('ATD233', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1511'),
	('ATD234', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1511'),
	('ATD235', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1511'),
	('ATD236', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1511'),
	('ATD237', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1511'),
	('ATD238', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1511'),
	('ATD239', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1511'),
	('ATD240', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1511'),

	('ATD241', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1002'),
	('ATD242', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1002'),
	('ATD243', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1002'),
	('ATD244', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1002'),
	('ATD245', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1002'),
	('ATD246', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1002'),
	('ATD247', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1002'),
	('ATD248', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1002'),

	('ATD249', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1534'),
	('ATD250', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1534'),
	('ATD251', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1534'),
	('ATD252', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1534'),
	('ATD253', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1534'),
	('ATD254', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1534'),
	('ATD255', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1534'),
	('ATD256', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1534'),

    ('ATD257', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1367'),
	('ATD258', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1367'),
	('ATD259', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1367'),
	('ATD260', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1367'),
	('ATD261', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1367'),
	('ATD262', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1367'),
	('ATD263', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1367'),
	('ATD264', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1367'),

	('ATD265', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1548'),
	('ATD266', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1548'),
	('ATD267', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1548'),
	('ATD268', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1548'),
	('ATD269', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1548'),
	('ATD270', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1548'),
	('ATD271', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1548'),
	('ATD272', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1548'),

	('ATD273', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1342'),
	('ATD274', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1342'),
	('ATD275', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1342'),
	('ATD276', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1342'),
	('ATD277', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1342'),
	('ATD278', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1342'),
	('ATD279', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1342'),
	('ATD280', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1342'),

	('ATD281', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1256'),
	('ATD282', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1256'),
	('ATD283', 'ICT1233', 'Theory', 'NOT-APPROVED', '03', '2024-01-22', 'TG1256'),
	('ATD284', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1256'),
	('ATD285', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1256'),
	('ATD286', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1256'),
	('ATD287', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1256'),
	('ATD288', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1256'),

    ('ATD289', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1490'),
	('ATD290', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1490'),
	('ATD291', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1490'),
	('ATD292', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1490'),
	('ATD293', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1490'),
	('ATD294', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1490'),
	('ATD295', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1490'),
	('ATD296', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1490'),

	('ATD297', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1682'),
	('ATD298', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1682'),
	('ATD299', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1682'),
	('ATD300', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1682'),
	('ATD301', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1682'),
	('ATD302', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1682'),
	('ATD303', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1682'),
	('ATD304', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1682'),

	('ATD305', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1745'),
	('ATD306', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1745'),
	('ATD307', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1745'),
	('ATD308', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1745'),
	('ATD309', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1745'),
	('ATD310', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1745'),
	('ATD311', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1745'),
	('ATD312', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1745'),

	('ATD313', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1573'),
	('ATD314', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1573'),
	('ATD315', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1573'),
	('ATD316', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1573'),
	('ATD317', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1573'),
	('ATD318', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1573'),
	('ATD319', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1573'),
	('ATD320', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1573'),

    ('ATD321', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1821'),
	('ATD322', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1821'),
	('ATD323', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1821'),
	('ATD324', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1821'),
	('ATD325', 'ICT1253', 'Theory', 'APPROVED', '03', '2024-01-24', 'TG1821'),
	('ATD326', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1821'),
	('ATD327', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1821'),
	('ATD328', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1821'),

	('ATD329', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-20', 'TG1995'),
	('ATD330', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '03', '2024-01-21', 'TG1995'),
	('ATD331', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-22', 'TG1995'),
	('ATD332', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-23', 'TG1995'),
	('ATD333', 'ICT1253', 'Theory', 'NOT-APPROVED', '03', '2024-01-24', 'TG1995'),
	('ATD334', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-25', 'TG1995'),
	('ATD335', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-26', 'TG1995'),
	('ATD336', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '03', '2024-01-27', 'TG1995'),




('ATD337', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1381'),
('ATD338', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1381'),
('ATD339', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1381'),
('ATD340', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1381'),
('ATD341', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1381'),
('ATD342', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1381'),
('ATD343', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1381'),
('ATD344', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1381'),

('ATD345', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1511'),
('ATD346', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1511'),
('ATD347', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1511'),
('ATD348', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1511'),
('ATD349', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1511'),
('ATD350', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1511'),
('ATD351', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1511'),
('ATD352', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1511'),

('ATD353', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1002'),
('ATD354', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1002'),
('ATD355', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1002'),
('ATD356', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1002'),
('ATD357', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1002'),
('ATD358', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1002'),
('ATD359', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1002'),
('ATD360', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1002'),

('ATD361', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1534'),
('ATD362', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1534'),
('ATD363', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1534'),
('ATD364', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1534'),
('ATD365', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1534'),
('ATD366', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1534'),
('ATD367', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1534'),
('ATD368', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1534'),

('ATD369', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1367'),
('ATD370', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1367'),
('ATD371', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1367'),
('ATD372', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1367'),
('ATD373', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1367'),
('ATD374', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1367'),
('ATD375', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1367'),
('ATD376', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1367'),

('ATD377', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1548'),
('ATD378', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1548'),
('ATD379', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1548'),
('ATD380', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1548'),
('ATD381', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1548'),
('ATD382', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1548'),
('ATD383', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1548'),
('ATD384', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1548'),

('ATD385', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1342'),
('ATD386', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1342'),
('ATD387', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1342'),
('ATD388', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1342'),
('ATD389', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1342'),
('ATD390', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1342'),
('ATD391', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1342'),
('ATD392', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1342'),

('ATD393', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1256'),
('ATD394', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1256'),
('ATD395', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1256'),
('ATD396', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1256'),
('ATD397', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1256'),
('ATD398', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1256'),
('ATD399', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1256'),
('ATD400', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1256'),

('ATD401', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1490'),
('ATD402', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1490'),
('ATD403', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1490'),
('ATD404', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1490'),
('ATD405', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1490'),
('ATD406', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1490'),
('ATD407', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1490'),
('ATD408', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1490'),

('ATD409', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1682'),
('ATD410', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1682'),
('ATD411', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1682'),
('ATD412', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1682'),
('ATD413', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1682'),
('ATD414', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1682'),
('ATD415', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1682'),
('ATD416', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1682'),

('ATD417', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1745'),
('ATD418', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1745'),
('ATD419', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1745'),
('ATD420', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1745'),
('ATD421', 'ICT1253', 'Theory', 'NOT-APPROVED', '04', '2024-02-01', 'TG1745'),
('ATD422', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1745'),
('ATD423', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1745'),
('ATD424', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1745'),

('ATD425', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1573'),
('ATD426', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1573'),
('ATD427', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1573'),
('ATD428', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1573'),
('ATD429', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1573'),
('ATD430', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1573'),
('ATD431', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1573'),
('ATD432', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1573'),

('ATD433', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1821'),
('ATD434', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1821'),
('ATD435', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1821'),
('ATD436', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1821'),
('ATD437', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1821'),
('ATD438', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1821'),
('ATD439', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1821'),
('ATD440', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-04', 'TG1821'),

('ATD441', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-28', 'TG1995'),
('ATD442', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '04', '2024-01-29', 'TG1995'),
('ATD443', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-30', 'TG1995'),
('ATD444', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '04', '2024-01-31', 'TG1995'),
('ATD445', 'ICT1253', 'Theory', 'APPROVED', '04', '2024-02-01', 'TG1995'),
('ATD446', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-02', 'TG1995'),
('ATD447', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '04', '2024-02-03', 'TG1995'),
('ATD448', 'ENG1212', 'Theory', 'NOT-APPROVED', '04', '2024-02-04', 'TG1995'),

('ATD449', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1381'),
('ATD450', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1381'),
('ATD451', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1381'),
('ATD452', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1381'),
('ATD453', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1381'),
('ATD454', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1381'),
('ATD455', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1381'),
('ATD456', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1381'),

('ATD457', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1511'),
('ATD458', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1511'),
('ATD459', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1511'),
('ATD460', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1511'),
('ATD461', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1511'),
('ATD462', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1511'),
('ATD463', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1511'),
('ATD464', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1511'),

('ATD465', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1002'),
('ATD466', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1002'),
('ATD467', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1002'),
('ATD468', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1002'),
('ATD469', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1002'),
('ATD470', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1002'),
('ATD471', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1002'),
('ATD472', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1002'),

('ATD473', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1534'),
('ATD474', 'ICT1222', 'Practical', 'NOT-APPROVED', '05', '2024-02-06', 'TG1534'),
('ATD475', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1534'),
('ATD476', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1534'),
('ATD477', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1534'),
('ATD478', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1534'),
('ATD479', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1534'),
('ATD480', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1534'),

('ATD481', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1367'),
('ATD482', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1367'),
('ATD483', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1367'),
('ATD484', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1367'),
('ATD485', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1367'),
('ATD486', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1367'),
('ATD487', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1367'),
('ATD488', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1367'),

('ATD489', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1548'),
('ATD490', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1548'),
('ATD491', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1548'),
('ATD492', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1548'),
('ATD493', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1548'),
('ATD494', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1548'),
('ATD495', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1548'),
('ATD496', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1548'),

('ATD497', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1342'),
('ATD498', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1342'),
('ATD499', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1342'),
('ATD500', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1342'),
('ATD501', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1342'),
('ATD502', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1342'),
('ATD503', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1342'),
('ATD504', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1342'),

('ATD505', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1256'),
('ATD506', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1256'),
('ATD507', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1256'),
('ATD508', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1256'),
('ATD509', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1256'),
('ATD510', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1256'),
('ATD511', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1256'),
('ATD512', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1256'),

('ATD513', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1490'),
('ATD514', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1490'),
('ATD515', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1490'),
('ATD516', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1490'),
('ATD517', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1490'),
('ATD518', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1490'),
('ATD519', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1490'),
('ATD520', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1490'),

('ATD521', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1682'),
('ATD522', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1682'),
('ATD523', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1682'),
('ATD524', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1682'),
('ATD525', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1682'),
('ATD526', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1682'),
('ATD527', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1682'),
('ATD528', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1682'),

('ATD529', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1745'),
('ATD530', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1745'),
('ATD531', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1745'),
('ATD532', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1745'),
('ATD533', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1745'),
('ATD534', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1745'),
('ATD535', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1745'),
('ATD536', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1745'),

('ATD537', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1573'),
('ATD538', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1573'),
('ATD539', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1573'),
('ATD540', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1573'),
('ATD541', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1573'),
('ATD542', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1573'),
('ATD543', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1573'),
('ATD544', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1573'),

('ATD545', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1821'),
('ATD546', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1821'),
('ATD547', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1821'),
('ATD548', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1821'),
('ATD549', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1821'),
('ATD550', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1821'),
('ATD551', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1821'),
('ATD552', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1821'),

('ATD553', 'ICT1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-05', 'TG1995'),
('ATD554', 'ICT1222', 'Practical', 'NOT-SUBMITTED', '05', '2024-02-06', 'TG1995'),
('ATD555', 'ICT1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-07', 'TG1995'),
('ATD556', 'ICT1242', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-08', 'TG1995'),
('ATD557', 'ICT1253', 'Theory', 'APPROVED', '05', '2024-02-09', 'TG1995'),
('ATD558', 'TMS1233', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-10', 'TG1995'),
('ATD559', 'TCS1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-11', 'TG1995'),
('ATD560', 'ENG1212', 'Theory', 'NOT-SUBMITTED', '05', '2024-02-12', 'TG1995');

