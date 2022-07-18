#include <stdlib.h>
#include <iostream>
#include <fstream>
#include <cstring>
#include <string>
#include <sstream>
#include <iomanip>
#include <time.h>
#include <cstdlib>
#include <vector>

using namespace std;


#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

//for demonstration only. never save your password in the code!
const string server = "tcp://localhost:3306";
const string username = "root";
const string password = "jaisriram";
fstream f2, f3, f4;
const auto SCREEN_WIDTH = 118;

class member
{
private:
	string name;
	string age;
	string phno;
	string total;
	string memid;
	int mem_id;
	string date;
	string date_time;
	string facilities1 = "f1", facilities2 = "f2", facilities3 = "f3", facilities4 = "f4";

public:
	void createmember()
	{

		cout << "enter your fullname:";
		cin >> name;
		cout << endl
			<< "enter your phno:";
		cin >> phno;
		while (phno.size() != 10)
		{
			cout << endl
				<< "enter correct phone number:";
			cin >> phno;
		}

		cout << endl
			<< "enter your age:";
		cin >> age;
		srand(time(0));

		mem_id = rand();

		cout << endl
			<< "your member id is:" << mem_id << endl;

		total = memid + "," + name + "," + age + "," + phno + "," + facilities1 + "," + facilities2 + "," + facilities3 + "," + facilities4 + "," + date_time;
	}
	string get_totstr()
	{
		return total;
	}
	string get_date()
	{
		return date;
	}
	string getphno()
	{
		return phno;
	}
	string getname()
	{
		return name;
	}
	string getage()
	{
		return age;
	}
	string getfac1()
	{
		return facilities1;
	}
	string getfac2()
	{
		return facilities2;
	}
	string getfac3()
	{
		return facilities3;
	}
	string getfac4()
	{
		return facilities4;
	}
	int get_memid()
	{
		return mem_id;
	}
};

member m1;

void add_member(int mem_id, string name, string ph_no, string age)
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::Statement* stmt;
	sql::PreparedStatement* pstmt;


	try
	{
		
		driver = get_driver_instance();
		con = driver->connect(server, username, password);
		con->setSchema("new_data");
		stmt = con->createStatement();

	
		pstmt = con->prepareStatement("INSERT INTO logindata(mem_id, name, ph_no, age) VALUES(? , ? , ? , ?  )");
		pstmt->setInt(1, mem_id);
		pstmt->setString(2, name);
		pstmt->setString(3, ph_no);
		pstmt->setString(4, age);
		pstmt->execute();

		delete pstmt;
		delete con;
	

	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}
	
}
void savemember()
{


	m1.createmember();
	add_member(m1.get_memid(), m1.getname(), m1.getphno(), m1.getage());





}
int is_a_member(int a)
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::PreparedStatement* pstmt;
	sql::ResultSet* result;
	try
	{
		driver = get_driver_instance();
		con = driver->connect(server, username, password);
		con->setSchema("new_data");

		//select  
		pstmt = con->prepareStatement("SELECT * FROM logindata;");
		result = pstmt->executeQuery();
		int found = 0;
		while (result->next())
		{
			if (a == result->getInt(1))
			{
				found = 1;
		   }
		}
			

		delete result;
		delete pstmt;
		delete con;
		return found;
	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}

}
int is_booked(int a)
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::PreparedStatement* pstmt;
	sql::ResultSet* result;
	try
	{
		driver = get_driver_instance();
		con = driver->connect(server, username, password);
		con->setSchema("new_data");

		pstmt = con->prepareStatement("SELECT * FROM new_data;");
		result = pstmt->executeQuery();
		int found = 0;
		while (result->next())
		{
			if (a == result->getInt(1))
			{
				found = 1;
			}
		}


		delete result;
		delete pstmt;
		delete con;
		return found;
	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}

}
int count_facility()
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::PreparedStatement* pstmt;
	sql::ResultSet* result;

	try
	{
		driver = get_driver_instance();
		//for demonstration only. never save password in the code!
		con = driver->connect(server, username, password);
		con->setSchema("new_data");

		//select  
		pstmt = con->prepareStatement("SELECT * FROM facility;");
		result = pstmt->executeQuery();
		int i = 1;
		while (result->next())
		{
			
			i++;
		}
		delete result;
		delete pstmt;
		delete con;
		return i;
	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}
}
void show_facility()
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::PreparedStatement* pstmt;
	sql::ResultSet* result;

	try
	{
		driver = get_driver_instance();
		//for demonstration only. never save password in the code!
		con = driver->connect(server, username, password);
		con->setSchema("new_data");

		//select  
		pstmt = con->prepareStatement("SELECT * FROM facility;");
		result = pstmt->executeQuery();
		int i = 1;
		while (result->next())
		{
			cout<<i<<"."<<result->getString(1) << endl;
			i++;
		}
		cout << setfill('*') << setw(SCREEN_WIDTH + 1) << " " << setfill(' ')
			<< endl;
		delete result;
		delete pstmt;
		delete con;
	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}
}
int is_prev(string date,int number)
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::PreparedStatement* pstmt;
	sql::ResultSet* result;

	try
	{
		driver = get_driver_instance();
		con = driver->connect(server, username, password);
		con->setSchema("new_data"); 
		string query = "SELECT * FROM prev_his ;";
		pstmt = con->prepareStatement(query);
		result = pstmt->executeQuery();
		int found = 0;
		while (result->next())
		{
			if(number==result->getInt(1) && date==result->getString(2))
			{
				found = 1;
			}
		}
		delete result;
		delete pstmt;
		delete con;
		return found;
	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}
}

