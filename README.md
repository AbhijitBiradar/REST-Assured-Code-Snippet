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
