# REST-Assured-Code-Snippet


# REST API Testing – Basics

How to validate HTTP response status code

```java

import static org.junit.Assert.*;
import org.testng.Assert;   
import org.testng.annotations.Test;
import io.restassured.RestAssured;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;

public class RestAssuredTestResponse {
	@Test
	public void GetBookDetails(){  
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

```

How to validate the HTTP error status code?

```java

import static org.junit.Assert.*;
import org.testng.Assert;   //used to validate response status 
import org.testng.annotations.Test;
import io.restassured.RestAssured;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;

public class RestAssuredTestResponse {

	@Test
	public void GetPetDetails()	{  
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

```

How to validate the response status line?

```java

@Test
public void GetBookDetails(){  
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

```

How to access and read HTTP Response headers using REST Assured?

```java

@Test 
public void IteratingHeaders(){ 
	RestAssured.baseURI = "https://demoqa.com/BookStore/v1/Books"; 
	RequestSpecification httpRequest = RestAssured.given(); 
	Response response = httpRequest.get(""); 
	// Get all the headers and then iterate over allHeaders to print each header 
	Headers allHeaders = response.headers(); 
	// Iterate over all the Headers 
 	for(Header header : allHeaders) { 
 		System.out.println("Key: " + header.getName() + " Value: " + header.getValue()); 
 	} 
}

```

Let us demonstrate the .header(String arg0) method to get a particular header. Here we pass the exact header name as an argument.

```java

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

```

How to validate HTTP Response Header using Rest Assured?

```java

@Test
public void ValidateBookHeaders(){ 
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
 
 ```
 
 
 Read JSON Response Body using Rest Assured
 
 ```java
 
 @Test
public void WeatherMessageBody(){
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/Hyderabad");

	// Retrieve the body of the Response
	ResponseBody body = response.getBody();

	// By using the ResponseBody.asString() method, we can convert the  body
	// into the string representation.
	System.out.println("Response Body is: " + body.asString());
}

```

How to Validate Response Body contains some String?

```java

@Test
public void WeatherMessageBody(){
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

```

Check String presence by ignoring alphabet casing

```java

@Test
public void WeatherMessageBody(){
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

```

How to Extract a Node text from Response using JsonPath?

```java

@Test
public void VerifyCityInJsonResponse(){
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

```

Sample Code to read all the nodes from Weather API Response

```java

@Test
public void DisplayAllNodesInWeatherAPI(){
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

```
------------------------------------------------------------------------------------------------------------------------------------------------



# REST API Testing – Advance


Changing the HTTP Method on a POST Request using Rest Assured

```java
@Test
public void UserRegistrationSuccessful(){ 
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

```


Basic Authentication Flow

```java

@Test
public void AuthenticationBasics(){
	RestAssured.baseURI = "https://restapi.demoqa.com/authentication/CheckForAuthentication";
	RequestSpecification request = RestAssured.given();

	Response response = request.get();
	System.out.println("Status code: " + response.getStatusCode());
	System.out.println("Status message " + response.body().asString());
}

```

How to implement a PUT Request using Rest Assured?

```java

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

```

How to make a Delete Request using Rest Assured?

```java

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
    
```

# JSON Manipulations

DeSerialize JSON Array to List

```java

@Test
public void JsonPathUsage() throws MalformedURLException{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/books/getallbooks";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("");

	// First get the JsonPath object instance from the Response interface
	JsonPath jsonPathEvaluator = response.jsonPath();

	// Read all the books as a List of String. Each item in the list
	// represent a book node in the REST service Response
	List<String> allBooks = jsonPathEvaluator.getList("books.title");

	// Iterate over the list and print individual book item
	for(String book : allBooks){
		System.out.println("Book: " + book);
	}
}

```

DeSerialize JSON Array to List of Class (Type T) Object using JSONPath


```java

@Test
public void JsonPathUsage() throws MalformedURLException{
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
	for(Book book : allBooks){
		System.out.println("Book: " + book.title);
	}
}

```

Deserialize JSON Array to an Array


```java

@Test
public void JsonArrayToArray(){

	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/books/getallbooks";
	RequestSpecification request = RestAssured.given();

	Response response = request.get();
	System.out.println("Response Body -> " + response.body().asString());

	// We can convert the Json Response directly into a Java Array by using
	// JsonPath.getObject method. Here we have to specify that we want to
	// deserialize the Json into an Array of Book. This can be done by specifying
	// Book[].class as the second argument to the getObject method.
	Book[] books = response.jsonPath().getObject("books",Book[].class );

	for(Book book : books){
		System.out.println("Book title " + book.title);
	}
}

```

---------------------------------------------------------------------------------------------------------------------------------------------------

# Abhijit Notes Program:

Q: Sample Programs

```java 

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

```

Q: Validate Response Status using Rest Assured

```java 

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

```

Q: Verify the Status Code returned by Weather web service on providing invalid City name.

```java 

@Test
public void GetWeatherDetailsInvalidCity()
{
	RestAssured.baseURI = "https://restapi.demoqa.com/utilities/weather/city";
	RequestSpecification httpRequest = RestAssured.given();
	Response response = httpRequest.get("/78789798798");
	int statusCode = response.getStatusCode();
	Assert.assertEquals(statusCode /*actual value*/, 200 /*expected value*/, "Correct status code returned");
}

```

Q: How to Validate Response Status Line?

```java 

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

```

Q: Validate Response Header using Rest Assured

```java 

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

```

Q: Read JSON Response Body using Rest Assured

```java 

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

```

Q: How to Validate Response Body contains some String?

```java 

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

```

Q: heck String presence by ignoring alphabet casing

```java 

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

```

Q: How to Extract a Node text from Response using JsonPath?

```java 

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

```

Q: Read all the nodes from Weather API Response

```java 

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

```

Q: Query Parameters

```java 

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

```

Q: Sample Post Program

```java 

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

```

Q: Serialization and Deserialization in Java

