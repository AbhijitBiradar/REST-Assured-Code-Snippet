# REST-Assured-Code-Snippet


# REST API Testing – Basics

How to validate HTTP response status code

import static org.junit.Assert.*;
import org.testng.Assert;   //used to validate response status 
import org.testng.annotations.Test;
import io.restassured.RestAssured;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;

public class RestAssuredTestResponse {
              @Test
              public void GetBookDetails()
       {  
           // Specify the base URL to the RESTful web service
           RestAssured.baseURI = "https://demoqa.com/BookStore/v1/Books";
           // Get the RequestSpecification of the request to be sent to the server
           RequestSpecification httpRequest = RestAssured.given();

           Response response = httpRequest.get("");

           // Get the status code of the request. 
           //If request is successful, status code will be 200
          int statusCode = response.getStatusCode();

           // Assert that correct status code is returned.
          Assert.assertEquals(statusCode /*actual value*/, 200 /*expected value*/, 
            "Correct status code returned");

    }
}



How to validate the HTTP error status code?

import static org.junit.Assert.*;
import org.testng.Assert;   //used to validate response status 
import org.testng.annotations.Test;
import io.restassured.RestAssured;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;

public class RestAssuredTestResponse {
              
              @Test
              public void GetPetDetails()
              {  
                 // Specify the base URL to the RESTful web service
           RestAssured.baseURI = "https://demoqa.com/Account/v1/User/";
           // Get the RequestSpecification of the request to be sent to the server
           RequestSpecification httpRequest = RestAssured.given();

           Response response = httpRequest.get("test");

           // Get the status code of the request. 
           //If request is successful, status code will be 200
           int statusCode = response.getStatusCode();

           // Assert that correct status code is returned.
           Assert.assertEquals(statusCode /*actual value*/, 200 /*expected value*/, 
           "Correct status code returned");
              }
}


How to validate the response status line?

@Test
public void GetBookDetails()
{  
    // Specify the base URL to the RESTful web service 
    RestAssured.baseURI = "https://demoqa.com/BookStore/v1/Books"; 
    // Get the RequestSpecification of the request to be sent to the server 
    RequestSpecification httpRequest = RestAssured.given(); 
    Response response = httpRequest.get(""); 

    // Get the status line from the Response in a variable called statusLine
    String statusLine = response.getStatusLine();
    Assert.assertEquals(statusLine /*actual value*/, "HTTP/1.1 200 OK" 
      /*expected value*/, "Correct status code returned");

}


How to access and read HTTP Response headers using REST Assured?

@Test 
public void IteratingHeaders() 
{ RestAssured.baseURI = "https://demoqa.com/BookStore/v1/Books"; 
 RequestSpecification httpRequest = RestAssured.given(); 
 Response response = httpRequest.get(""); 
 // Get all the headers and then iterate over allHeaders to print each header 
 Headers allHeaders = response.headers(); 
 // Iterate over all the Headers 
 for(Header header : allHeaders) { 
   System.out.println("Key: " + header.getName() + " Value: " + header.getValue()); 
 } 
}



Let us demonstrate the .header(String arg0) method to get a particular header. Here we pass the exact header name as an argument.

@Test
public void GetBookHeaders() { 
RestAssured.baseURI = "https://demoqa.com/BookStore/v1/Books";
RequestSpecification httpRequest = RestAssured.given();
Response response = httpRequest.get(""); 
// Access header with a given name. 
String contentType = response.header("Content-Type"); 
System.out.println("Content-Type value: " + contentType); 
// Access header with a given name. 
String serverType = response.header("Server"); 
System.out.println("Server value: " + serverType); 
// Access header with a given name. Header = Content-Encoding 
String acceptLanguage = response.header("Content-Encoding"); 
System.out.println("Content-Encoding: " + acceptLanguage); 
  } 
 } 
}


How to validate HTTP Response Header using Rest Assured?

@Test
public void ValidateBookHeaders() 
{ 
    RestAssured.baseURI = "https://demoqa.com/BookStore/v1/Books";
    RequestSpecification httpRequest = RestAssured.given();
    Response response = httpRequest.get("");
    // Access header with a given name. Header = Content-Type 
    String contentType = response.header("Content-Type"); 
    Assert.assertEquals(contentType /* actual value */, "application/json; charset=utf-8" /* expected value */); 
    // Access header with a given name. Header = Server 
    String serverType = response.header("Server"); 
    Assert.assertEquals(serverType /* actual value */, "nginx/1.17.10 (Ubuntu)" /* expected value */);
 }
 
 
 Read JSON Response Body using Rest Assured
 
 @Test
public void WeatherMessageBody()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// Retrieve the body of the Response
	ResponseBody body = response.getBody();

	// By using the ResponseBody.asString() method, we can convert the  body
	// into the string representation.
	System.out.println("Response Body is: " + body.asString());
}


How to Validate Response Body contains some String?

@Test
public void WeatherMessageBody()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// Retrieve the body of the Response
	ResponseBody body = response.getBody();

	// To check for sub string presence get the Response body as a String.
	// Do a String.contains
	String bodyAsString = body.asString();
	Assert.assertEquals(bodyAsString.contains("Hyderabad") /*Expected value*/, true /*Actual Value*/, "Response body contains Hyderabad");
}


Check String presence by ignoring alphabet casing

@Test
public void WeatherMessageBody()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// Retrieve the body of the Response
	ResponseBody body = response.getBody();

	// To check for sub string presence get the Response body as a String.
	// Do a String.contains
	String bodyAsString = body.asString();

	// convert the body into lower case and then do a comparison to ignore casing.
	Assert.assertEquals(bodyAsString.toLowerCase().contains("hyderabad") /*Expected value*/, true /*Actual Value*/, "Response body contains Hyderabad");
}


