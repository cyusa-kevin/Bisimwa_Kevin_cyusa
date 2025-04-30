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
// Abstract class to represent an Insurance Policy
public abstract class InsurancePolicy {
    protected String policyId;
    protected Vehicle vehicle;
    protected Person policyHolder;
    protected double coverageAmount;
    protected double premiumAmount;
    protected LocalDate policyStartDate;
    protected LocalDate policyEndDate;

    // Constructor for initializing policy details
    public InsurancePolicy(String policyId, Vehicle vehicle, Person policyHolder, double coverageAmount, LocalDate startDate, LocalDate endDate) {
        this.policyId = policyId;
        this.vehicle = vehicle;
        this.policyHolder = policyHolder;
        this.coverageAmount = coverageAmount;
        this.policyStartDate = startDate;
        this.policyEndDate = endDate;
    }

    // Abstract methods to be implemented by subclasses
    public abstract void calculatePremium();
    public abstract boolean processClaim(double claimAmount);
    public abstract void generatePolicyReport();
    public abstract boolean validatePolicy();
}
