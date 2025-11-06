# experiment-7



package experiment7;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class PartA {

    static class Employee {
        private String name;
        private double salary;

        public Employee(String name, double salary) {
            this.name = name;
            this.salary = salary;
        }

        public String getName() {
            return name;
        }

        public double getSalary() {
            return salary;
        }

        @Override
        public String toString() {
            return "Name: " + name + ", Salary: " + salary;
        }
    }

    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/your_database"; // Replace with your DB name
        String user = "root"; // Replace with your DB username
        String pass = "your_password"; // Replace with your DB password

        List<Employee> employees = new ArrayList<>();

        try {
            // Step 1: Load MySQL JDBC Driver
            Class.forName("com.mysql.cj.jdbc.Driver");

            // Step 2: Establish connection
            Connection con = DriverManager.getConnection(url, user, pass);

            // Step 3: Execute SELECT query
            String query = "SELECT Name, Salary FROM Employee";
            Statement stmt = con.createStatement();
            ResultSet rs = stmt.executeQuery(query);

            // Step 4: Add results to the list
            while (rs.next()) {
                String name = rs.getString("Name");
                double salary = rs.getDouble("Salary");
                employees.add(new Employee(name, salary));
            }

            // Step 5: Display filtered and sorted data
            System.out.println("Employees earning above 50000 sorted by salary:");
            employees.stream()
                    .filter(e -> e.getSalary() > 50000)
                    .sorted((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()))
                    .map(Employee::getName)
                    .forEach(System.out::println);

            // Step 6: Close connections
            rs.close();
            stmt.close();
            con.close();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}




package experiment7;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class PartA {

    static class Employee {
        private String name;
        private double salary;

        public Employee(String name, double salary) {
            this.name = name;
            this.salary = salary;
        }

        public String getName() {
            return name;
        }

        public double getSalary() {
            return salary;
        }

        @Override
        public String toString() {
            return "Name: " + name + ", Salary: " + salary;
        }
    }

    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/your_database"; // Replace with your DB name
        String user = "root"; // Replace with your DB username
        String pass = "your_password"; // Replace with your DB password

        List<Employee> employees = new ArrayList<>();

        try {
            // Step 1: Load MySQL JDBC Driver
            Class.forName("com.mysql.cj.jdbc.Driver");

            // Step 2: Establish connection
            Connection con = DriverManager.getConnection(url, user, pass);

            // Step 3: Execute SELECT query
            String query = "SELECT Name, Salary FROM Employee";
            Statement stmt = con.createStatement();
            ResultSet rs = stmt.executeQuery(query);

            // Step 4: Add results to the list
            while (rs.next()) {
                String name = rs.getString("Name");
                double salary = rs.getDouble("Salary");
                employees.add(new Employee(name, salary));
            }

            // Step 5: Display filtered and sorted data
            System.out.println("Employees earning above 50000 sorted by salary:");
            employees.stream()
                    .filter(e -> e.getSalary() > 50000)
                    .sorted((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()))
                    .map(Employee::getName)
                    .forEach(System.out::println);

            // Step 6: Close connections
            rs.close();
            stmt.close();
            con.close();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}


package experiment7;

import java.sql.*;
import java.util.*;

public class PartC {

    // Model
    static class Student {
        private int id;
        private String name;
        private String dept;
        private double marks;

        public Student(int id, String name, String dept, double marks) {
            this.id = id;
            this.name = name;
            this.dept = dept;
            this.marks = marks;
        }

        public int getId() { return id; }

        @Override
        public String toString() {
            return "ID: " + id + ", Name: " + name + ", Dept: " + dept + ", Marks: " + marks;
        }
    }

    // Controller (DAO Operations)
    static class StudentDAO {
        private Connection con;

        public StudentDAO(Connection con) {
            this.con = con;
        }

        public void addStudent(Student s) throws SQLException {
            String q = "INSERT INTO Student VALUES (?, ?, ?, ?)";
            PreparedStatement ps = con.prepareStatement(q);
            ps.setInt(1, s.getId());
            ps.setString(2, s.name);
            ps.setString(3, s.dept);
            ps.setDouble(4, s.marks);
            ps.executeUpdate();
        }

        public List<Student> getAllStudents() throws SQLException {
            List<Student> list = new ArrayList<>();
            Statement stmt = con.createStatement();
            ResultSet rs = stmt.executeQuery("SELECT * FROM Student");
            while (rs.next()) {
                list.add(new Student(rs.getInt(1), rs.getString(2), rs.getString(3), rs.getDouble(4)));
            }
            return list;
        }
        
        // Add update and delete methods similarly
    }

    // View / Main app
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/your_db";
        String user = "root";
        String pass = "your_password";
        Scanner sc = new Scanner(System.in);

        try (Connection con = DriverManager.getConnection(url, user, pass)) {
            Class.forName("com.mysql.cj.jdbc.Driver");
            StudentDAO dao = new StudentDAO(con);

            while (true) {
                System.out.println("\n1. Add Student\n2. View Students\n3. Exit");
                System.out.print("Enter choice: ");
                int choice = sc.nextInt();
                if (choice == 3) break;

                switch (choice) {
                    case 1:
                        System.out.print("Enter ID, Name, Dept, Marks: ");
                        dao.addStudent(new Student(sc.nextInt(), sc.next(), sc.next(), sc.nextDouble()));
                        System.out.println("Student Added!");
                        break;
                    case 2:
                        List<Student> students = dao.getAllStudents();
                        students.forEach(System.out::println);
                        break;
                    default:
                        System.out.println("Invalid choice!");
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