How to Extract a Node text from Response using JsonPath?

@Test
public void VerifyCityInJsonResponse()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// First get the JsonPath object instance from the Response interface
	JsonPath jsonPathEvaluator = response.jsonPath();

	// Then simply query the JsonPath object to get a String value of the node
	// specified by JsonPath: City (Note: You should not put $. in the Java code)
	String city = jsonPathEvaluator.get("City");

	// Let us print the city variable to see what we got
	System.out.println("City received from Response " + city);

	// Validate the response
	Assert.assertEquals(city, "Hyderabad", "Correct city name received in the Response");

}

Sample Code to read all the nodes from Weather API Response

@Test
public void DisplayAllNodesInWeatherAPI()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// First get the JsonPath object instance from the Response interface
	JsonPath jsonPathEvaluator = response.jsonPath();

	// Let us print the city variable to see what we got
	System.out.println("City received from Response " + jsonPathEvaluator.get("City"));

	// Print the temperature node
	System.out.println("Temperature received from Response " + jsonPathEvaluator.get("Temperature"));

	// Print the humidity node
	System.out.println("Humidity received from Response " + jsonPathEvaluator.get("Humidity"));

	// Print weather description
	System.out.println("Weather description received from Response " + jsonPathEvaluator.get("Weather"));

	// Print Wind Speed
	System.out.println("City received from Response " + jsonPathEvaluator.get("WindSpeed"));

	// Print Wind Direction Degree
	System.out.println("City received from Response " + jsonPathEvaluator.get("WindDirectionDegree"));
}


------------------------------------------------------------------------------------------------------------------------------------------------

# REST API Testing – Advance

Changing the HTTP Method on a POST Request using Rest Assured

public void UserRegistrationSuccessful() 
{ 
    RestAssured.baseURI ="https://demoqa.com/Account/v1"; 
    RequestSpecification request = RestAssured.given(); 
    JSONObject requestParams = new JSONObject();
    requestParams.put("userName", "test_rest");
    requestParams.put("password", "Testrest@123"); 
    request.body(requestParams.toJSONString());
    Response response = request.put("/User"); 
    ResponseBody body = response.getBody();
    System.out.println(response.getStatusLine());
    System.out.println(body.asString());
     
}

Basic Authentication Flow

@Test
public void AuthenticationBasics()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/authentication/CheckForAuthentication";
	RequestSpecification request = RestAssured.given();

	Response response = request.get();
	System.out.println("Status code: " + response.getStatusCode());
	System.out.println("Status message " + response.body().asString());
}

How to implement a PUT Request using Rest Assured?

@Test
public void updateBook() {
    RestAssured.baseURI = baseUrl;
    RequestSpecification httpRequest = RestAssured.given().header("Authorization", "Bearer " + token)
            .header("Content-Type", "application/json");

    //Calling the Delete API with request body
    Response res = httpRequest.body("{ \"isbn\": \"" + isbn + "\", \"userId\": \"" + userId + "\"}").put("/BookStore/v1/Book/9781449325862");

    //Fetching the response code from the request and validating the same
    System.out.println("The response code - " +res.getStatusCode());
    Assert.assertEquals(res.getStatusCode(),200);

}

How to make a Delete Request using Rest Assured?

@Test
	  public void deleteBook() {
		  RestAssured.baseURI = baseUrl;
		  RequestSpecification httpRequest = RestAssured.given().header("Authorization", "Bearer " + token)
			         .header("Content-Type", "application/json");
		 
		  //Calling the Delete API with request body
		  Response res = httpRequest.body("{ \"isbn\": \"" + isbn + "\", \"userId\": \"" + userId + "\"}").delete("/BookStore/v1/Book");
	 
		  //Fetching the response code from the request and validating the same
		  System.out.println("The response code is - " +res.getStatusCode());
	      Assert.assertEquals(res.getStatusCode(),204);
     
	  }
    
-----------------------------------------------------------------------------------------------------------------------------------

# JSON Manipulations

DeSerialize JSON Array to List

@Test
public void JsonPathUsage() throws MalformedURLException
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/books/getallbooks";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("");

	// First get the JsonPath object instance from the Response interface
	JsonPath jsonPathEvaluator = response.jsonPath();

	// Read all the books as a List of String. Each item in the list
	// represent a book node in the REST service Response
	List<String> allBooks = jsonPathEvaluator.getList("books.title");

	// Iterate over the list and print individual book item
	for(String book : allBooks)
	{
		System.out.println("Book: " + book);
	}
}

DeSerialize JSON Array to List of Class (Type T) Object using JSONPath

@Test
public void JsonPathUsage() throws MalformedURLException
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/books/getallbooks";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("");

	// First get the JsonPath object instance from the Response interface
	JsonPath jsonPathEvaluator = response.jsonPath();

	// Read all the books as a List of String. Each item in the list
	// represent a book node in the REST service Response
	List<Book> allBooks = jsonPathEvaluator.getList("books", Book.class);

	// Iterate over the list and print individual book item
	// Note that every book entry in the list will be complete Json object of book
	for(Book book : allBooks)
	{
		System.out.println("Book: " + book.title);
	}
}

Deserialize JSON Array to an Array

