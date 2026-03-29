JDBC (Database Connectivity)
20 Marks
Slip1:1.Write a Java program to accept the details of Employee (Eno, EName, Designation, Salary) from a user and store it into the database. (Use Swing)

postgres=# create database assignment3;
postgres=# \c assignment3
assignment3=# alter user root with password '12345';
assignment3=# CREATE TABLE EMPLOYEE (
    eno INT PRIMARY KEY,
    ename VARCHAR(50),
    designation VARCHAR(50),
    salary INT
);
assignment3=# grant all on EMPLOYEE to root;
assignment3=# select * from EMPLOYEE;
Employee.java
import java.sql.*;
import javax.swing.*;
import javax.swing.table.DefaultTableModel;
public class Employee {
    public static void main(String[] args) {
       JFrame frame = new JFrame("EMPLOYEE TABLE");
        DefaultTableModel model = new DefaultTableModel();
        JTable table = new JTable(model);
        model.addColumn("Employee No");
        model.addColumn("Employee Name");
        model.addColumn("Designation");
        model.addColumn("Salary");
        try {
            Class.forName("org.postgresql.Driver");
            Connection con = DriverManager.getConnection( "jdbc:postgresql://localhost:5432/assignment3", "root","12345");
            int eno = Integer.parseInt( JOptionPane.showInputDialog("Enter Employee No"));
            String ename =JOptionPane.showInputDialog("Enter Employee Name");
           String desig =JOptionPane.showInputDialog("Enter Designation");
           int salary = Integer.parseInt(JOptionPane.showInputDialog("Enter Salary"));
            String sql = "INSERT INTO EMPLOYEE VALUES (?,?,?,?)";
            PreparedStatement ps = con.prepareStatement(sql);
            ps.setInt(1, eno);
            ps.setString(2, ename);
            ps.setString(3, desig);
            ps.setInt(4, salary);
            ps.executeUpdate();
            JOptionPane.showMessageDialog(frame,"Record Inserted Successfully");
            Statement stmt = con.createStatement();
            ResultSet rs = stmt.executeQuery("SELECT * FROM EMPLOYEE");
            while (rs.next()) {
                model.addRow(new Object[]{
                    rs.getInt(1),
                    rs.getString(2),
                    rs.getString(3),
                    rs.getInt(4)
                });
            }
            con.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        frame.add(new JScrollPane(table));
        frame.setSize(600,300);
        frame.setVisible(true);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

Slip3:2.Write a Java program using JDBC to:Create Product(Pid, Pname, Price) table,Insert at least five records,Display all the records.
CREATE DATABASE assignment4;
\c assignment4
CREATE TABLE Product
(
Pid INT PRIMARY KEY,
Pname VARCHAR(50),
Price INT
);
GRANT ALL ON Product TO root;
ProductDemo.java
import java.sql.*;
public class ProductDemo{
    public static void main(String args[])    {
        try  {
            Class.forName("org.postgresql.Driver");
            Connection con = DriverManager.getConnection("jdbc:postgresql://localhost:5432/assignment4","root","12345");
            Statement stmt = con.createStatement();
            stmt.executeUpdate("insert into Product values(1,'Laptop',60000)");
            stmt.executeUpdate("insert into Product values(2,'Mouse',500)");
            stmt.executeUpdate("insert into Product values(3,'Keyboard',1200)");
            stmt.executeUpdate("insert into Product values(4,'Monitor',15000)");
            stmt.executeUpdate("insert into Product values(5,'Printer',8000)");
            System.out.println("Records Inserted Successfully");
            ResultSet rs = stmt.executeQuery("select * from Product");
            System.out.println("Pid\tPname\tPrice");
            while(rs.next()) {
                System.out.println(rs.getInt(1)+"\t"+rs.getString(2)+"\t"+rs.getInt(3));
            }
            con.close();
        }
        catch(Exception e)    {
            System.out.println(e);
        }
    }
}

Slip4:3.Write a Java program using JDBC and Swing to display the first record from the student() table into TextFields when a button is clicked
CREATE DATABASE assignment3;
\c assignment3
CREATE TABLE student(
rollno INT PRIMARY KEY,
name VARCHAR(50),
marks INT
);
INSERT INTO student VALUES (1,'Amit',85);
INSERT INTO student VALUES (2,'Neha',90);

StudentFirstRecord.java
import java.sql.*;
import javax.swing.*;
import java.awt.event.*;
public class StudentFirstRecord extends JFrame implements ActionListener{
    JLabel l1,l2,l3;
    JTextField t1,t2,t3;
    JButton b1;
    Connection con;
    ResultSet rs;
    Statement stmt;
    StudentFirstRecord() {
        l1=new JLabel("Roll No");
        l2=new JLabel("Name");
        l3=new JLabel("Marks");
        t1=new JTextField(20);
        t2=new JTextField(20);
        t3=new JTextField(20);
        b1=new JButton("Show First Record");
        setLayout(null);
        l1.setBounds(50,50,100,30);
        l2.setBounds(50,100,100,30);
        l3.setBounds(50,150,100,30);
        t1.setBounds(150,50,150,30);
        t2.setBounds(150,100,150,30);
        t3.setBounds(150,150,150,30);
        b1.setBounds(100,220,160,30);
        add(l1); add(t1);
        add(l2); add(t2);
        add(l3); add(t3);
        add(b1);
        b1.addActionListener(this);
        setSize(350,320);
        setVisible(true);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        try{
            Class.forName("org.postgresql.Driver");
    con = DriverManager.getConnection( "jdbc:postgresql://localhost:5432/assignment3", "root","12345");
            stmt = con.createStatement(
            ResultSet.TYPE_SCROLL_INSENSITIVE,
            ResultSet.CONCUR_READ_ONLY);
            rs = stmt.executeQuery("SELECT * FROM student");
        }
        catch(Exception e)  {
            System.out.println(e);
        }
    }
   public void actionPerformed(ActionEvent ae)  {
        try {
            if(rs.first()){   
             t1.setText(String.valueOf(rs.getInt(1)));
                t2.setText(rs.getString(2));
                t3.setText(String.valueOf(rs.getInt(3)));
            }
        }
        catch(Exception e) {
            System.out.println(e);
        }
    }
    public static void main(String args[]){
        new StudentFirstRecord();
    }
}

4.Write a Java program to:Create Employee table (ENo, EName, Salary),Insert records,Display all records.
CREATE DATABASE company;
\c company
CREATE TABLE Employee(
    ENo INT,
    EName VARCHAR(50),
    Salary INT
);

EmployeeDemo.java
import java.sql.*;
public class EmployeeDemo
{
    public static void main(String args[]){
        try
        {
            Class.forName("org.postgresql.Driver");
            Connection con = DriverManager.getConnection(
            "jdbc:postgresql://localhost:5432/company",
            "postgres",
            "1234");
            Statement stmt = con.createStatement();
            stmt.executeUpdate("INSERT INTO Employee VALUES(1,'Amit',30000)");
            stmt.executeUpdate("INSERT INTO Employee VALUES(2,'Neha',35000)");
            stmt.executeUpdate("INSERT INTO Employee VALUES(3,'Ravi',40000)");
            ResultSet rs = stmt.executeQuery("SELECT * FROM Employee");
            System.out.println("Employee Records");
            while(rs.next())  {
                System.out.println(
                rs.getInt(1)+" "+
                rs.getString(2)+" "+
                rs.getInt(3));
            }
            con.close();
        }
        catch(Exception e){
            System.out.println(e);
        }
    }
}

Slip7:5.Write a Java program to delete employee details using PreparedStatement. Accept employee ID through command line.
CREATE DATABASE assignment3;
\c assignment3
CREATE TABLE Employee
(
ENo INT PRIMARY KEY,
EName VARCHAR(50),
Salary INT
);
GRANT ALL ON Employee TO root;
INSERT INTO Employee VALUES (101,'Amit',50000);
INSERT INTO Employee VALUES (102,'Neha',45000);
INSERT INTO Employee VALUES (103,'Rahul',40000);
SELECT * FROM Employee;

DeleteEmployee.java

import java.sql.*;
public class DeleteEmployee{
public static void main(String args[]){
try{
Class.forName("org.postgresql.Driver");
Connection con = DriverManager.getConnection(
"jdbc:postgresql://localhost:5432/assignment3","root","12345");
int eno = Integer.parseInt(args[0]);
PreparedStatement ps = con.prepareStatement("delete from Employee where ENo=?");
ps.setInt(1, eno);
int i = ps.executeUpdate();
if(i>0)
System.out.println("Record Deleted Successfully");
else
System.out.println("Employee Not Found");
con.close();
}
catch(Exception e){
System.out.println(e);
}
}
}

slip18:6.Write a Java program that establishes JDBC connection with Student database and verifies connection status.
CREATE DATABASE Student;
\c Student;
CREATE TABLE Student(
rollno INT,
name VARCHAR(50)
);

StudentConnection.java
import java.sql.*;
public class StudentConnection
{
    public static void main(String args[])
    {
        try
        {
            Class.forName("org.postgresql.Driver");
            Connection con = DriverManager.getConnection(
            "jdbc:postgresql://localhost:5432/Student",
            "postgres",
            "1234");
            if(con != null)
            {
                System.out.println("Connection Successful");
            }
            else
            {
                System.out.println("Connection Failed");
            }
             con.close();
        }
        catch(Exception e) {
            System.out.println("Error: " + e);
        }
    }
}
________________________________________
  10 Marks
Slip14:1.Write a Java program to display information about all columns in the DONAR table using ResultSetMetaData.
CREATE DATABASE assignment3;
\c assignment3
CREATE TABLE donar(
donar_id INT,
donar_name VARCHAR(20),
blood_group VARCHAR(5),
city VARCHAR(20)
);
INSERT INTO donar VALUES(1,'Amit','A+','Pune');
INSERT INTO donar VALUES(2,'Riya','B+','Mumbai');
INSERT INTO donar VALUES(3,'Rahul','O+','Nashik');

DonarMetadata.java
import java.sql.*;
public class DonarMetadata {
    public static void main(String[] args) {
        try {
            Class.forName("org.postgresql.Driver");
            Connection con = DriverManager.getConnection(
            "jdbc:postgresql://localhost:5432/assignment3","root","12345");
            Statement stmt = con.createStatement();
            ResultSet rs = stmt.executeQuery("SELECT * FROM donar");
            ResultSetMetaData rsmd = rs.getMetaData();
            int columnCount = rsmd.getColumnCount();
            System.out.println("Number of Columns: " + columnCount);
            System.out.println("\nColumn Details:");
            for (int i = 1; i <= columnCount; i++) {
                System.out.println(
                "Column Name : " + rsmd.getColumnName(i) +
                " | Data Type : " + rsmd.getColumnTypeName(i) +
                " | Size : " + rsmd.getColumnDisplaySize(i)
                );
            }
            con.close();
        } 
        catch (Exception e) {
            System.out.println(e);
        }
    }
}
Slip21:2.Write a Java program to display information about all columns in the DOCTOR table using ResultSetMetaData.
CREATE DATABASE assignment3;
CREATE TABLE doctor (
    doctor_id INT,
    doctor_name VARCHAR(50),
    specialization VARCHAR(50)
);
INSERT INTO doctor VALUES (1,'Dr. Sharma','Cardiologist');
INSERT INTO doctor VALUES (2,'Dr. Patil','Neurologist');
INSERT INTO doctor VALUES (3,'Dr. Mehta','Dermatologist');

DoctorMetadata.java
import java.sql.*;
public class DoctorMetadata {
    public static void main(String[] args) {
        try {
            Connection con = DriverManager.getConnection( "jdbc:postgresql://localhost:5432/assignment3", "root",  "12345");
            Statement stmt = con.createStatement();
            ResultSet rs = stmt.executeQuery("SELECT * FROM doctor");
            ResultSetMetaData rsmd = rs.getMetaData();
            int columnCount = rsmd.getColumnCount();
            System.out.println("Number of Columns: " + columnCount);
            System.out.println("\nColumn Details:");
            for (int i = 1; i <= columnCount; i++) {
                System.out.println(
                        "Column Name : " + rsmd.getColumnName(i) +
                        " | Data Type : " + rsmd.getColumnTypeName(i) +
                        " | Size : " + rsmd.getColumnDisplaySize(i));
            }
            con.close();
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
________________________________________
2. Servlet
20 Marks
Slip10:1.Design a Servlet that provides information about HTTP request such as IP address and browser type and server information.
index.html
<html>
<head>
<title>Servlet Info</title>
</head>
<body>
<h2>Click Button to See Client & Server Information</h2>
<form action="InfoServlet" method="get">
<input type="submit" value="Show Information">
</form>
</body>
</html>

InfoServlet.java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.util.*;
import javax.servlet.annotation.WebServlet;
@WebServlet("/InfoServlet")
public class InfoServlet extends HttpServlet {
   public void doGet(HttpServletRequest request, HttpServletResponse  response)
           throws ServletException, IOException {
       response.setContentType("text/html");
       PrintWriter out = response.getWriter();
       out.println("<html><body>");
       out.println("<h2>Client Information</h2>");
       out.println("IP Address: " + request.getRemoteAddr() + "<br>");
       out.println("Browser Type: " + request.getHeader("User-Agent") + "<br>");
       out.println("<h2>Server Information</h2>");
       out.println("Operating System: " + System.getProperty("os.name") + "<br>");
       out.println("Server Name: " + request.getServerName() + "<br>");
       ServletContext context = getServletContext();
       Enumeration<String> servlets = context.getServletRegistrations().keySet().iterator();
       out.println("<h3>Loaded Servlets:</h3>");
       for(String s : context.getServletRegistrations().keySet()){
           out.println(s + "<br>");
       }
       out.println("</body></html>");
   }
}

2.Write a Java Servlet program named GreetingServlet that reads a name from URL and displays
 “Hello, [name]! Welcome to Servlet.”
GreetingServlet.java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;
@WebServlet("/greet")
public class GreetingServlet extends HttpServlet {
   protected void doGet(HttpServletRequest request, HttpServletResponse response)
           throws ServletException, IOException {
       String name = request.getParameter("name");
       if(name == null)
           name = "Guest";
       response.setContentType("text/html");
       PrintWriter out = response.getWriter();
       out.println("<h2>Hello, " + name + "! Welcome to Servlet.</h2>");

       out.close();
   }
}
Example URL
http://localhost:8080/myapp/greet?name=John

3.Write a Java Servlet program named ClientInfoServlet that displays client IP address and browser type.
ClientInfoServlet.java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;
@WebServlet("/clientinfo")
public class ClientInfoServlet extends HttpServlet {
   protected void doGet(HttpServletRequest request, HttpServletResponse response)
           throws ServletException, IOException {
       response.setContentType("text/html");
       PrintWriter out = response.getWriter();
       out.println("<h2>Client Information</h2>");
       out.println("IP Address: " + request.getRemoteAddr() + "<br>");
       out.println("Browser: " + request.getHeader("User-Agent") + "<br>");
       out.close();
   }
}

slip16:4.Write a Servlet named CounterServlet that counts how many times it is accessed and displays the count.
CounterServlet.java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;
@WebServlet("/counter")
public class CounterServlet extends HttpServlet {
   int count = 0;
   protected void doGet(HttpServletRequest request, HttpServletResponse response)
           throws ServletException, IOException {
       count++;
       response.setContentType("text/html");
       PrintWriter out = response.getWriter();
       out.println("<h2>Visitor Count: " + count + "</h2>");
       out.close();
   }
}
________________________________________
10 Marks
Slip5:1.Write a Servlet AddServlet that takes two numbers from HTML form and displays the result.
HTML Form (add.html)
<html>
<body>
<h2>Addition Form</h2>
<form action="add" method="post">
Number 1: <input type="text" name="num1"><br><br>
Number 2: <input type="text" name="num2"><br><br>
<input type="submit" value="Add">
</form>
</body>
</html>

AddServlet.java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;
@WebServlet("/add")
public class AddServlet extends HttpServlet {
   protected void doPost(HttpServletRequest request, HttpServletResponse response)
           throws ServletException, IOException {
       int num1 = Integer.parseInt(request.getParameter("num1"));
       int num2 = Integer.parseInt(request.getParameter("num2"));
       int sum = num1 + num2;
       response.setContentType("text/html");
       PrintWriter out = response.getWriter();
       out.println("<h2>Sum: " + sum + "</h2>");
       out.close();
   }
}

slip17:2.Write a Servlet DateTimeServlet that displays the current date and time when accessed.
DateTimeServlet.java
import java.io.*;
import java.util.Date;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;
@WebServlet("/datetime")
public class DateTimeServlet extends HttpServlet {
   protected void doGet(HttpServletRequest request, HttpServletResponse response)
           throws ServletException, IOException {
           response.setContentType("text/html");
      PrintWriter out = response.getWriter();
       Date now = new Date(); 
       out.println("<h2>Current Date and Time: " + now + "</h2>");
       out.close();
   }
}

________________________________________
3. JSP
20 Marks
Slip8:1.Write a JSP program which accepts UserName and greets the user according to the server time.
greet.html
<html> 
<head> 
<title> Greetings !! </title> 
</head> 
<body> 
<form method="POST" action="greet.jsp"> 
Enter Your Name <input type="text" name="txt" > <br> 
<input type="submit" value="Display Message" /> 
</body> 
</html> 

greet.jsp
<%@ page language="java" import="java.util.*" %> 
<html> 
<head> 
<title> Greetings!! </title> 
</head> 
<body text="red" > 
<% 
try { 
 String s= request.getParameter("txt"); 
 Date d=new Date(); 
 int t=d.getHours(); 
 if(t>0 && t<12) 
 out.print("Good Morning!!\t" +s); 
 else if(t>=12 && t<=16) 
 out.print("Good Evening!!\t" +s); 
 else 
 out.print("Good Night!!\t" +s); 
} 
catch (Exception e)  { 
 out.print(e); 
 }
%> 
</body> 
</html> 

Slip13:2.Write a JSP program to display PATIENT details in tabular form on browser.
CREATE DATABASE test;
\c test;
CREATE TABLE patient(
No INT,
Name VARCHAR(20),
Address VARCHAR(20),
Age INT,
Disease VARCHAR(20)
);
INSERT INTO patient VALUES(1,'Aishwarya','Pune',22,'Fever');
INSERT INTO patient VALUES(2,'Amit','Mumbai',30,'Cold');
INSERT INTO patient VALUES(3,'Riya','Nashik',25,'Cough');

patient.html 
<html>
<head>
<title>Patient Details</title>
</head>
<body>
<h2>Display Patient Details</h2>
<form action="patient.jsp" method="post">
<input type="submit" value="Show Patients">
</form>
</body>
</html>

patient.jsp
<%@ page import="java.sql.*" %>
<html>
<head>
<title>Patient Table</title>
</head>
<body>
<table border="1">
<tr>
<th>No</th>
<th>Name</th>
<th>Address</th>
<th>Age</th>
<th>Disease</th>
</tr>
<%
Class.forName("com.mysql.jdbc.Driver");
Connection con = DriverManager.getConnection(
"jdbc:mysql://localhost/test","root","");
Statement st = con.createStatement();
ResultSet rs = st.executeQuery("select * from patient");
while(rs.next()){
%>
<tr>
<td><%= rs.getInt(1) %></td>
<td><%= rs.getString(2) %></td>
<td><%= rs.getString(3) %></td>
<td><%= rs.getInt(4) %></td>
<td><%= rs.getString(5) %></td>
</tr>
<%
}
%>
</table>
</body>
</html>
________________________________________
10 Marks
Slip22:1.Write a JSP program to check voter eligibility./ 8.Write a JSP program to check voting eligibility using age
voter.html 
<html> 
<head> 
<title> Voter Login Page </title> 
</head> 
<body> 
<h2> Enter Your Details Here </h2> 
<form action="voter.jsp" method="POST"> 
Enter Your Name <input type="text" name="txt"><br> 
Enter Your Age<input type="text" name="n"><br> 
<input type="submit" value="Validate"> 
<input type="reset" value="Reset"> 
</form> 
</body> 
</html> 

voter.jsp 
<%@ page language="java" import="java.util.*" %> 
<html> 
<head> 
<title> Voter Demo </title> 
</head> 
<body text="green"> 
<% 
try { 
 int x=Integer.parseInt(request.getParameter("n")); 
 String s= request.getParameter("txt"); 
 if(x >=18 )
 out.print("Congratulations !!\t" +s+" you are eligible for  Voting"); 
 else 
 out.print("Sorry !!\t" +s+" you are not eligible for Voting"); 
} 
catch (Exception e) { 
 out.print(e); 
 } 
%> 
</body> 
</html>

slip6:2.Write a JSP program to check prime number and display result in red color.

prime.html
<html>
<head>
<title> CHECK PRIME </title>
</head>
<body>
<form method="POST" action="prime.jsp">
Enter a number <input type="text" name="n"> <br><br>
<input type="submit" value="PRIME/NOT PRIME" />
</form>
</body>
</html>

prime.jsp
<%@ page language="java" import="java.util.*" %>
<html>
<head>
<title> PRIME Demo </title>
</head>
<body>
<font color="red">
<%
try{
    int x = Integer.parseInt(request.getParameter("n"));
    int i, flag = 1;
    for(i = 2; i < x; i++) {
        if(x % i == 0)    {
            flag++;
            break;
        }
    }
    if(flag == 1)
        out.print("Number is Prime");
    else
        out.print("Number is not Prime");
}
catch(Exception e){
    out.print(e);
}
%>
</font>
</body>
</html>

Slip7:3.Write a JSP program to calculate factorial of a number.
factorial.html
<html>
<head>
<title>Factorial Program</title>
</head>
<body>
<form method="post" action="factorial.jsp">
Enter Number: <input type="text" name="n"><br><br>
<input type="submit" value="Find Factorial">
</form>
</body>
</html>

factorial.jsp
<%@ page language="java" %>
<html>
<head>
<title>Factorial Result</title>
</head>
<body>
<%
try{
    int n = Integer.parseInt(request.getParameter("n"));
    int fact = 1;
    for(int i=1;i<=n;i++) {
        fact = fact * i;
    }
%>
<h2>Factorial of <%=n%> is <%=fact%></h2>
<%
}
catch(Exception e){
    out.print("Invalid Input");
}
%>
</body>
</html>

Slip 9:4.Write a JSP program to check Perfect number (Use Include directive).
perfect.jsp 
<html>
<head>
<title>Perfect Number</title>
</head>
<body>
<form method="post">
Enter Number :
<input type="text" name="n"><br><br>
<input type="submit" value="Check Perfect">
</form>
<%@ include file="logic.jsp" %>
</body>
</html>
logic.jsp
<%
try{
    String num = request.getParameter("n");
    if(num != null)  {
        int n = Integer.parseInt(num);
        int sum = 0;
        for(int i=1;i<n;i++){
            if(n%i==0) {
                sum = sum + i;
            }
        }
        if(sum==n)
            out.print("Number is Perfect");
        else
            out.print("Number is Not Perfect");
    }
}
catch(Exception e){
    out.print("Invalid Input");
}
%>

Slip 15:5.Write a JSP program to reverse a string.
reverse.html
<html>
<head>
<title>Reverse String</title>
</head>
<body>
<form action="reverse.jsp" method="post">
Enter a String :
<input type="text" name="str"><br><br>
<input type="submit" value="Reverse String">
</form>
</body>
</html>

reverse.jsp
<%@ page language="java" %>
<html>
<head>
<title>Reverse Output</title>
</head>
<body>
<%
String s = request.getParameter("str");
String rev = "";
for(int i=s.length()-1;i>=0;i--)
{
rev = rev + s.charAt(i);
}
out.println("Reverse String is : " + rev);
%>
</body>
</html>

slip 19:6.Write a JSP program to calculate Simple Interest.

simpleinterest.jsp
<html>
<head>
<title>Simple Interest Calculator</title>
</head>
<body>
<h2>Simple Interest Calculator</h2>
<form method="post">
Principal: <input type="text" name="p"><br><br>
Rate of Interest: <input type="text" name="r"><br><br>
Time: <input type="text" name="t"><br><br>
<input type="submit" value="Calculate">
</form>
<%
String p = request.getParameter("p");
String r = request.getParameter("r");
String t = request.getParameter("t");
if(p != null && r != null && t != null)
{
    double principal = Double.parseDouble(p);
    double rate = Double.parseDouble(r);
    double time = Double.parseDouble(t);
    double si = (principal * rate * time) / 100;
    out.println("<h3>Simple Interest = " + si + "</h3>");
}
%>
</body>
</html>

Slip 21:7.Write a JSP program to display digits of a number in words.
number.html
<html>
<head>
<title> Number Demo </title>
</head>
<body>
<form method="POST" action="number.jsp">
Enter a number <input type="text" name="n" > <br>
<input type="submit" value="Display" />
</body>
</html>

number.jsp
<%@ page language="java" import="java.util.*" %>
<html>
<head>
<title>Number Demo</title>
</head>
<body text="red">
<%
try{
int x = Integer.parseInt(request.getParameter("n"));
int r = 0, l = 0;
while(x > 0){
    l = x % 10;
    r = r * 10 + l;
    x = x / 10;
}
while(r != 0){
    l = r % 10;
    r = r / 10;
    switch(l) {
        case 0: out.print("ZERO "); break;
        case 1: out.print("ONE "); break;
        case 2: out.print("TWO "); break;
        case 3: out.print("THREE "); break;
        case 4: out.print("FOUR "); break;
        case 5: out.print("FIVE "); break;
        case 6: out.print("SIX "); break;
        case 7: out.print("SEVEN "); break;
        case 8: out.print("EIGHT "); break;
        case 9: out.print("NINE "); break;
    }
}
}
catch(Exception e){
    out.print(e);
}
%>
</body>
</html>

8.Write a JSP program to find sum of first and last digits.
sum.html
<html>
<head>
<title > SUM OF DIGIT </title>
</head>
<body>
<form method="POST" action="sum.jsp">
Enter a number <input type="text" name="n" > <br>
<input type="submit" value="SUM OF FIRST & LAST DIGIT " />
</body>
</html>

sum.jsp
<%@ page language="java" import="java.util.*" %>
<html>
<head>
<title> SUM OF FIRST & LAST DIGIT </title>
</head>
<body text="red">
<%
try
{
int x=Integer.parseInt(request.getParameter("n"));
int l;
l=x%10;
while(x >10)
{
x=x/10;
}
out.print("Sum of first and last digit is:: " +(l+x));
}
catch (Exception e)
{
out.print(e);
}
%>
</body>
</html>
________________________________________
4. Threads / Multithreading
20 Marks
Slip2:1.Write a Java program implementing three threads where:
first thread generates random numbersecond prints square if even
third prints cube if odd.
Threaddemo.java
import java.io.*;
import java.util.*;
class square extends Thread{
int n;
square(int n){
this.n=n;
}
public void run(){
System.out.println("Square:"+n*n);
}
}
class cube extends Thread{
int n;
cube(int n){
this.n=n;
}
public void run(){
System.out.println("Cube:"+n*n*n);
}
}
class number extends Thread{
int n;
public void run(){
try{
Random r=new Random();
for(int i=1;i<=10;i++){
n=r.nextInt(20);
System.out.println("generated number is:"+n);
if(n%2==0){
square s=new square(n);
s.start();
}
else{
cube c=new cube(n);
c.start();
}
sleep(2000);
}
}
catch(Exception e){
System.out.println(e);
}
}
}
class threaddemo{
public static void main(String args[]){
number n=new number();
n.start();
}
}

slip5:2.Write a Java program to define a thread printing text n times (COVID19 and LOCKDOWN2020).
tdemo4.java
import java.io.*;
import java.util.*;
public class tdemo4 extends Thread{
int n;
String msg;
tdemo4(int n,String msg){
this.n=n;
this.msg=msg;
}
public void run(){
try{
for(int i=1;i<=n;i++){
System.out.println(msg);
}
}
catch(Exception e){
System.out.println(e);
}
}
public static void main(String args[]){
Thread t1=new Thread(new tdemo4(10,"COVID19"));
t1.start();
Thread t2=new Thread(new tdemo4(20,"LOCKDOWN2020"));
t2.start();
}
}

Slip17:3.Write a program creating 3 threads printing:Hello (10 times),
GoodMorning(20times),HaveANiceDay(30times)
tdemo4.java
import java.io.*;
import java.util.*;
public class tdemo4 extends Thread{
int n;
String msg;
tdemo4(int n,String msg){
this.n=n;
this.msg=msg;
}
public void run(){
try{
for(int i=1;i<=n;i++){
System.out.println(msg);
}
}
catch(Exception e){
System.out.println(e);
}
}
public static void main(String args[]){
Thread t1=new Thread(new tdemo4(10,"Hello"));
t1.start();
Thread t2=new Thread(new tdemo4(20,"GoodMorning"));
t2.start();
Thread t3=new Thread(new tdemo4(30,"HaveANiceDay"));
t3.start();
}
}

slip19:4.Write a program to solve Producer Consumer problem using thread synchronization.
ProducerConsumerExample.java
class SharedBuffer {
    private int data;
    private boolean hasData = false; 
    public synchronized void produce(int value) {
        while (hasData) {  
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        data = value;
        System.out.println("Produced: " + data);
        hasData = true;
        notify(); 
    }
    public synchronized int consume() {
        while (!hasData) {  
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("Consumed: " + data);
        hasData = false;
        notify(); 
        return data;
    }
}
class Producer extends Thread {
    private SharedBuffer buffer;
    public Producer(SharedBuffer buffer) {
        this.buffer = buffer;
    }
    public void run() {
        for (int i = 1; i <= 10; i++) { 
            buffer.produce(i);
            try {
                Thread.sleep(500); 
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
class Consumer extends Thread {
    private SharedBuffer buffer;
    public Consumer(SharedBuffer buffer) {
        this.buffer = buffer;
    }
    public void run() {
        for (int i = 1; i <= 10; i++) { 
            buffer.consume();
            try {
                Thread.sleep(500); 
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
public class ProducerConsumerExample {
    public static void main(String[] args) {
        SharedBuffer buffer = new SharedBuffer();
        Producer producer = new Producer(buffer);
        Consumer consumer = new Consumer(buffer);
        producer.start();
        consumer.start();
    }
}

Slip24:5.Write a Java program with three threads:generate numbers 1–20,print numbers divisible by 3,print numbers divisible by 5.
tdemo5.java
import java.io.*;
import java.util.*;
public class tdemo5 extends Thread{
    int n;
    String msg;
    tdemo5(int n,String msg)  {
        this.n=n;
        this.msg=msg;
 }
    public void run() {
        try    {
            for(int i=1;i<=n;i++)  {
                if(msg.equals("DIV3") && i%3==0)  {
                    System.out.println("Divisible by 3 : " + i);
                }
                if(msg.equals("DIV5") && i%5==0){
                    System.out.println("Divisible by 5 : " + i);
                }
               if(msg.equals("GEN"))  {
                    System.out.println("Number : " + i);
                }
            }
        }
        catch(Exception e)  {
            System.out.println(e);
        }
    }
    public static void main(String args[])   {
        Thread t1=new Thread(new tdemo5(20,"GEN"));
        t1.start();
        Thread t2=new Thread(new tdemo5(20,"DIV3"));
        t2.start();
        Thread t3=new Thread(new tdemo5(20,"DIV5"));
        t3.start();
    }
}


10 Marks
1.Write a Java program to generate 10 random numbers using thread.
Tdemo.java
import java.io.*;
import java.util.*;
class tdemo extends Thread
{
public static void main(String args[])
{
Thread t=new Thread(new tdemo());
t.start();
}
public void run()
{
for(int i=1;i<10;i++)
{
Random r=new Random();
int n=r.nextInt(50);
System.out.println(i);
}
}
}

Slip12:2.Write a Java program to display thread name and priority.
prioritydemo.java
import java.io.*; 
class prioritydemo extends Thread  { 
public void run() {
System.out.println("running thread name  is:"+Thread.currentThread().getName()); 
System.out.println("running thread priority  is:"+Thread.currentThread().getPriority()); 
} 
public static void main(String args[]) { 
prioritydemo m1=new prioritydemo(); 
prioritydemo m2=new prioritydemo(); 
m1.setPriority(Thread.MIN_PRIORITY); 
m2.setPriority(Thread.MAX_PRIORITY); 
m1.start(); 
m2.start(); 
} 
} 

Slip13:3.Write a Java program using thread to display vowels from string every 3 seconds.
VowelThread.java
import java.util.Scanner;
class VowelThread extends Thread{
    String str;
    VowelThread(String s) {
        str = s;
    }
    public void run() {
        try {
            for(int i=0;i<str.length();i++){
char ch = str.charAt(i);        if(ch=='a'||ch=='e'||ch=='i'||ch=='o'||ch=='u'||ch=='A'||ch=='E'||ch=='I'||ch=='O'||ch=='U'){
                    System.out.println("Vowel : " + ch);
                    Thread.sleep(3000);
                }
            }
        }
        catch(Exception e)  {
            System.out.println(e);
        }
    }
}
public class VowelThreadDemo{
    public static void main(String args[])  {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter a String : ");
        String s = sc.nextLine();
        VowelThread v = new VowelThread(s);
        v.start();
    }
}
4.Write a Java program to count even and odd numbers (1–100) using two threads.
tdemo3.java
import java.io.*;
import java.util.*;
public class tdemo3 extends Thread{
    int n;
    tdemo3(int n)
    {
        this.n = n;
    }
    public void run(){
        int even = 0, odd = 0;
        for(int i = 1; i <= n; i++){
            if(i % 2 == 0)
                even++;
            else
                odd++;
        }
        System.out.println("Total Even Numbers = " + even);
        System.out.println("Total Odd Numbers = " + odd);
    }
    public static void main(String args[]){
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter the value of n");
        int n = sc.nextInt();
        Thread t1 = new Thread(new tdemo3(n));
        Thread t2 = new Thread(new tdemo3(n));

        t1.start();
        t2.start();
    }
}

Slip1:5.Write a Java program to display A–Z alphabets every 2 seconds.
tdemo2.java
import java.io.*; 
class tdemo2 extends Thread { 
public static void main(String args[]) { 
Thread t1=new Thread(new tdemo2()); 
t1.start(); 
} 
public void run() { 
try { 
for(char ch='A';ch<='Z';ch++) { 
System.out.println(ch); 
sleep(2000); 
} 
} 
catch(Exception e) { 
System.out.println(e); 
} 
} 
} 

5. Collections Framework
20 Marks
Slip9:1.Write a Java program to store city names and STD codes using collections and perform add, remove, search.
CitySTDDirectory.java
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.HashMap;
public class CitySTDDirectory extends JFrame {
    private HashMap<String, String> cityMap = new HashMap<>();
    private JTextField cityField;
    private JTextField codeField;
    private JTextArea displayArea;
    public CitySTDDirectory() {
        setTitle("City STD Code Directory");
        setSize(400, 350);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);
        cityField = new JTextField(15);
        codeField = new JTextField(15);
        displayArea = new JTextArea(10, 30);
        displayArea.setEditable(false);
        JButton addButton = new JButton("Add City");
        JButton removeButton = new JButton("Remove City");
        JButton searchButton = new JButton("Search City");
        JPanel inputPanel = new JPanel();
        inputPanel.setLayout(new GridLayout(3, 2, 5, 5));
        inputPanel.add(new JLabel("City Name:"));
        inputPanel.add(cityField);
        inputPanel.add(new JLabel("STD Code:"));
        inputPanel.add(codeField);
        JPanel buttonPanel = new JPanel();
        buttonPanel.add(addButton);
        buttonPanel.add(removeButton);
        buttonPanel.add(searchButton);
        addButton.addActionListener(e -> addCity());
        removeButton.addActionListener(e -> removeCity());
        searchButton.addActionListener(e -> searchCity());
        setLayout(new BorderLayout());
        add(inputPanel, BorderLayout.NORTH);
        add(buttonPanel, BorderLayout.CENTER);
        add(new JScrollPane(displayArea), BorderLayout.SOUTH);
    }
    private void addCity() {
        String city = cityField.getText().trim();
        String code = codeField.getText().trim();
        if (city.isEmpty() || code.isEmpty()) {
            displayArea.setText("City or code cannot be empty!");
            return;
        }
        if (cityMap.containsKey(city)) {
            displayArea.setText("City already exists. Duplicate not allowed!");
        } else {
            cityMap.put(city, code);
            displayArea.setText("Added: " + city + " -> " + code);
        }
    }
    private void removeCity() {
        String city = cityField.getText().trim();
        if (city.isEmpty()) {
            displayArea.setText("Enter a city to remove!");
            return;
        }
        if (cityMap.remove(city) != null) {
            displayArea.setText("Removed city: " + city);
        } else {
            displayArea.setText("City not found!");
        }
    }
    private void searchCity() {
        String city = cityField.getText().trim();
        if (city.isEmpty()) {
            displayArea.setText("Enter a city to search!");
            return;
        }
        String code = cityMap.get(city);
        if (code != null) {
            displayArea.setText("STD Code for " + city + " = " + code);
        } else {
            displayArea.setText("City not found!");
        }
    }
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new CitySTDDirectory().setVisible(true));
    }
}

slip12:2.Write a Java program to create HashMap and sort it according to keys.
HashMapSort.java
import java.util.*;
public class HashMapSort {
    public static void main(String[] args) {
        HashMap<Integer, String> hm = new HashMap<>();
        hm.put(105, "sakshi");
        hm.put(101, "Rohit");
        hm.put(103, "Sneha");
        hm.put(102, "Amit");
        hm.put(104, "Neha");
        System.out.println("HashMap Before Sorting:");
        for (Map.Entry<Integer, String> entry : hm.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
        TreeMap<Integer, String> sortedMap = new TreeMap<>(hm);
        System.out.println("\nHashMap After Sorting (Ascending by Keys):");
        for (Map.Entry<Integer, String> entry : sortedMap.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
    }
}

slip15:3.Write a program to load names and phone numbers from text file using hash table.
B3.txt
ABC:8796465800
XYZ:9876543286
AQZ:78654324679
RMD:9087654325

B3.java
import java.io.*;
import java.util.Hashtable;
import java.util.Scanner;
public class B3 {
    public static void main(String[] args) {
        try {
            File f = new File("B3.txt"); 
            BufferedReader br = null;
            br = new BufferedReader(new FileReader(f));
            Hashtable<String, String> table = new Hashtable<>();
            Scanner sc = new Scanner(System.in);
            String line = "";
            while ((line = br.readLine()) != null) {
                String[] parts = line.split(":");
                String name = parts[0].trim();
                String number = parts[1].trim();
                if (!name.equals("") && !number.equals("")) {
                    table.put(name, number);
                }
            }
            System.out.println("Enter Name :");
            String key = sc.nextLine();
            if (table.containsKey(key)) {
                System.out.println(table.get(key));
                br.close();
                sc.close();
            }
        } catch (Exception e) {
            System.out.println(e);
        }  
    }
}

slip18:4.Write a program to load RollNo and Name from text file using hash table.
B3.txt
101	Amit
102	Riya
103	Rahul
104	Sneha

B3.java
import java.io.*;
import java.util.Hashtable;
import java.util.Scanner;
public class B3 {
    public static void main(String[] args) {
        try {
         File f = new File("B3.txt");
            BufferedReader br = new BufferedReader(new FileReader(f));
            Hashtable<String, String> table = new Hashtable<>();
            String line;
            while ((line = br.readLine()) != null) {
                String parts[] = line.split("\t");
                String roll = parts[0].trim();
                String name = parts[1].trim();
                table.put(roll, name);
                table.put(name, roll);
            }
            Scanner sc = new Scanner(System.in);
            System.out.println("Enter RollNo or Name : ");
            String key = sc.nextLine();
            if (table.containsKey(key)) {
                System.out.println("Result : " + table.get(key));
            } 
            else {
                System.out.println("Record Not Found");
            }
            br.close();
            sc.close();
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}

5.Write a Java program demonstrating ListIterator traversal of ArrayList forward and backward.
ListIteratorDemo.java
import java.util.*;
public class ListIteratorDemo{
    public static void main(String args[]){
        ArrayList<String> list = new ArrayList<String>();
        list.add("Java");
        list.add("Python");
        list.add("C++");
        list.add("JavaScript");
        list.add("SQL");
        ListIterator<String> itr = list.listIterator();
        System.out.println("Forward Traversal:");
        while(itr.hasNext()){
            System.out.println(itr.next());
        }
        System.out.println("\nBackward Traversal:");
        while(itr.hasPrevious()){
            System.out.println(itr.previous());
        }
    }
}

Slip21:6.Write a Java program to create TreeMap(BookID, BookName) and perform operations.
BookTreeMap.java
import java.util.*;
public class BookTreeMap {
    public static void main(String[] args) {
        TreeMap<Integer, String> books = new TreeMap<>();
        Scanner sc = new Scanner(System.in);
        books.put(101, "Java Programming");
        books.put(105, "Data Structures");
        books.put(103, "Operating Systems");
        books.put(102, "Computer Networks");
        books.put(104, "Database Systems");
        System.out.println("Books in Sorted Order (by BookID):");
        for(Map.Entry<Integer,String> entry : books.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
        System.out.print("\nEnter BookID to search: ");
        int id = sc.nextInt();
        if(books.containsKey(id)) {
            System.out.println("Book Found: " + books.get(id));
        } else {
            System.out.println("Book Not Found");
        }
        System.out.print("\nEnter BookID to remove: ");
        int removeId = sc.nextInt();
        if(books.remove(removeId) != null) {
            System.out.println("Book Removed Successfully");
        } else {
            System.out.println("BookID not found");
        }
        System.out.println("\nBooks after removal:");
        for(Map.Entry<Integer,String> entry : books.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
        sc.close();
    }
}

Slip22:7.Write a Java program using LinkedList to add first element, delete last element and display size.
LinkedListOperations.java
import java.util.LinkedList;
public class LinkedListOperations {
    public static void main(String[] args) {
        LinkedList<Integer> list = new LinkedList<>();
        list.addFirst(50);
        list.addFirst(30);
        list.addFirst(10);
        System.out.println("Linked List after adding elements at first: " + list);
        if (!list.isEmpty()) {
            int removed = list.removeLast();
            System.out.println("Removed last element: " + removed);
        } else {
            System.out.println("List is empty, nothing to remove!");
        }
        System.out.println("Size of Linked List: " + list.size());
    }
}

8.Write a Java program to store words in HashMap and count frequency.
WordFrequency.java
import java.util.*;
public class WordFrequency {
    public static void main(String[] args) {
        HashMap<String, Integer> hm = new HashMap<>();
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter a sentence:");
        String sentence = sc.nextLine();
        String words[] = sentence.split(" ");
        for(String w : words) {
            if(hm.containsKey(w)) {
                hm.put(w, hm.get(w) + 1);
            } else {
                hm.put(w, 1);
            }
        }
        System.out.println("\nWord Frequency:");
        for (Map.Entry<String, Integer> entry : hm.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
    }
}

________________________________________
10 Marks
Slip2:1.Write a Java program to read N names using HashSet and display in ascending order.
FriendNames.java
import java.util.*;
public class FriendNames{
    public static void main(String args[]) {
        int n;
        String name;
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter number of friends: ");
        n = sc.nextInt();
        HashSet<String> hs = new HashSet<String>();
        System.out.println("Enter friend names:");
        for(int i=1;i<=n;i++){
            name = sc.next();
            hs.add(name);
        }
        TreeSet<String> ts = new TreeSet<String>(hs);
        System.out.println("Names in Ascending Order:");
        for(String s : ts)  {
            System.out.println(s);
        }
    }
}

Slip4:2.Write a Java program to create Hashtable for mobile number and student name using Enumeration.
A4.java
import java.util.*;
public class A4{
    public static void main(String[] args) {
        Hashtable<String,String> ht = new Hashtable<String,String>();
        ht.put("Prasad","8796465800");
        ht.put("Ashish","8806503414");
        ht.put("Suhani","8629913414");
        ht.put("Sanket","7118919895");
        Enumeration<String> e = ht.keys();
        System.out.println("Student Name\tMobile Number");
        while(e.hasMoreElements()) {
            String name = e.nextElement();
            System.out.println(name + "\t\t" + ht.get(name));
        }
    }
}
Slip8:3.Write a Java program to create TreeSet of colors and display ascending order.
A3.java
import java.util.Scanner;
import java.util.TreeSet;
public class A3 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        TreeSet<String> ts = new TreeSet<String>();
        System.out.println("Enter How many Colours :");
        int n = sc.nextInt();
        System.out.println("Enter the " + n + " Colours :");
        sc.nextLine();
        for (int i = 0; i < n; i++) {
            String c = sc.nextLine();
            ts.add(c);
        }
        System.out.println("Colours in Ascending Order : " + ts);
        sc.close();
    }
}

Slip10:4.Write a Java program to store N integers in LinkedList and display negative integers.
NegativeLinkedList.java
import java.util.Scanner;
import java.util.LinkedList;
public class NegativeLinkedList {
  public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        LinkedList<Integer> list = new LinkedList<Integer>();
        System.out.print("Enter number of integers: ");
        int n = sc.nextInt();
        System.out.println("Enter " + n + " integers:");
        for(int i = 0; i < n; i++) {
            int num = sc.nextInt();
            list.add(num);
        }
        System.out.println("Negative Integers are:");
        for(int x : list) {
            if(x < 0) {
                System.out.println(x);
            }
        }
        sc.close();
    }
}

Slip16:5.Write a Java program to store N integers in collection without duplicates and display sorted order.
B1.java
import java.util.TreeSet; 
import java.util.Scanner;
public class B1 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        TreeSet<Object> ts = new TreeSet<>();
        System.out.println("Enter how many Numbers: ");
        int n = sc.nextInt();
        System.out.println("Enter the " + n + " Numbers: ");
        for (int i = 0; i < n; i++) {
            int num = sc.nextInt();
            ts.add(num);
        }
  System.out.println("Numbers in Sorted Order and without Duplication :" + ts);
    sc.close();
    }
}

Slip20:6.Create hash table maintaining mobile number and student name contact list.
A4.java
import java.util.Hashtable;
public class A4 {
    public static void main(String[] args) {
        Hashtable<String, String> hashtable = new Hashtable<String, String>();
        hashtable.put("Prasad", "8796465800");
        hashtable.put("Ashish", "8806503414");
        hashtable.put("Suhani", "8629913414");
        hashtable.put("Sanket", "7118919895");
        System.out.println(hashtable);
    }
}
  

