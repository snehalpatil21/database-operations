# database-operations
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;
using System.Data.SqlClient;

namespace databsedemo
{
    public partial class Form1 : Form
    {
        SqlConnection con = new SqlConnection();
        
        public Form1()
        {
            
            con.ConnectionString = "Data Source=.;Initial Catalog=HR;Integrated Security=True;Pooling=False";
            InitializeComponent();
            display();
        }

        private void btnsave_Click(object sender, EventArgs e)
        {
            con.Open();
            SqlCommand cmd = new SqlCommand("insert into emp values(@empid,@ename,@desig,@sal,@doj)", con);
            cmd.Parameters.AddWithValue("@empid", Convert.ToInt32(txteid.Text));
            cmd.Parameters.AddWithValue("@ename", txtename.Text);
            cmd.Parameters.AddWithValue("@desig", txtdesig.Text);
            cmd.Parameters.AddWithValue("@sal", Convert.ToInt32(txtsal.Text));
            cmd.Parameters.AddWithValue("@doj", txtdoj.Text);
            cmd.ExecuteNonQuery();
            MessageBox.Show("Record inserted");
            txteid.Text = "";
            txtename.Text = "";
            txtdesig.Text = "";
            txtsal.Text = "";
            txtdoj.Text = "";
            con.Close();
            display();
        }
        private void display()
        {
            DataTable dt = new DataTable();
            SqlDataAdapter da = new SqlDataAdapter("select * from emp", con);
            da.Fill(dt);
            dataGridView1.DataSource = dt;

        }

        private void dataGridView1_CellContentClick(object sender, DataGridViewCellEventArgs e)
        {

        }

        private void dataGridView1_RowHeaderMouseClick(object sender, DataGridViewCellMouseEventArgs e)
        {
            txteid.Text = dataGridView1.Rows[e.RowIndex].Cells[0].Value.ToString();
         txtename.Text = dataGridView1.Rows[e.RowIndex].Cells[1].Value.ToString();
    txtdesig.Text = dataGridView1.Rows[e.RowIndex].Cells[2].Value.ToString();
            txtsal.Text = dataGridView1.Rows[e.RowIndex].Cells[3].Value.ToString();
            txtdoj.Text = dataGridView1.Rows[e.RowIndex].Cells[4].Value.ToString();

        }

        private void btnupdate_Click(object sender, EventArgs e)
        {
            con.Open();
            SqlCommand cmd = new SqlCommand("update emp set ename=@ename,designation=@desig,salary=@sal,doj=@doj where empid=@eid", con);

            cmd.Parameters.AddWithValue("@eid", Convert.ToInt32(txteid.Text));
            cmd.Parameters.AddWithValue("@ename", txtename.Text);
            cmd.Parameters.AddWithValue("@desig", txtdesig.Text);
            cmd.Parameters.AddWithValue("@sal", Convert.ToInt32(txtsal.Text));
            cmd.Parameters.AddWithValue("@doj", txtdoj.Text);
            cmd.ExecuteNonQuery();
            MessageBox.Show("Record updated");
            txteid.Text = "";
            txtename.Text = "";
            txtdesig.Text = "";
            txtsal.Text = "";
            txtdoj.Text = "";

            con.Close();
            display();
        }

        private void btndelete_Click(object sender, EventArgs e)
        {
            con.Open();
            SqlCommand cmd = new SqlCommand("delete from emp where empid=@eid", con);

            cmd.Parameters.AddWithValue("@eid", Convert.ToInt32(txteid.Text));
          
            cmd.ExecuteNonQuery();
            MessageBox.Show("Record deleted");
            txteid.Text = "";
            txtename.Text = "";
            txtdesig.Text = "";
            txtsal.Text = "";
            txtdoj.Text = "";

            con.Close();
            display();

        }
    }
}