@Test
public void JsonArrayToArray()
{

	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/books/getallbooks";
	RequestSpecification request = RestAssured.given();

	Response response = request.get();
	System.out.println("Response Body -> " + response.body().asString());

	// We can convert the Json Response directly into a Java Array by using
	// JsonPath.getObject method. Here we have to specify that we want to
	// deserialize the Json into an Array of Book. This can be done by specifying
	// Book[].class as the second argument to the getObject method.
	Book[] books = response.jsonPath().getObject("books",Book[].class );

	for(Book book : books)
	{
		System.out.println("Book title " + book.title);
	}
}

-----------------------------------------------------------------------------------------------------------------------------

My Notes:

Q: Sample Programs
import org.testng.annotations.Test;
import io.restassured.RestAssured;
import io.restassured.http.Method;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;

public class SimpleGetTest {

	@Test
	public void GetWeatherDetails()
	{   
		// Specify the base URL to the RESTful web service
		RestAssured.baseURI = "https://demoqa.com/utilities/weather/city";

		// Get the RequestSpecification of the request that you want to sent
		// to the server. The server is specified by the BaseURI that we have
		// specified in the above step.
		RequestSpecification httpRequest = RestAssured.given();

		// Make a request to the server by specifying the method Type and the method URL.
		// This will return the Response from the server. Store the response in a variable.
		Response response = httpRequest.request(Method.GET, "/Hyderabad");

		// Now let us print the body of the message to see what response
		// we have recieved from the server
		String responseBody = response.getBody().asString();
		System.out.println("Response Body is =>  " + responseBody);

	}

}


Q: Validate Response Status using Rest Assured

@Test
public void GetWeatherDetails()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// Get the status code from the Response. In case of 	
	// a successfull interaction with the web service, we
	// should get a status code of 200.
	int statusCode = response.getStatusCode();

	// Assert that correct status code is returned.
	Assert.assertEquals(statusCode /*actual value*/, 200 /*expected value*/, "Correct status code returned");
}

Q: Verify the Status Code returned by Weather web service on providing invalid City name.
@Test
public void GetWeatherDetailsInvalidCity()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/78789798798");
	int statusCode = response.getStatusCode();
	Assert.assertEquals(statusCode /*actual value*/, 200 /*expected value*/, "Correct status code returned");
}

Q: How to Validate Response Status Line?
@Test
public void GetWeatherStatusLine()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");
	
	// Get the status line from the Response and store it in a variable called statusLine
	String statusLine = response.getStatusLine();
	Assert.assertEquals(statusLine /*actual value*/, "HTTP/1.1 200 OK" /*expected value*/, "Correct status code returned");
}

Q: Validate Response Header using Rest Assured
@Test
public void GetWeatherHeaders()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// Reader header of a give name. In this line we will get
	// Header named Content-Type
	String contentType = response.header("Content-Type");
	Assert.assertEquals(contentType /* actual value */, "application/json" /* expected value */);

	// Reader header of a give name. In this line we will get
	// Header named Server
	String serverType =  response.header("Server");
	Assert.assertEquals(serverType /* actual value */, "nginx/1.12.1" /* expected value */);

	// Reader header of a give name. In this line we will get
	// Header named Content-Encoding
	String contentEncoding = response.header("Content-Encoding");
	Assert.assertEquals(contentEncoding /* actual value */, "gzip" /* expected value */);
}

Q: Read JSON Response Body using Rest Assured

Example 1:
@Test
public void WeatherMessageBody()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// Retrieve the body of the Response
	ResponseBody body = response.getBody();

	// By using the ResponseBody.asString() method, we can convert the  body
	// into the string representation.
	System.out.println("Response Body is: " + body.asString());
}

Q: How to Validate Response Body contains some String?
@Test
public void WeatherMessageBody()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// Retrieve the body of the Response
	ResponseBody body = response.getBody();

	// To check for sub string presence get the Response body as a String.
	// Do a String.contains
	String bodyAsString = body.asString();
	Assert.assertEquals(bodyAsString.contains("Hyderabad") /*Expected value*/, true /*Actual Value*/, "Response body contains Hyderabad");
}

Q: heck String presence by ignoring alphabet casing
@Test
public void WeatherMessageBody()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// Retrieve the body of the Response
	ResponseBody body = response.getBody();

	// To check for sub string presence get the Response body as a String.
	// Do a String.contains
	String bodyAsString = body.asString();

	// convert the body into lower case and then do a comparison to ignore casing.
	Assert.assertEquals(bodyAsString.toLowerCase().contains("hyderabad") /*Expected value*/, true /*Actual Value*/, "Response body contains Hyderabad");
}

Q: How to Extract a Node text from Response using JsonPath?

Sample JSON:
{
    "City": "Hyderabad",
    "Temperature": "25.51 Degree celsius",
    "Humidity": "94 Percent",
    "Weather Description": "mist",
    "Wind Speed": "1 Km per hour",
    "Wind Direction degree": " Degree"
}

@Test
public void VerifyCityInJsonResponse()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// First get the JsonPath object instance from the Response interface
	JsonPath jsonPathEvaluator = response.jsonPath();

	// Then simply query the JsonPath object to get a String value of the node
	// specified by JsonPath: City (Note: You should not put $. in the Java code)
	String city = jsonPathEvaluator.get("City");

	// Let us print the city variable to see what we got
	System.out.println("City received from Response " + city);

	// Validate the response
	Assert.assertEquals(city, "Hyderabad", "Correct city name received in the Response");

}

Q: Read all the nodes from Weather API Response

