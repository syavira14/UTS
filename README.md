# UTS
import java.util.ArrayList; // Struktur data untuk menyimpan antrian pasien
import java.util.Scanner; // Untuk input dari user

public class HospitalQueueSystemQuestion { //Mendefinisikan kelas publik bernama HospitalQueueSystemQuestion, yang merupakan kelas utama dari program.
    private static ArrayList<Patient> patientQueue = new ArrayList<>(); //Mendeklarasikan dan menginisialisasi daftar statis untuk menyimpan objek Patient.
    private static Scanner scanner = new Scanner(System.in); //scanner: Objek untuk membaca input user secara global.

    public static void main(String[] args) { //Metode utama yang dijalankan saat program dieksekusi.
        boolean running = true; //Variabel boolean untuk mengontrol loop utama program.

        System.out.println("Welcome to Hospital Queue Management System"); //Menampilkan pesan sambutan kepada pengguna.

        while (running) { //while (running): Memulai loop yang akan terus berjalan selama running bernilai true.
            displayMenu(); // Memanggil metode untuk menampilkan menu pilihan.
            int choice = getValidIntInput("Enter your choice: "); // Mengambil input pilihan dari pengguna dengan memanggil metode untuk validasi input.
            switch (choice) { switch (choice): Memeriksa nilai choice untuk menentukan tindakan yang akan diambil.
                case 1:
                    addPatient(); //memanggil metode addPatient() untuk menambahkan pasien baru.
                    break;
                case 2:
                    serveNextPatient(); //memanggil serveNextPatient() untuk melayani pasien berikutnya.
                    break;
                case 3:
                    displayQueue(); //memanggil displayQueue() untuk menampilkan antrean pasien saat ini.
                    break;
                case 4:
                    updatePriority(); //memanggil updatePriority() untuk memperbarui prioritas pasien.
                    break;
                case 5:
                    searchPatient(); //memanggil searchPatient() untuk mencari pasien.
                    break;
                case 6:
                    System.out.println("Thank you for using Hospital Queue Management System. Goodbye!"); //menampilkan pesan terima kasih
                    running = false; //mengubah running menjadi false untuk keluar dari loop.
                    break;
                default: //Jika pilihan tidak valid, menampilkan pesan kesalahan.
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

    private static void addPatient() { // Menampilkan pesan untuk menambahkan pasien baru.
        System.out.println("\n--- Add New Patient ---");
        String name = getValidStringInput("Enter patient name: "); //Mengambil nama pasien dengan memanggil metode getValidStringInput.
        int age = getValidIntInRange("Enter patient age: ", 0, 120); //Mengambil usia pasien dengan memanggil metode getValidIntInRange untuk memastikan usia dalam rentang yang valid.
        String condition = getValidStringInput("Enter patient condition: ");
        int priority = getValidIntInRange("Enter priority (1-Critical to 5-Non-urgent): ", 1, 5); //Mengambil prioritas pasien dengan memanggil metode getValidIntInRange.

        Patient newPatient = new Patient(name, age, condition, priority); //Membuat objek Patient baru dan menambahkannya ke dalam antrean pasien.
        patientQueue.add(newPatient);
        System.out.println("Patient added successfully!"); //Menampilkan pesan konfirmasi bahwa pasien telah berhasil ditambahkan.
    }

    private static void serveNextPatient() {
        if (patientQueue.isEmpty()) { //Memeriksa apakah antrean pasien kosong dan menampilkan pesan jika tidak ada pasien.
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
        if (patientQueue.isEmpty()) { //Memeriksa apakah antrean pasien kosong dan menampilkan pesan jika tidak ada pasien.
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
