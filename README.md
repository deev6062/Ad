

<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="Registration.aspx.cs" Inherits="Crud.Registration" %>

<!DOCTYPE html>

<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
    <title></title>
</head>
<body>
    <form id="form1" runat="server">
        <div>
            <h2>User Registration Form</h2>
            <asp:Label ID="lblusername" runat="server" text="Enter Username"></asp:Label>
            <asp:textbox ID="txtusername" runat="server"></asp:textbox><br />
            <asp:Label id="lblemail" runat="server" text="Enter Emailid"></asp:Label>
            <asp:textbox ID="txtemail" runat="server"></asp:textbox><br />
            <asp:Label id="lblpassword" runat="server" text="Enter Password"></asp:Label>
            <asp:textbox ID="txtpassword" TextMode="Password" runat="server"></asp:textbox><br />
            <asp:button ID="btnregister" Text="Register" OnClick="btnRegister" runat="server" />
        </div>

    </form>
</body>
</html>



=============================================================================================================

using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

namespace Crud
{
    public partial class Viewusers : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
		
		string connection = "";
		string username = "txtusername.Text";
		string email = "email.Text";
		string password = "password.Text";

		using(Sqlconnection con = new Sqlconnection(connection))
		{

			string query = "insert into Reguser(username,email,password) values (@username,@eamil,				@password)";
			using(SqlCommnd cmd = new SqlCommand(query,con))
			{
				cmd.Parameters.AddWithValues("username",username);
				cmd.Parameters.AddWithValues("email",email);
				cmd.Parameters.AddWithValues("password",password);
				con.Open();
				cmd.ExecuteNonQuery();
			}	
		}
        }
    }
}


===============================================================================================


<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="Crud.Default" %>

<!DOCTYPE html>

<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
    <title>CRUD Operations</title>
</head>
<body>
    <form id="form1" runat="server">
        <div>
            <h2>User Details</h2>
            <div>
                <asp:TextBox ID="txtName" runat="server" placeholder="Name"></asp:TextBox>
                <asp:TextBox ID="txtAge" runat="server" placeholder="Age"></asp:TextBox>
                <asp:DropDownList ID="ddlGender" runat="server">
                    <asp:ListItem Text="Male" Value="Male"></asp:ListItem>
                    <asp:ListItem Text="Female" Value="Female"></asp:ListItem>
                </asp:DropDownList>
                <asp:Button ID="btnAdd" runat="server" Text="Add" OnClick="btnAdd_Click" />
            </div>
            <br />
            <asp:GridView ID="GridView1" runat="server" AutoGenerateColumns="False" OnRowDeleting="GridView1_RowDeleting" DataKeyNames="Id">
                <Columns>
                    <asp:BoundField DataField="Id" HeaderText="ID" ReadOnly="True" />
                    <asp:BoundField DataField="Name" HeaderText="Name" />
                    <asp:BoundField DataField="Age" HeaderText="Age" />
                    <asp:BoundField DataField="Gender" HeaderText="Gender" />
                    <asp:CommandField ShowDeleteButton="True" />
                </Columns>
            </asp:GridView>
        </div>
    </form>
</body>
</html>


=======================================================================================


using System;
using System.Configuration;
using System.Data.SqlClient;
using System.Web.UI.WebControls;

namespace Crud
{
    public partial class Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
            if (!IsPostBack)
            {
                BindGridView();
            }
        }

        protected void BindGridView()
        {
            string connectionString = "Data Source=DEVXX\\SQLEXPRESS;Initial Catalog=test;Integrated Security=True;";
            string query = "SELECT Id, Name, Age, Gender FROM Users";

            using (SqlConnection connection = new SqlConnection(connectionString))
            {
                SqlCommand command = new SqlCommand(query, connection);
                connection.Open();
                SqlDataReader reader = command.ExecuteReader();
                GridView1.DataSource = reader;
                GridView1.DataBind();
            }
        }

        protected void btnAdd_Click(object sender, EventArgs e)
        {
            string name = txtName.Text;
            int age = Convert.ToInt32(txtAge.Text);
            string gender = ddlGender.SelectedValue;

            string connectionString = "Data Source=DEVXX\\SQLEXPRESS;Initial Catalog=test;Integrated Security=True;";
            string query = "INSERT INTO Users (Name, Age, Gender) VALUES (@Name, @Age, @Gender)";

            using (SqlConnection connection = new SqlConnection(connectionString))
            {
                SqlCommand command = new SqlCommand(query, connection);
                command.Parameters.AddWithValue("@Name", name);
                command.Parameters.AddWithValue("@Age", age);
                command.Parameters.AddWithValue("@Gender", gender);
                connection.Open();
                command.ExecuteNonQuery();
            }

            BindGridView();
        }

        protected void GridView1_RowDeleting(object sender, GridViewDeleteEventArgs e)
        {
            int userId = Convert.ToInt32(GridView1.DataKeys[e.RowIndex].Value);

            string connectionString = "Data Source=DEVXX\\SQLEXPRESS;Initial Catalog=test;Integrated Security=True;";
            string query = "DELETE FROM Users WHERE Id = @Id";

            using (SqlConnection connection = new SqlConnection(connectionString))
            {
                SqlCommand command = new SqlCommand(query, connection);
                command.Parameters.AddWithValue("@Id", userId);
                connection.Open();
                command.ExecuteNonQuery();
            }

            BindGridView();
        }
    }
}