@Test
public void DisplayAllNodesInWeatherAPI()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// First get the JsonPath object instance from the Response interface
	JsonPath jsonPathEvaluator = response.jsonPath();

	// Let us print the city variable to see what we got
	System.out.println("City received from Response " + jsonPathEvaluator.get("City"));

	// Print the temperature node
	System.out.println("Temperature received from Response " + jsonPathEvaluator.get("Temperature"));

	// Print the humidity node
	System.out.println("Humidity received from Response " + jsonPathEvaluator.get("Humidity"));

	// Print weather description
	System.out.println("Weather description received from Response " + jsonPathEvaluator.get("Weather"));

	// Print Wind Speed
	System.out.println("City received from Response " + jsonPathEvaluator.get("WindSpeed"));

	// Print Wind Direction Degree
	System.out.println("City received from Response " + jsonPathEvaluator.get("WindDirectionDegree"));
}


Q: Query Parameters
import org.testng.Assert;
import org.testng.annotations.Test;
import io.restassured.RestAssured;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;

public class QUERYPARAMETERRequest {

	@Test
        public void queryParameter() {
			
		RestAssured.baseURI ="https://samples.openweathermap.org/data/2.5/"; 
		RequestSpecification request = RestAssured.given();
		
		Response response = request.queryParam("q", "London,UK") 
				                   .queryParam("appid", "2b1fd2d7f77ccf1b7de9b441571b39b8") 
				                   .get("/weather");
		
		String jsonString = response.asString();
		System.out.println(response.getStatusCode()); 
		Assert.assertEquals(jsonString.contains("London"), true);
		
	}
}


Q: Sample Post Program
@Test
public void RegistrationSuccessful()
{		
	RestAssured.baseURI ="https://restapi.demoqa.com/customer";
	RequestSpecification request = RestAssured.given();

	JSONObject requestParams = new JSONObject();
	requestParams.put("FirstName", "Virender"); // Cast
	requestParams.put("LastName", "Singh");
	requestParams.put("UserName", "sdimpleuser2dd2011");
	requestParams.put("Password", "password1");

	requestParams.put("Email",  "sample2ee26d9@gmail.com");
	request.body(requestParams.toJSONString());
	Response response = request.post("/register");

	int statusCode = response.getStatusCode();
	Assert.assertEquals(statusCode, "201");
	String successCode = response.jsonPath().get("SuccessCode");
	Assert.assertEquals( "Correct Success code was returned", successCode, "OPERATION_SUCCESS");
}

Q: Serialization and Deserialization in Java

public class Rectangle implements Serializable{

	private double height;
	private double width;

	public Rectangle(double height, double width)
	{
		this.height = height;
		this.width = width;
	}

	public double Area()
	{
		return height * width;
	}

	public double Perimeter()
	{
		return 2 * (height + width);
	}
}


public static void SerializeToFile(Object classObject, String fileName)
{
	try {

		// Step 1: Open a file output stream to create a file object on disk.
		// This file object will be used to write the serialized bytes of an object
		FileOutputStream fileStream = new FileOutputStream(fileName);

		// Step 2: Create a ObjectOutputStream, this class takes a files stream.
		// This class is responsible for converting the Object of any type into
		// a byte stream
		ObjectOutputStream objectStream = new ObjectOutputStream(fileStream);

		// Step 3: ObjectOutputStream.writeObject method takes an Object and 
		// converts it into a ByteStream. Then it writes the Byte stream into
		// the file using the File stream that we created in step 1.
		objectStream.writeObject(classObject);

		// Step 4: Gracefully close the streams
		objectStream.close();
		fileStream.close();

	} catch (FileNotFoundException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	} catch (IOException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}
}

public static void main(String[] args)
{
	Rectangle rect = new Rectangle(18, 78);
	SerializeToFile(rect, "rectSerialized");

}


public static Object DeSerializeFromFileToObject(String fileName)
{
	try {

		// Step 1: Create a file input stream to read the serialized content
		// of rectangle class from the file
		FileInputStream fileStream = new FileInputStream(new File(fileName));

		// Step 2: Create an object stream from the file stream. So that the content
		// of the file is converted to the Rectangle Object instance
		ObjectInputStream objectStream = new ObjectInputStream(fileStream);

		// Step 3: Read the content of the stream and convert it into object
		Object deserializeObject = objectStream.readObject();

		// Step 4: Close all the resources
		objectStream.close();
		fileStream.close();

		// return the deserialized object
		return deserializeObject;

	} catch (FileNotFoundException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	} catch (IOException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	} catch (ClassNotFoundException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}
	return null;
}

public static void main(String[] args)
{
	Rectangle rect = new Rectangle(18, 78);
	SerializeToFile(rect, "rectSerialized");

	Rectangle deSerializedRect = (Rectangle) DeSerializeFromFileToObject("rectSerialized");
	System.out.println("Rect area is " + deSerializedRect.Area());
}

Q: Deserialize Json Response

Sample JSON Response:
{
    "SuccessCode": "OPERATION_SUCCESS",
    "Message": "Operation completed successfully"
}

public class RegistrationSuccessResponse {

	// Variable where value of SuccessCode node
	// will be copied
	// Note: The name should be exactly as the node name is 
	// present in the Json
	public String SuccessCode;

	// Variable where value of Message node will
	// be copied
	// Note: The name should be exactly as the node name is 
	// present in the Json
	public String Message;
}

