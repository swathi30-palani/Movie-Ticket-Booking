import java.util.*;

public class Main {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // ================= SEATS =================
        Map<Integer, ArrayList<String>> seatMap = new HashMap<>();

        seatMap.put(1, new ArrayList<>(Arrays.asList(
                "A1","A2","A3","A4","A5","A6","A7","A8","A9","A10")));

        seatMap.put(2, new ArrayList<>(Arrays.asList(
                "B1","B2","B3","B4","B5","B6","B7","B8","B9","B10")));

        seatMap.put(3, new ArrayList<>(Arrays.asList(
                "C1","C2","C3","C4","C5","C6","C7","C8","C9","C10")));

        // ================= THEATERS =================
        System.out.println("===== THEATERS =====");
        System.out.println("1. PVR Cinemas");
        System.out.println("2. INOX");
        System.out.println("3. Sathyam Cinemas");
        System.out.println("4. Cinepolis");

        System.out.print("Enter Theater Number: ");
        int theaterChoice = sc.nextInt();

        String theaterName = switch (theaterChoice) {
            case 1 -> "PVR Cinemas";
            case 2 -> "INOX";
            case 3 -> "Sathyam Cinemas";
            case 4 -> "Cinepolis";
            default -> {
                System.out.println("Invalid Theater!");
                yield null;
            }
        };

        if (theaterName == null) return;

        // ================= MOVIES =================
        System.out.println("\n===== MOVIES =====");
        System.out.println("1. Inception");
        System.out.println("2. Interstellar");
        System.out.println("3. Avengers");
        System.out.println("4. Joker");

        System.out.print("Enter Movie Number: ");
        int movieChoice = sc.nextInt();

        String movieName = switch (movieChoice) {
            case 1 -> "Inception";
            case 2 -> "Interstellar";
            case 3 -> "Avengers";
            case 4 -> "Joker";
            case 5 -> "Leo";
            default -> {
                System.out.println("Invalid Movie!");
                yield null;
            }
        };

        if (movieName == null) return;

        // ================= LANGUAGE =================
        System.out.println("\n===== LANGUAGES =====");
        System.out.println("1. English");
        System.out.println("2. Hindi");
        System.out.println("3. Tamil");
        System.out.println("4. Telugu");

        System.out.print("Choose Language: ");
        int langChoice = sc.nextInt();

        String language = switch (langChoice) {
            case 1 -> "English";
            case 2 -> "Hindi";
            case 3 -> "Tamil";
            case 4 -> "Telugu";
            default -> {
                System.out.println("Invalid Language!");
                yield null;
            }
        };

        if (language == null) return;

        // ================= SHOW TIME =================
        System.out.println("\n===== SHOW TIMINGS =====");
        System.out.println("1. Morning (10 AM)");
        System.out.println("2. Afternoon (2 PM)");
        System.out.println("3. Evening (6 PM)");
        System.out.println("4. Night (9 PM)");

        System.out.print("Choose Timing: ");
        int timeChoice = sc.nextInt();

        String showTime = switch (timeChoice) {
            case 1 -> "Morning (10 AM)";
            case 2 -> "Afternoon (2 PM)";
            case 3 -> "Evening (6 PM)";
            case 4 -> "Night (9 PM)";
            default -> {
                System.out.println("Invalid Time!");
                yield null;
            }
        };

        if (showTime == null) return;

        // ================= TICKETS =================
        System.out.print("\nEnter Number of Tickets: ");
        int tickets = sc.nextInt();
        sc.nextLine();

        ArrayList<String> bookedSeats = new ArrayList<>();
        int totalTicketCost = 0;

        // ================= SEAT BOOKING =================
        for (int i = 0; i < tickets; i++) {

            System.out.println("\n===== TICKET " + (i + 1) + " =====");
            System.out.println("1. Normal  - Rs.150");
            System.out.println("2. Premium - Rs.250");
            System.out.println("3. VIP     - Rs.400");

            System.out.print("Choose Category: ");
            int category = sc.nextInt();
            sc.nextLine();

            if (!seatMap.containsKey(category)) {
                System.out.println("Invalid Category!");
                i--;
                continue;
            }

            String categoryName;
            int price;

            if (category == 1) {
                categoryName = "Normal";
                price = 150;
            } else if (category == 2) {
                categoryName = "Premium";
                price = 250;
            } else {
                categoryName = "VIP";
                price = 400;
            }

            System.out.println("Available Seats: " + seatMap.get(category));

            System.out.print("Enter Seat: ");
            String seat = sc.nextLine().toUpperCase();

            if (!seatMap.get(category).contains(seat)) {
                System.out.println("Seat Not Available!");
                i--;
                continue;
            }

            bookedSeats.add(seat + " (" + categoryName + ")");
            totalTicketCost += price;

            seatMap.get(category).remove(seat);

            // continuous option
            System.out.print("Do you want continuous next seats? (yes/no): ");
            String cont = sc.nextLine();

            if (cont.equalsIgnoreCase("yes")) {
                for (int j = i + 1; j < tickets; j++) {
                    if (!seatMap.get(category).isEmpty()) {
                        String nextSeat = seatMap.get(category).get(0);
                        bookedSeats.add(nextSeat + " (" + categoryName + ")");
                        totalTicketCost += price;
                        seatMap.get(category).remove(0);
                        i++;
                    }
                }
            }
        }

