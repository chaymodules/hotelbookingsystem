# Hotel Booking System

## Object Oriented Programming Code

(package = HMS)

### hotelManagement.java

```java
package Hotel;
//SID 1610314 Object Oriented Programming Assign2
import java.util.*;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.FileWriter;

import javax.xml.bind.*;
import javax.xml.namespace.QName;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;

import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.NamedNodeMap;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

public class hotelManagement {
	// boolean User type set to false for detecting of manager or client user
	private static Boolean mUserType = false;
	// The URI for the directory tree of the bookings.xml
	public static final String bookingURI = "C:\\Users\\chay\\Documents\\bookings.xml";
	// The URI for the directory of the confirmed bookings by the hotel manager
	public static final String apartmentsURI = "C:\\Users\\chay\\Documents\\confirmed.xml";
	// XMLBooking writer used to edit the XML file
	private static XMLBookingWriter myXMLBookingWriter;
	// data type Document used to write to the XML
	private static Document writeDoc;
	// The username for the hotel manager
	private static String adminUsername = "chay";
	// The password for the hotel manager
	private static String adminPassword = "password1";
	// The username for the client
	private static String clientUsername = "joe";
	// The password for the client
	private static String clientPassword = "123";

	// The main method used for the main execution of the program
	public static void main(String[] args) {
		// Scanner used to get input from the user
		Scanner input = new Scanner(System.in);
		// Checks the type of the user, whether it be an admin or a client
		checkUserType();

		// Asks the user for the input
		System.out.println("Please enter your username: ");
		// retrives the input and stores it in username String
		String username = input.nextLine();

		// Asks for the password
		System.out.println("Please enter your password: ");
		// retrives the password from user
		String password = input.nextLine();

		// authenticates the input of the user
		// If the username is true and the password is true then if the user typer is
		// false it will take the user to the clientoptions page
		if (checkUser(username).equals(true) && checkPass(password).equals(true)) {
			if (mUserType.equals(false)) {
				clientOptions();
			} else {
				// or it will take the user to the admin options if the user enters the admin
				// password and username. if they tried to log in as an admin
				adminOptions();
			}
		}
	}

	// method used to check the user type
	private static void checkUserType() {
		// used to get input when the user enters either 1 or 2 for client and admin
		Scanner input = new Scanner(System.in);
		// give selection of user types
		// asks the user to enter either 1 or 2 for admin or client
		System.out.println("Please select user type:\nAdmin: 1\nClient: 2");

		// take input
		// data type integer used to get the next integer in memory from scammer
		int userType = input.nextInt();

		// user input selection to global boolean, to be used in authenitcation methods
		// if the user type is a 1 then the user is a admin representing the number 1
		// and the boolean true
		if (userType == 1) {
			// admin user
			mUserType = true;
		} else if (userType == 2) {
			// client user
			mUserType = false;
		} else {
			// Check if they have entered a correct input type, if not then return to
			// beginning of method
			System.out.println("Invalid input");
			// prints invalid input if the user does not type in a valid number at entry
			checkUserType();
		}
	}

	// the boolean method used for checking the user, input is string of user when
	// used.
	private static Boolean checkUser(String user) {
		// dec and init username
		// default username is nothing, in string format
		String username = "";

		// check the selected user type, and assign the correct login to "username"
		if (mUserType.equals(true)) {
			// the username is a admin user if the usertype is equal to true
			username = adminUsername;
		} else {
			// otherwise the user type is detected as a client
			username = clientUsername;
		}

		// if the user input is equal to the correct login, then return true, else
		// return false
		if (user.contentEquals(username)) {
			// returns true when method used
			return true;
		} else {
			// returns invalid username and returns false
			System.out.print("Invalid username");
			return false;
		}
	}

//checks the pass
	private static Boolean checkPass(String pass) {
		// dec and init password
		String password = "";

		// check the selected user type, and assign the correct login to "password"
		if (mUserType.equals(true)) {
			password = adminPassword;
		} else {
			password = clientPassword;
		}

		// if the user input is equal to the correct login, then return true, else
		// return false;
		if (pass.contentEquals(password)) {
			// returns true if the pass contents is not equal to the saved password
			return true;
		} else {
			// if the password is not correct it will display Invalid password
			System.out.print("Invalid password");
			return false;
		}
	}

	// method returns nothing, method used for the admin options of the program.
	private static void adminOptions() {
		// scanner object used to get input of user
		Scanner input = new Scanner(System.in);
		// prints out the hotel manager options, such as view booking
		System.out.println("Hotel Manager options: \n1. View and manage bookings\n2. Exit");
		// stores users input in integer option
		int option = input.nextInt();
		// if what the user enters is 1 then view booking
		if (option == 1) {
			// if the booking does exist it will view the booking
			if (checkFileSize() > 0) {
				// view bookings method runs
				viewBookings();
			} else {
				// if this doesnt occur then it will tell the user the bookings are not found
				System.out.println("No bookings found");
			}
			// otherwise manage a booking
			manageABooking();
		} else if (option == 2) {
			// or it will take the user back to main if they use the vaule 2 instead
			main(null);
		} else {
			// if the user enters a invalid option this will display and then run the
			// adminOptions method
			System.out.println("Invalid option");
			adminOptions();
		}
	}

	// method , which returns nothing used for managing a booking
	private static void manageABooking() {
		// creating object for getting user input
		Scanner input = new Scanner(System.in);
		// Asks user if they would like to confirm the booking, decline, or exit
		System.out.println("Would you like to confirm this booking?\n1. Confirm\n2. Decline\n3. Exit");
		// gets the users option by getting next integer in memory
		int option = input.nextInt();
		// if the user enter 1 then the program will try to confirm the booking
		if (option == 1) {
			try {
				confirmBooking();
			} catch (IOException e) {
				// otherwise catch IOexception
				e.printStackTrace();
			}
		}
		// if the user enters 2 then it will cancel the booking by referring to the
		// cancelBooking method
		else if (option == 2) {
			try {
				// refers to cancel booking method
				cancelBooking();
			} catch (IOException e) {
				// catches error if it is to occur
				e.printStackTrace();
			}
		}
		// if the user enters the option 3 then it will return to the main method in the
		// menu
		else if (option == 3) {
			main(null);
		} else {
			// otherwise the program will print invalid option then go to the manageabooking
			// method
			System.out.println("Invalid Option");
			manageABooking();
		}
	}

	// method which returns nothing used for client options
	private static void clientOptions() {
		// object used for creation of and use of user input
		Scanner input = new Scanner(System.in);
		// prints the options for the client menu , such as do your booking and exit
		System.out.println("Client options:\n1. Do your booking\n2. Manage your booking\n3. Exit");
		// stores the users input into the variable integer option
		int option = input.nextInt();
		// if the user enters 1, then the program will make the booking by referring to
		// the makeBooking method
		if (option == 1) {
			makeBooking();
		}
		// else if they dont use option 1 and they use 2 as an option then they will use
		// the Managebooking method
		else if (option == 2) {
			cManageBooking();
		}
		// else if they dont use the manage booking method and they use 3 as an option
		// then the program will take them back to the main method
		else if (option == 3) {
			main(null);
		}
		// otherwise what the user entered is not a valid option , prints ivalid option
		// and returns to clientOptions method
		else {
			System.out.println("Invalid option");
			// returns to client options method
			clientOptions();
		}

	}

	// private method returns nothing , throws an IOException if theirs errors/
	private static void confirmBooking() throws IOException {
		// creating a buffered writer object called writer which is used for the out
		// stream of the fileoutputstream for the apartmentsURI which would be linked to
		// the apartments.xml
		BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(new FileOutputStream(apartmentsURI)));
		// creating a buffered reader object called reader which is used for the input
		// stream of the fileinputstream for the bookingURI which would be linked to the
		// bookings.xml
		BufferedReader reader = new BufferedReader(new InputStreamReader(new FileInputStream(bookingURI)));
		// string reader starts of as null,
		String line = null;
		// while the string reader reads the line and is not equal to null then the
		// writer will write. This is mainly to prevent errors when confirming a booking
		while ((line = reader.readLine()) != null) {
			// the writer overwrites null
			writer.write(line);
		}
		// the reader closes
		reader.close();
		// the writer closes
		writer.close();
		// the filewriter object creation of fw , which has fields bookingURI for
		// writing to URI, of FileWriter constructor
		FileWriter fw = new FileWriter(bookingURI, false);
		// the printwriter object creation of pw, which has fields BookingURI for
		// printing to fw, of Print Writer constructor
		PrintWriter pw = new PrintWriter(fw, false);
		// flushes current print writer
		pw.flush();
		// closes print writer
		pw.close();
		// closes file writer
		fw.close();
		// shows adminoptions
		adminOptions();
	}

	// method used for canceling the booking, also throws IOException if error
	// occurs, doesnt return a vaule.
	private static void cancelBooking() throws IOException {
		// the filewriter object creation of fw , which has fields bookingURI for
		// writing to URI, of FileWriter constructor
		FileWriter fw = new FileWriter(bookingURI, false);
		// the printwriter object creation of pw, which has fields BookingURI for
		// printing to fw, of Print Writer constructor
		PrintWriter pw = new PrintWriter(fw, false);
		// flushes current print writer
		pw.flush();
		// closes print writer
		pw.close();
		// closes file writer
		fw.close();
		// shows client options and tells user booking cancelled
		System.out.println("Booking canceled!");
		clientOptions();
	}

	// method used for making a booking , returns nothing
	private static void makeBooking() {
		// creating xml booking writer object
		myXMLBookingWriter = new XMLBookingWriter();
		// using server connection for writing to the document
		writeDoc = myXMLBookingWriter.serverConnection();
		// Name of client, num of guest, booking dates, name of apartment, number of
		// bedrooms, seperate living room or not,
		// number of bathrooms
		// catering or no
		// creation of scanner object for reading user input
		Scanner input = new Scanner(System.in);
		// asks users to enter there details
		System.out.println("Please enter your details:\n\nYour first name:");
		// stores their details (first name)
		String fName = input.nextLine();
		// skips line in scanner input buffer in memory
		input.nextLine();
		// asks for last name
		System.out.println("Your last name:");
		// stores their last name in lName
		String lName = input.nextLine();
		// skips line in scanner input buffer in memory
		input.nextLine();
		// asks user for their apartment address
		System.out.println("Chosen apartment address: ");
		// stores their apartment address in apName
		String apName = input.nextLine();
		// skips line in scanner input buffer in memory
		input.nextLine();
		// asks user for number of guests staying
		System.out.println("The Number of guest that will be staying:");
		// stores number of guests in noGuests String
		String noGuests = input.nextLine();
		// skips line in scanner input buffer in memory
		input.nextLine();
		// Prints start date, asking for start date from user for booking
		System.out.println("Start date:");
		// stores booking date in bookingdate from scanner next line
		String bookingDate = input.nextLine();
		// asks for the end date of the booking
		System.out.println("End date:");
		// stores end date in the endBookingDate String
		String endBookingDate = input.nextLine();
		// Asks user if they require catering
		System.out.println("Do you require catering? - Y/N");
		// stores their input in catering String
		String catering = input.nextLine();
		// asks user if they want to confirm their booking
		System.out.println("Would you like to confirm your booking? - Y/N");
		String confirm = input.nextLine();
//if the user confirms their booking , their input is converted to lowercase, and then a XMLbooking writer is started 
		if (confirm.toLowerCase().contentEquals("y")) {
			// new booking writer object creation
			XMLBookingWriter xmlbr = new XMLBookingWriter();
			// writes these variables to the document from user input , locating the booking
			// URI , using writeDoc variable
			xmlbr.bookingWriter(writeDoc, bookingURI, apName, fName, lName, noGuests, bookingDate, endBookingDate,
					catering);
			// then the program displays the client options
			clientOptions();
		} // otherwise it navigates to makeBooking
		else {
			makeBooking();
		}
		// input closed from scanner
		input.close();
	}

	// method used for managing the booking
	private static void cManageBooking() {
		// creation of scanner object for user input
		Scanner input = new Scanner(System.in);
		// checkas the file exists before viewing booking
		if (checkFileSize() > 0) {
			// viewing booking method
			viewBookings();
		} else {
			// otherwise prints that no booking was found
			System.out.println("No bookings found");
		}
		// takes user back to menu for managing the booking
		System.out.println("Managing booking:\n1. Cancel booking\n2. Back");
		// gets option from user and stores in option
		int option = input.nextInt();
		// compares option to 1, if option 1 then cancel booking
		if (option == 1) {
			try {
				// cancels booking method
				cancelBooking();
			} catch (IOException e) {
				// if error occurs
				e.printStackTrace();
			}
		}
		// if the user enters option 2 then it will take the user back to clientoptions
		else if (option == 2) {
			clientOptions();
		} else {
			// if the user does not type in a valid option i.e 1, or 2 then takes the user
			// back to Managebookings
			System.out.println("Invalid option");
			cManageBooking();
		}
	}

	// used to check if file exists
	private static int checkFileSize() {
		// creation of object called file of bookingURI
		File file = new File(bookingURI);
		// returns file size (length) ( int)
		int filesize = (int) file.length();
		return filesize;
	}

	// method used to view bookings
	private static void viewBookings() {
		// creation of bookings object
		XMLBookings bookings = new XMLBookings();
		// reads bookings from XML
		bookings.bookingsreader();
	}
}
```