@Test
public void RegistrationSuccessful()
{		
	RestAssured.baseURI ="https://restapi.demoqa.com/customer";
	RequestSpecification request = RestAssured.given();

	JSONObject requestParams = new JSONObject();
	requestParams.put("FirstName", "Virender"); // Cast
	requestParams.put("LastName", "Singh");
	requestParams.put("UserName", "63userf2d3d2011");
	requestParams.put("Password", "password1");	
	requestParams.put("Email",  "ed26dff39@gmail.com");

	request.body(requestParams.toJSONString());
	Response response = request.post("/register");

	ResponseBody body = response.getBody();

	// Deserialize the Response body into RegistrationSuccessResponse
	RegistrationSuccessResponse responseBody = body.as(RegistrationSuccessResponse.class);

	// Use the RegistrationSuccessResponse class instance to Assert the values of 
	// Response.
	Assert.assertEquals("OPERATION_SUCCESS", responseBody.SuccessCode);
	Assert.assertEquals("Operation completed successfully", responseBody.Message);
}

Q: How to Deserialize JSON Response Body based on Response Status?

Sample JSON Response:
{
    "FaultId": "User already exists",
    "fault": "FAULT_USER_ALREADY_EXISTS"
}

public class RegistrationFailureResponse {

	String FaultId;
	String fault;
}

@Test
public void RegistrationSuccessful()
{		
	RestAssured.baseURI ="https://restapi.demoqa.com/customer";
	RequestSpecification request = RestAssured.given();

	JSONObject requestParams = new JSONObject();
	requestParams.put("FirstName", "Virender"); // Cast
	requestParams.put("LastName", "Singh");
	requestParams.put("UserName", "63userf2d3d2011");
	requestParams.put("Password", "password1");	
	requestParams.put("Email",  "ed26dff39@gmail.com");

	request.body(requestParams.toJSONString());
	Response response = request.post("/register");

	ResponseBody body = response.getBody();
	System.out.println(response.body().asString());

	if(response.statusCode() == 200)
	{
		// Deserialize the Response body into RegistrationFailureResponse
		RegistrationFailureResponse responseBody = body.as(RegistrationFailureResponse.class);

		// Use the RegistrationFailureResponse class instance to Assert the values of 
		// Response.
		Assert.assertEquals("User already exists", responseBody.FaultId);
		Assert.assertEquals("FAULT_USER_ALREADY_EXISTS", responseBody.fault);	
	}
	else if (response.statusCode() == 201)
	{
		// Deserialize the Response body into RegistrationSuccessResponse
		RegistrationSuccessResponse responseBody = body.as(RegistrationSuccessResponse.class);
		// Use the RegistrationSuccessResponse class instance to Assert the values of 
		// Response.
		Assert.assertEquals("OPERATION_SUCCESS", responseBody.SuccessCode);
		Assert.assertEquals("Operation completed successfully", responseBody.Message);	
	}	
}

Q: How to make a PUT Request using Rest Assured?
import org.testng.Assert;
import org.testng.annotations.Test;
import io.restassured.RestAssured;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;
import net.minidev.json.JSONObject;


public void UpdateRecords(){
		int empid = 15410;

		RestAssured.baseURI ="https://dummy.restapiexample.com/api/v1";
		RequestSpecification request = RestAssured.given();
		
		JSONObject requestParams = new JSONObject();
		requestParams.put("name", "Zion"); // Cast
		requestParams.put("age", 23);
		requestParams.put("salary", 12000);

		request.body(requestParams.toJSONString());
		Response response = request.put("/update/"+ empid);

		int statusCode = response.getStatusCode();
		System.out.println(response.asString());
		Assert.assertEquals(statusCode, 200); 

	}
	
Q: How to make a DELETE Request using Rest Assured?

import org.testng.Assert;
import org.testng.annotations.Test;

import io.restassured.RestAssured;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;

@Test
public void deleteEmpRecord() {
		
	int empid = 15410;
		
	RestAssured.baseURI = "https://dummy.restapiexample.com/api/v1";
	RequestSpecification request = RestAssured.given();	
		
	// Add a header stating the Request body is a JSON
	request.header("Content-Type", "application/json");	
	
       // Delete the request and check the response
	Response response = request.delete("/delete/"+ empid);		
		
	int statusCode = response.getStatusCode();
	System.out.println(response.asString());
	Assert.assertEquals(statusCode, 200);
		
	String jsonString =response.asString();
	Assert.assertEquals(jsonString.contains("successfully! deleted Records"), true);
	}	

Q: DeSerialize JSON Array to List
@Test
public void JsonPathUsage() throws MalformedURLException
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/books/getallbooks";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("");

	// First get the JsonPath object instance from the Response interface
	JsonPath jsonPathEvaluator = response.jsonPath();

	// Read all the books as a List of String. Each item in the list
	// represent a book node in the REST service Response
	List<String> allBooks = jsonPathEvaluator.getList("books.title");

	// Iterate over the list and print individual book item
	for(String book : allBooks)
	{
		System.out.println("Book: " + book);
	}
}

Q: DeSerialize JSON Array to List of Class (Type T) Object using JSONPath

public class Book {

    String isbn;
    String title;
    String subtitle;
    String author;
    String published;
    String publisher;
    int pages;
    String description;
    String website;
}

@Test
public void JsonPathUsage() throws MalformedURLException
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/books/getallbooks";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("");

	// First get the JsonPath object instance from the Response interface
	JsonPath jsonPathEvaluator = response.jsonPath();

	// Read all the books as a List of String. Each item in the list
	// represent a book node in the REST service Response
	List<Book> allBooks = jsonPathEvaluator.getList("books", Book.class);

	// Iterate over the list and print individual book item
	// Note that every book entry in the list will be complete Json object of book
	for(Book book : allBooks)
	{
		System.out.println("Book: " + book.title);
	}
}

