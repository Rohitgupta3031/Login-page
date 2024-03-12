

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/Myservlet")
public class Myservlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public Myservlet() {
        super();
        
    }
    
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

	
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		PrintWriter out=response.getWriter();
		
		String username = request.getParameter("user");
		String password = request.getParameter("pass");
		
		request.setAttribute("name1",username);
		request.setAttribute("passvalue", password);
		
		boolean isValid = false;
		
		try{
			
		Class.forName("com.mysql.cj.jdbc.Driver");
		
		Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3307/rohit","root","root");
		System.out.println("Connected");
		
		String str = "select * from demo where email=? and password=?";
		PreparedStatement ps = con.prepareStatement(str);
		ps.setString(1, username);
		ps.setString(2, password);
		
		ResultSet rs = ps.executeQuery();
		isValid = rs.next();
		
		if(isValid==true) {
			out.print("Successfully login..");
			RequestDispatcher rd = request.getRequestDispatcher("success.jsp");
			rd.forward(request, response);
			
		}
		
		else {
			response.sendRedirect("error.jsp");
		}
		}
		catch (Exception e) {
			
			e.printStackTrace();
		}
	}

}
