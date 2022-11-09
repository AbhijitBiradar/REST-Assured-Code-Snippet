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

# Abhijit Program Doc :

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


-------------------------------------------------------------------------------------------------------------------------------------------------------------

# Abhijit API Testing FAQs Doc :

Q: Script to fetch different parts of a response

Header
Rest Assured is a very straightforward language, and fetching headers is just as simple. The method name is headers(). Like before, we will create a standalone method to do the same.

```java 

public static void getResponseHeaders(){
	System.out.println("The headers in the response "+ get(url).
		then().
			extract()
				.headers());
}

```

Response Time
To get the time needed to fetch the response from the backend or other downstream systems, Rest Assured provides a method called ‘timeIn’ with a suitable timeUnit to get the time taken to return the response.

```java 

public static void getResponseTime(){
  System.out.println("The time taken to fetch the response "+get(url).timeIn(TimeUnit.MILLISECONDS) + " milliseconds");
}

```

Content-Type
You can get the content-Type of the response returned using the method “contentType ()”.

```java 

public static void getResponseContentType(){
	System.out.println("The content type of response "+
		get(url).
		then().
			extract()
				.contentType());
}

```

Q: Default Values Configuration
We can configure a lot of default values for the tests:

```java 

@Before
public void setup() {
    RestAssured.baseURI = "https://api.github.com";
    RestAssured.port = 443;
}

```

Q: Measure Response Time
Let's see how we can measure the response time using the time() and timeIn() methods of the Response object:

```java 

@Test
public void whenMeasureResponseTime_thenOK() {
    Response response = RestAssured.get("/users/eugenp");
    long timeInMS = response.time();
    long timeInS = response.timeIn(TimeUnit.SECONDS);
    
    assertEquals(timeInS, timeInMS/1000);
}

```

Note that:
time() is used to get response time in milliseconds
timeIn() is used to get response time in the specified time unit


Q: Validate Response Time
We can also validate the response time – in milliseconds – with the help of simple long Matcher:

```java 

@Test
public void whenValidateResponseTime_thenSuccess() {
	when().
  	get("/users/eugenp").
   	then().
		time(lessThan(5000L));
}

```

Q: XML Response Verification
Not only can it validate a JSON response, it can validate XML as well.

Let's assume we make a request to http://localhost:8080/employees and we get the following response:

```java 

<employees>
    <employee category="skilled">
        <first-name>Jane</first-name>
        <last-name>Daisy</last-name>
        <sex>f</sex>
    </employee>
</employees>

We can verify that the first-name is Jane like so:

@Test
public void givenUrl_whenXmlResponseValueTestsEqual_thenCorrect() {
    post("/employees").
    then().
	assertThat()
      		.body("employees.employee.first-name", equalTo("Jane"));
}

We can also verify that all values match our expected values by chaining body matchers together like so:

@Test
public void givenUrl_whenMultipleXmlValuesTestEqual_thenCorrect() {
    post("/employees").
    then().
    	assertThat()
    		.body("employees.employee.first-name", equalTo("Jane"))
            	.body("employees.employee.last-name", equalTo("Daisy"))
            	.body("employees.employee.sex", equalTo("f"));
}

Or using the shorthand version with variable arguments:

@Test
public void givenUrl_whenMultipleXmlValuesTestEqualInShortHand_thenCorrect() {
    post("/employees").
    then().
    	assertThat().
		body("employees.employee.first-name", 
    		equalTo("Jane"),"employees.employee.last-name", 
           	equalTo("Daisy"), "employees.employee.sex", 
            	equalTo("f"));
}

```

Q: XPath for XML
We can also verify our responses using XPath. Consider the example below that executes a matcher on the first-name:

```java 

@Test
public void givenUrl_whenValidatesXmlUsingXpath_thenCorrect() {
    post("/employees").
    then().
	assertThat().
      		body(hasXPath("/employees/employee/first-name", containsString("Ja")));
}

XPath also accepts an alternate way of running the equalTo matcher:

@Test
public void givenUrl_whenValidatesXmlUsingXpath2_thenCorrect() {
    post("/employees").
    then().
	assertThat()
      		.body(hasXPath("/employees/employee/first-name[text()='Jane']"));
}

```

Q: Log Request Details
First, let's see how to log entire request details using log().all():

```java 

@Test
public void whenLogRequest_thenOK() {
	given().
		log().all().
	when().
	get("/users/eugenp").
	then().
		statusCode(200);
}

```

Q: Log Response Details
Similarly, we can log the response details.

In the following example we're logging the response body only:

```java 

@Test
public void whenLogResponse_thenOK() {
	when().
	get("/repos/eugenp/tutorials").
	then().
		log().body().statusCode(200);
}

```

Q: Log Response if Condition Occurred
We also have the option of logging the response only if an error occurred or the status code matches a given value:

```java 

@Test
public void whenLogResponseIfErrorOccurred_thenSuccess() { 
	when().
	get("/users/eugenp").
	then().
		log().ifError();
		
	when().
	get("/users/eugenp").
	then().
		log().ifStatusCodeIsEqualTo(500);
		
	when().
	get("/users/eugenp").
	then().
	log().ifStatusCodeMatches(greaterThan(200));
}

```

Q: Log if Validation Failed
We can also log both request and response only if our validation failed:

```java 

@Test
public void whenLogOnlyIfValidationFailed_thenSuccess() {
	when().
	get("/users/eugenp").
	then().
		log().ifValidationFails().statusCode(200);

	given().
		log().ifValidationFails().
	when().
	get("/users/eugenp").
	then().
		statusCode(200);
}

```

Q: Validate Header

```java 

@Test
public void test_ResponseHeaderData_ShouldBeCorrect() {        
    given().
    when().
        get("http://ergast.com/api/f1/2017/circuits.json").
    then().
        assertThat().
        statusCode(200).
    and().
        contentType(ContentType.JSON).
    and().
        header("Content-Length",equalTo("4567"));
}

```

Q: Query Parameter Example
REST Assured can work with both types of parameters. First, let's see how you can specify and use the query parameter from the example above:

```java 

@Test
public void test_Md5CheckSumForTest_ShouldBe098f6bcd4621d373cade4e832627b4f6() {    
    String originalText = "test";
    String expectedMd5CheckSum = "098f6bcd4621d373cade4e832627b4f6";        
    
    given().
        param("text",originalText).
    when().
        get("http://md5.jsontest.com").
    then().
        assertThat().
        	body("md5",equalTo(expectedMd5CheckSum));
}

```

Q: Path Parameter Example:

```java 

@Test
public void test_NumberOfCircuits_ShouldBe20_Parameterized() {        
    String season = "2017";
    int numberOfRaces = 20;
        
    given().
        pathParam("raceSeason",season).
    when().
        get("http://ergast.com/api/f1/{raceSeason}/circuits.json").
    then().
        assertThat().
        	body("MRData.CircuitTable.Circuits.circuitId",hasSize(numberOfRaces));
}

```

Q: Data Driven REST Assured Test

```java 

@DataProvider(name="seasonsAndNumberOfRaces")
public Object[][] createTestDataRecords() {
    return new Object[][] {
        {"2017",20},
        {"2016",21},
        {"1966",9}
    };
}

Then you just pass the test data object to the parameterized test through test method parameters:

@Test(dataProvider="seasonsAndNumberOfRaces")
public void test_NumberOfCircuits_ShouldBe_DataDriven(String season, int numberOfRaces) {
    given().
        pathParam("raceSeason",season).
    when().
        get("http://ergast.com/api/f1/{raceSeason}/circuits.json").
    then().
        assertThat().
        body("MRData.CircuitTable.Circuits.circuitId",hasSize(numberOfRaces));
}

```

Q: Data Driven REST Assured Test

```java 

@DataProvider(name="seasonsAndNumberOfRaces")
public Object[][] createTestDataRecords() {
    return new Object[][] {
        {"2017",20},
        {"2016",21},
        {"1966",9}
    };
}

Then you just pass the test data object to the parameterized test through test method parameters:

@Test(dataProvider="seasonsAndNumberOfRaces")
public void test_NumberOfCircuits_ShouldBe_DataDriven(String season, int numberOfRaces) {
    given().
        pathParam("raceSeason",season).
    when().
        get("http://ergast.com/api/f1/{raceSeason}/circuits.json").
    then().
        assertThat().
        body("MRData.CircuitTable.Circuits.circuitId",hasSize(numberOfRaces));
}

```

Q: Passing parameters between tests
Often, when testing RESTful APIs, you might need to create more complex test scenarios where you'll need to capture a value from the response of one API call and reuse it in a subsequent call. This is supported by REST Assured using the extract() method. As an example, here's a test scenario that extracts the ID for the first circuit of the 2017 Formula 1 season and uses it to retrieve and verify additional information on that circuit (in this case, the circuit is located in Australia):

```java 

@Test
public void test_ScenarioRetrieveFirstCircuitFor2017SeasonAndGetCountry_ShouldBeAustralia() {        
    // First, retrieve the circuit ID for the first circuit of the 2017 season
    String circuitId = given().
    when().
        get("http://ergast.com/api/f1/2017/circuits.json").
    then().
        extract().
        path("MRData.CircuitTable.Circuits.circuitId[0]");
        
    // Then, retrieve the information known for that circuit and verify it is located in Australia
    given().
        pathParam("circuitId",circuitId).
    when().
        get("http://ergast.com/api/f1/circuits/{circuitId}.json").
    then().
        assertThat().
        body("MRData.CircuitTable.Circuits.Location[0].country",equalTo("Australia"));
}

```

Note: For Remaining Program refer below

Automation Teting\ 01 Interview - FAQs\ API Testing FQAs 1 Document

Topic Name:
Q: XMLPath Validation


-------------------------------------------------------------------------------------------------------------------------------------------------------------
# Notes From REST Assured Wiki Docs

```java 

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

```

Root path
To avoid duplicated paths in body expectations you can specify a root path. E.g. instead of writing:

```java 

expect().
	 body("x.y.firstName", is(..)).
	 body("x.y.lastName", is(..)).
	 body("x.y.age", is(..)).
	 body("x.y.gender", is(..)).
when().
	 get("/something");
		 
You can also set a default root path using:

RestAssured.rootPath = "x.y";
	
```

Session Filter	

SessionFilter sessionFilter = new SessionFilter();

```java 

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
	  
```	  
			  
To get session id caught by the SessionFilter you can do like this:

```java 

String sessionId = sessionFilter.getSessionId();	

```


----------------------------------------------------------------------------------------------------------------------------------------------------

# Program from MakeSaleniumEasy

For More indept knowladge refer all articles from below URL
http://makeseleniumeasy.com/rest-assured-tutorials/