Q: Deserialize JSON Array to an Array
@Test
public void JsonArrayToArray()
{

	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/books/getallbooks";
	RequestSpecification request = RestAssured.given();

	Response response = request.get();
	System.out.println("Response Body -> " + response.body().asString());

	// We can convert the Json Response directly into a Java Array by using
	// JsonPath.getObject method. Here we have to specify that we want to
	// deserialize the Json into an Array of Book. This can be done by specifying
	// Book[].class as the second argument to the getObject method.
	Book[] books = response.jsonPath().getObject("books",Book[].class );

	for(Book book : books)
	{
		System.out.println("Book title " + book.title);
	}
}

-------------------------------------------------------------------------------------------------------------------------------------------------------------


Schema and DTD validation
XML response bodies can also be verified against an XML Schema (XSD) or DTD.

XSD example:

expect().body(matchesXsd(xsd)).when().get("/carRecords");
DTD example:
 expect().body(matchesDtd(dtd)).when().get("/videos");
 
 
 
You can also change the default base URI, base path, port and authentication scheme for all subsequent requests:

RestAssured.baseURI = "http://myhost.org";
RestAssured.port = 80;
RestAssured.basePath = "/resource";
RestAssured.authentication = basic("username", "password");
RestAssured.rootPath = "x.y.z"; 

RestAssured.filters(..); // List of default filters
RestAssured.requestContentType(..); // Specify the default request content type
RestAssured.responseContentType(..); // Specify the default response content type
RestAssured.requestSpecification = .. // Default request specification
RestAssured.responseSpecification = .. // Default response specification
RestAssured.urlEncodingEnabled = .. // Specify if Rest Assured should URL encoding the parameters
RestAssured.defaultParser = .. // Specify a default parser for response bodies if no registered parser can handle data of the response content-type
RestAssured.registerParser(..) // Specify a parser for the given content-type
RestAssured.unregisterParser(..) // Unregister a parser for the given content-type


Request Logging
	given().log().all(). .. // Log all request specification details including parameters, headers and body
	given().log().params(). .. // Log only the parameters of the request
	given().log().body(). .. // Log only the request body
	given().log().headers(). .. // Log only the request headers
	given().log().cookies(). .. // Log only the request cookies
	
	
Response Logging	
expect().log().body() ..
expect().log().ifError(). ..
expect().log().all(). .. 
expect().log().statusLine(). .. // Only log the status line
expect().log().headers(). .. // Only log the response headers
expect().log().cookies(). .. // Only log the response cookies
expect().log().ifStatusCodeIsEqualTo(302). .. // Only log if the status code is equal to 302
expect().log().ifStatusCodeMatches(matcher). .. // Only log if the status code matches the supplied Hamcrest matcher


Root path
	To avoid duplicated paths in body expectations you can specify a root path. E.g. instead of writing:

	expect().
			 body("x.y.firstName", is(..)).
			 body("x.y.lastName", is(..)).
			 body("x.y.age", is(..)).
			 body("x.y.gender", is(..)).
	when().
			 get("/something");
			 
	You can also set a default root path using:

	RestAssured.rootPath = "x.y";
	
	
Session Filter	

	SessionFilter sessionFilter = new SessionFilter();

	given().
			  auth().form("John", "Doe").
			  filter(sessionFilter).
	expect().
			  statusCode(200).
	when().
			  get("/formAuth");

	given().
			  filter(sessionFilter). // Reuse the same session filter instance to automatically apply the session id from the previous response
	expect().
			  statusCode(200).
	when().
			  get("/x");
			  
	To get session id caught by the SessionFilter you can do like this:

	String sessionId = sessionFilter.getSessionId();		  


----------------------------------------------------------------------------------------------------------------------------------------------------

GET Request In REST Assured

import org.hamcrest.Matchers;
import org.testng.annotations.Test;
 
import io.restassured.RestAssured;
 
public class GetBookingIds_RestfulBookerUsingBDDStyle {
 
	
	@Test
	public void GetBookingIds_VerifyStatusCode() {
		
		// Given
		RestAssured.given()
			.baseUri("https://restful-booker.herokuapp.com")
		// When
		.when()
			.get("/booking")
		// Then
		.then()
			.statusCode(200)
			.statusLine("HTTP/1.1 200 OK")
			// To verify booking count
			.body("bookingid", Matchers.hasSize(10))
			// To verify booking id at index 3
			.body("bookingid[3]", Matchers.equalTo(1));			
		
 
	}
 
}


POST Request In REST Assured

import org.hamcrest.Matchers;
 
import io.restassured.RestAssured;
import io.restassured.http.ContentType;
 
public class BDDStylePostRequest {
	
	
	public static void main(String[] args) {
		
		// There is no need to add escape character manually. Just paste string within double 
		// quotes. It will automatically add escape sequence as required. 
		String jsonString = "{\"username\" : \"admin\",\"password\" : \"password123\"}";
		
		
		//GIVEN
		RestAssured
		.given()
			.baseUri("https://restful-booker.herokuapp.com/auth")
			.contentType(ContentType.JSON)
			.body(jsonString)
		// WHEN
		.when()
			.post()
		// THEN
		.then()
			.assertThat()
			.statusCode(200)
			.body("token", Matchers.notNullValue())
			.body("token.length()", Matchers.is(15))
			.body("token", Matchers.matchesRegex("^[a-z0-9]+$"));
		
	
	}
 
}


PUT Request In REST Assured

import org.hamcrest.Matchers;
 
import io.restassured.RestAssured;
import io.restassured.http.ContentType;
import io.restassured.response.Response;
import io.restassured.response.ValidatableResponse;
import io.restassured.specification.RequestSpecification;
 
public class BDDStylePutRequest {
 