        // ================= SNACKS + OFFERS =================
        System.out.println("\n===== SNACKS MENU =====");
        System.out.println("1. Popcorn - Rs.100");
        System.out.println("2. Soft Drink - Rs.80");
        System.out.println("3. Combo - Rs.160");
        System.out.println("4. Family Combo - Rs.300");
        System.out.println("5. None");

        System.out.println("\nOFFERS:");
        System.out.println("- VIP seats get 20% OFF snacks");
        System.out.println("- Family combo save money");

        System.out.print("Choose Snack: ");
        int snackChoice = sc.nextInt();

        int snackCost = switch (snackChoice) {
            case 1 -> 100;
            case 2 -> 80;
            case 3 -> 160;
            case 4 -> 300;
            case 5 -> 0;
            default -> 0;
        };

        // VIP discount on snacks
        boolean vip = bookedSeats.stream().anyMatch(s -> s.contains("VIP"));
        if (vip && snackCost > 0) {
            snackCost = (int) (snackCost * 0.8);
            System.out.println("VIP 20% Snack Discount Applied!");
        }

        // ================= COUPON =================
        sc.nextLine();
        System.out.print("\nEnter Coupon: ");
        String coupon = sc.nextLine();

        double discount = 0;
        if (coupon.equalsIgnoreCase("MOVIE10")) discount = 0.10;
        if (coupon.equalsIgnoreCase("VIP50")) discount = 0.50;

        // ================= AC + PARKING =================
        System.out.print("\nDo you require AC? (yes/no): ");
        String ac = sc.nextLine();

        System.out.print("Do you require Parking? (yes/no): ");
        String park = sc.nextLine();

        int parkingCost = 0;
        String vehicleType = "None";

        if (park.equalsIgnoreCase("yes")) {

            System.out.println("\nVehicle Type:");
            System.out.println("1. Bike");
            System.out.println("2. Car");
            System.out.println("3. Van");

            int v = sc.nextInt();

            System.out.print("Enter Number of Vehicles: ");
            int count = sc.nextInt();

            if (v == 1) {
                vehicleType = "Bike";
                parkingCost = count * 20;
            } else if (v == 2) {
                vehicleType = "Car";
                parkingCost = count * 50;
            } else {
                vehicleType = "Van";
                parkingCost = count * 100;
            }
        }

        // ================= PAYMENT =================
        System.out.println("\nPayment:");
        System.out.println("1. Cash");
        System.out.println("2. Card");
        System.out.println("3. UPI");

        int pay = sc.nextInt();

        String payment = switch (pay) {
            case 1 -> "Cash";
            case 2 -> "Card";
            case 3 -> "UPI";
            default -> "Unknown";
        };

        // ================= BILL =================
        double total = totalTicketCost + snackCost + parkingCost;
        total -= total * discount;

        String bookingId = "MOV" + (int)(Math.random() * 9000 + 1000);

        // ================= OUTPUT =================
        System.out.println("\n========== BOOKING ==========");
        System.out.println("ID: " + bookingId);
        System.out.println("Theater: " + theaterName);
        System.out.println("Movie: " + movieName);
        System.out.println("Language: " + language);
        System.out.println("Time: " + showTime);
        System.out.println("Seats: " + bookedSeats);
        System.out.println("AC: " + ac);
        System.out.println("Parking: " + park);
        System.out.println("Vehicle: " + vehicleType);
        System.out.println("Tickets Cost: " + totalTicketCost);
        System.out.println("Snacks Cost: " + snackCost);
        System.out.println("Parking Cost: " + parkingCost);
        System.out.println("Total Bill: " + total);
        System.out.println("Payment: " + payment);

        // ================= CANCEL =================
        sc.nextLine();
        System.out.print("\nCancel booking? (yes/no): ");
        String cancel = sc.nextLine();

        if (cancel.equalsIgnoreCase("yes")) {
            System.out.println("\nBooking Cancelled");
            System.out.println("Refund: " + totalTicketCost);
        } else {
            System.out.println("\nEnjoy your movie ");
        }

        sc.close();
    }
}