```java 

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


public static void SerializeToFile(Object classObject, String fileName){
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

public static void main(String[] args){
	Rectangle rect = new Rectangle(18, 78);
	SerializeToFile(rect, "rectSerialized");

}


public static Object DeSerializeFromFileToObject(String fileName){
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

public static void main(String[] args){
	Rectangle rect = new Rectangle(18, 78);
	SerializeToFile(rect, "rectSerialized");

	Rectangle deSerializedRect = (Rectangle) DeSerializeFromFileToObject("rectSerialized");
	System.out.println("Rect area is " + deSerializedRect.Area());
}

```

Q: Deserialize Json Response

```java 

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

```

Q: How to Deserialize JSON Response Body based on Response Status?

```java 

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

```

Q: How to make a PUT Request using Rest Assured?

```java 

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

```

Q: How to make a DELETE Request using Rest Assured?

```java 

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

```

Q: DeSerialize JSON Array to List

```java 

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

```

Q: DeSerialize JSON Array to List of Class (Type T) Object using JSONPath

```java 

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

```

Q: Deserialize JSON Array to an Array

```java 

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

```

-------------------------------------------------------------------------------------------------------------------------------------------------------------

# Notes From REST Assured Wiki Docs

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
	
	



Get & Assert Response Time Of Request

import java.util.concurrent.TimeUnit;
 
import org.hamcrest.Matchers;
import org.testng.annotations.Test;
 
import io.restassured.RestAssured;
import io.restassured.http.ContentType;
import io.restassured.response.Response;
import io.restassured.response.ValidatableResponse;
import io.restassured.specification.RequestSpecification;
 
public class MeasuringResponseTimeInRestAssured {
	
	@Test
	public void mesaureResponseTimeUsingResponseOptionsMethods()
	{
		// There is no need to add escape character manually. Just paste string within double 
		// quotes. It will automatically add escape sequence as required. 
		String jsonString = "{\"username\" : \"admin\",\"password\" : \"password123\"}";
		
		// Create a request specification 
		RequestSpecification request= RestAssured.given();
		
		// Setting content type to specify format in which request payload will be sent.
		// ContentType is an ENUM. 
		request.contentType(ContentType.JSON);
		//Adding URI
		request.baseUri("https://restful-booker.herokuapp.com/auth");
		// Adding body as string
		request.body(jsonString);
		
		// Calling POST method on URI. After hitting we get Response
		Response response = request.post();
		
		// By default response time is given in milliseconds
		long responseTime1 = response.getTime();
		System.out.println("Response time in ms using getTime():"+responseTime1);
		
		// we can get response time in other format as well
		long responseTimeInSeconds = response.getTimeIn(TimeUnit.SECONDS);
		System.out.println("Response time in seconds using getTimeIn():"+responseTimeInSeconds);
		
		
		// Similar methods 
		long responseTime2 = response.time();
		System.out.println("Response time in ms using time():"+responseTime2);
		
		long responseTimeInSeconds1 = response.timeIn(TimeUnit.SECONDS);
		System.out.println("Response time in seconds using timeIn():"+responseTimeInSeconds1);
		
	}
 
	@Test
	public void mesaureResponseTimeUsingValidatableResponseOptionsMethods() {
		
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
 
		// Getting ValidatableResponse type
		ValidatableResponse valRes = response.then();
		// Asserting response time is less than 2000 milliseconds
		// L just represent long. It is in millisecond by default.
		valRes.time(Matchers.lessThan(2000L));
		// Asserting response time is greater than 2000 milliseconds
		valRes.time(Matchers.greaterThan(2000L));
		// Asserting response time in between some values
		valRes.time(Matchers.both(Matchers.greaterThanOrEqualTo(2000L)).and(Matchers.lessThanOrEqualTo(1000L)));
 
		// If we want to assert in different time units
		valRes.time(Matchers.lessThan(2L), TimeUnit.SECONDS);
		
				
				
	}
	
}

Creating JSON Object Request Body Using Java Map

import java.util.HashMap;
import java.util.Map;
 
import org.testng.annotations.Test;
 
import io.restassured.RestAssured;
import io.restassured.http.ContentType;
 
public class CreatingNestedJsonObject {
	
	@Test
	public void CreatingNestedJsonObjectTest()
	{
		Map<String,Object> jsonBodyUsingMap = new HashMap<String,Object>();
		jsonBodyUsingMap.put("firstname", "Jim");
		jsonBodyUsingMap.put("lastname", "Brown");
		jsonBodyUsingMap.put("totalprice", 111);
		jsonBodyUsingMap.put("depositpaid", true);
		
		Map<String,String> bookingDatesMap = new HashMap<>();
		bookingDatesMap.put("checkin", "2021-07-01");
		bookingDatesMap.put("checkout", "2021-07-01");
		
		jsonBodyUsingMap.put("bookingdates", bookingDatesMap);
		jsonBodyUsingMap.put("additionalneeds", "Breakfast");
		
		
		//GIVEN
		RestAssured
		   .given()
			  .baseUri("https://restful-booker.herokuapp.com/booking")
			  .contentType(ContentType.JSON)
			  .body(jsonBodyUsingMap)
			  .log()
			  .all()
		// WHEN
		   .when()
			   .post()
		// THEN
		   .then()
			   .assertThat()
			   .statusCode(200)
			   .log()
			   .all();
	}
 
}


Creating JSON Array Request Body Using List

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
 
import org.testng.annotations.Test;
 
import io.restassured.RestAssured;
import io.restassured.http.ContentType;
 
public class CreatingNestedJsonArray {
	
