#include<iostream>		
#include<fstream>
#include<conio.h>
#include<string.h>
#include<string>
using namespace std;
struct node //node type struct
{
	int id;//node has a id
	int product_quantity;
	string food_name;
	int foodPrice;
	node *next;  //next is a pointer to point the next node
	node()        //constructor without parameter
	{
		id = 0;
		product_quantity = 0;
		food_name = "/0";
		foodPrice = 0;
		next = NULL;

	}
	node(int d, string h, int product_quantity, int mon)  //constructor with one parameter "d"
	{
		id = d;
		food_name = h;
		product_quantity = product_quantity;
		foodPrice = mon;
	}
};
//now we can create class linklist
class linklist
{
	node *head;       //first is a pointer to point the first node
public:
	//intilization of the functions
	linklist();
	linklist(int dt, string g, int k, int m);
	~linklist();
	void openAcount();
	void safeRecord();
	void display();
	bool delete_first();
	bool delete_last();
	void deleteat();
	void search();
	void addamount(int id, int g);
	void withdraw_foodPrice(int id, int m);
	void inputdata();
	void getdata();
};
// definations of the functions
linklist::linklist() // constructor without parameter
{
	head = NULL;
	//sinputdata();
	//getdata();
}
linklist::linklist(int dt, string d, int k, int m)  //constructor with parameter
{
	head = new node(dt, d, k, m);  //create one new node
								   //	head->id = dt;  //assign value to id that is dt
	head->next = NULL;  //assign Null to the next of the id
						//only one node is created
}
//this fuction write data into file from node
void linklist::inputdata()
{

	ofstream q;
	q.open("supplier_detail.txt", ios::out | ios::app);
	if (head->next == NULL)
	{
		q << head->id << endl;
		q << head->food_name << endl;
		q << head->product_quantity << endl;
		q << head->foodPrice << endl;

	}
	else if (head->next != NULL)
	{
		node *temp = head;
		while (temp != NULL)
		{
			q << temp->id << endl;
			q << temp->food_name << endl;
			q << temp->product_quantity << endl;
			q << temp->foodPrice << endl;

			temp = temp->next;
			if (temp != NULL)
			{
				q << endl;
			}
		}
	}
}
//this function read data from file then load into nodes
void linklist::getdata()
{
	head = NULL;
	ifstream f;
	f.open("supplier_detail.txt", ios::in);
	if (!f)
	{
		cout << "Supplier's File Not Found";
		exit(1);
	}
	node *temp = NULL;
	//node *temp1 = NULL;
	//temp1 = head;
	while (!f.eof())
	{
		if (head == NULL)
		{
			head = new node();
			f >> head->id;
			f.clear();
			f.get();
			f >> head->food_name;
			f >> head->product_quantity;
			f >> head->foodPrice;
			head->next = NULL;
			temp = head;
		}
		else if (!f.eof())
		{
			temp->next = new node();
			temp = temp->next;
			f >> temp->id;
			f.clear();
			f.get();
			f >> temp->food_name;
			f >> temp->product_quantity;
			f >> temp->foodPrice;
			temp->next = NULL;
		}
	}
	f.close();
}
//this function safe record after update
void linklist::safeRecord() {

	ofstream q;
	q.open("supplier_detail.txt", ios::out);
	if (head->next == NULL)
	{
		q << head->id << endl;
		q << head->food_name << endl;
		q << head->product_quantity << endl;
		q << head->foodPrice << endl;

	}
	else if (head->next != NULL)
	{
		node *temp = head;
		while (temp != NULL)
		{
			q << temp->id << endl;
			q << temp->food_name << endl;
			q << temp->product_quantity << endl;
			q << temp->foodPrice << endl;

			temp = temp->next;
			if (temp != NULL)
			{
				q << endl;
			}
		}
	}
	q.close();
	cout << "RECORD WILL BE SAVE " << endl;
}
//this finction delete first 
bool linklist::delete_first() {

	node *t;
	t = head;
	
	if (t != NULL) {
		head = head->next;//move head on next node
		t->next = NULL;
		delete t; //delete head
		return true;
	}
//	safeRecord();
	return false;
}
//the function delete last
bool linklist::delete_last() {
	node *t;
	t = head;
	

	//t = head->next;
	while (t != NULL) {// check all list
		if (t->next == NULL) {
			delete t; //delete last node
			return true;
		}
		else
			//r = r->next;
			t = t->next; //change position 

	}
	return false;
}
// this function input new data 
void linklist::openAcount()  //insert the value at the first position of linklist
{
	char a;
	do {	 
		if (head == NULL)  //if there is no node is present
		{
			head = new node(); //one node is created
			cout << "*************/*********" << endl;
			cout << "GIVE food ID" << endl;
			cin >> head->id;
			cout << "**********************" << endl;
			cout << "INPUT FOOD NAME" << endl;
			cin >> head->food_name;
			cout << "**********************" << endl;
			cout << "INPUT food quantity" << endl;
			cin >> head->product_quantity;
			cout << "**********************" << endl;
			cout << "ENTER price" << endl;
			cin >> head->foodPrice;

			//	head->id = d;    //assign the value of the id
			head->next = NULL; //assign the next of id is null
			inputdata();
		}
		else   //if already nodes is present then place node at the first position
		{
			node *temp = new node();//new node is created
			cout << "**********************" << endl;
			cout << "GIVE food ID" << endl;
			cin >> temp->id;
			cout << "**********************" << endl;
			cout << "INPUT food NAME" << endl;
			cin >> temp->food_name;
			cout << "**********************" << endl;
			cout << "INPUT quantity" << endl;
			cin >> temp->product_quantity;
			cout << "**********************" << endl;
			cout << "ENTER price" << endl;
			cin >> temp->foodPrice;
			//temp->id = d;      //id is assigned to new node
			temp->next = head;  //new node is linked already exist first node
			head = temp;        //Now first is point to new created node
			inputdata();
		}
		cout << "If you want again open next account (Y/y)" << endl;
		cin >> a;
	} while (a == 'Y' || a == 'y');
}
//this function use delete every where
void linklist::deleteat()
{
	int dt;
	cout << "ENTER THE ID YOU WANT TO DEL" << endl;
	cin >> dt;
	node *p = head;
	node *temp;
	node *t = head;
	while (t->next == NULL)//go to end list
	{
		t = t->next;
	}
	if (dt == head->id)//if first node
	{
		delete_first();
	}
	else if(dt==t->id)//if last node
	{
		delete_last();
	}
	else
	{
		while (p->next != NULL)  //it check the whole linklist
		{
			if (p->next->id == dt)  //it check and compare the id.
			{
				temp = p->next;
				p->next = temp->next;
				cout << "food item deleted at" << temp << endl;
				delete temp;
			}
			p = p->next;
		}
	}
	
	
}
linklist::~linklist()
{

	//inputdata();
	//getdata();
	system("pause");
}
// this fucntion use for search record in file
void linklist::search()
{
	getdata();
	int id;
	cout << "Enter the Id: ";
	cin >> id;
	int count = 0;
	node *temp = head;
	
	if (head == NULL)//check list is empty
	{
		cout << "linklist is empty." << endl;
	}
	else
	{
		while (temp->next != NULL)//check the whole list 
		{
			if (temp->id == id)//find id
			{
				cout << "the value was found at the index: " << count << endl;
				cout << "foodID" << " " << "Foodname" << " " << "foodQuantity" << " " << "price" << endl << endl;
				cout << temp->id << "    " << temp->food_name << "   " << temp->product_quantity << "   " << temp->foodPrice << endl;
				cout << "/*************************/" << endl;
			}
			count++;
			temp = temp->next;
		}
		if (temp->id == id) 
		{
			cout << "the value was found at the index: " << count << endl;
			cout << "/*************************/" << endl;
		}
	}
}
//use for display
void linklist::display() //To display the value
{
	getdata();
	int total_balance = 0;
	int length = 0;
	// to traverse the linklist
	node *temp = head;  //we use temprary variable otherwise value of first is changed
	while (temp != NULL) //if the list is not empty
	{
		if (temp->id != 0)
		{
			cout << "The value of the linklist is  " << temp->id << "\n Product Name :  " << temp->food_name
				<< "\n Quantity  " << temp->product_quantity
				<< "\n Price  " << temp->foodPrice << endl; //print id of linklist
			total_balance = total_balance + temp->foodPrice;
			
			length++;
		}
		temp = temp->next; //increment in node
	}
	cout << "/**********************************************/" << endl;
	cout << "total balance::" << total_balance << endl;
	cout << "/**********************************************/" << endl;
	cout << "The number of the node is:  " << length << endl;
	cout << "/**********************************************/" << endl;
}
void linklist::addamount(int id, int l)
{
	getdata();
	int count = 0;
	node *temp = head;
	if (head == NULL)
	{
		cout << "linklist is empty." << endl;
	}
	else
	{
		while (temp->next != NULL)
		{
			if (temp->id == id)
			{
				cout << "the value was found at the index: " << count << endl;
				cout << "/*************************/" << endl;
				temp->foodPrice = temp->foodPrice + l;
			}
			count++;
			temp = temp->next;
		}
		if (temp->id == id)
		{
			cout << "the value was found at the index: " << count << endl;
			cout << "/*************************/" << endl;
		}

	}

}
void linklist::withdraw_foodPrice(int id, int m)
{
	getdata();
	int count = 0;
	node *temp = head;
	if (head == NULL)
	{
		cout << "linklist is empty." << endl;
	}
	else
	{
		while (temp->next != NULL)
		{
			if (temp->id == id)
			{
				cout << "the value was found at the index: " << count << endl;
				cout << "/*************************/" << endl;
				temp->foodPrice = temp->foodPrice - m;
			}
			count++;
			temp = temp->next;
		}
		if (temp->id == id)
		{
			cout << "the value was found at the index: " << count << endl;
			cout << "/*************************/" << endl;
		}

	}

}
int main()
{
	linklist l;
	system("color b1");
	int p;
	char ch, e = 'v';
	cout << "              			          ***************************************          " << endl
		<< "           			  ********WELLCOME TO MARKET PLACE  MANAGEMENT SYSTEM********	 " << endl
		<< "           			          ***************************************          " << endl
		<<  endl;
	do {
		cout 
			<< " 			 If you want to open a new acount then press             (n) " << endl
			<< " 			 If you want to search a product then press              (x)" << endl
			
			<< " 			 If you want to delete this position then press	         (d)" << endl
			<< " 			 If you want to add amount then press  	          	 (a)" << endl
			<< " 			 If you want to withdraw a price then press 		 (w)" << endl
			<< " 			 If you want to save the record then press   		 (s)" << endl
			<< "      		         If you want to display the record then press            (z)" << endl
			<< "                         If you want to exit				         (e)" << endl;
		cin >> ch;
	  system("cls");
	 
		switch (ch) {
		
		case 'n':
			l.openAcount();
			break;
		case 'x':
			l.search();
			break;
		
		case 'z':
			l.display();
			break;
		case 'd':
			l.deleteat();
			break;
		case 'a':
			int id, z;
			cout << "ENTER A ID NUMBER  " << endl;
			cin >> id;
			cout << "FOOD PRICE WHICH YOU WANT INCREASE" << endl;
			cin >> z;
			l.addamount(id, z);
			break;
		
		case 'w':
			int t, d;
			cout << "ENTER THE ID NUMBER " << endl;
			cin >> t;
			cout << "FOOD THE VALUE YOU WANT BE RAISED" << endl;
			cin >> d;
			l.withdraw_foodPrice(t, d);
			break;
		case 's':
			l.safeRecord();
			break;
		default:
			cout << "INVELID KEY" << endl;
		}
		
		
		
		
			cout << "press 1 key to continue otherwise press 2" << endl;
			cin >> p;
		
	} while (p == 1);



	system("pause");
	_getch();
}
