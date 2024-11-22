
AADAM_NAIDOO_ST10280755
# **POE_Part1_Contract_Monthly_Claim_System**

## **Project Overview**

The **POE_Part1_Contract_Monthly_Claim_System** is a C# and SQL-based application designed to manage and track claims submitted by users (lecturers or employees). It allows users to submit claims, view claim status, and manage claim approvals. The system uses a database to store user data, claims, and status history.

The system includes the following main features:
- **Claim Submission**: Allows lecturers to submit claims for reimbursement, specifying the amount and description.
- **Status Tracker**: Tracks the status of claims, displaying the history and current status.
- **Approval Dashboard**: Provides an interface for the administrator to approve or reject claims.
- **Login System**: User authentication based on roles (Admin/Regular User).

## **Features**

- **User Roles**: The system differentiates between admin and regular users.
  - **Admin**: Can view, approve, or reject claims.
  - **Regular User**: Can submit claims and track their status.

- **Claim Tracking**: Allows users to track the status of their claims, including submission, review, and approval/rejection history.

- **Claims Submission**: Users can submit claims specifying the amount, description, hours worked, and hourly rate.

## **Technologies Used**

- **C#**: The application is developed using C# as the primary programming language.
- **SQL Server**: The backend uses SQL Server for data storage (Claims, Users, Claim Status History).
- **XAML**: User interfaces are developed using XAML for a Windows desktop application.
- **Entity Framework** (if applicable): For database interaction, an ORM can be used, although direct SQL commands are also written in this example.

## **Installation and Setup**

### **Prerequisites**
1. **SQL Server**: Make sure you have **SQL Server** installed (or SQL Server Express).
2. **Visual Studio**: Ensure that you have **Visual Studio** installed with the necessary C# and SQL tools.
3. **.NET Framework**: This project targets the **.NET Framework** (or **.NET Core**, depending on your configuration).

### **Setting Up the Database**
Before running the application, you need to set up the database. Here are the SQL scripts used for the database:

1. **Create the Database**:
   ```sql
   CREATE DATABASE ClaimSystemDB_2;
   GO
   ```

2. **Create the Tables**:
   Use the SQL script below to create the tables and populate them with sample data:

   ```sql
   -- Step 1: Create the Database
   CREATE DATABASE ClaimSystemDB_2;
   GO

   -- Step 2: Use the newly created database
   USE ClaimSystemDB_2;
   GO

   -- Step 3: Create the Users table (with IsAdmin column)
   CREATE TABLE Users (
       UserID INT IDENTITY(1,1) PRIMARY KEY,
       Username NVARCHAR(50) NOT NULL UNIQUE,
       Password NVARCHAR(255) NOT NULL,
       Role NVARCHAR(20) NOT NULL,
       IsAdmin BIT NOT NULL DEFAULT 0
   );
   GO

   -- Step 4: Create the Claims table (with additional fields for Hours Worked and Hourly Rate)
   CREATE TABLE Claims (
       ClaimID INT IDENTITY(1,1) PRIMARY KEY,
       UserID INT NOT NULL FOREIGN KEY REFERENCES Users(UserID),
       ClaimDate DATETIME NOT NULL DEFAULT GETDATE(),
       ClaimAmount DECIMAL(18,2) NOT NULL,
       ClaimStatus NVARCHAR(50) NOT NULL,
       ClaimDescription NVARCHAR(MAX) NOT NULL,
       HoursWorked DECIMAL(18,2) NOT NULL,
       HourlyRate DECIMAL(18,2) NOT NULL
   );
   GO

   -- Step 5: Create the ClaimStatusHistory table
   CREATE TABLE ClaimStatusHistory (
       StatusID INT IDENTITY(1,1) PRIMARY KEY,
       ClaimID INT NOT NULL FOREIGN KEY REFERENCES Claims(ClaimID),
       StatusDate DATETIME NOT NULL DEFAULT GETDATE(),
       StatusDescription NVARCHAR(255) NOT NULL
   );
   GO

   -- Step 6: Insert sample users (admin and regular users)
   INSERT INTO Users (Username, Password, Role, IsAdmin) VALUES
   ('admin', 'admin123', 'Admin', 1),
   ('user1', 'password123', 'User', 0);
   GO

   -- Step 7: Insert sample claims
   INSERT INTO Claims (UserID, ClaimAmount, ClaimStatus, ClaimDescription, HoursWorked, HourlyRate) VALUES
   (1, 250.50, 'Pending', 'Office supplies claim', 10, 25.05),
   (2, 500.75, 'Approved', 'Travel reimbursement', 15, 33.38);
   GO
   ```

3. **Configure the Connection String**:
   In the application code (`StatusTrackerWindow.xaml.cs` or similar), ensure the connection string points to the correct SQL Server instance and database:
   ```csharp
   private string connectionString = @"Data Source=LAPTOP-LR4U8DK7\SQLEXPRESS01;Initial Catalog=ClaimSystemDB_2;Integrated Security=True;";
   ```

### **Running the Application**
1. Open the **solution** in Visual Studio.
2. Build the solution by going to **Build > Build Solution**.
3. Run the application by pressing **F5** or using the **Start Debugging** option.

### **Usage**

1. **Login**: Enter the username and password in the login window. 
   - For **Admin**: Use `Username: admin` and `Password: admin123`.
   - For **Regular User**: Use `Username: user1` and `Password: password123`.

2. **Submit a Claim**: Regular users can submit a claim by entering the details (description, amount, hours worked, hourly rate) and clicking "Submit Claim".

3. **Track Claim Status**: View the status of submitted claims under the **Status Tracker** tab. Admins can approve or reject claims.

4. **Approve Dashboard**: Admins can review and approve/reject claims using the **Approve Dashboard**.

---

## **Contributing**

We welcome contributions! If you'd like to contribute, please fork the repository, make your changes, and submit a pull request.

 **Steps for Contributing**:
1. Fork the repository.
2. Create a new branch for your changes.
3. Commit your changes.
4. Push to your forked repository.
5. Submit a pull request describing your changes.

---

## **License**

This project is open-source and available under the [MIT License](LICENSE).
