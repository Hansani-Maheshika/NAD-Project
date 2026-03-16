# Student Management System

A comprehensive web-based application designed to manage student information using enterprise Java technologies.

## Project Overview

This application implements a complete student information management system following the **Model-View-Controller (MVC)** architectural pattern. It provides functionality for registering, viewing, updating, and deleting student records with a user-friendly web interface.

## Architecture

### MVC Pattern

The application follows the clean MVC architectural pattern:

- **Model** (`com.student.model`): Data representation
  - `Student.java` - Student entity class

- **View** (`src/main/webapp/jsp`): User interface
  - `student-list.jsp` - Display all students in tabular format
  - `student-form.jsp` - Form for adding/editing students
  - `index.jsp` - Home page
  - `error.jsp` - Error page handling

- **Controller** (`com.student.controller`): Request handling
  - `StudentController.java` - Servlet for managing HTTP requests and routing

### Data Access Layer

- **DAO** (`com.student.dao`): Database operations
  - `StudentDAO.java` - Handles all database operations using JDBC

## Technology Stack

### Backend
- **Java 11+** - Core programming language
- **Servlets** - Web request handling
- **JSP (JavaServer Pages)** - Dynamic web pages
- **JDBC** - Database connectivity
- **Maven** - Build management

### Frontend
- **HTML5** - Markup
- **CSS3** - Styling with gradients and animations
- **JavaScript** - Client-side validation
- **Bootstrap-like design** - Responsive UI

### Database
- **MySQL 8.0+** - Relational database management system
- **MySQL Connector/J** - JDBC driver for MySQL

### Server
- **Apache Tomcat 9+** - Application server

## Project Structure

```
student-management-system/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/student/
│   │   │       ├── model/
│   │   │       │   └── Student.java
│   │   │       ├── controller/
│   │   │       │   └── StudentController.java
│   │   │       └── dao/
│   │   │           └── StudentDAO.java
│   │   ├── resources/
│   │   │   └── database/
│   │   │       └── init.sql
│   │   └── webapp/
│   │       ├── jsp/
│   │       │   ├── student-list.jsp
│   │       │   └── student-form.jsp
│   │       ├── css/
│   │       │   └── style.css
│   │       ├── js/
│   │       │   └── validation.js
│   │       ├── WEB-INF/
│   │       │   └── web.xml
│   │       ├── index.jsp
│   │       └── error.jsp
│   └── test/
└── pom.xml
```

## Features

### Student Management Functions

1. **Register New Student**
   - Add new student records with complete information
   - Form validation (client-side and server-side)
   - Data includes: Name, Registration Number, Email, Phone, Field, Date of Birth, Address

2. **View All Students**
   - Display all student records in a structured table
   - Responsive table design
   - Search and filter capabilities

3. **View Student Details**
   - Click on any student to view complete information
   - Detailed student profile view

4. **Update Student Information**
   - Edit existing student records
   - Modify any student field
   - Update reflected immediately in database

5. **Delete Student Records**
   - Remove student profiles from the system
   - Confirmation before deletion
   - Cascading delete support

## Database Schema

### Students Table

