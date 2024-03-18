<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.Connection, java.sql.DriverManager, java.sql.PreparedStatement, java.sql.SQLException" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Registration</title>
</head>
<body>
<h2>Registration Form</h2>
    <form method="post">
Name:<input type="text" name="username" required/><br/>
Password:<input type="password" name="password" required/><br/>
Email Id:<input type="email" name="email" required/><br/>
Country: <select name="country" required>
<option value="india">India</option>
<option value="usa">USA</option>
<option value="uk">UK</option>
</select> <br/>
Address: <textarea name="address" required></textarea><br/>
Phone Number: <input type="tel" name="phone" pattern="[0-9]{10}" required/><br/>
Date of Birth: <input type="date" name="dob" required/><br/>
Gender: 
<input type="radio" name="gender" value="male" required/> Male
<input type="radio" name="gender" value="female" required/> Female<br/>
Interests: 
<input type="checkbox" name="interests" value="sports" required/> Sports
<input type="checkbox" name="interests" value="music" required/> Music
<input type="checkbox" name="interests" value="movies" required/> Movies
<br/>
<input type="submit" value="Submit"/>
</form>

<%
try{
    if(request.getMethod().equals("POST")){
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        String email = request.getParameter("email");
        String country = request.getParameter("country");
        String address = request.getParameter("address");
        String phone = request.getParameter("phone");
        String dob = request.getParameter("dob");
        String gender = request.getParameter("gender");
        String[] interests = request.getParameterValues("interests");
       
        Class.forName("com.mysql.jdbc.Driver");
        Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","root");
        String sql = "insert into employee(Name,Password,Email,Country,Address,PhoneNumber,DOB,Gender,Interests) values(?,?,?,?,?,?,?,?,?)";
        PreparedStatement ps = con.prepareStatement(sql);
        ps.setString(1,username);
        ps.setString(2, password);
        ps.setString(3, email);
        ps.setString(4, country);
        ps.setString(5, address);
        ps.setString(6, phone);
        ps.setString(7, dob);
        ps.setString(8, gender);
        // Join interests array into a single string separated by comma
        String interestsStr = String.join(",", interests);
        ps.setString(9, interestsStr);
       
        int i = ps.executeUpdate();
        if(i>0)
        {
            out.print("You are successfully registered");
        }
        else
        {
            out.print("You are not registered");
        }
        con.close(); // Close the connection after usage
    }
}catch(ClassNotFoundException | SQLException ex)
{
    ex.printStackTrace();
    out.print(ex.getMessage());
}
%>

</body>
</html>

==============================================================================

viewData


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.Connection, java.sql.DriverManager, java.sql.PreparedStatement, java.sql.ResultSet, java.sql.SQLException" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>View Data</title>
</head>
<body>
<h2>View Data</h2>

<table border="1">
    <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Email</th>
        <th>Country</th>
        <th>Address</th>
        <th>Phone Number</th>
        <th>Date of Birth</th>
        <th>Gender</th>
        <th>Interests</th>
        <th>Actions</th>
    </tr>

<%
try {
    Class.forName("com.mysql.jdbc.Driver");
    Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","root");
    String sql = "SELECT * FROM employee";
    PreparedStatement ps = con.prepareStatement(sql);
    ResultSet rs = ps.executeQuery();

    while(rs.next()) {
%>
    <tr>
        <td><%= rs.getInt("id") %></td>
        <td><%= rs.getString("Name") %></td>
        <td><%= rs.getString("Email") %></td>
        <td><%= rs.getString("Country") %></td>
        <td><%= rs.getString("Address") %></td>
        <td><%= rs.getString("PhoneNumber") %></td>
        <td><%= rs.getString("DOB") %></td>
        <td><%= rs.getString("Gender") %></td>
        <td><%= rs.getString("Interests") %></td>
        <td>
            <form action="updateUser.jsp" method="post">
                <input type="hidden" name="userID" value="<%= rs.getInt("id") %>">
                <input type="submit" value="Update">
            </form>
            <form action="deleteUser.jsp" method="post">
                <input type="hidden" name="userID" value="<%= rs.getInt("id") %>">
                <input type="submit" value="Delete">
            </form>
        </td>
    </tr>
<%
    }
    con.close();
} catch (ClassNotFoundException | SQLException ex) {
    ex.printStackTrace();
    out.print(ex.getMessage());
}
%>