	@Test
	public void CreatingNestedJsonObjectTest()
	{
		
		// JSON Object for first guest
		Map<String,Object> bookingOne = new HashMap<String,Object>();
		bookingOne.put("firstname", "Amod");
		bookingOne.put("lastname", "Mahajan");
		bookingOne.put("totalprice", 222);
		bookingOne.put("depositpaid", true);
		
		Map<String,String> bookingDatesMapForAmod = new HashMap<>();
		bookingDatesMapForAmod.put("checkin", "2021-08-01");
		bookingDatesMapForAmod.put("checkout", "2021-08-02");
		
		bookingOne.put("bookingdates", bookingDatesMapForAmod);
		bookingOne.put("additionalneeds", "Breakfast");
		
		// JSON Object for second guest
		Map<String,Object> bookingTwo = new HashMap<String,Object>();
		bookingTwo.put("firstname", "Animesh");
		bookingTwo.put("lastname", "Prashant");
		bookingTwo.put("totalprice", 111);
		bookingTwo.put("depositpaid", true);
		
		Map<String,String> bookingDatesMapForAnimesh = new HashMap<>();
		bookingDatesMapForAnimesh.put("checkin", "2021-07-01");
		bookingDatesMapForAnimesh.put("checkout", "2021-07-01");
		
		bookingTwo.put("bookingdates", bookingDatesMapForAnimesh);
		bookingTwo.put("additionalneeds", "Breakfast");
		
		// Creating JSON array to add both JSON objects
		List<Map<String,Object>> jsonArrayPayload = new ArrayList<>();
		
		jsonArrayPayload.add(bookingOne);
		jsonArrayPayload.add(bookingTwo);
		
		
		
		//GIVEN
		RestAssured
		   .given()
			  .baseUri("https://restful-booker.herokuapp.com/booking")
			  .contentType(ContentType.JSON)
			  .body(jsonArrayPayload)
			  .log()
			  .all()
		// WHEN
		   .when()
			   .post()
		// THEN
		   .then()
			   .assertThat()
			   // Asserting status code as 500 as it does not accept json array payload
			   .statusCode(500)
			   .log()
			   .all();
	}
 
}


How To Create A JSON Object Using Jackson API – ObjectMapper – CreateObjectNode()

import java.util.Iterator;
import java.util.Map.Entry;
 
import org.testng.annotations.Test;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.node.ObjectNode;
 
import io.restassured.RestAssured;
import io.restassured.http.ContentType;
 
public class CreateJsonObjectUsingJacksonAPI {
	
	@Test
	public void CreatingNestedJsonObjectTest() throws JsonProcessingException
	{
		// Create an object to ObjectMapper
		ObjectMapper objectMapper = new ObjectMapper();
		
		// Creating Node that maps to JSON Object structures in JSON content
		ObjectNode bookingDetails = objectMapper.createObjectNode();
		
		// It is similar to map put method. put method is overloaded to accept different types of data
		// String as field value
		bookingDetails.put("firstname", "Jim");
		bookingDetails.put("lastname", "Brown");
		// integer as field value
		bookingDetails.put("totalprice", 111);
		// boolean as field value
		bookingDetails.put("depositpaid", true);
		bookingDetails.put("additionalneeds", "Breakfast");
		// Duplicate field name. Will override value
		bookingDetails.put("additionalneeds", "Lunch");
		// To print created json object
		String createdPlainJsonObject = objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(bookingDetails);
		System.out.println("Created plain JSON Object is : \n"+ createdPlainJsonObject);
		
		// Since requirement is to create a nested JSON Object
		ObjectNode bookingDateDetails = objectMapper.createObjectNode();
		bookingDateDetails.put("checkin", "2021-07-01");
		bookingDateDetails.put("checkout", "2021-07-01");
		
		// Since 2.4 , put(String fieldName, JsonNode value) is deprecated. So use either set(String fieldName, JsonNode value) or replace(String fieldName, JsonNode value)
		bookingDetails.set("bookingdates", bookingDateDetails);
		
		// To get the created json object as string. Use writerWithDefaultPrettyPrinter() for proper formatting
		String createdNestedJsonObject = objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(bookingDetails);
		System.out.println("Created nested JSON Object is : \n"+ createdNestedJsonObject);
		
		// We can retrieve field value by passing field name. Since it is string, use asText().
		String firstName = bookingDetails.get("firstname").asText();
		System.out.println("First name is : "+firstName);
		
		// We can use asText() as well but return type will be string
		boolean depositpaid = bookingDetails.get("depositpaid").asBoolean();
		System.out.println("deposit paid is : "+depositpaid);
		
		// To retrieve value of nested ObjectNode
		bookingDetails.get("bookingdates").get("checkin").asText();
		System.out.println("Checkin date is : "+depositpaid);
		
		// To get all field names
		System.out.println("Count of fields in ObjectNode : "+ bookingDetails.size());
		Iterator<String> allFieldNames = bookingDetails.fieldNames();
		System.out.println("Fields are : ");
		while(allFieldNames.hasNext())
		{
			System.out.println(allFieldNames.next());
		}
		
		// To get all field values
		Iterator<JsonNode> allFieldValues = bookingDetails.elements();
		System.out.println("Fields values are : ");
		while(allFieldValues.hasNext())
		{
			System.out.println(allFieldValues.next());
		}
		
		// To get all key-value pair
		Iterator<Entry<String, JsonNode>> allFieldsAndValues = bookingDetails.fields();
		System.out.println("All fields and their values are : ");
		while(allFieldsAndValues.hasNext())
		{
			Entry<String, JsonNode> node = allFieldsAndValues.next();
			System.out.println("Key is : "+node.getKey()+" and its value is : "+node.getValue());
		}
		
		// To remove a field
		String removedFieldValue = bookingDetails.remove("firstname").asText();
		System.out.println("Value of Removed field is " + removedFieldValue);
		String removedJsonObject = objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(bookingDetails);
		System.out.println("After removing field , JSON Object is : \n"+ removedJsonObject);
		
		// To replace a field value, use put() method for non ObjectNode type and replace() or set() for ObjectNode
		bookingDetails.put("firstname", "Amod");
		bookingDetails.put("firstname", "Aaditya");
		String updatedJsonObject = objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(bookingDetails);
		System.out.println("After updating field , JSON Object is : \n"+ updatedJsonObject);
		
	}
 
}


How To Use Java Object As Payload For API Request

import org.testng.annotations.Test;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.node.ObjectNode;
 
import io.restassured.RestAssured;
import io.restassured.http.ContentType;
 
public class UseCreatedJsonObjectUsingJacksonAPIPayload {
	
