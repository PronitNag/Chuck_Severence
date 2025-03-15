# JSON - JavaScript Object Notation - JSON

JSON (JavaScript Object Notation) is a lightweight data interchange format that is easy to read and write for humans and easy to parse and generate for machines. It is widely used for transmitting data in web applications and APIs.

### Features of JSON:
- Lightweight and easy to read
- Uses key-value pairs (similar to JavaScript objects)
- Supports nested structures (arrays and objects)
- Language-independent format

### Example of JSON:
```json
{
    "name": "John Doe",
    "age": 30,
    "city": "New York",
    "skills": ["JavaScript", "PHP", "Python"]
}
```

---

# JSON and PHP - Part 2

PHP provides built-in functions to handle JSON data efficiently. JSON data can be encoded and decoded easily in PHP.

### Encoding JSON in PHP:
```php
$data = [
    "name" => "John Doe",
    "age" => 30,
    "city" => "New York"
];

$json_data = json_encode($data);
echo $json_data;
```
**Output:**
```json
{"name":"John Doe","age":30,"city":"New York"}
```

### Decoding JSON in PHP:
```php
$json_string = '{"name":"John Doe","age":30,"city":"New York"}';
$decoded_data = json_decode($json_string, true);
print_r($decoded_data);
```
**Output:**
```
Array (
    [name] => John Doe
    [age] => 30
    [city] => New York
)
```

---

# JSON and CRUD - Part 3

CRUD (Create, Read, Update, Delete) operations can be performed using JSON data. Below is an example of basic CRUD operations using PHP and JSON.

### Create (Write) JSON File:
```php
$data = [
    ["id" => 1, "name" => "Alice", "email" => "alice@example.com"],
    ["id" => 2, "name" => "Bob", "email" => "bob@example.com"]
];
file_put_contents('data.json', json_encode($data, JSON_PRETTY_PRINT));
```

### Read JSON File:
```php
$json_data = file_get_contents('data.json');
$data = json_decode($json_data, true);
print_r($data);
```

### Update JSON Data:
```php
$json_data = file_get_contents('data.json');
$data = json_decode($json_data, true);
$data[0]['email'] = "alice_new@example.com";
file_put_contents('data.json', json_encode($data, JSON_PRETTY_PRINT));
```

### Delete JSON Data:
```php
$json_data = file_get_contents('data.json');
$data = json_decode($json_data, true);
array_splice($data, 1, 1); // Removes second item
file_put_contents('data.json', json_encode($data, JSON_PRETTY_PRINT));
```

This demonstrates how JSON can be used in PHP for CRUD operations efficiently.