### XMLApartmentsReader.java

```java
package Hotel;
//SID 1610314 Object Oriented Programming Assign2
//import all needed APIs for functional program
import java.io.File;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.DocumentBuilder;
import org.w3c.dom.Document;
import org.w3c.dom.NodeList;
import org.w3c.dom.Node;
import org.w3c.dom.Element;
import javax.swing.*;
import java.awt.*;

//GUI only 
//extends to abract class XMLReader
public class XMLApartmentsReader extends XMLReader
{
	//creates document data type aDoc
	private Document aDoc;
	//new object String[][] for formatting of XML 
	private String[][] data = new String[10][9];	
	//private String column[]={"Apartment ID","Price per Night","Start Date", "End Date", "Max Guests", "Number Beds", "Number Baths", "Living Room"};  
	//counter for the rows of the XML
	private int rowCounter = 0;
	//counter for the columns of the XML
	private int columnCounter = 0;
	//JTable and JScrollPane for displaying XML with GUI rather than console
	private JTable apartmentsTable;   
	private JScrollPane displayScrollPanel;
	//constructor of apartmentsReader
	public String[][] apartmentsReader()
	{
		//commented out code for creating of JTable , and Scroll Pane (used for debugging purposes)
		//apartmentsTable = new JTable(data,column);    
		//apartmentsTable.setBounds(30,40,200,300);
		//apartmentsScrollPanel = new JScrollPane(apartmentsTable);
		//Document server connection to the apartmentsURI of the xml file 
		aDoc = serverConnection(hotelManagement.apartmentsURI);
		//tells the user the root element (for debugging purposes
		System.out.println("Root element :" + aDoc.getDocumentElement().getNodeName()); //nodename of docuement element (root element)
		//gets the node listing of the document by the apartment tag element 
		NodeList nList = aDoc.getElementsByTagName("apartment");
		//prints this as a seperator in the console 
		System.out.println("----------------------------");
		//counts through 0 to list length (acts as counter)
		for (int temp = 0; temp < nList.getLength(); temp++)
		{
			//for node list item, counts through all 
			Node nNode = nList.item(temp);
			//displays current element print everytime 
			System.out.println("\nCurrent Element :" + nNode.getNodeName());
			//if the node type is equal to the node element then 
			if (nNode.getNodeType() == Node.ELEMENT_NODE) 
			{
				// the element becomes the node 
				Element eElement = (Element) nNode;
				// string ID stored as string aptID_table string variable 
				String aptID_Table = eElement.getAttribute("ID");
				//String element apartment name stored as aptname (after getting text) same occurs for each element tag item
				String aptName_Table = eElement.getElementsByTagName("apartmentname").item(0).getTextContent();
				String price_Table = eElement.getElementsByTagName("price").item(0).getTextContent();
				String startDate_Table = eElement.getElementsByTagName("startdate").item(0).getTextContent();
				String endDate_Table = eElement.getElementsByTagName("enddate").item(0).getTextContent();
				String maxGuests_Table = eElement.getElementsByTagName("maxguests").item(0).getTextContent();
				String numBeds_Table = eElement.getElementsByTagName("numberbeds").item(0).getTextContent();
				String numBaths_Table = eElement.getElementsByTagName("numberbaths").item(0).getTextContent();
				String livingRoom_Table = eElement.getElementsByTagName("livingroom").item(0).getTextContent();
				//use counter to display all 
				data[rowCounter][columnCounter] = aptName_Table;
				columnCounter = columnCounter + 1;
				
				data[rowCounter][columnCounter] = aptID_Table;
				columnCounter = columnCounter + 1;
				
				data[rowCounter][columnCounter] = price_Table;
				columnCounter = columnCounter + 1;
				
				data[rowCounter][columnCounter]  = startDate_Table;
				columnCounter = columnCounter + 1;
				
				data[rowCounter][columnCounter] = endDate_Table;
				columnCounter = columnCounter + 1;
				
				data[rowCounter][columnCounter]  = maxGuests_Table;
				columnCounter = columnCounter + 1;
				
				data[rowCounter][columnCounter] = numBeds_Table;
				columnCounter = columnCounter + 1;
				
				data[rowCounter][columnCounter] = numBaths_Table;
				columnCounter = columnCounter + 1;
				
				data[rowCounter][columnCounter] = livingRoom_Table;
				
				// Debugging statements				
				System.out.println("Apartment ID: " +  aptID_Table);
				System.out.println("Price per Night : " + price_Table);				
			    System.out.println("Start Date: " + startDate_Table);
				System.out.println("End Date: " + endDate_Table);
				System.out.println("Max number of guests: " + maxGuests_Table);
				System.out.println("Number of Bedrooms: " + numBeds_Table);
				System.out.println("Number of Bathrooms: " + numBaths_Table);
				System.out.println("Separate Living Room: " + livingRoom_Table);
			}// end if
			//adds to the counter for each row of the table 
			rowCounter = rowCounter + 1;
			//column counter does not change 
			columnCounter = 0;
		}// end for
		//returns data (table)
		return data;
	}
	//used for displaying the table , dataTable , column table, Jpanel  (GUI is used)
	public void tableDisplay(String[][] dataTable, String columnTable[], JPanel displayPanel, JFrame displayFrame)
	{
		//creation of new JTable apartments table for displaying datTable, and columntable , uses JTable constructor 
			apartmentsTable = new JTable(dataTable,columnTable); 
			//sets bounds of the table (formatting)
			apartmentsTable.setBounds(30,40,1200,200);
			//used for the displays scroll pane , object creation of display Scroll Pane 
			displayScrollPanel = new JScrollPane(apartmentsTable);
			// Adds scroll panel to display panel 
		 	displayPanel.add(displayScrollPanel);
		    //adds display pane to display frame 
		    displayFrame.add(displayPanel);          
		    //displayFrame.setSize(300,400);    
		    //visiblilty set to true 
		    displayFrame.setVisible(true);   
	}
		
	
}
```