	@Test
	public void CreatingNestedJsonObjectTest() throws JsonProcessingException
	{
		// Create an object to ObjectMapper
		ObjectMapper objectMapper = new ObjectMapper();
		
		// Creating Node that maps to JSON Object structures in JSON content
		ObjectNode bookingDetails = objectMapper.createObjectNode();
		
		// It is similar to map put method. put method is overloaded to accept different types of data
		// String as field value
		bookingDetails.put("firstname", "Jim");
		bookingDetails.put("lastname", "Brown");
		// integer as field value
		bookingDetails.put("totalprice", 111);
		// boolean as field value
		bookingDetails.put("depositpaid", true);
		bookingDetails.put("additionalneeds", "Breakfast");
		// Duplicate field name. Will override value
		bookingDetails.put("additionalneeds", "Lunch");
		
		// Since requirement is to create a nested JSON Object
		ObjectNode bookingDateDetails = objectMapper.createObjectNode();
		bookingDateDetails.put("checkin", "2021-07-01");
		bookingDateDetails.put("checkout", "2021-07-01");
		
		// Since 2.4 , put(String fieldName, JsonNode value) is deprecated. So use either set(String fieldName, JsonNode value) or replace(String fieldName, JsonNode value)
		bookingDetails.set("bookingdates", bookingDateDetails);
		
		
		//GIVEN
		RestAssured
		   .given()
			  .baseUri("https://restful-booker.herokuapp.com/booking")
			  .contentType(ContentType.JSON)
			  // Pass JSON pay load directly
			  .body(bookingDetails)
			  .log()
			  .all()
		// WHEN
		   .when()
			   .post()
		// THEN
		   .then()
			   .assertThat()
			   .statusCode(200)
			   .log()
			   .all();
	}
 
}


How To Create JSON Array Using Jackson API – ObjectMapper – CreateArrayNode()	

import java.util.Arrays;
import java.util.Iterator;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.node.ArrayNode;
import com.fasterxml.jackson.databind.node.ObjectNode;
 
/*
 * [
   {
      "firstname":"Jim",
      "lastname":"Brown",
      "totalprice":111,
      "depositpaid":true,
      "additionalneeds":"Breakfast",
      "bookingdates":{
         "checkin":"2021-07-01",
         "checkout":"2021-07-01"
      }
   },
   {
      "firstname":"Amod",
      "lastname":"Mahajan",
      "totalprice":222,
      "depositpaid":true,
      "additionalneeds":"Lunch",
      "bookingdates":{
         "checkin":"2022-07-01",
         "checkout":"2022-07-01"
      }
   }
]
 * 
 */
 
public class CreateJsonArrayUsingJacksonAPI {
	
	public static void main(String[] args) throws JsonProcessingException {
		
		ObjectMapper objectMapper = new ObjectMapper();
		
		// Create an array which will hold two JSON objects
		ArrayNode parentArray =  objectMapper.createArrayNode();
		
		// Creating Node that maps to JSON Object structures in JSON content
		ObjectNode firstBookingDetails = objectMapper.createObjectNode();
		
		// It is similar to map put method. put method is overloaded to accept different types of data
		// String as field value
		firstBookingDetails.put("firstname", "Jim");
		firstBookingDetails.put("lastname", "Brown");
		// integer as field value
		firstBookingDetails.put("totalprice", 111);
		// boolean as field value
		firstBookingDetails.put("depositpaid", true);
		firstBookingDetails.put("additionalneeds", "Breakfast");
		
		// Since requirement is to create a nested JSON Object
		ObjectNode firstBookingDateDetails = objectMapper.createObjectNode();
		firstBookingDateDetails.put("checkin", "2021-07-01");
		firstBookingDateDetails.put("checkout", "2021-07-01");
		
		// Since 2.4 , put(String fieldName, JsonNode value) is deprecated. So use either set(String fieldName, JsonNode value) or replace(String fieldName, JsonNode value)
		firstBookingDetails.set("bookingdates", firstBookingDateDetails);
		
		
		// Creating Node that maps to JSON Object structures in JSON content
		ObjectNode secondBookingDetails = objectMapper.createObjectNode();
		
		// It is similar to map put method. put method is overloaded to accept different types of data
		// String as field value
		secondBookingDetails.put("firstname", "Amod");
		secondBookingDetails.put("lastname", "Mahajan");
		// integer as field value
		secondBookingDetails.put("totalprice", 222);
		// boolean as field value
		secondBookingDetails.put("depositpaid", true);
		secondBookingDetails.put("additionalneeds", "Breakfast");
		
		// Since requirement is to create a nested JSON Object
		ObjectNode secondBookingDateDetails = objectMapper.createObjectNode();
		secondBookingDateDetails.put("checkin", "2022-07-01");
		secondBookingDateDetails.put("checkout", "2022-07-01");
		
		// Since 2.4 , put(String fieldName, JsonNode value) is deprecated. So use either set(String fieldName, JsonNode value) or replace(String fieldName, JsonNode value)
		secondBookingDetails.set("bookingdates", secondBookingDateDetails);
		
		
		parentArray.add(firstBookingDetails);
		parentArray.add(secondBookingDetails);
		
		// OR
		parentArray.addAll(Arrays.asList(firstBookingDetails,secondBookingDetails));
		
		String jsonArrayAsString = objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(parentArray);
		System.out.println("Created Json Array is : ");
		System.out.println(jsonArrayAsString);
		System.out.println("=======================================================================================");
		// To get json array element using index
		JsonNode firstElement = parentArray.get(0);
		System.out.println(objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(firstElement));
		System.out.println("=======================================================================================");
		// To get size of JSON array
		int sizeOfArray = parentArray.size();
		System.out.println("Size of array is "+sizeOfArray);
		System.out.println("=======================================================================================");
		// To iterate JSON Array
		Iterator<JsonNode> iteraor = parentArray.iterator();
		System.out.println("Prining Json Node using iterator : ");
		while(iteraor.hasNext())
		{
			JsonNode currentJsonNode = iteraor.next();
			System.out.println(objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(currentJsonNode));
		}
		System.out.println("=======================================================================================");
		// To remove an element from array
		parentArray.remove(0);
		System.out.println("After removing first element from array : "+ objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(parentArray));
		System.out.println("=======================================================================================");
		// To empty JSON Array
		parentArray.removeAll();
		System.out.println("After removing all elements from array : "+ objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(parentArray));
			
	}
 
}


What Is Plain Old Java Object (POJO) ?

public class EmployeePojo {
	
	// Private fields
	private String firstName;
	private String lastName;
	private double salary;
	private String cityName;
	private boolean isMarried;
	private char gender;
	private String fullName;
	
