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
# Advanced Online Shopping System

## Introduction

The **Advanced Online Shopping System** is a Java-based application designed to simulate an advanced online shopping experience. It supports a variety of product categories, including **Electronics**, **Clothing**, **Groceries**, **Books**, and **Accessories**, each with specific attributes and functionality. This system provides a realistic shopping interface, where users can browse products, add them to their cart, and proceed with payments. The application demonstrates advanced object-oriented principles such as **polymorphism**, **inheritance**, and **encapsulation**.

### Key Features:

1. **Product Catalog**: 
    - **ElectronicsItem**: Items such as laptops with warranty support.
    - **ClothingItem**: Apparel like T-shirts with size information.
    - **GroceriesItem**: Perishable items like food, with expiration date tracking.
    - **BooksItem**: Literature items with ISBN and description.
    - **AccessoriesItem**: Fashion and gadget accessories, including customer reviews.

2. **Shopping Cart**: 
    - Allows customers to add items to their cart and view the total price.

3. **Stock Management**: 
    - Items can be updated in stock by quantity.

4. **Item Validation**: 
    - Items are validated based on stock availability or expiration dates (for groceries).

5. **Invoice Generation**: 
    - Once an item is added to the cart, an invoice for the purchase is generated, detailing the product and price.

6. **Payment Processing**: 
    - Supports payments through **Credit Card** or **PayPal** with receipt generation.

7. **Customer Management**: 
    - The system allows creation and management of customer information, including email validation and phone number validation.

---

## Technologies Used

