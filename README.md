# Bisimwa Kevin Project

This repository contains three Java-based applications that demonstrate core programming concepts through real-world applications. The project includes:

1. **Advanced Motor Vehicle Insurance System**
2. **Advanced Stock Management System**
3. **Online Shopping System**

These applications are integrated into one shared system, providing a menu-driven interface that allows the user to interact with any of the systems.

## Project Structure

### 1. Advanced Motor Vehicle Insurance System
This system manages vehicle insurance policies. It includes different types of policies, such as Comprehensive, Third Party, Collision, Liability, and Roadside Assistance. The system processes claims, validates policies, and generates reports.

#### Key Classes:
- **InsurancePolicy (Abstract Class)** - The base class for all insurance policies.
- **ComprehensivePolicy, ThirdPartyPolicy, CollisionPolicy, etc.** - Concrete policy classes.
- **Vehicle, Person, Claim** - Supporting classes for vehicle, policyholder, and claims.
- **Main** - The class for interacting with the user.

#### Code: `InsurancePolicy` Class (Abstract Class)
```java
package AdvancedMotorVehicleInsuranceSystem;

import java.time.LocalDate;
import java.util.*;

abstract class InsurancePolicy {
    protected String policyId;
    protected Vehicle vehicle;
    protected Person policyHolder;
    protected double coverageAmount;
    protected double premiumAmount;
    protected LocalDate policyStartDate;
    protected LocalDate policyEndDate;

    public InsurancePolicy(String policyId, Vehicle vehicle, Person policyHolder, double coverageAmount, LocalDate startDate, LocalDate endDate) {
        this.policyId = policyId;
        this.vehicle = vehicle;
        this.policyHolder = policyHolder;
        this.coverageAmount = coverageAmount;
        this.policyStartDate = startDate;
        this.policyEndDate = endDate;
    }

    public abstract void calculatePremium();
    public abstract boolean processClaim(double claimAmount);
    public abstract void generatePolicyReport();
    public abstract boolean validatePolicy();
}

class Vehicle {
    private String vehicleId, vehicleMake, vehicleModel, vehicleType;
    private int vehicleYear;

    public Vehicle(String vehicleId, String make, String model, int year, String type) {
        if (year < 1900 || year > LocalDate.now().getYear()) throw new IllegalArgumentException("Invalid year");
        List<String> validTypes = Arrays.asList("Car", "SUV", "Truck", "Motorcycle");
        if (!validTypes.contains(type)) throw new IllegalArgumentException("Invalid vehicle type");
        this.vehicleId = vehicleId;
        this.vehicleMake = make;
        this.vehicleModel = model;
        this.vehicleYear = year;
        this.vehicleType = type;
    }

    public int getVehicleYear() { return vehicleYear; }
    public String getVehicleType() { return vehicleType; }
    public String getFullInfo() {
        return vehicleMake + " " + vehicleModel + " (" + vehicleYear + ", " + vehicleType + ")";
    }
}

class Person {
    private String personId, fullName, email, phone;
    private LocalDate dob;

    public Person(String id, String name, LocalDate dob, String email, String phone) {
        if (!email.contains("@")) throw new IllegalArgumentException("Invalid email");
        if (phone.length() != 10 || !phone.matches("\\d+")) throw new IllegalArgumentException("Invalid phone");
        this.personId = id;
        this.fullName = name;
        this.dob = dob;
        this.email = email;
        this.phone = phone;
    }

    public String getFullName() { return fullName; }
}

class Claim {
    private String claimId;
    private double claimAmount;
    private LocalDate claimDate;
    private String claimStatus;

    public Claim(String claimId, double amount) {
        this.claimId = claimId;
        this.claimAmount = amount;
        this.claimDate = LocalDate.now();
        this.claimStatus = "Pending";
    }

    public double getClaimAmount() { return claimAmount; }
    public void approve() { claimStatus = "Approved"; }
    public String getClaimStatus() { return claimStatus; }
}

class ComprehensivePolicy extends InsurancePolicy {
    public ComprehensivePolicy(String policyId, Vehicle vehicle, Person policyHolder, double coverageAmount, LocalDate startDate, LocalDate endDate) {
        super(policyId, vehicle, policyHolder, coverageAmount, startDate, endDate);
    }

    @Override
    public void calculatePremium() {
        premiumAmount = coverageAmount * 0.05; // 5% premium
    }

    @Override
    public boolean processClaim(double claimAmount) {
        if (claimAmount <= coverageAmount) {
            coverageAmount -= claimAmount;
            return true;
        }
        return false;
    }

    @Override
    public void generatePolicyReport() {
        System.out.println("==== Comprehensive Policy Report ====");
        System.out.println("Policy ID: " + policyId);
        System.out.println("Holder: " + policyHolder.getFullName());
        System.out.println("Vehicle: " + vehicle.getFullInfo());
        System.out.println("Coverage Left: $" + coverageAmount);
        System.out.println("Premium: $" + premiumAmount);
    }

    @Override
    public boolean validatePolicy() {
        return coverageAmount > 1000; // must be above a threshold
    }
}

class ThirdPartyPolicy extends InsurancePolicy {
    private double engineCapacity;

    public ThirdPartyPolicy(String policyId, Vehicle vehicle, Person policyHolder, double coverageAmount, LocalDate startDate, LocalDate endDate, double engineCapacity) {
        super(policyId, vehicle, policyHolder, coverageAmount, startDate, endDate);
        this.engineCapacity = engineCapacity;
    }

    @Override
    public void calculatePremium() {
        premiumAmount = coverageAmount * 0.03 + (engineCapacity * 0.001); // premium based on engine capacity
    }

    @Override
    public boolean processClaim(double claimAmount) {
        if (claimAmount <= coverageAmount) {
            coverageAmount -= claimAmount;
            return true;
        }
        return false;
    }

    @Override
    public void generatePolicyReport() {
        System.out.println("==== Third Party Policy Report ====");
        System.out.println("Policy ID: " + policyId);
        System.out.println("Holder: " + policyHolder.getFullName());
        System.out.println("Vehicle: " + vehicle.getFullInfo());
        System.out.println("Coverage Left: $" + coverageAmount);
        System.out.println("Premium: $" + premiumAmount);
        System.out.println("Engine Capacity: " + engineCapacity + "cc");
    }

    @Override
    public boolean validatePolicy() {
        return engineCapacity > 1000; // engine capacity must be greater than 1000cc
    }
}

class CollisionPolicy extends InsurancePolicy {
    public CollisionPolicy(String policyId, Vehicle vehicle, Person policyHolder, double coverageAmount, LocalDate startDate, LocalDate endDate) {
        super(policyId, vehicle, policyHolder, coverageAmount, startDate, endDate);
    }

    @Override
    public void calculatePremium() {
        premiumAmount = coverageAmount * 0.07; // 7% premium
    }

    @Override
    public boolean processClaim(double claimAmount) {
        if (claimAmount <= coverageAmount) {
            coverageAmount -= claimAmount;
            return true;
        }
        return false;
    }

    @Override
    public void generatePolicyReport() {
        System.out.println("==== Collision Policy Report ====");
        System.out.println("Policy ID: " + policyId);
        System.out.println("Holder: " + policyHolder.getFullName());
        System.out.println("Vehicle: " + vehicle.getFullInfo());
        System.out.println("Coverage Left: $" + coverageAmount);
        System.out.println("Premium: $" + premiumAmount);
    }

    @Override
    public boolean validatePolicy() {
        return coverageAmount > 500; // must be above a threshold for collision policies
    }
}

class LiabilityPolicy extends InsurancePolicy {
    public LiabilityPolicy(String policyId, Vehicle vehicle, Person policyHolder, double coverageAmount, LocalDate startDate, LocalDate endDate) {
        super(policyId, vehicle, policyHolder, coverageAmount, startDate, endDate);
    }

    @Override
    public void calculatePremium() {
        premiumAmount = coverageAmount * 0.04; // 4% premium
    }

    @Override
    public boolean processClaim(double claimAmount) {
        if (claimAmount <= coverageAmount) {
            coverageAmount -= claimAmount;
            return true;
        }
        return false;
    }

    @Override
    public void generatePolicyReport() {
        System.out.println("==== Liability Policy Report ====");
        System.out.println("Policy ID: " + policyId);
        System.out.println("Holder: " + policyHolder.getFullName());
        System.out.println("Vehicle: " + vehicle.getFullInfo());
        System.out.println("Coverage Left: $" + coverageAmount);
        System.out.println("Premium: $" + premiumAmount);
    }

    @Override
    public boolean validatePolicy() {
        return coverageAmount > 500; // must be above a threshold
    }
}

class RoadsideAssistancePolicy extends InsurancePolicy {
    public RoadsideAssistancePolicy(String policyId, Vehicle vehicle, Person policyHolder, double coverageAmount, LocalDate startDate, LocalDate endDate) {
        super(policyId, vehicle, policyHolder, coverageAmount, startDate, endDate);
    }

    @Override
    public void calculatePremium() {
        premiumAmount = coverageAmount * 0.02; // 2% premium
    }

    @Override
    public boolean processClaim(double claimAmount) {
        if (claimAmount <= coverageAmount) {
            coverageAmount -= claimAmount;
            return true;
        }
        return false;
    }

    @Override
    public void generatePolicyReport() {
        System.out.println("==== Roadside Assistance Policy Report ====");
        System.out.println("Policy ID: " + policyId);
        System.out.println("Holder: " + policyHolder.getFullName());
        System.out.println("Vehicle: " + vehicle.getFullInfo());
        System.out.println("Coverage Left: $" + coverageAmount);
        System.out.println("Premium: $" + premiumAmount);
    }

    @Override
    public boolean validatePolicy() {
        return coverageAmount > 100; // must be above a threshold
    }
}

 public class Main {
    private static Map<String, InsurancePolicy> policies = new HashMap<>();

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\nMenu:");
            System.out.println("1. Add New Policy");
            System.out.println("2. View Existing Policies");
            System.out.println("3. Exit");

            int choice = Integer.parseInt(scanner.nextLine());

            switch (choice) {
                case 1:
                    addNewPolicy(scanner);
                    break;
                case 2:
                    viewPolicies();
                    break;
                case 3:
                    System.out.println("Exiting program.");
                    scanner.close();
                    return;
                default:
                    System.out.println("Invalid choice, try again.");
            }
        }
    }

    public static void addNewPolicy(Scanner scanner) {
        String personId, fullName, email, phone;
        LocalDate dob;
        String vehicleId, make, model, type;
        int year = 0;
        double coverageAmount = 0;

        while (true) {
            try {
                System.out.print("Enter Person ID: ");
                personId = scanner.nextLine();
                System.out.print("Enter Full Name: ");
                fullName = scanner.nextLine();
                System.out.print("Enter DOB (yyyy-mm-dd): ");
                dob = LocalDate.parse(scanner.nextLine());
                System.out.print("Enter Email: ");
                email = scanner.nextLine();
                System.out.print("Enter Phone (10 digits): ");
                phone = scanner.nextLine();
                Person person = new Person(personId, fullName, dob, email, phone);
                break;
            } catch (Exception e) {
                System.out.println("Invalid input. Try again: " + e.getMessage());
            }
        }

        while (true) {
            try {
                System.out.print("Enter Vehicle ID: ");
                vehicleId = scanner.nextLine();
                System.out.print("Enter Vehicle Make: ");
                make = scanner.nextLine();
                System.out.print("Enter Vehicle Model: ");
                model = scanner.nextLine();
                System.out.print("Enter Vehicle Year: ");
                year = Integer.parseInt(scanner.nextLine());
                System.out.print("Enter Vehicle Type (Car, SUV, Truck, Motorcycle): ");
                type = scanner.nextLine();
                Vehicle vehicle = new Vehicle(vehicleId, make, model, year, type);
                break;
            } catch (Exception e) {
                System.out.println("Invalid vehicle input. Try again: " + e.getMessage());
            }
        }

        int policyChoice;
        while (true) {
            try {
                System.out.println("\nChoose Policy Type:");
                System.out.println("1. Comprehensive\n2. Third Party\n3. Collision\n4. Liability\n5. Roadside Assistance");
                policyChoice = Integer.parseInt(scanner.nextLine());
                if (policyChoice < 1 || policyChoice > 5) throw new Exception("Choice must be 1-5");
                break;
            } catch (Exception e) {
                System.out.println("Invalid choice. Try again: " + e.getMessage());
            }
        }

        while (true) {
            try {
                System.out.print("Enter Coverage Amount: ");
                coverageAmount = Double.parseDouble(scanner.nextLine());
                if (coverageAmount <= 0) throw new Exception("Coverage must be positive");
                break;
            } catch (Exception e) {
                System.out.println("Invalid coverage amount. Try again: " + e.getMessage());
            }
        }

        LocalDate start = LocalDate.now();
        LocalDate end = start.plusYears(1);
        InsurancePolicy policy = null;

        switch (policyChoice) {
            case 1:
                policy = new ComprehensivePolicy("POL-COMP-" + policies.size(), new Vehicle(vehicleId, make, model, year, type), new Person(personId, fullName, dob, email, phone), coverageAmount, start, end);
                break;
            case 2:
                System.out.print("Enter Engine Capacity (cc): ");
                double engineCC = Double.parseDouble(scanner.nextLine());
                policy = new ThirdPartyPolicy("POL-TP-" + policies.size(), new Vehicle(vehicleId, make, model, year, type), new Person(personId, fullName, dob, email, phone), coverageAmount, start, end, engineCC);
                break;
            case 3:
                policy = new CollisionPolicy("POL-COL-" + policies.size(), new Vehicle(vehicleId, make, model, year, type), new Person(personId, fullName, dob, email, phone), coverageAmount, start, end);
                break;
            case 4:
                policy = new LiabilityPolicy("POL-LIA-" + policies.size(), new Vehicle(vehicleId, make, model, year, type), new Person(personId, fullName, dob, email, phone), coverageAmount, start, end);
                break;
            case 5:
                policy = new RoadsideAssistancePolicy("POL-RSA-" + policies.size(), new Vehicle(vehicleId, make, model, year, type), new Person(personId, fullName, dob, email, phone), coverageAmount, start, end);
                break;
        }

        policy.calculatePremium();
        policies.put(policy.policyId, policy);
        System.out.println("Policy Added Successfully.");
    }

    public static void viewPolicies() {
        if (policies.isEmpty()) {
            System.out.println("No policies available.");
            return;
        }

        System.out.println("Existing Policies:");
        for (InsurancePolicy policy : policies.values()) {
            policy.generatePolicyReport();
            System.out.println("-----------------------------------------");
        }
    }
}

```