```sql
CREATE TABLE students (
    studentId INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    regNo VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL,
    phone VARCHAR(20) NOT NULL,
    field VARCHAR(100) NOT NULL,
    dateOfBirth DATE NOT NULL,
    address VARCHAR(255) NOT NULL,
    createdDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updatedDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### Indexes
- `idx_regNo` - On registration number for quick lookup
- `idx_email` - On email for validation
- `idx_name` - On student name for searching
- `idx_field` - On field for filtering

## Installation & Setup

### Prerequisites

1. **Java Development Kit (JDK)**
   - Version 11 or higher
   - Set JAVA_HOME environment variable

2. **Apache Tomcat**
   - Version 9.0 or higher
   - Download from: https://tomcat.apache.org/

3. **MySQL Database**
   - Version 8.0 or higher
   - Download from: https://www.mysql.com/

4. **Maven**
   - Version 3.6 or higher
   - Download from: https://maven.apache.org/

### Step 1: Database Setup

1. Open MySQL command line or MySQL Workbench
2. Execute the SQL script to create database and tables:
   ```bash
   mysql -u root -p < src/main/resources/database/init.sql
   ```

3. Update database credentials in `web.xml`:
   ```xml
   <context-param>
       <param-name>jdbcURL</param-name>
       <param-value>jdbc:mysql://localhost:3306/student_management_db</param-value>
   </context-param>
   
   <context-param>
       <param-name>jdbcUsername</param-name>
       <param-value>root</param-value>
   </context-param>
   
   <context-param>
       <param-name>jdbcPassword</param-name>
       <param-value>your_password</param-value>
   </context-param>
   ```

### Step 2: Build the Project

Navigate to the project directory and run:

```bash
mvn clean install
```

This will:
- Compile all Java source files
- Package the application as a WAR file
- Create `student-management-system.war` in the target directory

### Step 3: Deploy to Tomcat

#### Option A: Manual Deployment
1. Copy the WAR file to Tomcat's webapps directory:
   ```bash
   cp target/student-management-system.war $TOMCAT_HOME/webapps/
   ```

2. Tomcat will automatically extract and deploy the application

#### Option B: Using Maven Plugin
```bash
mvn tomcat7:deploy
```

### Step 4: Start Tomcat Server

```bash
$TOMCAT_HOME/bin/startup.sh       # Linux/Mac
$TOMCAT_HOME\bin\startup.bat      # Windows
```

### Step 5: Access the Application

Open your web browser and navigate to:
```
http://localhost:8080/student-management-system
```

Or directly to the student list:
```
http://localhost:8080/student-management-system/student?action=list
```

## Usage Guide

### Registering a New Student

1. Click "Add New Student" button
2. Fill in all required fields:
   - Full Name (letters and spaces only)
   - Registration Number (unique)
   - Email Address (valid format)
   - Phone Number (10-15 digits)
   - Field of Study (dropdown selection)
   - Date of Birth (must be before today)
   - Address

3. Click "Register" button
4. You'll be redirected to the student list

### Viewing All Students

1. Click "Home" or "Dashboard"
2. All registered students are displayed in a table format
3. Table shows: ID, Name, RegNo, Email, Phone, Field, DOB, Address

### Editing Student Information

1. In the student list, click the "Edit" button for any student
2. Modify the desired fields
3. Click "Update" to save changes
4. Changes are immediately reflected in the database

### Deleting a Student

1. In the student list, click the "Delete" button
2. Confirm the deletion when prompted
3. The student record is permanently removed from the system

## Input Validation

### Client-Side Validation

Implemented in `validation.js`:

- **Name**: 
  - Required, minimum 3 characters
  - Letters and spaces only
  
- **Email**: 
  - Required, valid email format
  - Pattern: `name@domain.com`

- **Phone**: 
  - Required, 10-15 digits
  - Supports dashes and spaces

- **Date of Birth**: 
  - Required, must be before today
  - Valid date format

- **Address**: 
  - Required, minimum 5 characters

### Server-Side Validation

Implemented in `StudentController.java`:

- All inputs are validated on the server
- SQL injection prevention using PreparedStatements
- Data type validation
- Business logic validation

## API Endpoints

### Student Controller Servlet

**Base URL**: `/student`

| Action | Method | URL | Purpose |
|--------|--------|-----|---------|
| List | GET | `/student?action=list` | Get all students |
| New Form | GET | `/student?action=new` | Show add form |
| Insert | POST | `/student?action=insert` | Add new student |
| Edit Form | GET | `/student?action=edit&id=1` | Show edit form |
| Update | POST | `/student?action=update` | Update student |
| Delete | GET | `/student?action=delete&id=1` | Delete student |

## Error Handling

### Exception Handling

- SQLException handling in DAO layer
- Servlet exception handling in Controller
- Error page (`error.jsp`) for user-friendly error display

### Error Pages

- **404 Not Found**: Redirects to error.jsp
- **500 Server Error**: Redirects to error.jsp
- **Form Validation Errors**: Inline error messages

## Security Features

1. **SQL Injection Prevention**
   - Uses PreparedStatements
   - Input parameterization

2. **Form Validation**
   - Client-side validation
   - Server-side validation

3. **Session Management**
   - Cookie tracking
   - HTTP-only cookies
   - Secure session configuration

4. **Input Sanitization**
   - Special character handling
   - HTML escaping in JSP (JSTL tags)

## Performance Optimization

1. **Database Indexes**
   - Indexes on frequently searched fields
   - Composite indexes for complex queries

2. **Caching**
   - Student list caching capability
   - Connection pooling ready

3. **Form Optimization**
   - JavaScript validation reduces server load
   - Real-time validation feedback

## Testing

### Running Tests

```bash
mvn test
```

### Manual Testing Checklist

- [ ] User can add new student
- [ ] User can view all students
- [ ] User can edit student information
- [ ] User can delete student
- [ ] Form validation works correctly
- [ ] Database operations are successful
- [ ] Error messages display properly
- [ ] Responsive design works on mobile

## Troubleshooting

### Common Issues

1. **Database Connection Error**
   - Verify MySQL is running
   - Check credentials in web.xml
   - Ensure database and tables are created

2. **Port Already in Use**
   ```bash
   # Change Tomcat port in $TOMCAT_HOME/conf/server.xml
   <Connector port="8080" protocol="HTTP/1.1" />
   ```

3. **MySQL Driver Not Found**
   - Ensure mysql-connector-java dependency is in pom.xml
   - Run `mvn clean install` to download dependencies

4. **JSP Not Found Error**
   - Verify JSP files are in correct directory
   - Check file paths in servlet

5. **Form Validation Not Working**
   - Check browser console for JavaScript errors
   - Verify validation.js is loaded

## Deployment on Production

### Pre-deployment Checklist

- [ ] Update database credentials for production
- [ ] Enable HTTPS/SSL in Tomcat
- [ ] Set appropriate CORS headers if needed
- [ ] Configure connection pooling
- [ ] Enable logging for monitoring
- [ ] Set up backup mechanisms
- [ ] Configure firewall rules

### Production Configuration

1. Update web.xml with production database URL
2. Enable SSL in Tomcat server.xml
3. Configure logging levels
4. Set up database replication
5. Implement monitoring and alerts

## Future Enhancements

1. **Advanced Features**
   - Student login system
   - Email notifications
   - Bulk import/export functionality
   - Advanced search and filtering
   - Student attendance tracking
   - Grade management

2. **Technical Improvements**
   - Migrate to Spring Framework
   - Implement JPA/Hibernate ORM
   - Add REST API
   - Docker containerization
   - Unit and integration tests
   - CI/CD pipeline

3. **Security Enhancements**
   - Two-factor authentication
   - Role-based access control
   - Audit logging
   - Data encryption

## Dependencies

See `pom.xml` for complete list:

- servlet-api 4.0.1
- jsp-api 2.3.3
- jstl 1.2
- mysql-connector-java 8.0.33
- junit 4.13.2
- log4j 1.2.17

## Configuration Files

### web.xml
- Servlet mappings
- Context parameters (database credentials)
- Welcome files
- Error pages
- Session configuration
- MIME types

### pom.xml
- Maven project configuration
- Dependencies
- Build plugins
- Tomcat deployment configuration

## Logging

Logs are configured in log4j:
- Logs are printed to console by default
- Can be configured to write to files
- Supported log levels: DEBUG, INFO, WARN, ERROR, FATAL

## License

This project is provided as-is for educational purposes.

## Support & Contact

For issues, bug reports, or feature requests, please document them clearly with:
- Steps to reproduce
- Error messages/logs
- Expected vs. actual behavior
- Environment details

## Version History

### Version 1.0.0 (Initial Release)
- Student registration
- View all students
- Update student information
- Delete records
- Form validation
- Responsive design
- MVC architecture

---

**Happy Learning! Build with Enterprise Java Technologies** 🚀