### XMLBookings.java

```java
package Hotel;
//SID 1610314 Object Oriented Programming Assign2
//importing API's Used 
import java.io.File;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.DocumentBuilder;
import org.w3c.dom.Document;
import org.w3c.dom.NodeList;
import org.w3c.dom.Node;
import org.w3c.dom.Element;
import javax.swing.*;
import java.awt.*;

//extends to abstract class 
public class XMLBookings extends XMLReader {
	// creates document data type aDoc
	private Document aDoc;
	// new object String[][] for formatting of XML
	private String[][] data = new String[10][9];
	// counter for the rows of the XML
	private int rowCounter = 0;
	// counter for the columns of the XML
	private int columnCounter = 0;
	// JTable and JScrollPane for displaying XML with GUI rather than console
	private JTable apartmentsTable;
	private JScrollPane displayScrollPanel;

	// constructor of bookingsreader
	public String[][] bookingsreader() {
		// Document server connection to the bookingsURI of the xml file
		aDoc = serverConnection(hotelManagement.bookingURI);
		// tells the user the root element (for debugging purposes
		System.out.println("Root element :" + aDoc.getDocumentElement().getNodeName());
		// gets the node listing of the document by booking tag element
		NodeList nList = aDoc.getElementsByTagName("booking");
		// prints this as a seperator in the console
		System.out.println("----------------------------");
		// counts through 0 to list length (acts as counter)
		for (int temp = 0; temp < nList.getLength(); temp++) {
			// for node list item, counts through all
			Node nNode = nList.item(temp);
			// displays current element print everytime
			System.out.println("\nCurrent Element :" + nNode.getNodeName());
			// if the node type is equal to the node element then
			if (nNode.getNodeType() == Node.ELEMENT_NODE) {
				// the element becomes the node
				Element eElement = (Element) nNode;
				// String element apartment name stored as aptname_table (after getting text)
				// same occurs for each element tag item
				String aptName_Table = eElement.getElementsByTagName("apartmentname").item(0).getTextContent();
				String catering_Table = eElement.getElementsByTagName("catering").item(0).getTextContent();
				String clientName_Table = eElement.getElementsByTagName("firstname").item(0).getTextContent();
				String clientLName_Table = eElement.getElementsByTagName("lastname").item(0).getTextContent();
				String startDate_Table = eElement.getElementsByTagName("startdate").item(0).getTextContent();
				String endDate_Table = eElement.getElementsByTagName("enddate").item(0).getTextContent();
				String Guests_Table = eElement.getElementsByTagName("numberguests").item(0).getTextContent();

				// Debugging statements
				System.out.println("Apartment ID: " + "1");
				System.out.println("Apartment name: " + aptName_Table);
				System.out.println("Client name : " + clientName_Table + " " + clientLName_Table);
				System.out.println("Start Date: " + startDate_Table);
				System.out.println("Start Date: " + endDate_Table);
				System.out.println("Number of guests: " + Guests_Table);
				System.out.println("Date: " + startDate_Table);
				System.out.println("Required Catering?: " + catering_Table);
			} // end if
				// adds to the counter for each row of the table
			rowCounter = rowCounter + 1;
			// column counter does not change
			columnCounter = 0;
		} // end for
			// returns data (table)
		return data;
	}

	// used for displaying the table , dataTable , column table, Jpanel (GUI is
	// used)
	public void tableDisplay(String[][] dataTable, String columnTable[], JPanel displayPanel, JFrame displayFrame) {
		// creation of new JTable apartments table for displaying datTable, and
		// columntable , uses JTable constructor

		apartmentsTable = new JTable(dataTable, columnTable);
		// sets bounds of the table (formatting)

		apartmentsTable.setBounds(30, 40, 1200, 200);
		// used for the displays scroll pane , object creation of display Scroll Pane

		displayScrollPanel = new JScrollPane(apartmentsTable);
		// Adds scroll panel to display panel

		displayPanel.add(displayScrollPanel);
		// adds display pane to display frame

		displayFrame.add(displayPanel);
		// displayFrame.setSize(300,400);
		// visiblilty set to true

		displayFrame.setVisible(true);
	}

}
```

