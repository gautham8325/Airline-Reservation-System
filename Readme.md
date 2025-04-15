# Airline Reservation System

## Instructions to Compile and Run the Program

### Method 1: Using Terminal

1. Navigate to the `src/` directory:

    ```bash
    cd src/
    ```

2. Compile the Java files:

    ```bash
    javac *.java
    ```

3. Run the program:

    ```bash
    java SystemRunner
    ```

    - You can define your input and output paths here.

### Method 2: Using Eclipse

1. Open Eclipse and create a new Java project.
2. Add the source files from the `src/` directory to the project.
3. Locate the `SystemRunner.java` file in the project explorer.
4. Right-click on `SystemRunner.java` and select **Run As > Run Configurations**.
5. In the **Run Configurations** window:
    - Go to the **Arguments** tab.
    - In the **Program Arguments** field, enter the following paths:
      ```
      ./in/inputfile1.txt ./in/inputfile2.txt ./out/output.txt
      ```

6. Click **Apply** and then **Run** to execute the program.

---

## Brief Description of the Implementation

### 1. Coding Environment

- **Language**: Java 8
- Some methods or members are marked with "default" access (instead of private) to facilitate unit and integration testing.

### 2. Assumptions

- Prices are integers (as per the example in the provided PDF).
- Passengers choose random seats when booking a flight.
- A passenger can book the same flight only once. For example:
  - If `BookPassenger,MikeSmith,LAS,LAX` appears twice in `inputfile2.txt`, only one ticket is booked for MikeSmith on flight `A124`.
  - However, if `BookPassenger,KennethHarris,CHI,DFW` appears twice and there are two flights (`K792` and `A792`), KennethHarris books both flights.
- If a passenger books a flight at a specific price and the price later changes, the original price remains unless the passenger cancels and rebooks.
- If a passenger cancels one of two flights with the same origin and destination, the system cancels the most expensive flight.
- Only one type of passenger is considered.
- Transactions are handled in a single-threaded manner.

### 3. Implementation Details

#### Classes, Data Structures, and Logic

- **Flight**: Represents flight information and reservations.
  - Members:
    - Flight number
    - Total seats
    - Price per seat
    - Origin and destination codes
    - Reservation list
    - Seat pool (stored as a `LinkedList`)
  - Key Operations:
    - Reservations are stored in a `HashMap<Passenger, ReservationItem>`.
    - Available seats are managed using a `LinkedList<Integer>`.
    - Booking a passenger generates a `ReservationItem` and removes a seat from the pool.
    - Canceling a passenger restores the seat to the pool.
    - Price changes update the flight price.

- **FlightReservationSystem**: Manages all flights and handles transactions.
  - Data Structures:
    - `HashMap<OriginDestinationPair, TreeSet<Flight>>`: Groups flights by origin and destination, sorted by price.
    - `HashMap<String, Flight>`: Maps flight numbers to flight objects for quick access.
  - Key Operations:
    - Booking a passenger selects the cheapest available flight.
    - Canceling a passenger removes the most expensive flight.
    - Price changes reorder the `TreeSet` of flights.

- **SystemRunner**: Entry point of the program, processes transactions, and generates reports.

- **FlightInCSVIndexEnum**: Enum representing the column indices of flight data in the CSV file.

- **FlightSummary**: Contains flight statistics such as total revenue and available seats.

- **OriginDestinationPair**: Represents an origin-destination pair.

- **Passenger**: Represents passenger information, including the name.

- **ReservationItem**: Represents a reservation, including passenger, seat number, and price.

- **TransactionTypeEnum**: Enum for transaction types (e.g., book, cancel, change price).

---