	// Public constructor
	public EmployeePojo()
	{
		
	}
	
	// Business logic to get full name
	public String getFulName()
	{
		this.fullName =  this.firstName + " "+ this.lastName;
		return fullName;
	}
	
	// Public getter setter methods
	public String getFirstName() {
		return firstName;
	}
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	public String getCityName() {
		return cityName;
	}
	public void setCityName(String cityName) {
		this.cityName = cityName;
	}
	public boolean isMarried() {
		return isMarried;
	}
	public void setMarried(boolean isMarried) {
		this.isMarried = isMarried;
	}
	public char getGender() {
		return gender;
	}
	public void setGender(char gender) {
		this.gender = gender;
	}
	
}

package PojoPayloads;
 
public class EmployeePojoUsage {
	
	public static void main(String[] args) {
		
		EmployeePojo Amod;
		EmployeePojo Animesh;
		
		// Setting employees details 
		Amod = new EmployeePojo();
		Amod.setFirstName("Amod");
		Amod.setLastName("Mahajan");
		Amod.setCityName("Benagluru");
		Amod.setGender('M');
		Amod.setMarried(false);
		Amod.setSalary(10000.54);
		
		Animesh = new EmployeePojo();
		Animesh.setFirstName("Animesh");
		Animesh.setLastName("Prashant");
		Animesh.setCityName("Kolkata");
		Animesh.setGender('M');
		Animesh.setMarried(false);
		Animesh.setSalary(23232.45);
		
		// Printing details of employees
		System.out.println("Details of Employees :-");
		System.out.println("First Name : "+ Amod.getFirstName());
		System.out.println("Last Name  : "+ Amod.getLastName());
		System.out.println("Full Name  : "+ Amod.getFulName());
		System.out.println("City Name  : "+ Amod.getCityName());
		System.out.println("Is Married?: "+ Amod.isMarried());
		System.out.println("Gender     : "+ Amod.getGender());
		System.out.println("Salary     : "+ Amod.getSalary());
		
		
		System.out.println("==========================================");
		System.out.println("First Name : "+ Animesh.getFirstName());
		System.out.println("Last Name  : "+ Animesh.getLastName());
		System.out.println("Full Name  : "+ Animesh.getFulName());
		System.out.println("City Name  : "+ Animesh.getCityName());
		System.out.println("Is Married?: "+ Animesh.isMarried());
		System.out.println("Gender     : "+ Animesh.getGender());
		System.out.println("Salary     : "+ Animesh.getSalary());
		
	}
}


How To Create POJO Classes Of A JSON Object Payload

package RestfulBookerPojo;
 
public class Employee {
 
	// private variables or data members of pojo class
	private String firstName;
	private String lastName;
	private String gender;
	private int age;
	private double salary;
	private boolean married;
	
	// Getter and setter methods
	public String getFirstName() {
		return firstName;
	}
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	public boolean getMarried() {
		return married;
	}
	public void setMarried(boolean married) {
		this.married = married;
	} 
}

package RestfulBookerPojo;
 
import org.testng.annotations.Test;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
 
public class EmployeeSerializationDeserialization {
 
	@Test
	public void createEmployeeJSONFromEmployeePOJOClass() throws JsonProcessingException
	{
		// Just create an object of Pojo class
		Employee employee = new Employee();
		// Set value as you wish
		employee.setFirstName("Amod");
		employee.setLastName("Mahajan");
		employee.setAge(29);
		employee.setGender("Male");
		employee.setSalary(3434343);
		employee.setMarried(false);
		
		// Converting a Java class object to a JSON payload as string
		ObjectMapper objectMapper = new ObjectMapper();
		String employeeJson = objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(employee);
		System.out.println(employeeJson);
	}
	
	
	@Test
	public void getPojoFromEmployeeObject() throws JsonProcessingException
	{
		// Just create an object of Pojo class
		Employee employee = new Employee();
		// Set value as you wish
		employee.setFirstName("Amod");
		employee.setLastName("Mahajan");
		employee.setAge(29);
		employee.setGender("Male");
		employee.setSalary(3434343);
		employee.setMarried(false);
		
		// Converting a Java class object to a JSON payload as string
		ObjectMapper objectMapper = new ObjectMapper();
		String employeeJson = objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(employee);
		
		
		// Converting EMployee json string to Employee class object
		Employee employee2 = objectMapper.readValue(employeeJson, Employee.class);
		System.out.println("First Name of employee : "+employee2.getFirstName());
		System.out.println("Last Name of employee : "+employee2.getLastName());
		System.out.println("Age of employee : "+employee2.getAge());
		System.out.println("Gender of employee : "+employee2.getGender());
		System.out.println("Salary of employee : "+employee2.getSalary());
		System.out.println("Marital status of employee : "+employee2.getMarried());
	}
}


How To Create POJO Classes Of A JSON Array Payload

package RestfulBookerPojo;
 
public class Employee {
 
	// private variables or data members of pojo class
	private String firstName;
	private String lastName;
	private String gender;
	private int age;
	private double salary;
	private boolean married;
	
	// Getter and setter methods
	public String getFirstName() {
		return firstName;
	}
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	public boolean getMarried() {
		return married;
	}
	public void setMarried(boolean married) {
		this.married = married;
	} 
}

package RestfulBookerPojo;
 
import java.util.ArrayList;
import java.util.List;
 
import org.testng.annotations.Test;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;
 
public class ListOfEmployeesSerializationDeserialization {
 
	public String allEmployeeJson;
 
	@Test
	public void createListOfEmployeesJSONArrayFromEmployeePOJOClass() throws JsonProcessingException {
		// Create first employee
		Employee amod = new Employee();
		amod.setFirstName("Amod");
		amod.setLastName("Mahajan");
		amod.setAge(29);
		amod.setGender("Male");
		amod.setSalary(10000.56);
		amod.setMarried(false);
 
		// Create second employee
		Employee animesh = new Employee();
		animesh.setFirstName("Animesh");
		animesh.setLastName("Prashant");
		animesh.setAge(30);
		animesh.setGender("Male");
		animesh.setSalary(20000.56);
		animesh.setMarried(true);
 
		// Create third employee
		Employee kitty = new Employee();
		kitty.setFirstName("Kitty");
		kitty.setLastName("Gupta");
		kitty.setAge(27);
		kitty.setGender("Female");
		kitty.setSalary(30000.56);
		kitty.setMarried(false);
 
		// Creating a List of Employees
		List<Employee> allEMployees = new ArrayList<Employee>();
		allEMployees.add(amod);
		allEMployees.add(animesh);
		allEMployees.add(kitty);
 
		// Converting a Java class object to a JSON Array payload as string
		ObjectMapper objectMapper = new ObjectMapper();
		allEmployeeJson = objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(allEMployees);
		System.out.println(allEmployeeJson);
	}
 