void update_prev(int memberid,string date,string facility)
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::PreparedStatement* pstmt;
	try
	{
		driver = get_driver_instance();
		//for demonstration only. never save password in the code!
		con = driver->connect(server, username, password);
	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}

	con->setSchema("new_data");
	string number = to_string(memberid);
	if (is_prev(date,memberid ) == 1)
	{
		string query1 = "UPDATE prev_his SET " + facility + " = ? WHERE mem_id = " + number + ";";
		pstmt = con->prepareStatement(query1);
		pstmt->setString(1, facility);
		pstmt->executeQuery();
		delete pstmt;
  }
	else
	{

		string query3 = "INSERT INTO prev_his(mem_id,date," + facility + ") VALUES(? ,?,?)";
		pstmt = con->prepareStatement(query3);
		pstmt->setInt(1, memberid);
		pstmt->setString(2, date);
		pstmt->setString(3, facility);
		pstmt->executeQuery();
		delete pstmt;
	}
	delete con;
}
	
void booking_facility()
{
	int number;
	cout << "enter your member id:";
	cin >> number;
	string str = to_string(number);
	sql::Driver* driver;
	sql::Connection* con;
	sql::PreparedStatement* pstmt;

	try
	{
		driver = get_driver_instance();
		//for demonstration only. never save password in the code!
		con = driver->connect(server, username, password);
		con->setSchema("new_data");

		if (is_a_member(number) == 1)
		{
			string facility;
			show_facility();
			cout << "enter the facility name:";
			cin >> facility;
			cout << endl;
			string date;
			cout << "enter date(xx:xx:xxxx):";
			cin >> date;

			if (is_booked(number) == 1)
			{
				string time;
				cout << "enter time slot(xx:xx):";
				cin >> time;

				string query1 = "UPDATE new_data SET " + facility + " = ? WHERE mem_id = " + str + ";";
				pstmt = con->prepareStatement(query1);
				pstmt->setString(1, facility);
				pstmt->executeQuery();
				delete pstmt;
				string query2 = "UPDATE new_data SET " + facility + "time= ? WHERE mem_id = " + str + ";";
				pstmt = con->prepareStatement(query2);
				pstmt->setString(1, time);
				pstmt->executeQuery();
				delete pstmt;
			}
			else
			{
				
				string time;
				cout << "enter time slot(xx:xx):";
				cin >> time;
				string query3 = "INSERT INTO new_data(mem_id,date," + facility + "," + facility + "time) VALUES(? ,?, ? ,?);";
				pstmt = con->prepareStatement(query3);
				pstmt->setInt(1, number);
				pstmt->setString(2, date);
				pstmt->setString(3, facility);
				pstmt->setString(4, time);
				pstmt->executeQuery();
				delete pstmt;

			}
			update_prev(number, date, facility);
		}
		else
		{
			cout << "not registered in this club" << endl;
		}
		
	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}
	delete con;
	


}


