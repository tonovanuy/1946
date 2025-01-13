import java.io.FileWriter;
import java.io.IOException;
import java.util.Scanner;

public class WriteToFile {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter file name: ");
        String filename = scanner.nextLine();
        System.out.print("Enter text: ");
        String text = scanner.nextLine();
        
        try (FileWriter writer = new FileWriter(filename)) {
            writer.write(text);
            System.out.println("Text saved to '" + filename + "'.");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
2
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Scanner;

public class CopyFile {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter source file: ");
        String source = scanner.nextLine();
        System.out.print("Enter destination file: ");
        String destination = scanner.nextLine();
        
        try (FileReader reader = new FileReader(source); FileWriter writer = new FileWriter(destination)) {
            int c;
            while ((c = reader.read()) != -1) {
                writer.write(c);
            }
            System.out.println("File copied.");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
3
import java.io.File;
import java.util.Scanner;

public class DeleteFile {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter file path: ");
        String filePath = scanner.nextLine();
        
        File file = new File(filePath);
        if (file.delete()) {
            System.out.println("File deleted.");
        } else {
            System.out.println("Deletion failed.");
        }
    }
}
4
import java.io.*;
import java.util.List;
import java.util.ArrayList;

class Student implements Serializable {
    private String name;
    private int age;
    private int id;

    public Student(String name, int age, int id) {
        this.name = name;
        this.age = age;
        this.id = id;
    }

    @Override
    public String toString() {
        return "Name: " + name + ", Age: " + age + ", ID: " + id;
    }
}

public class StudentApp {
    public static void main(String[] args) {
        List<Student> students = List.of(
            new Student("John", 20, 101),
            new Student("Anna", 22, 102),
            new Student("Mike", 19, 103)
        );

        try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("students.dat"))) {
            out.writeObject(students);
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }

        try (ObjectInputStream in = new ObjectInputStream(new FileInputStream("students.dat"))) {
            List<Student> loadedStudents = (List<Student>) in.readObject();
            loadedStudents.forEach(System.out::println);
        } catch (IOException | ClassNotFoundException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
5
import java.io.*;
import java.util.ArrayList;
import java.util.List;

class StudentManager {
    private String filename;

    public StudentManager(String filename) {
        this.filename = filename;
    }

    public void saveStudents(List<Student> students) {
        try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream(filename))) {
            out.writeObject(students);
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }

    public List<Student> loadStudents() {
        try (ObjectInputStream in = new ObjectInputStream(new FileInputStream(filename))) {
            return (List<Student>) in.readObject();
        } catch (IOException | ClassNotFoundException e) {
            return new ArrayList<>();
        }
    }

    public void addStudent(Student student) {
        List<Student> students = loadStudents();
        students.add(student);
        saveStudents(students);
    }
}

public class StudentManagerApp {
    public static void main(String[] args) {
        StudentManager manager = new StudentManager("students.dat");
        manager.addStudent(new Student("John", 20, 101));
        manager.addStudent(new Student("Anna", 22, 102));
        
        for (Student student : manager.loadStudents()) {
            System.out.println(student);
        }
    }
}