	@Test
	public void getPojoFromEmployeeObject() throws JsonProcessingException {
		// Converting EMployee json Array string to Employee class object
		ObjectMapper objectMapper = new ObjectMapper();
		List<Employee> allEmploeesDetails = objectMapper.readValue(allEmployeeJson,
				new TypeReference<List<Employee>>() {
				});
		for (Employee emp : allEmploeesDetails) {
			System.out.println("========================================================");
			System.out.println("First Name of employee : " + emp.getFirstName());
			System.out.println("Last Name of employee : " + emp.getLastName());
			System.out.println("Age of employee : " + emp.getAge());
			System.out.println("Gender of employee : " + emp.getGender());
			System.out.println("Salary of employee : " + emp.getSalary());
			System.out.println("Marital status of employee : " + emp.getMarried());
			System.out.println("========================================================");
		}
	}
}





How To Create POJO Classes Of A Nested JSON Payload

public class Employee {
 
	// private variables or data members of pojo class
	private String firstName;
	private String lastName;
	private String gender;
	private int age;
	private double salary;
	private boolean married;
	
	// Getter and setter methods
	public String getFirstName() {
		return firstName;
	}
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	public boolean getMarried() {
		return married;
	}
	public void setMarried(boolean married) {
		this.married = married;
	} 	
}


package RestfulBookerPojo;
 
public class Contractors {
	private String firstName;
	private String lastName;
	private String contractFrom;
	private String contractTo;
	
	public String getFirstName() {
		return firstName;
	}
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
	public String getContractFrom() {
		return contractFrom;
	}
	public void setContractFrom(String contractFrom) {
		this.contractFrom = contractFrom;
	}
	public String getContractTo() {
		return contractTo;
	}
	public void setContractTo(String contractTo) {
		this.contractTo = contractTo;
	}
 
}


package RestfulBookerPojo;
 
public class CompanyPFDeails {
	private String pfName;
	private String pfCity;
	private int pfYear;
	private int noOfEmployees;
	
	public String getPfName() {
		return pfName;
	}
	public void setPfName(String pfName) {
		this.pfName = pfName;
	}
	public String getPfCity() {
		return pfCity;
	}
	public void setPfCity(String pfCity) {
		this.pfCity = pfCity;
	}
	public int getPfYear() {
		return pfYear;
	}
	public void setPfYear(int pfYear) {
		this.pfYear = pfYear;
	}
	public int getNoOfEmployees() {
		return noOfEmployees;
	}
	public void setNoOfEmployees(int noOfEmployees) {
		this.noOfEmployees = noOfEmployees;
	}
}


package RestfulBookerPojo;
 
import java.util.List;
 
public class NestedPOJO {
	
	private String companyName;
	private String companyHOCity;
	private String companyCEO;
	private List<String> supportedSalaryBanks;
	private List<Integer> pincodesOfCityOffice;
	List<Employee> employee;
	List<Contractors> contractors;
	CompanyPFDeails companyPFDeails;
	
	public String getCompanyName() {
		return companyName;
	}
	public void setCompanyName(String companyName) {
		this.companyName = companyName;
	}
	public String getCompanyHOCity() {
		return companyHOCity;
	}
	public void setCompanyHOCity(String companyHOCity) {
		this.companyHOCity = companyHOCity;
	}
	public String getCompanyCEO() {
		return companyCEO;
	}
	public void setCompanyCEO(String companyCEO) {
		this.companyCEO = companyCEO;
	}
	public List<String> getSupportedSalaryBanks() {
		return supportedSalaryBanks;
	}
	public void setSupportedSalaryBanks(List<String> supportedSalaryBanks) {
		this.supportedSalaryBanks = supportedSalaryBanks;
	}
	public List<Integer> getPincodesOfCityOffice() {
		return pincodesOfCityOffice;
	}
	public void setPincodesOfCityOffice(List<Integer> pincodesOfCityOffice) {
		this.pincodesOfCityOffice = pincodesOfCityOffice;
	}
	
	public List<Employee> getEmployee() {
		return employee;
	}
	public void setEmployee(List<Employee> employee) {
		this.employee = employee;
	}
	public List<Contractors> getContractors() {
		return contractors;
	}
	public void setContractors(List<Contractors> contractors) {
		this.contractors = contractors;
	}
	public CompanyPFDeails getCompanyPFDeails() {
		return companyPFDeails;
	}
	public void setCompanyPFDeails(CompanyPFDeails companyPFDeails) {
		this.companyPFDeails = companyPFDeails;
	}
 
}


package RestfulBookerPojo;
 
import java.util.ArrayList;
import java.util.List;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
 
public class CreateNestedJSONFromPOJOClasses {
 
