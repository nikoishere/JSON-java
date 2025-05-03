# SWE262P-Milestone 2

This milestone enhances the original `org.json.XML` library with two new overloads of the `toJSONObject()` method. These methods allow users to **extract** or **replace** sub-objects from an XML stream using a specified JSONPointer path—without parsing the entire XML document unnecessarily.

**Author:**

Yifei Wang ([yifew59@uci.edu](mailto:yifew59@uci.edu))

Cherine Cho ([cherinec@uci.edu](mailto:cherinec@uci.edu))



## Implementation (starting from line 1042 in XML.java)

This milestone introduces two new overloaded static methods in the org.json.XML class, implemented starting from **line 1042** in `src/main/java/org/json/XML.java`. 



### **Method 1: Extracting a Sub-object by Path**

```
public static JSONObject toJSONObject(Reader reader, JSONPointer path)
```



**Purpose**

This method is designed to efficiently extract a specific sub-object from an XML stream using a JSONPointer, without needing to parse the entire XML document. 



**How it works**

The input JSONPointer is first split into its individual tag components to represent the hierarchical path. The method then uses a XMLTokener to traverse the XML content token by token. As it encounters each tag, it checks whether it matches the expected structure of the path. When the target depth is reached, matching tags and their contents are collected into a list of XML fragments. The parser stops immediately once the entire sub-object has been captured. Finally, the collected XML fragment is reassembled and converted into a JSONObject using the existing XML.toJSONObject(String) method.

------

### **Method 2: Replacing a Sub-object by Path**

```
public static JSONObject toJSONObject(Reader reader, JSONPointer path, JSONObject replacement)
```



**Purpose**

This method allows replacing a specific sub-object in an XML document with a user-provided JSONObject, using a given JSONPointer path.



**How it works**

Similar to the extraction method, this function parses the XML stream token by token and collects the opening tags that lead to the target path. Once the target sub-object is found, it is replaced by converting the provided JSONObject into its XML form. The new XML fragment is then inserted in place of the original one. Afterward, any necessary closing tags are appended in reverse order to properly close the surrounding XML structure. The resulting XML string is finally converted back into a JSONObject using the existing XML.toJSONObject(String) method.

------



## Test Cases (starting from line 1431 in XMLTest.java)

The test cases for Milestone 2 begin at **line 1431** in `src/test/java/org/json/junit/XMLTest.java.`.



### **Test 1: testExtractJSONObject**

**Purpose**

This test checks `XML.toJSONObject(Reader reader, JSONPointer path)` and verifies the correct extraction of a <book> element from a sample XML string. The JSONPointer navigates to /catalog/book, and the method is expected to return a JSONObject that accurately reflects the structure and content of the <book> element.

### **Test 2: testReplaceJSONObject**

**Purpose**

This test checks `XML.toJSONObject(Reader reader, JSONPointer path, JSONObject replacement)` and whether the replacement logic correctly replaces the entire <book> element with a placeholder JSONObject containing "NA" values for all fields.

### **Test 3: testReplaceJSONObjectWithInvalidPath**

**Purpose**

This test checks `XML.toJSONObject(Reader reader, JSONPointer path, JSONObject replacement)` and handles the case where the given path does not exist in the XML document. In this case, no replacement should occur, and the original structure should remain unchanged.