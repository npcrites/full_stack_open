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
    Note right of browser: Server saves new note's `content` and `date` as new note object, could save to DB at this point

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

```mermaid
sequenceDiagram
    participant browser
    participant server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/notes_spa
    activate server
    server-->>browser: HTML document (not specifically tailored for notes page)
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.css
    activate server
    server-->>browser: the css file (not specifically tailored for notes page)
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.js
    activate server
    server-->>browser: the JavaScript file
    deactivate server

    Note right of browser: The browser starts executing the JavaScript code that fetches the JSON from the server via API

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate server
    server-->>browser: [{ "content": "HTML is easy", "date": "2023-1-1" }, ... ]
    deactivate server

    Note right of browser: The browser executes the fetched JS code to dynamically generate the HTML for the Notes page and insert it into the existing HTML structure via the DOM-API.
```
* HTTP GET request for SPA or SPA uses previously fetched HTML, JavaScript, and CSS files
* Browser executes JavaScript code and calls JSON files from server via API
* Browser uses JavaScript code to dynamically generate HTML & CSS code / structure via DOM-API

### Loading / Rendering a new note in a Single Page Application

```mermaid
sequenceDiagram
    participant browser
    participant server

    browser->>server: GET all files (if not already stored locally)
    activate server
    server-->>browser: HTML (not specifically tailored for notes page), CSS  (not specifically tailored for notes page), JavaScript files
    deactivate server

    Note right of browser: The browser starts executing the JavaScript code that fetches the JSON from the server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json (prompted by JS file)
    activate server
    server-->>browser: the JSON file
    deactivate server

    Note right of browser: The browser executes JS code to dynamically generate the HTML for the Notes page and insert it into the existing HTML structure via the DOM-API.

    Note right of browser: The user now creates a new note

    browser->>server: POST https://studies.cs.helsinki.fi/exampleapp/new_note_spa
    activate server
    server-->>browser: server processes data using `content_type` header. throws status code 201 with no redirect instructions.
    deactivate server

    Note right of browser: The browser executes an event handler that prevents default handling of the form's submit

    browser->>server: adds note (li) to list (ul) and renders new notes page locally via DOM-API. JS instructs browser to send POST req with JSON string attched
    activate server
    deactivate server

    Note right of browser: The parses JSON string to create new Notes object
```
* Render SPA
* Upon new note entry, browser sends a POST request to server, to which server throws with status code 201 and no redirection
* User opts to `Submit` their note
* JS code prevents browser from sending GET request to server upon submission
* New note is locally rendered
* Event handler instructs browser to send POST request with JSON string attached
* Server parses JSON string and creates new note object
