# UTS
import java.util.ArrayList; // Struktur data untuk menyimpan antrian pasien
import java.util.Scanner; // Untuk input dari user

public class HospitalQueueSystemQuestion {
    private static ArrayList<Patient> patientQueue = new ArrayList<>(); //patientQueue: Menyimpan daftar pasien dalam bentuk ArrayList.
    private static Scanner scanner = new Scanner(System.in); //scanner: Objek untuk membaca input user secara global.

    public static void main(String[] args) {
        boolean running = true; //Loop utama program yang terus berjalan sampai user memilih exit

        System.out.println("Welcome to Hospital Queue Management System");

        while (running) { //Loop while (running): Program berjalan sampai user memilih opsi 6 (Exit).
            displayMenu();
            int choice = getValidIntInput("Enter your choice: "); //Menampilkan menu dan memproses pilihan user

            switch (choice) { //switch-case: Memproses pilihan menu user.
                case 1:
                    addPatient();
                    break;
                case 2:
                    serveNextPatient();
                    break;
                case 3:
                    displayQueue();
                    break;
                case 4:
                    updatePriority();
                    break;
                case 5:
                    searchPatient();
                    break;
                case 6:
                    System.out.println("Thank you for using Hospital Queue Management System. Goodbye!");
                    running = false;
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }

        scanner.close(); //scanner.close(): Membersihkan resource scanner setelah program selesai.
    }

    private static void displayMenu() { //Menampilkan menu teks dengan 6 opsi.
        System.out.println("\n===== HOSPITAL QUEUE SYSTEM =====");
        System.out.println("1. Add a new patient to the queue");
        System.out.println("2. Serve next patient");
        System.out.println("3. Display current queue");
        System.out.println("4. Update patient priority");
        System.out.println("5. Search for a patient");
        System.out.println("6. Exit");
        System.out.println("=================================");
    }

    private static void addPatient() { //Meminta input nama, usia, kondisi medis, dan prioritas.
        System.out.println("\n--- Add New Patient ---");
        String name = getValidStringInput("Enter patient name: ");
        int age = getValidIntInRange("Enter patient age: ", 0, 120); Validasi input: Usia antara 0-120. Prioritas antara 1-5.
        String condition = getValidStringInput("Enter patient condition: ");
        int priority = getValidIntInRange("Enter priority (1-Critical to 5-Non-urgent): ", 1, 5);

        Patient newPatient = new Patient(name, age, condition, priority); //Membuat objek Patient baru dan menambahkannya ke patientQueue.
        patientQueue.add(newPatient);
        System.out.println("Patient added successfully!");
    }

    private static void serveNextPatient() {
        if (patientQueue.isEmpty()) {
            System.out.println("\nNo patients in the queue!");
            return;
        }

        // Sort by priority (lower number = higher priority)
        if (patientQueue.isEmpty()) {
            System.out.println("No patients in queue.");
        } else {
            Patient next = patientQueue.remove(0); //Menghapus pasien prioritas tertinggi (indeks 0) dari antrian.
            System.out.println("Serving patient: " + next.getName() + //Menampilkan detail pasien yang dilayani.
                " | Age: " + next.getAge() +
                " | Condition: " + next.getCondition() +
                " | Priority: " + getPriorityText(next.getPriority()));
        }
    }

    private static void displayQueue() {
        if (patientQueue.isEmpty()) {
            System.out.println("No patients in queue.");
        } else {
            System.out.println("\n--- Current Patient Queue ---");
            for (int i = 0; i < patientQueue.size(); i++) { //Daftar pasien dalam format: No. Nama | Usia | Kondisi | Prioritas. Jika antrian kosong, tampilkan pesan.
                Patient p = patientQueue.get(i);
                System.out.println((i + 1) + ". " + p.getName() +
                    " | Age: " + p.getAge() +
                    " | Condition: " + p.getCondition() +
                    " | Priority: " + getPriorityText(p.getPriority()));
            }
        }
    }

    private static void updatePriority() {
        if (patientQueue.isEmpty()) { //Tampilkan daftar pasien.
            System.out.println("\nNo patients in the queue!");
            return;
        }

        System.out.println("\n--- Update Patient Priority ---");
        displayQueue(); //Minta input nomor urut pasien dan prioritas baru.
        int patientIndex = getValidIntInRange("Enter patient number to update (1-" + patientQueue.size() + "): ", 1, patientQueue.size());
        int newPriority = getValidIntInRange("Enter new priority (1-Critical to 5-Non-urgent): ", 1, 5); //Update prioritas pasien terpilih.

        Patient patient = patientQueue.get(patientIndex - 1);
        patient.setPriority(newPriority);
        System.out.println("Priority updated successfully!");
    }

    private static void searchPatient() {
        if (patientQueue.isEmpty()) {
            System.out.println("\nNo patients in the queue!");
            return;
        }

        System.out.println("\n--- Search Patient ---");
        String searchTerm = getValidStringInput("Enter patient name or condition to search: ").toLowerCase(); //Mencari pasien berdasarkan nama atau kondisi medis (tidak case-sensitive).

        System.out.println("\nSearch Results:");
        System.out.printf("%-10s %-5s %-20s %-15s%n", "Priority", "Age", "Name", "Condition");
        System.out.println("--------------------------------------------------");

        boolean found = false;
        for (Patient patient : patientQueue) {
            if (patient.getName().toLowerCase().contains(searchTerm) ||
                patient.getCondition().toLowerCase().contains(searchTerm)) {
                System.out.printf("%-10s %-5d %-20s %-15s%n",
                        getPriorityText(patient.getPriority()),
                        patient.getAge(),
                        patient.getName(),
                        patient.getCondition());
                found = true;
            }
        }
        if (!found) {
            System.out.println("No matching patients found.");
        }
    }

    private static String getPriorityText(int priority) { //Mengonversi angka prioritas (1-5) ke teks (e.g., 1 → "Critical").
        switch (priority) {
            case 1:
                return "1-Critical";
            case 2:
                return "2-Urgent";
            case 3:
                return "3-High";
            case 4:
                return "4-Medium";
            case 5:
                return "5-Non-urgent";
            default:
                return "Unknown";
        }
    }

    private static int getValidIntInput(String prompt) { // Memastikan input adalah integer
        int value;
        while (true) {
            System.out.print(prompt);
            try {
                value = Integer.parseInt(scanner.nextLine().trim());
                break;
            } catch (NumberFormatException e) {
                System.out.println("Invalid input. Please enter a number.");
            }
        }
        return value;
    }

    private static int getValidIntInRange(String prompt, int min, int max) { // Memastikan input integer dalam range tertentu
        int value;
        while (true) {
            value = getValidIntInput(prompt);
            if (value >= min && value <= max) {
                break;
            }
            System.out.println("Please enter a value between " + min + " and " + max + ".");
        }
        return value;
    }

    private static String getValidStringInput(String prompt) { // Memastikan input string tidak kosong
        String value;
        while (true) {
            System.out.print(prompt);
            value = scanner.nextLine().trim();
            if (!value.isEmpty()) {
                break;
            }
            System.out.println("Input cannot be empty. Please try again.");
        }
        return value;
    }
}

class Patient { //name, age, condition: Data dasar pasien.
    private String name; //Variabel private dengan getter/setter.
    private int age;
    private String condition;
    private int priority; // 1 (Critical) to 5 (Non-urgent)

    public Patient(String name, int age, String condition, int priority) {
        this.name = name;
        this.age = age;
        this.condition = condition;
        this.priority = priority;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getCondition() {
        return condition;
    }

    public int getPriority() {
        return priority;
    }

    public void setPriority(int priority) {
        this.priority = priority;
    }
}