	public static void main(String[] args) throws JsonProcessingException {
			
		NestedPOJO nestedPOJO = new NestedPOJO();
		nestedPOJO.setCompanyName("MSE");
		nestedPOJO.setCompanyHOCity("Benagluru");
		nestedPOJO.setCompanyCEO("Amod");
		
		List<String> supportedSalaryBanks = new ArrayList<String>();
		supportedSalaryBanks.add("HDFC");
		supportedSalaryBanks.add("ICICI");
		supportedSalaryBanks.add("AXIS");
		nestedPOJO.setSupportedSalaryBanks(supportedSalaryBanks);
		
		List<Integer> pincodesOfCityOffice = new ArrayList<Integer>();
		pincodesOfCityOffice.add(560037);
		pincodesOfCityOffice.add(360034);
		pincodesOfCityOffice.add(456343);
		nestedPOJO.setPincodesOfCityOffice(pincodesOfCityOffice);
		
		// Create first employee
		Employee amod = new Employee();
		amod.setFirstName("Amod");
		amod.setLastName("Mahajan");
		amod.setAge(28);
		amod.setGender("Male");
		amod.setSalary(10000.56);
		amod.setMarried(false);
 
		// Create second employee
		Employee animesh = new Employee();
		animesh.setFirstName("Animesh");
		animesh.setLastName("Prashant");
		animesh.setAge(30);
		animesh.setGender("Male");
		animesh.setSalary(20000.56);
		animesh.setMarried(true);
 
		// Create third employee
		Employee kitty = new Employee();
		kitty.setFirstName("Kitty");
		kitty.setLastName("Gupta");
		kitty.setAge(26);
		kitty.setGender("Female");
		kitty.setSalary(30000.56);
		kitty.setMarried(false);
 
		// Creating a List of Employees
		List<Employee> allEMployees = new ArrayList<Employee>();
		allEMployees.add(amod);
		allEMployees.add(animesh);
		allEMployees.add(kitty);
		nestedPOJO.setEmployee(allEMployees);
		
		Contractors seema = new Contractors();
		seema.setFirstName("Seema");
		seema.setLastName("Singh");
		seema.setContractFrom("Jan-2019");
		seema.setContractTo("JAN-2025");
		
		Contractors hari = new Contractors();
		hari.setFirstName("Hari");
		hari.setLastName("Prasad");
		hari.setContractFrom("Jan-2017");
		hari.setContractTo("JAN-2030");
		
		List<Contractors> allContractors = new ArrayList<Contractors>();
		allContractors.add(seema);
		allContractors.add(hari);
		nestedPOJO.setContractors(allContractors);
		
		CompanyPFDeails companyPFDeails = new CompanyPFDeails();
		companyPFDeails.setPfName("XYZ");
		companyPFDeails.setPfCity("Benagluru");
		companyPFDeails.setPfYear(2012);
		companyPFDeails.setNoOfEmployees(10);
		nestedPOJO.setCompanyPFDeails(companyPFDeails);
		
		ObjectMapper objectMapper = new ObjectMapper();
		String nestedJsonPayload = objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(nestedPOJO);
		System.out.println(nestedJsonPayload);
	}
}


Serialization – Java Object To JSON Object Using Jackson API

package PojoPayloads;
 
import java.io.File;
import java.io.IOException;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
 
class MSE_EmployeePojo {
 
	private String firstName;
	private String lastName;
	private String gender;
 
	private int age;
	private double salary;
	private boolean isMarried;
 
	public String getFirstName() {
		return firstName;
	}
 
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
 
	public String getLastName() {
		return lastName;
	}
 
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
 
	public String getGender() {
		return gender;
	}
 
	public void setGender(String gender) {
		this.gender = gender;
	}
 
	public int getAge() {
		return age;
	}
 
	public void setAge(int age) {
		this.age = age;
	}
 
	public double getSalary() {
		return salary;
	}
 
	public void setSalary(double salary) {
		this.salary = salary;
	}
 
	public boolean isMarried() {
		return isMarried;
	}
 
	public void setMarried(boolean isMarried) {
		this.isMarried = isMarried;
	}
}
 
public class UsageOfMSE_EmployeePojo {
	
	public static void main(String[] args) throws IOException {
		
		MSE_EmployeePojo mse_EmployeePojo = new MSE_EmployeePojo();
		mse_EmployeePojo.setFirstName("Amod");
		mse_EmployeePojo.setLastName("Mahajan");
		mse_EmployeePojo.setAge(29);
		mse_EmployeePojo.setSalary(10987.77);
		mse_EmployeePojo.setMarried(false);
		mse_EmployeePojo.setGender("M");
		
		ObjectMapper objectMapper = new ObjectMapper();
		String convertedJson = objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(mse_EmployeePojo);
		System.out.println(convertedJson);
		
		String userDir = System.getProperty("user.dir");
		File outputJsonFile = new File(userDir+ "\\src\\test\\resources\\EmployeePayload.json");
		objectMapper.writerWithDefaultPrettyPrinter().writeValue(outputJsonFile, mse_EmployeePojo);
	}
 
}


Serialization – JSON Object To Java Object Using Jackson API

package SerializationDeserialization;
 
public class Employee {
 
	private String firstName;
	private String lastName;
	private String gender;
 
	private int age;
	private double salary;
	private boolean married;
 
	public String getFirstName() {
		return firstName;
	}
 
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
 
	public String getLastName() {
		return lastName;
	}
 
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
 
	public String getGender() {
		return gender;
	}
 
	public void setGender(String gender) {
		this.gender = gender;
	}
 
	public int getAge() {
		return age;
	}
 
	public void setAge(int age) {
		this.age = age;
	}
 
	public double getSalary() {
		return salary;
	}
 
	public void setSalary(double salary) {
		this.salary = salary;
	}
 
	public boolean getMarried() {
		return married;
	}
 
	public void setMarried(boolean married) {
		this.married = married;
	}
 
}


package SerializationDeserialization;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.JsonMappingException;
import com.fasterxml.jackson.databind.ObjectMapper;
 
public class DeserializeJsonToJava {
 
	public static void main(String[] args) throws JsonMappingException, JsonProcessingException {
		
		String jsonString = "{\r\n" + 
				"  \"firstName\" : \"Amod\",\r\n" + 
				"  \"lastName\" : \"Mahajan\",\r\n" + 
				"  \"gender\" : \"M\",\r\n" + 
				"  \"age\" : 29,\r\n" + 
				"  \"salary\" : 10987.77,\r\n" + 
				"  \"married\" : false\r\n" + 
				"}";
		
		ObjectMapper objectMapper = new ObjectMapper();
		// Pass JSON string and the POJO class 
		Employee employeeObject = objectMapper.readValue(jsonString, Employee.class);
		// Now use getter method to retrieve values
		String firsName = employeeObject.getFirstName();
		String lastName = employeeObject.getLastName();
		String gender = employeeObject.getGender();
		int age = employeeObject.getAge();
		double salary = employeeObject.getSalary();
		boolean married = employeeObject.getMarried();
		
		System.out.println("Details of Employee is as below:-");
		System.out.println("First Name : "+firsName);
		System.out.println("Last Name : "+lastName); 
		System.out.println("Gender : "+gender);
		System.out.println("Age : "+age);
		System.out.println("Salary : "+salary);
		System.out.println("Married : "+married);
	}
}