	public static void main(String[] args) {
 
		// There is no need to add escape character manually. Just paste string within
		// double
		// quotes. It will automatically add escape sequence as required.
		String jsonString = "{\r\n" + "    \"firstname\" : \"Amod\",\r\n" + "    \"lastname\" : \"Mahajan\",\r\n"
				+ "    \"totalprice\" : 111,\r\n" + "    \"depositpaid\" : true,\r\n" + "    \"bookingdates\" : {\r\n"
				+ "        \"checkin\" : \"2018-01-01\",\r\n" + "        \"checkout\" : \"2019-01-01\"\r\n"
				+ "    },\r\n" + "    \"additionalneeds\" : \"Breakfast\"\r\n" + "}";
 
		//GIVEN
		RestAssured
			.given()
					.baseUri("https://restful-booker.herokuapp.com/booking/1")
					.cookie("token", "e88375c0fde687a")
					.contentType(ContentType.JSON)
					.body(jsonString)
			// WHEN
			.when()
					.put()
			// THEN
			.then()
					.assertThat()
					.statusCode(200)
					.body("firstname", Matchers.equalTo("Amod"))
					.body("lastname", Matchers.equalTo("Mahajan"));
 
	}
}



PATCH Request In REST Assured

import org.hamcrest.Matchers;
 
import io.restassured.RestAssured;
import io.restassured.http.ContentType;
import io.restassured.response.Response;
import io.restassured.response.ValidatableResponse;
import io.restassured.specification.RequestSpecification;
 
public class BDDStylePatchRequest {
 
	public static void main(String[] args) {
		
 
		// There is no need to add escape character manually. Just paste string within
		// double
		// quotes. It will automatically add escape sequence as required.
		String jsonString = "{\r\n" + 
				"    \"firstname\" : \"Amod\",\r\n" + 
				"    \"lastname\" : \"Mahajan\"}";
 
		//GIVEN
		RestAssured
			.given()
					.baseUri("https://restful-booker.herokuapp.com/booking/1")
					.cookie("token", "6608dc75eedd44f")
					.contentType(ContentType.JSON)
					.body(jsonString)
			// WHEN
			.when()
					.patch()
			// THEN
			.then()
					.assertThat()
					.statusCode(200)
					.body("firstname", Matchers.equalTo("Amod"))
					.body("lastname", Matchers.equalTo("Mahajan"));
 
	}
}


DELETE Request In REST Assured

import io.restassured.RestAssured;
 
public class BDDStyleDeleteRequest {
 
	public static void main(String[] args) {
		
 
		// Delete Booking
 
		//GIVEN
		RestAssured
			.given()
					.baseUri("https://restful-booker.herokuapp.com/booking/1")
					.cookie("token", "f7dddb1093eab19")
			// WHEN
			.when()
					.delete()
			// THEN
			.then()
					.assertThat()
					.statusCode(201);
		
		// Verifying booking is deleted
		// Given
		RestAssured
		    .given()
				    .baseUri("https://restful-booker.herokuapp.com/booking/1")
		// When
			.when()
					.get()
		// Then
			.then()
					.statusCode(404);
 
	}
}



Writing response into a JSON file

mport java.io.File;
import java.io.IOException;
import java.io.InputStream;
import com.google.common.io.Files;
 
import io.restassured.RestAssured;
import io.restassured.http.ContentType;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;
 
public class WriteResponseInTextFile {
 
	public static void main(String[] args) throws IOException {
 
		// There is no need to add escape character manually. Just paste string within
		// double
		// quotes. It will automatically add escape sequence as required.
		String jsonString = "{\"username\" : \"admin\",\"password\" : \"password123\"}";
 
		// Create a request specification
		RequestSpecification request = RestAssured.given();
 
		// Setting content type to specify format in which request payload will be sent.
		// ContentType is an ENUM.
		request.contentType(ContentType.JSON);
		// Adding URI
		request.baseUri("https://restful-booker.herokuapp.com/auth");
		// Adding body as string
		request.body(jsonString);
 
		// Calling POST method on URI. After hitting we get Response
		Response response = request.post();
 
		// Getting response as a string and writing in to a file
		String responseAsString = response.asString();
		// Converting in to byte array before writing
		byte[] responseAsStringByte = responseAsString.getBytes();
		// Creating a target file
		File targetFileForString = new File("src/main/resources/targetFileForString.json");
		// Writing into files
		Files.write(responseAsStringByte, targetFileForString);
 
		// Getting response as input stream and writing in to a file
		InputStream responseAsInputStream = response.asInputStream();
		// Creating a byte array with number of bytes of input stream (available()
		// method)
		byte[] responseAsInputStreamByte = new byte[responseAsInputStream.available()];
		// Reads number of bytes from the input stream and stores them into the byte
		// array responseAsInputStreamByte.
		responseAsInputStream.read(responseAsInputStreamByte);
		// Creating a target file
		File targetFileForInputStream = new File("src/main/resources/targetFileForInputStream.json");
		// Writing into files
		Files.write(responseAsInputStreamByte, targetFileForInputStream);
 
		// Directly getting a byte array
		byte[] responseAsByteArray = response.asByteArray();
		// Creating a target file
		File targetFileForByteArray = new File("src/main/resources/targetFileForByteArray.json");
		// Writing into files
		Files.write(responseAsByteArray, targetFileForByteArray);
 
	}
 
}


Building RequestSpecification Using RequestSpecBuilder

import io.restassured.RestAssured;
import io.restassured.builder.RequestSpecBuilder;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;
 
public class RequestSpecBuilderExample {
 