</table>

</body>
</html>

========================================================================

updateDetails


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.Connection, java.sql.DriverManager, java.sql.PreparedStatement, java.sql.SQLException" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Update User</title>
</head>
<body>
<h2>Update User</h2>

<form method="post">
    Enter User ID to Update: <input type="text" name="userID" required/><br/>
    New Name: <input type="text" name="newName"/><br/>
    New Password: <input type="password" name="newPassword"/><br/>
    New Email: <input type="text" name="newEmail"/><br/>
    New Country: <input type="text" name="newCountry"/><br/>
    New Address: <input type="text" name="newAddress"/><br/>
    New Phone Number: <input type="tel" name="newPhone" pattern="[0-9]{10}"/><br/>
    New Date of Birth: <input type="date" name="newDob"/><br/>
    New Gender: <input type="radio" name="newGender" value="male"/> Male
                <input type="radio" name="newGender" value="female"/> Female<br/>
    New Interests: <input type="text" name="newInterests"/><br/>
    <input type="submit" value="Update"/>
</form>

<%
try {
    if(request.getMethod().equals("POST")){
        String userID = request.getParameter("userID");
        String newName = request.getParameter("newName");
        String newPassword = request.getParameter("newPassword");
        String newEmail = request.getParameter("newEmail");
        String newCountry = request.getParameter("newCountry");
        String newAddress = request.getParameter("newAddress");
        String newPhone = request.getParameter("newPhone");
        String newDob = request.getParameter("newDob");
        String newGender = request.getParameter("newGender");
        String newInterests = request.getParameter("newInterests");
        
        Class.forName("com.mysql.jdbc.Driver");
        Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","root");
        String updateSql = "UPDATE employee SET Name=?, Password=?, Email=?, Country=?, Address=?, PhoneNumber=?, DOB=?, Gender=?, Interests=? WHERE id=?";
        PreparedStatement updatePs = con.prepareStatement(updateSql);
        updatePs.setString(1, newName);
        updatePs.setString(2, newPassword);
        updatePs.setString(3, newEmail);
        updatePs.setString(4, newCountry);
        updatePs.setString(5, newAddress);
        updatePs.setString(6, newPhone);
        updatePs.setString(7, newDob);
        updatePs.setString(8, newGender);
        updatePs.setString(9, newInterests);
        updatePs.setString(10, userID);
        int rowsAffected = updatePs.executeUpdate();
        
        if (rowsAffected > 0) {
            out.println("User with ID " + userID + " updated successfully.");
        } else {
            out.println("No user found with ID " + userID);
        }
        
        con.close();
    }
} catch (ClassNotFoundException | SQLException ex) {
    ex.printStackTrace();
    out.print(ex.getMessage());
}
%>

</body>
</html>


===========================================================

deleteUsers


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.Connection, java.sql.DriverManager, java.sql.PreparedStatement, java.sql.SQLException" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Delete User</title>
</head>
<body>
<h2>Delete User</h2>

<form method="post">
    Enter User ID to Delete: <input type="text" name="userID" required/><br/>
    <input type="submit" value="Delete"/>
</form>

<%
try {
    if(request.getMethod().equals("POST")){
        String userID = request.getParameter("userID");
        Class.forName("com.mysql.jdbc.Driver");
        Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","root");
        String sql = "DELETE FROM employee WHERE id=?";
        PreparedStatement ps = con.prepareStatement(sql);
        ps.setString(1, userID);
        int rowsAffected = ps.executeUpdate();
        
        if (rowsAffected > 0) {
            out.println("User with ID " + userID + " deleted successfully.");
        } else {
            out.println("No user found with ID " + userID);
        }
        
        con.close();
    }
} catch (ClassNotFoundException | SQLException ex) {
    ex.printStackTrace();
    out.print(ex.getMessage());
}
%>

</body>
</html>
