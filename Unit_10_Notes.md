# IS 597 — Unit 10 Study Notes
## Programming & Quality in Analytics

---

## Topics Covered
1. DOM & HTML Basics
2. HTTP Requests (`requests` package)
3. XML & JSON
4. `lxml` + XPath in Python
5. `json` package
6. RegEx (`re` package)
7. Processing Pipelines

---

## 1. DOM & HTML Basics

### The `<script>` Tag
The `<script>` tag embeds JavaScript inside HTML. It runs when the page loads.
```html
<h1 id="demo">This is a Heading</h1>
<script>
  document.getElementById("demo").innerHTML = "Hello World!";
</script>
```

- `document` — built-in JavaScript object representing the entire page
- `getElementById("demo")` — finds the element with id="demo" in the DOM tree
- `.innerHTML` — property that gets/sets the HTML content inside a tag

### What is the DOM?
When a browser loads HTML, it converts it into a **tree of objects** in memory — this is the DOM (Document Object Model).
```
Document
└── html
    ├── head
    └── body
        ├── h1  ← Element node (object)
        │   └── id="demo"  ← Attribute (property of the object)
        └── p
```

Every HTML element becomes an **object** with **properties** you can read or change.

### Container Tags vs Self-Closing Tags

| Type | Example | Use `.innerHTML`? |
|---|---|---|
| Container tag | `<h1>`, `<p>`, `<button>`, `<div>` | ✅ Yes |
| Self-closing tag | `<input>`, `<img>` | ❌ No |

For self-closing tags, use the relevant property instead:
- `<input>` → use `.value`
- `<img>` → use `.src`

### Common DOM Properties

| Property | What it does |
|---|---|
| `.innerHTML` | Gets/sets HTML content inside a tag |
| `.value` | Gets/sets value of input fields |
| `.src` | Gets/sets image source |
| `.id` | The id attribute |
| `.style` | CSS styling |
| `.className` | CSS class |

---

## 2. HTTP Requests — `requests` Package

### What is it?
`requests` is a Python library for making HTTP requests — like what your browser does, but from Python code.
```bash
pip install requests
```
```python
import requests
```

### Making Requests
```python
r = requests.get('https://api.github.com/events')
r = requests.post(url, data={'key': 'value'})
r = requests.put(url, data={'key': 'value'})
r = requests.delete(url)
```

### The Response Object

| Property | What it gives you |
|---|---|
| `r.status_code` | 200 = OK, 404 = Not Found, 500 = Server Error |
| `r.text` | Response as a string |
| `r.json()` | Response parsed as JSON |
| `r.headers` | Response metadata |
| `r.encoding` | Text encoding (e.g. UTF-8) |

### Passing URL Parameters
```python
payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.get('https://httpbin.org/get', params=payload)
print(r.url)
# https://httpbin.org/get?key1=value1&key2=value2
```

### Custom Headers
```python
headers = {'user-agent': 'my-app/0.0.1'}
r = requests.get(url, headers=headers)
```

---

## 3. XML & JSON

### What is XML?
XML (eXtensible Markup Language) is a format for storing and transferring structured data — similar to JSON but older.
```xml
<person id="123" active="true">
  <name>John</name>
  <age>30</age>
</person>
```

- **Attributes** (`id="123"`) → metadata *about* the element
- **Children** (`<name>John</name>`) → the actual data content

### What is JSON?
```json
{
  "id": 123,
  "active": true,
  "name": "John",
  "age": 30
}
```

JSON has no concept of attributes vs content — everything is just a key-value pair at the same level.

### XML vs JSON Comparison

| | XML | JSON |
|---|---|---|
| Age | Older (1998) | Newer (2001) |
| Verbosity | More verbose | More compact |
| Data types | Everything is text | Numbers, booleans, arrays natively |
| Attributes | Yes (metadata on tags) | No attributes concept |
| Querying | XPath / XQuery | JSONPath / direct access |
| Used today | Config files, legacy APIs, documents | Modern APIs, web apps |

### Where is XML used today?
- Configuration files (Maven, Android, Spring)
- Microsoft Office files (`.docx`, `.xlsx` are zipped XML)
- RSS/Atom feeds (news, podcasts)
- SOAP web services (banking, government, healthcare)
- SVG image files
- Sitemap files (`sitemap.xml`)

### What is Metadata?
Metadata is **data about the data** — it describes the content rather than being the content itself.
```xml
<book isbn="978-3-16" language="en" available="true">
  <title>Harry Potter</title>
  <author>J.K. Rowling</author>
</book>
```

Here `isbn`, `language`, `available` are metadata — they describe the book element. `title` and `author` are the actual content.

---

## 4. `lxml` + XPath in Python

### What is `lxml`?
`lxml` is a Python library that parses XML and HTML into a navigable tree structure (DOM). XPath is then used to query that tree.
```
lxml  → loads and parses the document into a tree
XPath → queries that tree to find/extract what you need
```
```bash
pip install lxml
```
```python
from lxml import etree   # for XML
from lxml import html    # for HTML
```

### Building XML
```python
from lxml import etree

root = etree.Element("person")
name = etree.SubElement(root, "name")
name.text = "John"

print(etree.tostring(root, pretty_print=True).decode())
# <person>
#   <name>John</name>
# </person>
```

### Reading Attributes
```python
root = etree.Element("person", id="123", active="true")
print(root.get("id"))      # "123"
print(root.get("active"))  # "true"
```

### Parsing XML
```python
# From a string
root = etree.fromstring("<person id='123'><name>John</name></person>")

# From a file
tree = etree.parse("data.xml")
root = tree.getroot()
```