- **Java**: Core language for the implementation.
- **IntelliJ IDEA** or other Java IDEs for development.
- **Java Collections Framework**: For handling the shopping cart and item management.
  ```java
package AdvancedOnlineShoppingSystem;
import java.util.*;

public class AdvancedOnlineShoppingSystem {

    // Abstract Class: ShoppingItem
    abstract static class ShoppingItem {
        protected String itemId, itemName, itemDescription;
        protected double price;
        protected int stockAvailable;

        public ShoppingItem(String itemId, String itemName, String itemDescription, double price, int stockAvailable) {
            if (price <= 0) throw new IllegalArgumentException("Price must be greater than 0.");
            if (stockAvailable < 0) throw new IllegalArgumentException("Stock cannot be negative.");
            this.itemId = itemId;
            this.itemName = itemName;
            this.itemDescription = itemDescription;
            this.price = price;
            this.stockAvailable = stockAvailable;
        }

        public abstract void updateStock(int quantity);
        public abstract void addToCart(Customer customer);
        public abstract void generateInvoice(Customer customer);
        public abstract void validateItem();
    }

    // Concrete Classes (5 Total):
    // ElectronicsItem
    static class ElectronicsItem extends ShoppingItem {
        private int warrantyMonths;

        public ElectronicsItem(String itemId, String itemName, String itemDescription, double price, int stockAvailable, int warrantyMonths) {
            super(itemId, itemName, itemDescription, price, stockAvailable);
            this.warrantyMonths = warrantyMonths;
        }

        public void updateStock(int quantity) {
            this.stockAvailable += quantity;
        }

        public void addToCart(Customer customer) {
            if (this.stockAvailable <= 0) {
                System.out.println("Item out of stock: " + itemName);
                return;
            }
            customer.getCart().addItem(this);
            this.stockAvailable--;
        }

        public void generateInvoice(Customer customer) {
            System.out.println("Invoice for Electronics Item: " + itemName + ", Warranty: " + warrantyMonths + " months, Price: " + price);
        }

        public void validateItem() {
            if (stockAvailable <= 0) {
                System.out.println("Validation failed: No stock for " + itemName);
            }
        }
    }

    // ClothingItem
    static class ClothingItem extends ShoppingItem {
        private String size;

        public ClothingItem(String itemId, String itemName, String itemDescription, double price, int stockAvailable, String size) {
            super(itemId, itemName, itemDescription, price, stockAvailable);
            this.size = size;
        }

        public void updateStock(int quantity) {
            this.stockAvailable += quantity;
        }

        public void addToCart(Customer customer) {
            if (this.stockAvailable <= 0) {
                System.out.println("Item out of stock: " + itemName);
                return;
            }
            customer.getCart().addItem(this);
            this.stockAvailable--;
        }

        public void generateInvoice(Customer customer) {
            System.out.println("Invoice for Clothing Item: " + itemName + ", Size: " + size + ", Price: " + price);
        }

        public void validateItem() {
            if (stockAvailable <= 0) {
                System.out.println("Validation failed: No stock for " + itemName);
            }
        }
    }

    // GroceriesItem
    static class GroceriesItem extends ShoppingItem {
        private Date expirationDate;

        public GroceriesItem(String itemId, String itemName, String itemDescription, double price, int stockAvailable, Date expirationDate) {
            super(itemId, itemName, itemDescription, price, stockAvailable);
            this.expirationDate = expirationDate;
        }

        public void updateStock(int quantity) {
            this.stockAvailable += quantity;
        }

        public void addToCart(Customer customer) {
            if (this.stockAvailable <= 0) {
                System.out.println("Item out of stock: " + itemName);
                return;
            }
            customer.getCart().addItem(this);
            this.stockAvailable--;
        }

        public void generateInvoice(Customer customer) {
            System.out.println("Invoice for Grocery Item: " + itemName + ", Expiration: " + expirationDate + ", Price: " + price);
        }

        public void validateItem() {
            Date today = new Date();
            if (expirationDate.before(today)) {
                System.out.println("Validation failed: Expired product " + itemName);
            }
        }
    }

    // BooksItem
    static class BooksItem extends ShoppingItem {
        private String ISBN;

        public BooksItem(String itemId, String itemName, String itemDescription, double price, int stockAvailable, String ISBN) {
            super(itemId, itemName, itemDescription, price, stockAvailable);
            this.ISBN = ISBN;
        }

        public void updateStock(int quantity) {
            this.stockAvailable += quantity;
        }

        public void addToCart(Customer customer) {
            if (this.stockAvailable <= 0) {
                System.out.println("Item out of stock: " + itemName);
                return;
            }
            customer.getCart().addItem(this);
            this.stockAvailable--;
        }

        public void generateInvoice(Customer customer) {
            System.out.println("Invoice for Book: " + itemName + ", ISBN: " + ISBN + ", Price: " + price);
        }

        public void validateItem() {
            if (stockAvailable <= 0) {
                System.out.println("Validation failed: No stock for " + itemName);
            }
        }
    }

    // AccessoriesItem
    static class AccessoriesItem extends ShoppingItem {
        private String customerReview;

        public AccessoriesItem(String itemId, String itemName, String itemDescription, double price, int stockAvailable) {
            super(itemId, itemName, itemDescription, price, stockAvailable);
            this.customerReview = "";
        }

        public void addReview(String review) {
            this.customerReview = review;
        }

        public void updateStock(int quantity) {
            this.stockAvailable += quantity;
        }

        public void addToCart(Customer customer) {
            if (this.stockAvailable <= 0) {
                System.out.println("Item out of stock: " + itemName);
                return;
            }
            customer.getCart().addItem(this);
            this.stockAvailable--;
        }

        public void generateInvoice(Customer customer) {
            System.out.println("Invoice for Accessory: " + itemName + ", Price: " + price + ", Review: " + customerReview);
        }

        public void validateItem() {
            if (stockAvailable <= 0) {
                System.out.println("Validation failed: No stock for " + itemName);
            }
        }
    }

    // Customer Class
    static class Customer {
        private String customerId, customerName, email, address, phone;
        private ShoppingCart cart;

        public Customer(String customerId, String customerName, String email, String address, String phone) {
            if (!email.contains("@")) throw new IllegalArgumentException("Invalid email.");
            if (phone.length() < 8) throw new IllegalArgumentException("Invalid phone number.");
            this.customerId = customerId;
            this.customerName = customerName;
            this.email = email;
            this.address = address;
            this.phone = phone;
            this.cart = new ShoppingCart(this);
        }

        public ShoppingCart getCart() {
            return cart;
        }
    }

    // ShoppingCart Class
    static class ShoppingCart {
        private static int cartCounter = 1;
        private String cartId;
        private List<ShoppingItem> cartItems;
        private double totalPrice;
        private Customer customer;

        public ShoppingCart(Customer customer) {
            this.cartId = "CART" + (cartCounter++);
            this.cartItems = new ArrayList<>();
            this.totalPrice = 0;
            this.customer = customer;
        }

        public void addItem(ShoppingItem item) {
            cartItems.add(item);
            totalPrice += item.price;
        }

        public double getTotalPrice() {
            return totalPrice;
        }

        public List<ShoppingItem> getCartItems() {
            return cartItems;
        }
    }

    // Payment Class
    static class Payment {
        private String paymentId;
        private String paymentMethod;
        private double amountPaid;
        private Date transactionDate;

        public Payment(String paymentId, String paymentMethod, double amountPaid) {
            if (!(paymentMethod.equalsIgnoreCase("Credit Card") || paymentMethod.equalsIgnoreCase("PayPal"))) {
                throw new IllegalArgumentException("Invalid payment method!");
            }
            this.paymentId = paymentId;
            this.paymentMethod = paymentMethod;
            this.amountPaid = amountPaid;
            this.transactionDate = new Date();
        }

        public void printReceipt() {
            System.out.println("Payment ID: " + paymentId);
            System.out.println("Method: " + paymentMethod);
            System.out.println("Amount Paid: $" + amountPaid);
            System.out.println("Date: " + transactionDate);
        }
    }

    // Main Class
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.println("Enter Customer Details:");
        System.out.print("Name: ");
        String name = sc.nextLine();
        System.out.print("Email: ");
        String email = sc.nextLine();
        System.out.print("Address: ");
        String address = sc.nextLine();
        System.out.print("Phone: ");
        String phone = sc.nextLine();

        Customer customer = new Customer("CUST001", name, email, address, phone);

        ElectronicsItem laptop = new ElectronicsItem("E001", "Laptop", "Gaming Laptop", 1200, 5, 24);
        ClothingItem shirt = new ClothingItem("C001", "T-Shirt", "Summer Collection", 30, 20, "M");

        System.out.println("Available Items:");
        System.out.println("1. Laptop - $1200");
        System.out.println("2. T-Shirt - $30");

        System.out.print("Enter choice to add to cart (1 or 2): ");
        int choice = sc.nextInt();

        if (choice == 1) {
            laptop.addToCart(customer);
        } else if (choice == 2) {
            shirt.addToCart(customer);
        } else {
            System.out.println("Invalid choice.");
        }

        System.out.println("Cart Total: $" + customer.getCart().getTotalPrice());

        System.out.print("Enter Payment Method (Credit Card/PayPal): ");
        sc.nextLine(); // consume newline
        String paymentMethod = sc.nextLine();
        Payment payment = new Payment("PAY001", paymentMethod, customer.getCart().getTotalPrice());

        payment.printReceipt();
    }
}
```