	public static void main(String[] args) {
 
		// Creating an object of RequestSpecBuilder
		RequestSpecBuilder reqBuilder = new RequestSpecBuilder();
		// Setting Base URI
		reqBuilder.setBaseUri("https://restful-booker.herokuapp.com");
		// Setting Base Path
		reqBuilder.setBasePath("/booking");
		// Getting RequestSpecification reference using builder() method
		RequestSpecification reqSpec = reqBuilder.build();
		
		// Usage in different styles
		// We can directly call http verbs on RequestSpecification
		Response res1= reqSpec.get();
		System.out.println(res1.asString());
		System.out.println("======================");
		
		// We can also pass RequestSpecification reference variable in overloaded given() method
		Response res2 = RestAssured.given(reqSpec).get();
		System.out.println(res2.asString());
		System.out.println("======================");
				
		// We can also pass RequestSpecification using spec() method
		Response res3 = RestAssured.given().spec(reqSpec).get();
		System.out.println(res3.asString());
 
	}
}



Setting A Default RequestSpecification In Rest Assured

import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;
 
import io.restassured.RestAssured;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;
 
public class DefaultRequestSpecificationExample {
 
	@BeforeClass
	public void setupDefaultRequestSpecification()
	{
		// Creating request specification using given()
		RequestSpecification request1= RestAssured.given();
		// Setting Base URI
		request1.baseUri("https://restful-booker.herokuapp.com");
		// Setting Base Path
		request1.basePath("/booking");
		
		RestAssured.requestSpecification = request1;
	}
	
	
	@Test
	public void useDefaultRequestSpecification()
	{
		// It will use default RequestSpecification
		Response res = RestAssured.when().get();
		System.out.println("Response for default: "+res.asString());
	}
	
	@Test
	public void overrideDefaultRequestSpecification()
	{
		// Creating request specification using with()
		RequestSpecification request2= RestAssured.with();
		// Setting Base URI
		request2.baseUri("https://restful-booker.herokuapp.com");
		// Setting Base Path
		request2.basePath("/ping");
		// Overriding default request specification
		Response res = RestAssured.given().spec(request2).get();
		System.out.println("Response for overriding: "+res.asString());
	}
}


Querying RequestSpecification In Rest Assured

import io.restassured.RestAssured;
import io.restassured.http.Header;
import io.restassured.http.Headers;
import io.restassured.specification.QueryableRequestSpecification;
import io.restassured.specification.RequestSpecification;
import io.restassured.specification.SpecificationQuerier;
 
public class QueryingRequestSpecificationExample {
	
	public static void main(String[] args) {
		
		// I am adding dummy request details for example
		String JsonBody = "{\"firstName\":\"Amod\"}";
		
		// Creating request specification using given()
		RequestSpecification request1= RestAssured.given();
		// Setting Base URI
		request1.baseUri("https://restful-booker.herokuapp.com")
		// Setting Base Path
		.basePath("/booking")
		.body(JsonBody)
		.header("header1", "headerValue1")
		.header("header2", "headerValue2");
		
		// Querying RequestSpecification
		// Use query() method of SpecificationQuerier class to query 
		QueryableRequestSpecification queryRequest = SpecificationQuerier.query(request1);
		
		// get base URI
		String retrieveURI = queryRequest.getBaseUri();
		System.out.println("Base URI is : "+retrieveURI);
		
		// get base Path
		String retrievePath = queryRequest.getBasePath();
		System.out.println("Base PATH is : "+retrievePath);
		
		// get Body
		String retrieveBody = queryRequest.getBody();
		System.out.println("Body is : "+retrieveBody);
		
		// Get Headers
		Headers allHeaders = queryRequest.getHeaders();
		System.out.println("Printing all headers: ");
		for(Header h : allHeaders)
		{
			System.out.println("Header name : "+ h.getName()+" Header value is : "+h.getValue());
		}
	}
 
}


Default Host And Port In Rest Assured

RestAssured
	.given()
		.baseUri("https://restful-booker.herokuapp.com")
		.basePath("/ping")
		.port(8181)
	.when()
		.get();
		
		
How To Send A JSON File As Payload To Request	

import java.io.File;
 
import org.hamcrest.Matchers;
import org.testng.annotations.Test;
 
import io.restassured.RestAssured;
import io.restassured.http.ContentType;
 
public class PassFileAsPayload {
	
	@Test
	public void passFileAsPayload()
	{
		// Creating a File instance 
		File jsonDataInFile = new File("src/test/resources/Payloads/AuthPayload.json");
		
		//GIVEN
		RestAssured
		    .given()
				.baseUri("https://restful-booker.herokuapp.com/auth")
				.contentType(ContentType.JSON)
				.body(jsonDataInFile)
		// WHEN
			.when()
				.post()
				// THEN
			.then()
				.assertThat()
				.statusCode(200)
				.body("token", Matchers.notNullValue())
				.body("token.length()", Matchers.is(15))
				.body("token", Matchers.matchesRegex("^[a-z0-9]+$"));
	}
 
}



How To Send A XML File As Payload To Request	

@Test
	public void passXMLFileAsPayload()
	{
		// Creating a File instance 
		File jsonDataInFile = new File("src/test/resources/Payloads/AuthPayload.xml");
		
		//GIVEN
		RestAssured
		    .given()
				.baseUri("https://restful-booker.herokuapp.com/auth")
				// Since I am passing payload as xml. Anyway it is optional as Rest Assured automatically identifies.
				.contentType(ContentType.XML)
				.body(jsonDataInFile)
		// WHEN
			.when()
				.post()
				// THEN
			.then()
				.assertThat()
				.statusCode(200);
	}
	
	