### Using XPath
```python
xml_string = """
<people>
  <person id="123"><name>John</name><city>Chicago</city></person>
  <person id="456"><name>Jane</name><city>Boston</city></person>
</people>
"""

root = etree.fromstring(xml_string)

# Get all names
names = root.xpath("//name/text()")
print(names)  # ['John', 'Jane']

# Get person with id=123
person = root.xpath("//person[@id='123']")
print(person[0].find("name").text)  # 'John'
```

### Key Element Properties

| Property/Method | What it does |
|---|---|
| `element.tag` | Tag name (e.g. "person") |
| `element.text` | Text content inside the tag |
| `element.get("attr")` | Get an attribute value |
| `element.attrib` | All attributes as a dictionary |
| `element.xpath("...")` | Run an XPath query |
| `etree.tostring(element)` | Convert back to XML string |

---

## 5. Python `json` Package

Built into Python — no installation needed.
```python
import json
```

### The 4 Core Functions

| Function | Direction | Works with |
|---|---|---|
| `json.dumps(obj)` | Python → JSON | String |
| `json.dump(obj, file)` | Python → JSON | File |
| `json.loads(string)` | JSON → Python | String |
| `json.load(file)` | JSON → Python | File |

**Memory tip:** `s` = string, no `s` = file

### Examples
```python
# Python → JSON string
data = {"name": "John", "age": 30, "active": True}
json_string = json.dumps(data)
# '{"name": "John", "age": 30, "active": true}'

# Pretty print
print(json.dumps(data, indent=4))

# JSON string → Python
data = json.loads(json_string)
print(data["name"])  # "John"

# Write to file
with open("data.json", "w") as f:
    json.dump(data, f, indent=4)

# Read from file
with open("data.json", "r") as f:
    data = json.load(f)
```

### Type Conversions

| JSON | Python |
|---|---|
| `{}` object | `dict` |
| `[]` array | `list` |
| string | `str` |
| number (int) | `int` |
| number (float) | `float` |
| `true` | `True` |
| `false` | `False` |
| `null` | `None` |

### With `requests`
```python
import requests

r = requests.get("https://api.github.com/events")
data = r.json()   # built-in JSON parser in requests
```

---

## 6. RegEx — `re` Package

Built into Python — no installation needed.
```python
import re
```

### The 5 Most Used Functions

| Function | What it does |
|---|---|
| `re.search(pattern, text)` | Find **first** match anywhere |
| `re.match(pattern, text)` | Match only at **start** of text |
| `re.findall(pattern, text)` | Return **list** of all matches |
| `re.sub(pattern, replacement, text)` | **Replace** matches |
| `re.compile(pattern)` | Pre-compile pattern for reuse |

### Essential Pattern Syntax

| Pattern | Matches |
|---|---|
| `.` | Any single character (except newline) |
| `\d` | Any digit (0-9) |
| `\w` | Any word character (a-z, A-Z, 0-9, _) |
| `\s` | Any whitespace |
| `^` | Start of string |
| `$` | End of string |
| `*` | 0 or more |
| `+` | 1 or more |
| `?` | 0 or 1 |
| `[abc]` | Any one of a, b, or c |
| `[^abc]` | Anything except a, b, or c |
| `(abc)` | Capture group |
| `{n}` | Exactly n times |
| `{m,n}` | Between m and n times |

### Examples
```python
import re

# Find phone number
text = "My number is 123-456-7890"
match = re.search(r'\d{3}-\d{3}-\d{4}', text)
print(match.group())  # '123-456-7890'

# Find all prices
text = "Prices: $10, $25, $300"
prices = re.findall(r'\$\d+', text)
print(prices)  # ['$10', '$25', '$300']

# Clean up extra spaces
text = "Hello   World"
clean = re.sub(r'\s+', ' ', text)
print(clean)  # 'Hello World'
```

### Always Use Raw Strings (`r'...'`)
```python
re.search(r'\d+', text)   # correct
re.search('\\d+', text)   # also works but harder to read
```

### XPath vs RegEx

| | XPath | RegEx |
|---|---|---|
| Works on | Structured XML/HTML trees | Any plain text |
| Query style | Path-based (`//person/name`) | Pattern-based (`\d{3}-\d{4}`) |
| Best for | Navigating structured documents | Finding patterns in raw text |

**Rule:** Use XPath for XML/HTML, use RegEx for plain text.

---

## 7. Processing Pipelines

### What is a Pipeline?
A chain of steps where the **output of one step becomes the input of the next** — like an assembly line.
```
Step 1 → Step 2 → Step 3 → Final Output
```

### Shell Pipelines (Terminal)
```bash
cat data.txt | grep "error" | sort | uniq
```

- `cat` reads the file
- `grep` filters lines containing "error"
- `sort` sorts them
- `uniq` removes duplicates

The `|` (pipe) symbol connects each program's output to the next's input.

### Writing Pipe-Compatible Python Scripts
```python
import sys

for line in sys.stdin:         # read from previous step
    processed = line.upper()   # do something
    print(processed)           # pass to next step
```

Use it in a pipeline:
```bash
cat data.txt | python myscript.py | sort
```

### Data Analytics Pipeline (Full Picture)
```
requests      → fetch raw data from web/API
     ↓
lxml / json   → parse and structure it
     ↓
RegEx         → clean and extract patterns
     ↓
pandas        → analyze the data
     ↓
CSV / Excel   → output / report
```

---

## Key Takeaways

- **CSV/Excel** is for when someone already collected the data for you.
- **`requests` + `lxml`/`json` + RegEx** is for when you need to go collect it yourself.
- **lxml** builds the tree, **XPath** searches it — they work as a team.
- **XML** and **JSON** both store structured data; XML adds attributes for metadata, JSON keeps everything flat.
- **Pipelines** make your code modular — each step does one thing and passes results forward.

---

*Notes from IS 597 Unit 10 — Spring 2026*