### XMLBookingWriter.java

```java
package Hotel;
//SID 1610314 Object Oriented Programming Assign2
import javax.xml.parsers.DocumentBuilderFactory;
import javax.swing.JOptionPane;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.w3c.dom.Attr;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;

import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;

public class XMLBookingWriter extends XMLWriter {
	// Used to store Element rootElement
	private Element rootElement;
	// Used to store Element theBooking
	private Element theBooking;
	// Used to store Element theApartmentName
	private Element theApartmentName;
	// Used to store Atrr attr
	private Attr attr;
	// Used to store Element theFirstName
	private Element theFirstName;
	// Used to store Element theLastName
	private Element theLastName;
	// Used to store Element theMaxGuests
	private Element theMaxGuests;
	// Used to store Element theStartDate
	private Element theStartDate;
	// Used to store Element theEndDate
	private Element theEndDate;
	// Used to store Element theCatering
	private Element theCatering;
	// Used to store TransformerFactory transformerFactory
	private TransformerFactory transformerFactory;
	// Used to store Transformer transformer
	private Transformer transformer;
	// Used to store DOMSource source
	private DOMSource source;
	// Used to store StreamResult result
	private StreamResult result;
	// Used to store StreamResult consoleResult
	private StreamResult consoleResult;
	// Used to store BID integer
	private int BID = 101;

//public method returns nothing , data needed to feed into bookingWriter 
	public void bookingWriter(Document doc, String filenameToWrite, String clientApartmentName, String clientFirstName,
			String clientLastName, String maxGuests, String bookingStartDate, String bookingEndDate,
			String bookingCatering) {
		try {
			// root element of XML is bookings (creates element)
			rootElement = doc.createElement("bookings");
			// appends childen rootElement
			doc.appendChild(rootElement);
			// creates the booking based on follow data type information
			rootElement.appendChild(createBooking(doc, filenameToWrite, clientApartmentName, clientFirstName,
					clientLastName, maxGuests, bookingStartDate, bookingEndDate, bookingCatering));

			// write the content into xml file by creatin transformfactory instance (object)
			TransformerFactory transformerFactory = TransformerFactory.newInstance();
			Transformer transf = transformerFactory.newTransformer();
			// type of encoding used for the XML
			transf.setOutputProperty(OutputKeys.ENCODING, "UTF-8");
			transf.setOutputProperty(OutputKeys.INDENT, "yes");
			transf.setOutputProperty("{http://xml.apache.org/xslt}indent-amount", "2");
			// DOM source , object used for constructor of doc
			DOMSource source = new DOMSource(doc);
			// storing bookingURI as new file object
			File myFile = new File(hotelManagement.bookingURI);
			// results is in console object
			StreamResult console = new StreamResult(System.out);
			// file object stream stored in file, using myfile in constructor of file
			StreamResult file = new StreamResult(myFile);
			// setting transform type source = console, source =file
			transf.transform(source, console);
			transf.transform(source, file);
		} catch (Exception e) {// incase of possible errors catch them (Exceptions)
			e.printStackTrace();
		} // end try...catch
	}

	// returns node from method createbooking , revies information for booking
	// elements
	private static Node createBooking(Document doc, String filenameToWrite, String clientApartmentName,
			String clientFirstName, String clientLastName, String maxGuests, String bookingStartDate,
			String bookingEndDate, String bookingCatering) {
		// booking element booking store the creation of booking
		Element booking = doc.createElement("booking");
		// id set to 1
		String id = "1";
		// setting attribute to id
		booking.setAttribute("id", id);
		// setting first name from program to firstname in XML
		booking.appendChild(createBookingElement(doc, "firstname", clientFirstName));
		// setting lastname from program to lastname in XML
		booking.appendChild(createBookingElement(doc, "lastname", clientLastName));
		// setting apartment name from program to XML
		booking.appendChild(createBookingElement(doc, "apartmentname", clientApartmentName));
		// setting maxGuests name from program to XML
		booking.appendChild(createBookingElement(doc, "numberguests", maxGuests));
		// setting startdate name from program to XML

		booking.appendChild(createBookingElement(doc, "startdate", bookingStartDate));
		// setting enddate name from program to XML

		booking.appendChild(createBookingElement(doc, "enddate", bookingEndDate));
		// setting cateringbooking variable from program to XML element
		booking.appendChild(createBookingElement(doc, "catering", bookingCatering));
//returns booking node
		return booking;
	}

	// method which returns Node for creating booking element
	private static Node createBookingElement(Document doc, String name, String value) {
//creates element node based on element node 
		Element node = doc.createElement(name);
		// appends vaule of text node
		node.appendChild(doc.createTextNode(value));
//returns node from method when used 
		return node;
	}

}
```

