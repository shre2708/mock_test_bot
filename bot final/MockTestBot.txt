import java.util.*;
class Question {
    String question;
    String[] options;
    int correctAnswerIndex;
    String explanation;
    public Question(String question, String[] options, int correctAnswerIndex, String explanation) {
        this.question = question;
        this.options = options;
        this.correctAnswerIndex = correctAnswerIndex;
        this.explanation = explanation;
    }
    public boolean checkAnswer(int userAnswer) {
        return userAnswer == correctAnswerIndex;
    }
}
class User {
    String username;
    String password;
    int totalScore;
    public User(String username, String password) {
        this.username = username;
        this.password = password;
        this.totalScore = 0;
    }
    public void addScore(int score) {
        totalScore += score;
    }
    public int getTotalScore() {
        return totalScore;
    }
}
public class MockTestBot {
    static HashMap<String, Question[]> subjects = new HashMap<>();
    static HashMap<String, User> users = new HashMap<>();
    static User currentUser = null;
    public static void initializeSubjects() {
        subjects.put("Java", new Question[]{
            new Question("What is the size of the int data type in Java?",new String[]{"2 bytes","4 bytes","8 bytes","depends on platform"},1,"In Java, the int data type always occupies 4 bytes (32 bits), regardless of the underlying platform, making Java platform-independent in terms of data type sizes."),new Question("What is method overloading in Java?",new String[]{"Using the same method name in a subclass","Defining multiple methods with the same name but different parameters","Defining methods with the same name and same parameters","Inheriting methods from a parent class"},1,"In method overloading, multiple methods in the same class share the same name but differ in number or type of parameters. It provides flexibility in method calling."),new Question("What is the purpose of the finally block in exception handling?",new String[]{"To handle exceptions","To execute code only if an exception is thrown","To execute code regardless of whether an exception is thrown or not","To catch multiple exceptions"},2,"The finally block always executes, whether an exception is thrown or not. It is typically used for resource cleanup (like closing files or releasing database connections)."),new Question("What is the extension of a compiled Java class file?",new String[]{".java",".obj",".exe",".class"},3,"ava source code is saved with the .java extension. After compilation by the Java compiler (javac), it is converted into bytecode, which is stored in a .class file.")
        });
        subjects.put("Data Structure", new Question[]{
            new Question("Parenthesis checker can be best implemented using which data structure?", 
            new String[]{"Queue", "Stack", "Tree", "Graph"}, 1, 
            "A stack is best suited for checking balanced parentheses."),new Question("What is the time complexity to search an element in a binary search tree (BST) in the best case?",new String[]{"O(n)","O(log n)","O(1)","O(n log n)"},1,"In a Binary Search Tree (BST), each comparison allows you to ignore half of the remaining nodes. This logarithmic reduction gives a best-case time complexity of O(log n) if the tree is balanced."),new Question("Which traversal technique in trees visits nodes in the order Left-Root-Right?",new String[]{"Pre order","In order","Post order","Level Order"},1,"In inorder traversal, the nodes are visited in the order Left-Root-Right. This traversal results in nodes being accessed in ascending order in a Binary Search Tree (BST)."),new Question("Which data structure is used in function calls and recursion?",new String[]{"Queue","Array","Stack","tree"},2,"Stacks are used to store information about active function calls. Each recursive call pushes data onto the stack, and when a function completes, its data is popped off."),new Question("How many children can a node have in a binary tree?",new String[]{"1","2","At most 2","3"},2,"In a binary tree, each node can have at most two children, referred to as the left and right children. Trees with nodes having more than two children are known as general trees."),new Question("Which of the following data structures is used to implement Breadth-First Search (BFS)?",new String[]{"Stack","Queue","Array","Priority Queue"},1,"n Breadth-First Search (BFS), nodes are visited level by level (or breadth-wise). A queue is used to keep track of nodes to be visited, following the FIFO (First In First Out) principle. The next node is dequeued, and its neighbors are enqueued for later exploration")
        });
                subjects.put("Computer Graphics", new Question[]{
            new Question("What is a major advantage of Bresenham's Line Drawing algorithm over DDA?",new String[]{"It uses floating-point arithmetic.","It is more accurate than DDA.","It is faster because it uses integer arithmetic.","It can only draw horizontal lines."},2,"Bresenham algorithm eliminates the use of floating-point operations and only relies on integer arithmetic, making it faster and suitable for raster devices."),new Question("Which algorithm is commonly used for drawing ellipses?",new String[]{"Breshenham Algorithm","DDA algorithm","Midpoint Circle Algorithm","Midoint ellipse Algorithm"},3,"The Midpoint Ellipse Algorithm calculates points in the first quadrant and uses symmetry to draw the complete ellipse. It is an extension of the midpoint circle algorithm."),new Question("Which of the following algorithms fills a closed boundary by propagating outwards until it meets the boundary?",new String[]{"Boundary fill","Flood Fill","Scanline fill","Liang Barsky"},0,"The Boundary Fill Algorithm starts from a given point inside a region and propagates outwards until it encounters the boundary color."),new Question(" What is the Liang-Barsky algorithm used for?",new String[]{"line clipping","polygon clipping","scan conversion","circle drawing"},0,"The Liang-Barsky algorithm efficiently clips a line against a rectangular clipping window by using parametric equations."),new Question("Which of the following algorithms uses symmetry to reduce computation?",new String[]{"DDA Line Algorithm","Midpoint Circle Algorithm","Liang-Barsky Algorithm","Flood Fill Algorithm"},1,"The Midpoint Circle Algorithm calculates points only for the first octant and mirrors them to complete the circle, reducing the overall computations.")
        });
                subjects.put("DLCA", new Question[]{
            new Question("What is the primary function of a multiplexer?",new String[]{"To generate multiple signals","To select one input from many inputs","To add binary numbers","To convert analog signals to digital"},1,"A multiplexer (MUX) selects one input from multiple inputs and forwards it to the output, based on the select lines."),new Question("Which of the following gates is known as a universal gate?",new String[]{"AND","OR","XOR","NAND"},3,"The NAND gate is called a universal gate because it can be used to implement any logic function, including AND, OR, and NOT gates."),new Question(" How many flip-flops are required to build a 4-bit counter?",new String[]{"2","4","8","16"},1,"A 4-bit counter needs 4 flip-flops since each flip-flop stores 1 bit, and 4 bits are required to represent values from 0 to 15."),new Question("What is the minimum number of select lines required for a 16-to-1 multiplexer?",new String[]{"2","4","8","16"},1,"The number of select lines (n) required for an N-to-1 multiplexer is calculated as 2^n = N. For a 16-to-1 multiplexer, 4 select lines are required."),new Question("What is the role of the Program Counter (PC) in a CPU?",new String[]{"To store instructions","To store operands","To point to the next instruction to be executed"," To hold the result of the ALU"},2,"The Program Counter (PC) holds the address of the next instruction to be fetched and executed by the CPU.")
        });

    }
public static void startTest(String subject) {
    Scanner scanner = new Scanner(System.in);
    Question[] questions = subjects.get(subject);
    if (questions == null) {
        System.out.println("Sorry, subject not found.");
        return;
    }
    List<Question> questionList = Arrays.asList(questions);
    Collections.shuffle(questionList);
    int score = 0;
    for (int i = 0; i < questionList.size(); i++) {
        Question q = questionList.get(i);
        System.out.println("Question " + (i + 1) + ": " + q.question);
        for (int j = 0; j < q.options.length; j++) {
            System.out.println((j + 1) + ". " + q.options[j]);
        }
        System.out.print("Your answer: ");
        int userAnswer = scanner.nextInt() - 1;
        if (q.checkAnswer(userAnswer)) {
            System.out.println("Correct!\n");
            score++;
        } else {
            System.out.println("Wrong! " + q.explanation + "\n");
        }
    }
    System.out.println("Test finished! You scored: " + score + "/" + questions.length);
    currentUser.addScore(score);
    System.out.println("Your total score is now: " + currentUser.getTotalScore());
}
    public static void loginUser() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter username: ");
        String username = scanner.nextLine();
        System.out.print("Enter password: ");
        String password = scanner.nextLine();
        if (username.equals("admin") && password.equals("admin")) {
            adminPanel();
            return;
        }
        if (users.containsKey(username) && users.get(username).password.equals(password)) {
            currentUser = users.get(username);
            System.out.println("Login successful. Welcome, " + currentUser.username + "!");
        } else {
            System.out.println("Invalid username or password.");
        }
    }
    public static void signUpUser() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Create username: ");
        String username = scanner.nextLine();
        if (users.containsKey(username)) {
            System.out.println("Username already taken. Try again.");
            return;
        }
        System.out.print("Create password: ");
        String password = scanner.nextLine();
        users.put(username, new User(username, password));
        System.out.println("Sign-up successful! Please log in now.");
    }
    public static void adminPanel() {
        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.println("Welcome, Admin!");
            System.out.println("1. View All Users and Scores");
            System.out.println("2. Add Question to Subject");
            System.out.println("3. Remove Question from Subject");
            System.out.println("4. Log out");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); 
            switch (choice) {
                case 1:
                    users.forEach((username, user) -> 
                        System.out.println("Username: " + username + " | Total Score: " + user.getTotalScore()));
                    break;
                case 2:
                    System.out.print("Enter the subject to add a question to: ");
                    String subject = scanner.nextLine();
                    addQuestion(subject);
                    break;
                case 3:
                    System.out.print("Enter the subject to remove a question from: ");
                    String subjectToRemove = scanner.nextLine();
                    removeQuestion(subjectToRemove);
                    break;
                case 4:
                    System.out.println("Logging out from Admin Panel...");
                    return;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }

    public static void addQuestion(String subject) {
        Scanner scanner = new Scanner(System.in);
        Question[] questions = subjects.getOrDefault(subject, new Question[0]);
        System.out.print("Enter the question text: ");
        String questionText = scanner.nextLine();
        System.out.print("Enter the number of options: ");
        int optionCount = scanner.nextInt();
        scanner.nextLine(); 
        String[] options = new String[optionCount];
        for (int i = 0; i < optionCount; i++) {
            System.out.print("Enter option " + (i + 1) + ": ");
            options[i] = scanner.nextLine();
        }

        System.out.print("Enter the index of the correct answer: ");
        int correctAnswerIndex = scanner.nextInt() - 1;
        scanner.nextLine();
        System.out.print("Enter the explanation for the correct answer: ");
        String explanation = scanner.nextLine();
        Question newQuestion = new Question(questionText, options, correctAnswerIndex, explanation);
        Question[] newQuestionsArray = new Question[questions.length + 1];
        System.arraycopy(questions, 0, newQuestionsArray, 0, questions.length);
        newQuestionsArray[questions.length] = newQuestion;
        subjects.put(subject, newQuestionsArray);
        System.out.println("Question added successfully.");
    }
    public static void removeQuestion(String subject) {
        Scanner scanner = new Scanner(System.in);
        Question[] questions = subjects.get(subject);
        if (questions == null || questions.length == 0) {
            System.out.println("No questions available in this subject.");
            return;
        }
        for (int i = 0; i < questions.length; i++) {
            System.out.println((i + 1) + ". " + questions[i].question);
        }
        System.out.print("Enter the number of the question to remove: ");
        int questionIndex = scanner.nextInt() - 1;

        if (questionIndex < 0 || questionIndex >= questions.length) {
            System.out.println("Invalid question number.");
            return;
        }
        Question[] newQuestionsArray = new Question[questions.length - 1];
        for (int i = 0, j = 0; i < questions.length; i++) {
            if (i != questionIndex) newQuestionsArray[j++] = questions[i];
        }
        subjects.put(subject, newQuestionsArray);
        System.out.println("Question removed successfully.");
    }
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        initializeSubjects();
        System.out.println("Welcome to the Mock Test Bot!");
        boolean a=false;
        System.out.println("Created by Shreyas, Abhishek, Amay, Ved.");
        System.out.println("Press 1 to Start");
        int s=scanner.nextInt();
        if(s==1)
        {
            a=true;
        }
        while (a) {
            System.out.println("\n1. Sign Up");
            System.out.println("2. Log In");
            System.out.println("3. Exit");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine();
            switch (choice) {
                case 1:
                    signUpUser();
                    break;
                case 2:
                    loginUser();
                    if (currentUser != null && !currentUser.username.equals("admin")) {
                        System.out.println("Available subjects:");
                        subjects.keySet().forEach(subject -> System.out.println("- " + subject));
                        System.out.print("Choose a subject: ");
                        String chosenSubject = scanner.nextLine();
                        startTest(chosenSubject);
                    }
                    break;
                case 3:
                    System.out.println("Thank you for using the Mock Test Bot!");
                    return;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }
}