void display_mem_det()
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::PreparedStatement* pstmt;
	sql::ResultSet* result;

	try
	{
		driver = get_driver_instance();
		//for demonstration only. never save password in the code!
		con = driver->connect(server, username, password);
		con->setSchema("new_data");

		//select  
		pstmt = con->prepareStatement("SELECT * FROM logindata;");
		result = pstmt->executeQuery();

		while (result->next())
		{
			cout << result->getInt(1) << "\t" << result->getString(2).c_str() << "\t" << result->getString(3).c_str() << "\t"
				<< result->getString(4).c_str() << endl;
		}
		cout << setfill('*') << setw(SCREEN_WIDTH + 1) << " " << setfill(' ')
			<< endl;
		delete result;
		delete pstmt;
		delete con;
	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}
}
void search_particulars(int a)
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::PreparedStatement* pstmt;
	sql::ResultSet* result;

	try
	{
		driver = get_driver_instance();
		//for demonstration only. never save password in the code!
		con = driver->connect(server, username, password);
		con->setSchema("new_data");

		
	;
		pstmt = con->prepareStatement("SELECT * FROM new_data  WHERE mem_id = ? ;");
		pstmt->setInt(1, a);
		result = pstmt->executeQuery();
		int x = count_facility()-1;
		x = 2 + (2 * x)+1;
		
		while (result->next())
		{
			int i = 1;
			for (i; i < x; i++)
			{
				
					if (i == 1)
					{
						cout << result->getInt(i) << "\t";
					}
					else {
						cout << result->getString(i).c_str() << "\t";
					}
				
			}
		
		}
		cout << endl;
		cout << setfill('*') << setw(SCREEN_WIDTH + 1) << " " << setfill(' ')
			<< endl;
		delete result;
		delete pstmt;
		delete con;
	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}
}
int count_fac(string facility, string number)
{
	int count = 0;
	sql::Driver* driver;
	sql::Connection* con;
	sql::PreparedStatement* pstmt;
	sql::ResultSet* result;
	try
	{
		driver = get_driver_instance();
		con = driver->connect(server, username, password);
		con->setSchema("new_data");
		string query = "SELECT * FROM prev_his WHERE " + facility + "=" + facility + " AND mem_id= " + number + ";";
		pstmt = con->prepareStatement(query);
		result = pstmt->executeQuery();
		while (result->next())
		{
			count++;
			
		}
		
		delete result;
		delete pstmt;
	}
		catch (sql::SQLException e)
		{
			cout << "Could not connect to server. Error message: " << e.what() << endl;
			system("pause");
			exit(1);
		}



		delete con;
		return count;
}
void full_summary(int a)
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::PreparedStatement* pstmt;
	sql::ResultSet* result;
	string str = to_string(a);
	string query;
	vector<string>facility;

	try
	{
		driver = get_driver_instance();
		con = driver->connect(server, username, password);
		con->setSchema("new_data");
		pstmt = con->prepareStatement("SELECT * FROM facility;");
		result = pstmt->executeQuery();
		int i = 1;
		while (result->next())
		{
			
			string temp = result->getString(i);
			cout<<temp<<":" << count_fac(temp, str)<<endl;
		}
		cout << setfill('*') << setw(SCREEN_WIDTH + 1) << " " << setfill(' ')
			<< endl;
		delete result;
		delete pstmt;
		delete con;
	}
	
		
		
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}

	

	


}
void see_his()
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::Statement* stmt;
	sql::PreparedStatement* pstmt;
	sql::ResultSet* result;

	try
	{
		driver = get_driver_instance();
		con = driver->connect(server, username, password);
	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}
	
	con->setSchema("new_data");
	pstmt = con->prepareStatement("SELECT * FROM prev_his ;");
	result = pstmt->executeQuery();
	int x = count_facility();
	x = 2 + x ;
	
	while (result->next())
	{
		int i = 1;
		for(i;i<x;i++)
		{
			if (i == 1)
			{
				cout << result->getInt(i) << "\t";
			}
			else {
				cout << result->getString(i) << "\t";
			}

		}
		cout << endl;
		
	}
	delete pstmt;
	delete con;
}
void see_all_prev()
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::Statement* stmt;
	sql::PreparedStatement* pstmt;
	sql::ResultSet* result;

	try
	{
		driver = get_driver_instance();
		con = driver->connect(server, username, password);
	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}
	string str;
	cout << "enter your member id:";
	cin >> str;
	con->setSchema("new_data");
	pstmt = con->prepareStatement("SELECT * FROM prev_his WHERE  mem_id=" + str + ";");
	result = pstmt->executeQuery();
	int x = count_facility();
	x = 2 + x ;
	
	while (result->next())
	{
		int i = 1;
		for(i;i<x;i++)
		{ 
			if (i == 1)
			{
				cout << result->getInt(i) << "\t";
			}
			else 
			{
				cout << result->getString(i) << "\t";
			}

		}
		
		
	}
	cout << endl;
	delete pstmt;
	delete con;
}
void summary()
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::Statement* stmt;
	sql::PreparedStatement* pstmt;
	sql::ResultSet* result;

	try
	{
		driver = get_driver_instance();
		con = driver->connect(server, username, password);
	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}

	con->setSchema("new_data");
	pstmt = con->prepareStatement("SELECT mem_id FROM new_data;");
	result = pstmt->executeQuery();

	while (result->next())
	{
		int a = 0;
		a = result->getInt(1);
		cout << setfill('*') << setw(SCREEN_WIDTH + 1) << " " << setfill(' ')
			<< endl;
		cout << "member id:" << a << endl;
		full_summary(a);
	}

	delete result;
	delete pstmt;
	delete con;
}
void add_facility()
{
	sql::Driver* driver;
	sql::Connection* con;
	sql::Statement* stmt;
	sql::PreparedStatement* pstmt;


	try
	{

		driver = get_driver_instance();
		con = driver->connect(server, username, password);
		con->setSchema("new_data");
		

		string fac1;
		int ch=0;
		do {
			cout << "enter facility name:";
			cin >> fac1;
			pstmt = con->prepareStatement("INSERT INTO facility( fac1) VALUES(?  )");


			pstmt->setString(1, fac1);

			pstmt->execute();

			delete pstmt;
			cout << "enter 1 to continue to add facility:";
			cin >> ch;
			cout << endl;
		} while (ch == 1);
	
		string query1 = "ALTER TABLE prev_his ADD " + fac1 + " varchar(50)";
		pstmt = con->prepareStatement(query1);
		pstmt->executeQuery();
		delete pstmt;
		string query2 = "ALTER TABLE new_data ADD " + fac1 + " varchar(50)";
		pstmt = con->prepareStatement(query2);
		pstmt->executeQuery();
		delete pstmt;
		string query3 = "ALTER TABLE new_data ADD " + fac1 + "time " + " varchar(50)";
		pstmt = con->prepareStatement(query3);
		pstmt->executeQuery();
		delete pstmt;




	}
	catch (sql::SQLException e)
	{
		cout << "Could not connect to server. Error message: " << e.what() << endl;
		system("pause");
		exit(1);
	}
	delete con;
}
	void managementactivities()

	{
		int i=0;
		cout << "1.display all member details" << endl
			<< "2.search for particular record" << endl
			<< "3.add facility" << endl
			<< "4.display all bookings" << endl << "5.summary of a member" << endl
			<< "6.go back to main menu" << endl;
		cin >> i;
		cout << endl;
		switch (i)
		{
		case 1:
		{
			display_mem_det();
			break;
		}
		case 2:
		{

			cout << endl;
			int a;
			cout << "enter member id:";
			cin >> a;
			search_particulars(a);
			break;
		}

		case 3:
		{
			show_facility();
			cout << endl;
			add_facility();
			break;
		}
		case 4:
		{
			see_his();
			cout << endl;
			cout << setfill('*') << setw(SCREEN_WIDTH + 1) << " " << setfill(' ')
				<< endl;

			break;
		}
		case 5:
		{

			summary();
			break;
		}
		default:
		{
			cout << "invalid option" << endl;
		}
		}
		
		
	}


	void users()
	{
	

		int i;
		cout << setfill('*') << setw(SCREEN_WIDTH + 1) << " " << setfill(' ')
			<< endl;
		cout << "select option" << endl
			<< "1.book facility" << endl
			<< "2.new registration" << endl
			<< "3.display my booking details" << endl
			<< "4.see all previous booking" << endl
			<< "5.exit" << endl;
		cin >> i;
		cout << endl;
		cout << setfill('*') << setw(SCREEN_WIDTH + 1) << " " << setfill(' ')
			<< endl;
		switch (i)
		{



		case 1:
		{
			booking_facility();
			break;
		}
		case 2:
		{

			savemember();
			break;
		}
		case 3:
		{
			int a;
			cout << "enter your member id:";
			cin >> a;
			search_particulars(a);

			break;
		}
		case 4:
		{
			see_all_prev();
			break;
		}
		case 5:
		{
			exit(0);
			break;
		}
		default:
		{
			cout << "enter a valid option" << endl;
		}
		}
	}


	int main()
	{
		int ss = 0;
	
		
		
		

		do
		{
			cout << "1.truncate member detail" << endl << "2.truncate previous details" << endl << "3.truncate login details"<<endl<<"anyother number to exit"<<endl;
			cin >> ss;
			if (ss == 1)
			{
				sql::Driver* driver;
				sql::Connection* con;
				sql::Statement* stmt;
			

				try
				{
					driver = get_driver_instance();
					con = driver->connect(server, username, password);
					con->setSchema("new_data");

					stmt = con->createStatement();
					stmt->execute("TRUNCATE TABLE  new_data");
					
					
				}
				catch (sql::SQLException e)
				{
					cout << "Could not connect to server. Error message: " << e.what() << endl;
					system("pause");
					exit(1);
				}

				
				
				delete stmt;
				delete con;
			}
			if (ss == 2)
			{
				sql::Driver* driver;
				sql::Connection* con;
				sql::Statement* stmt;

				try
				{
					driver = get_driver_instance();
					con = driver->connect(server, username, password);
					con->setSchema("new_data");

					stmt = con->createStatement();
					stmt->execute("TRUNCATE TABLE  prev_his");

					
					cout << "Finished deleting data" << endl;
				}
				catch (sql::SQLException e)
				{
					cout << "Could not connect to server. Error message: " << e.what() << endl;
					system("pause");
					exit(1);
				}


			
				delete stmt;
				delete con;
			}
			if (ss == 3)
			{
				sql::Driver* driver;
				sql::Connection* con;
				sql::Statement* stmt;

				try
				{
					driver = get_driver_instance();
					con = driver->connect(server, username, password);
					con->setSchema("new_data");

					stmt = con->createStatement();
					stmt->execute("TRUNCATE TABLE  logindata");

					
					cout << "Finished deleting data" << endl;
				}
				catch (sql::SQLException e)
				{
					cout << "Could not connect to server. Error message: " << e.what() << endl;
					system("pause");
					exit(1);
				}



				delete stmt;
				delete con;
			}
		} while (ss > 0 && ss < 4);
		cout << endl;
		cout << setfill('*') << setw(SCREEN_WIDTH + 1) << " " << setfill(' ')
			<< endl;
		
		int i=0;
		do
		{
			cout << "SELECT MODE" << endl
				<< "1.Member" << endl
				<< "2.Club management" << endl;
			cin >> i;
			cout << endl;
			cout << setfill('*') << setw(SCREEN_WIDTH + 1) << " " << setfill(' ')
				<< endl;
			if (i == 1)
			{
				users();
			}
			if (i == 2)
			{
				managementactivities();
			}
		} while (i < 3 && i > 0);
	}



		
	
