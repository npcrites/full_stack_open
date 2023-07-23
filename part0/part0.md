# Part 0: Basic JavaScript Webpage Diagrams

The below sequence diagrams were created using Mermaid-syntax in a GitHub .md file. 

### Loading / Rendering a traditional JavaScript webpage

```mermaid
sequenceDiagram
    participant browser
    participant server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/notes
    activate server
    server-->>browser: HTML document
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.css
    activate server
    server-->>browser: the css file
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.js
    activate server
    server-->>browser: the JavaScript file
    deactivate server

    Note right of browser: The browser starts executing the JavaScript code that fetches the JSON from the server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate server
    server-->>browser: [{ "content": "HTML is easy", "date": "2023-1-1" }, ... ]
    deactivate server

    Note right of browser: The browser executes the callback function that renders the notes
```
* The browser fetches the HTML code defining the content and the structure of the page from the server using an HTTP GET request.
* Links in the HTML code cause the browser to also fetch the CSS style sheet main.css...
...and the JavaScript code file main.js
* The browser executes the JavaScript code. The code makes an HTTP GET request to the address https://studies.cs.helsinki.fi/exampleapp/data.json, which returns the notes as JSON data.
* When the data has been fetched, the browser executes an event handler, which renders the notes to the page using the DOM-API.

### New Note Creation (basic JavaScript web app)

```mermaid
sequenceDiagram
    participant browser
    participant server

    browser->>server: POST https://studies.cs.helsinki.fi/exampleapp/new_note
    activate server
    server-->>browser: status code 302, HTTP GET request to the address defined in the header's Location - the address notes.
    deactivate server
    Note right of browser: Server saves new note's `content` and `date`, could save to DB at this point

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/notes (server GET request to address in header's `Location`)

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.css
    activate server
    server-->>browser: CSS file
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.js
    activate server
    server-->>browser: the CSS file
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate server
    server-->>browser: the JSON file including new note entry
    deactivate server

    Note right of browser: The browser executes the callback function that renders the notes
```
* The browser fetches the HTML code defining the content and the structure of the page from the server using an HTTP GET request.
* The server throws a status code 302 and requets the browser to fetch the notes address defined in the header's `Location`
* The browser fetches the requested notes file
* Links in the HTML code cause the browser to also fetch the CSS style sheet main.css...
...and the JavaScript code file main.js
* The browser executes the JavaScript code. The code makes an HTTP GET request to the address https://studies.cs.helsinki.fi/exampleapp/data.json, which returns the notes as JSON data.
* When the data has been fetched, the browser executes an event handler, which renders the notes to the page using the DOM-API.


### Loading / Rendering a Single Page Application