Serialization – Java Object To JSON Object Using Gson API

package SerializationDeserialization;
 
public class Employee {
 
	private String firstName;
	private String lastName;
	private String gender;
 
	private int age;
	private double salary;
	private boolean married;
 
	public String getFirstName() {
		return firstName;
	}
 
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
 
	public String getLastName() {
		return lastName;
	}
 
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
 
	public String getGender() {
		return gender;
	}
 
	public void setGender(String gender) {
		this.gender = gender;
	}
 
	public int getAge() {
		return age;
	}
 
	public void setAge(int age) {
		this.age = age;
	}
 
	public double getSalary() {
		return salary;
	}
 
	public void setSalary(double salary) {
		this.salary = salary;
	}
 
	public boolean getMarried() {
		return married;
	}
 
	public void setMarried(boolean married) {
		this.married = married;
	}
 
}



package SerializationDeserialization;
 
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
 
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonIOException;
 
public class SerializeJavaToJsonObjectUsingGSON {
 
	public static void main(String[] args) throws JsonIOException, IOException {
		
		// Create a Employee java object
		Employee employeeObject = new Employee();
		employeeObject.setFirstName("Amod");
		employeeObject.setLastName("Mahajan");
		employeeObject.setAge(29);
		employeeObject.setSalary(10987.77);
		employeeObject.setMarried(false);
		employeeObject.setGender("M");
		
		// Create a Gson object
		Gson gson = new Gson();
		// toJson(Object src) method converts Java object to JSON object
		String employeeJsonSring = gson.toJson(employeeObject);
		// Printing json string. It will be pretty print 
		System.out.println("Non-pretty JSON String :- ");
		System.out.println(employeeJsonSring);
		
		// We can create a configurable Gson instance using GsonBuilder class
		Gson gsonBuilder = new GsonBuilder().setPrettyPrinting().create();
		String employeeJsonSringUsingJsonBuilder = gsonBuilder.toJson(employeeObject);
		System.out.println("Pretty JSON String :- ");
		System.out.println(employeeJsonSringUsingJsonBuilder);
		
		// To write Json object in to a file, we need to pass a FileWriter object which is in direct implementation of 
		// Appendable interface. Make sure you call flush() method otherwise json file will be empty.
		String userDir = System.getProperty("user.dir");
		File outputJsonFile = new File(userDir+ "\\src\\test\\resources\\EmployeePayloadUsingGson.json");
		FileWriter fileWriter = new FileWriter(outputJsonFile);
		gsonBuilder.toJson(employeeObject,fileWriter);
		fileWriter.flush();
		
	}
}



De-Serialization – JSON Object To Java Object Using Gson API

package SerializationDeserialization;
 
public class Employee {
 
	private String firstName;
	private String lastName;
	private String gender;
 
	private int age;
	private double salary;
	private boolean married;
 
	public String getFirstName() {
		return firstName;
	}
 
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
 
	public String getLastName() {
		return lastName;
	}
 
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
 
	public String getGender() {
		return gender;
	}
 
	public void setGender(String gender) {
		this.gender = gender;
	}
 
	public int getAge() {
		return age;
	}
 
	public void setAge(int age) {
		this.age = age;
	}
 
	public double getSalary() {
		return salary;
	}
 
	public void setSalary(double salary) {
		this.salary = salary;
	}
 
	public boolean getMarried() {
		return married;
	}
 
	public void setMarried(boolean married) {
		this.married = married;
	}
 
}


package SerializationDeserialization;
 
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.JsonMappingException;
import com.google.gson.Gson;
 
public class DeserializeJsonToJavaObjectUsingGson {
 
	public static void main(String[] args) throws JsonMappingException, JsonProcessingException, FileNotFoundException {
 
		// De-serializing from JSON String
		String jsonString = "{\r\n" + "  \"firstName\" : \"Amod\",\r\n" + "  \"lastName\" : \"Mahajan\",\r\n"
				+ "  \"gender\" : \"M\",\r\n" + "  \"age\" : 29,\r\n" + "  \"salary\" : 10987.77,\r\n"
				+ "  \"married\" : false\r\n" + "}";
 
		Gson gson = new Gson();
		// Pass JSON string and the POJO class
		Employee employeeObject = gson.fromJson(jsonString, Employee.class);
		// Now use getter method to retrieve values
		String firsName = employeeObject.getFirstName();
		String lastName = employeeObject.getLastName();
		String gender = employeeObject.getGender();
		int age = employeeObject.getAge();
		double salary = employeeObject.getSalary();
		boolean married = employeeObject.getMarried();
 
		System.out.println("Details of Employee is as below:-");
		System.out.println("First Name : " + firsName);
		System.out.println("Last Name : " + lastName);
		System.out.println("Gender : " + gender);
		System.out.println("Age : " + age);
		System.out.println("Salary : " + salary);
		System.out.println("Married : " + married);
 
		// De-serializing from a json file
		String userDir = System.getProperty("user.dir");
		File inputJsonFile = new File(userDir + "\\src\\test\\resources\\EmployeePayloadUsingGson.json");
		FileReader fileReader = new FileReader(inputJsonFile);
		Employee employeeObject1 = gson.fromJson(fileReader, Employee.class);
 
		// Now use getter method to retrieve values
		String firsName1 = employeeObject1.getFirstName();
		String lastName1 = employeeObject1.getLastName();
		String gender1 = employeeObject1.getGender();
		int age1 = employeeObject1.getAge();
		double salary1 = employeeObject1.getSalary();
		boolean married1 = employeeObject1.getMarried();
 
		System.out.println("Details of Employee from json file is as below:-");
		System.out.println("First Name : " + firsName1);
		System.out.println("Last Name : " + lastName1);
		System.out.println("Gender : " + gender1);
		System.out.println("Age : " + age1);
		System.out.println("Salary : " + salary1);
		System.out.println("Married : " + married1);
	}
}



For More indept knowladge refer all articles from below URL
http://makeseleniumeasy.com/rest-assured-tutorials/
