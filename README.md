# Student-Management-System-using-Java-JDBC-MySQL
This project is a console-based application developed using Java and MySQL that allows users to manage student records efficiently. The system performs basic CRUD operations such as adding, viewing, updating, and deleting student details stored in the database.
import java.sql.*;
import java.util.Scanner;

public class StudentManagementSystem {

    static final String url = "jdbc:mysql://localhost:3306/studentdb";
    static final String user = "root";
    static final String password = "root";

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        try {
            Connection con = DriverManager.getConnection(url, user, password);

            while (true) {
                System.out.println("\n1. Add Student");
                System.out.println("2. View Students");
                System.out.println("3. Update Student");
                System.out.println("4. Delete Student");
                System.out.println("5. Exit");

                System.out.print("Enter Choice: ");
                int choice = sc.nextInt();

                switch (choice) {

                    case 1:
                        System.out.print("Enter Name: ");
                        String name = sc.next();
                        System.out.print("Enter Age: ");
                        int age = sc.nextInt();
                        System.out.print("Enter Course: ");
                        String course = sc.next();

                        PreparedStatement ps = con.prepareStatement(
                                "INSERT INTO students(name, age, course) VALUES(?,?,?)");
                        ps.setString(1, name);
                        ps.setInt(2, age);
                        ps.setString(3, course);
                        ps.executeUpdate();
                        System.out.println("Student Added!");
                        break;

                    case 2:
                        Statement st = con.createStatement();
                        ResultSet rs = st.executeQuery("SELECT * FROM students");

                        while (rs.next()) {
                            System.out.println(rs.getInt(1) + " "
                                    + rs.getString(2) + " "
                                    + rs.getInt(3) + " "
                                    + rs.getString(4));
                        }
                        break;

                    case 3:
                        System.out.print("Enter ID to Update: ");
                        int uid = sc.nextInt();
                        System.out.print("Enter New Course: ");
                        String newCourse = sc.next();

                        PreparedStatement ups = con.prepareStatement(
                                "UPDATE students SET course=? WHERE id=?");
                        ups.setString(1, newCourse);
                        ups.setInt(2, uid);
                        ups.executeUpdate();
                        System.out.println("Updated!");
                        break;

                    case 4:
                        System.out.print("Enter ID to Delete: ");
                        int did = sc.nextInt();

                        PreparedStatement dps = con.prepareStatement(
                                "DELETE FROM students WHERE id=?");
                        dps.setInt(1, did);
                        dps.executeUpdate();
                        System.out.println("Deleted!");
                        break;

                    case 5:
                        System.exit(0);
                }
            }
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