### XMLReader.java

```java
package Hotel;
//SID 1610314 Object Oriented Programming Assign2
//imports API's needed for program 
import java.io.File;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.xml.parsers.DocumentBuilder;
import org.w3c.dom.Document;
import org.w3c.dom.NodeList;
import org.w3c.dom.Node;
import org.w3c.dom.Element;

//abstract class XML reader
public abstract class XMLReader {
	// used to store file input
	private File inputFile;
	// used to store DocumentBuilder dBuilder
	private DocumentBuilder dBuilder;
	// used to store DocumentBuilderFactory dbFactory
	private DocumentBuilderFactory dbFactory;
	// used to store Document doc (works now)
	private Document doc;

	// method serverconnection returns doc
	public Document serverConnection(String filename) {
		try {
			// inputting file from filename
			inputFile = new File(filename);
			// dbFactory stores new instance of builder
			dbFactory = DocumentBuilderFactory.newInstance();
			// factory creates new document builder , stored in dBuilder
			dBuilder = dbFactory.newDocumentBuilder();
			// doc = new Document(); //can cause error
			// parsing input file
			doc = dBuilder.parse(inputFile);
			// normalizing input to prevent errors (correct format)
			doc.getDocumentElement().normalize();
			// catch Exception for possible errors
		} catch (Exception e) {
			// print error
			e.printStackTrace();
		} // end try...catch
		return doc;

	}// end method
		// abstract table Display

	abstract void tableDisplay(String[][] aDataTable, String aColumn[], JPanel displayPanel, JFrame displayFrame);

}
```

### XMLWriter.java

```java
package Hotel;//SID 1610314 Object Oriented Programming Assign2

//importing needed API'S
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.w3c.dom.Attr;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;

import java.io.ByteArrayInputStream;
import java.io.File;

//public class XMLWriter
public class XMLWriter {
	// factory for docuement builder
	private DocumentBuilderFactory dbFactory;
	// docuement builder data type as dbuilder
	private DocumentBuilder dBuilder;
	// the editable document
	private Document doc;

	// method returns doc (serverConnection < name)
	public Document serverConnection() {
		try {
			// create the three layers for the XML writer
			// dbFactory stores new instance of builder

			dbFactory = DocumentBuilderFactory.newInstance();
			// factory creates new document builder , stored in dBuilder

			dBuilder = dbFactory.newDocumentBuilder();
			// new document stored in doc
			doc = dBuilder.newDocument();
		} // used to catch exception errors
		catch (Exception e) {
			e.printStackTrace();
		} // end try...catch
			// returns document
		return doc;

	}
}
```

[chaymodules/hotelbookingsystem](https://github.com/chaymodules/hotelbookingsystem)
