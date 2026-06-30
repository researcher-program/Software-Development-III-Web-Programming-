# <h1 align="center"> Software Development III (Web Programming) </h1>
## <h2 align="center"> Week-1:Study Guide & Lab Manual </h2>
### <h3 align="center"> Topic: Web Architecture & System Requirement Modeling </h3>
---
___

#### 1. **Introduction To Web Architecture**
> To build high-performance,source,& maintainable Web application,developers must master the core mechanisms that drive the WWW(World Wide Web).At its simplest,every web interaction is a request-and-response transaction taking place over a global network.

1.1 **The Client-Server Paradigm**
> Modern web programming relies on a distributed application structure known as the **Client-Server Model**. The workload is partitioned between two distinct entities:

 1. **The Client(Frontend)**:
> The user-facing software application. In web environments,this is almost always a web browser4 (e.g., Google Chrome,Mozilla Firefox,Safari).The client's primary responsibility is presentation.It requests resources from the server, receives data formats like HTML,CSS, & JS(JavaScript),& renders them into an interactive graphical user interface(GUI).

 2. **The Server(Backend)**:
> A high-powered, remote computer or cluster that hosts application code, manages relational databases, and processes business logic. The server remains in an active listening state, waiting to intercept requests over designated network ports, processing those requests securely, and serving responses.

1.1.1 **Web Application Taxonomy**
> Web applications have evolved from basic static documents into highly complex, distributed dynamic systems:

 - **Static Websites:**
 > Pre-rendered HTML, CSS, and asset files stored on a server’s file system. When requested, the server sends these files to the client as-is. There is no real-time processing or database query involved. Every user sees the exact same content.
 - **Dynamic Web Applications:**
 > Built using server-side runtimes (such as PHP, Node.js, Python,or Go). When a request arrives, the application executes programming logic, communicates with database engines, dynamically merges data with presentation templates, and returns customized HTML or structured JSON to the specific user.

1.2 **Under the Hood: The Lifecycle of a Web Request**
> Before writing backend or frontend code, a web developer must understand exactly what occurs when a user enters a URL like https://www.universityportal.edu/dashboard into a browser address bar:

- 1. **DNS Resolution:**
> The browser reads the hostname (www.universityportal.edu). Because computers communicate via IP addresses, the browser queries a Domain Name System (DNS) resolver to map the human-readable domain to an IP address (e.g., 192.0.2.1).

- **2. TCP Connection Establishment (The 3-Way Handshake):**
> The client establishes a reliable transport connection with the server via Transmission Control Protocol (TCP) on Port 80 (HTTP) or Port 443 (HTTPS). This requires a synchronized sequence: 
> - **SYN:** Client sends a synchronize sequence number to the server.
> - **SYN-ACK:** Server responds with an acknowledgment and its own sequence number.
> - **ACK:** Client acknowledges the server’s sequence number.

- 3. **TLS Cryptographic Handshake (HTTPS Only):**
> The client and server establish an encrypted connection. The server shares its SSL/TLS Certificate containing its public key. The browser verifies the authenticity of this certificate against built-in certificate authorities.Once validated, both entities agree on a symmetric session key to encrypt all subsequent communication.

- 4. **HTTP Request Transmission:**
> The client compiles and transmits an HTTP request packet across the secure connection.

- 5. **Server Processing:**
> The web server (e.g., Apache or Nginx) intercepts the request and routes it to the application execution layer (e.g., PHP). The application validates parameters, executes business rules, runs SQL database queries, and formats the output payload.

- 6. **HTTP Response Transmission:**
> The server writes an HTTP response packet containing status codes, headers, and the processed output (such as an HTML document or JSON file) back to the socket.

- 7. **Client Rendering:**
> The browser parser reads the returned HTML document, requests supplementary assets (CSS, JS, images) indicated in the markup, executes JavaScript code, and visually updates the screen for the user.

1.3 **The HTTP and HTTPS Protocols**
> Communication between the client and server is governed by the HyperText Transfer Protocol (HTTP) and its secure extension, HTTPS. HTTP is an application-layer protocol designed to transmit structured hypermedia files.

   Two absolute rules define HTTP:
   - **Request-Response Loop:**
   > The communication is always unidirectional in its origin. The client must initiate a transaction by sending an HTTP Request. The server processes the request and returns a corresponding HTTP Response. The server cannot initiate communication with a client out of nowhere.

   - **Statelessness:**
   > HTTP is entirely stateless. Each request is processed as a completely independent execution. The server does not naturally remember prior requests. To construct features like shopping carts or active logins, developers must artificially manage state utilizing web utilities such as Cookies, Sessions, or cryptographically signed tokens.

`HTTPS (HTTP Secure)` wraps the raw HTTP payload inside a secure encryption layer called
**TLS (Transport Layer Security)**.This guarantees three properties: 

- **Encryption:** 
> Prevents attackers from eavesdropping on network traffic (e.g., stealing passwords on public Wi-Fi).
- **Data Integrity:** 
> Ensures data cannot be modified in transit without detection.
- **Authentication:** 
> Verifies that the client is connected to the actual intended server, protecting against domain-spoofing attacks.


- 1.4 **Anatomy of HTTP Transactions**

Understanding the literal structure of HTTP network packets is critical for debugging APIs and
configuring backends.

- 1.4.1 **The HTTP Request Packet**

An HTTP request consists of a Request Line, Request Headers, an empty line, and an optional
Request Body.

# <h3 align="center"> Listing 1: Example of a raw HTTP GET request </h3>
```HTTP
GET /profile/settings.php HTTP/1.1
Host: www.universityportal.edu
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/120.0.0.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9
Accept -Language: en-US,en;q=0.5
Cookie: PHPSESSID=abc123xyz789
Connection: keep-alive
[Empty Line - No Body for standard GET requests]
```

- **Request Line:**
 Consists of the method (GET), the request URI (/profile/settings.php),
and the protocol version (HTTP/1.1).
- **Host Header:**

 Dictates the exact domain name of the destination server, allowing virtual hosting on shared server IPs.

- **User-Agent Header:**

 Identifies the requesting browser, OS version, and client engine,which is useful for responsive rendering choices.

- **Cookie Header:**

 Sends saved cookies back to the server to establish state.


- 1.4.2 **The HTTP Response Packet**

An HTTP response consists of a Status Line, Response Headers, an empty line, and the Response
Body (containing the page markup or API JSON data).

# <h3 align="center"> Listing 2: Example of a raw HTTP response </h3>
```HTTP
HTTP/1.1 200 OK
Date: Mon, 01 Jun 2026 20:00:00 GMT
Server: Apache/2.4.41 (Unix) OpenSSL/1.1.1d
Content -Type: text/html; charset=UTF -8
Content -Length: 104
Set-Cookie: PHPSESSID=abc123xyz789; Path=/; Secure; HttpOnly
Connection: close
```
```html
<!DOCTYPE html>
<html>
<head><title >Dashboard </title ></head>
<body><h1>Welcome Back Student!</h1></body>
</html>
```

-  **Status Line:** 
Dictates the protocol version (HTTP/1.1) and the transaction status code (200
OK).

 - **Content-Type Header:** 
Tells the browser how to parse the incoming byte stream (e.g.,
render as HTML text, process as JSON, or download as a PDF binary).
- **Set-Cookie Header:** 
Commands the browser to store a specific token value in local secure storage, which will be returned in future HTTP request packets.

- 1.4.3 **Common HTTP Methods**

• **GET:** Requests a representation of the specified resource. It should only retrieve data and
have no side effects on the server database.
• **POST:** Submits data to be processed by the identified resource, often resulting in a change
in state or creation of database entries.
• **PUT/PATCH:** Replaces (PUT) or modifies (PATCH) an existing resource on the database.
• **DELETE:** Deletes the specified resource.

- 1.4.4 **HTTP Status Categories**
• **1xx** (Informational): Request received, continuing process.
• **2xx** (Success): The action was successfully received, understood, and accepted (e.g., 200 OK, 201 Created).
• **3xx** (Redirection): Further action must be taken to complete the request (e.g., 301 Moved
Permanently, 302 Found).
• **4xx** (Client Error): The request contains bad syntax or cannot be fulfilled (e.g., 401 Unauthorized,403 Forbidden, 404 Not Found).
• **5xx** (Server Error): The server failed to fulfill an apparently valid request (e.g., 500 Internal Server Error, 503 Service Unavailable).

2 **The Model-View-Controller (MVC) Pattern**

To build expandable, professional-grade systems, software engineering mandates a strict **Separation of Concerns (SoC).** Code that generates HTML layouts should never be mixed with backend database query operations. The industry-standard pattern for accomplishing this is **Model-View-Controller (MVC).**

>   The core rule of MVC: Keep your data access, your control logic, and your visual layout separated into clearly defined, isolated files and directories.


- 2.1 **MVC Component Breakdown**
• **The Model:** Represents the data structure, state definitions, and backend validation laws. It directly communicates with databases, queries files, and performs mathematical calculations. It does not know or care how its information will be visually rendered.

• **The View:** The presentation layout. It receives processed data from the Controller and
maps it directly into HTML, CSS, or JSON packages. The View is strictly passive; it does not
process user inputs or run complex queries.


• **The Controller:** The coordinator. It intercepts HTTP requests from the client, reads parameters (such as form fields or URL inputs), queries the corresponding Model for information, and passes that structured data directly to the appropriate View.

- 2.2 **Contrast: Spaghetti Architecture vs. MVC Pattern**

To understand the necessity of MVC, consider the following structural design layouts:

1. **Spaghetti Code (Coupled Architecture):** A single PHP file contains SQL database connection code, runs a raw select query, contains loops to process rows, prints raw HTML table tags, and injects custom styles.
• **Failure modes:** High maintainability cost. Changing the database table column name requires editing HTML styling files. UI designers risk breaking backend database connectivity if they change page layouts.

2. **MVC Code (Decoupled Architecture):**
• **Model file:** Manages a database class containing parameterized query definitions. It
exposes clean methods like getAllProducts().
• **Controller file:** Captures incoming requests, handles authorization, instances the
Model, collects data, and instantiates the proper View template.
• **View file:** Contains simple loop placeholders inside structural HTML templates to echo sanitized data.

- 2.3 **Side-by-Side Architectural Conceptualization**

Below is a concrete pseudo-code walkthrough demonstrating the MVC pattern applied to a
Product Inventory system.

# <h3 align="center"> Listing 3: Model pseudo-code: ProductModel.php </h3>
```php
class ProductModel {
private $db;
public function __construct($databaseConnection) {
$this ->db = $databaseConnection;
}
public function getInStockProducts() {
// Pure data retrieval. No HTML exists here.
$query = "SELECT id, name, price FROM products WHERE stock_count > 0";
return $this ->db->execute($query);
}
}
```
# <h3 align="center"> Listing 4: Controller pseudo-code: ProductController.php </h3>
```php
class ProductController {
private $model;
public function __construct($productModel) {
$this ->model = $productModel;
}
public function handleRequest() {
// Step 1: Handle business authorization rule
if (!isset($_SESSION['user_id'])) {
header("Location: /login.php");
return;
}
// Step 2: Query the model for clean structured data
$products = $this ->model ->getInStockProducts();
// Step 3: Pass data payload to view presentation layer
include("views/inventory_page.php");
}
}

```
# <h3 align="center"> Listing 5: View pseudo-code: views/inventory_page.php </h3>
```php
 <!-- Simple presentation layout. No SQL connections exist here. -->
<!DOCTYPE html>
<html>
<head><title>Product Inventory</title></head>
<body>
<h1>In-Stock Products</h1>
<table>
<?php foreach ($products as $item): ?>
<tr>
<td><?php echo htmlspecialchars($item['name ']); ?></td>
<td>$<?php echo htmlspecialchars($item['price ']); ?></td>
</tr>
<?php endforeach; ?>
</table>
</body>
</html>
```

3 **Requirements Engineering for Web applications**

Software projects frequently fail because teams begin writing code before defining what they
are building. Requirements engineering is the process of discovering, analyzing, specifying,
and verifying the exact features a system must have.
- 3.1 **Functional vs. Non-Functional Requirements**

Requirements are divided into two fundamental classifications:
- 3.1.1 **Functional Requirements (FR)**

These describe the system’s active behaviors, commands, and operations. They must be highly
specific, trackable, and verifiable.
• **Bad Functional Requirement:**  “The system should support an administrator dashboard.”
(Too vague).
• **Good Functional Requirement (FR-1):** “An administrator must be able to view, suspend, or
reactivate user accounts through a centralized management panel.”
• **Good Functional Requirement (FR-2):** “The system must send a password reset verification link to the user’s registered email address upon request.”


- 3.1.2 **Non-Functional Requirements (NFR)**

These represent the operational constraints, system qualities, performance targets, and overall
usability rules of the software. They define how well the system performs its functions.
• **Security:** All passwords must be dynamically hashed using the bcrypt algorithm prior to
storage in the database.
• **Performance (Latency):** Standard application query routes must return responses to the
user in under 1.5 seconds under a load of 100 concurrent users.

• **Scalability & Responsiveness:** The interface layouts must scale responsively across vary-
ing browser dimensions, supporting screen widths ranging from 320px to 1920px.

• **Accessibility:** The markup structure must fulfill WCAG 2.1 AA accessibility guidelines, ensuring compliance for screen readers.

- 3.2 **Eliciting Requirements: The Case Study Method**

To practice requirement analysis, students must analyze realistic business contexts.Consider this scenario:

> A local clinic requires an online web portal to replace its manual phone-scheduling system. Patients should be able to create an account using their phone number and email, search for doctors by specialty, and view available time slots. After selecting a slot, patients must receive a confirmation email. Admins must have the power to create doctor profiles and cancel appointments,which triggers an automated cancellation alert email to the patient.

Based on this scenario, a requirements engineer would extract key specifications:

• **FR-1 (Registration):** Patients can create secure profile accounts containing phone numbers, emails, and passwords.

• **FR-2 (Search):** Patients can filter available scheduling schedules based on doctor name or
clinical specialty.

• **FR-3 (Scheduling):** Patients can reserve an active appointment slot, changing its state to ”Reserved”.

• **FR-4 (Notification):** The application triggers a confirmation notification dispatch to the corresponding patient upon slot booking.

• **FR-5 (Admin Control):** Administrators can generate, update, or deprecate doctor directories.

• **FR-6 (Cancellation):** Admins can cancel scheduled appointments, which automatically
launches a cancellation alert email.

4 **UML Diagramming for Web Systems**

Unified Modeling Language (UML) provides standard visual blueprints to plan and communicate a web system’s scope and database design.

- 4.1 **Use Case Diagrams**

Use case diagrams represent the system’s behavioral boundaries, showcasing the interactions
between external users (Actors) and the system’s processes (Use Cases).

- 4.1.1 **Key Structural Rules**

• **Actors:** External entities (e.g., Guest User, Admin, Bank Gateway). They are drawn as stick
figures outside the system boundary.
• **System Boundary:** A clear box enclosing the use cases, representing the scope of your
software application.
• **Use Cases:** Illustrated as simple horizontal ellipses labeled with active verb phrases (e.g., ”Verify Credentials”).

• **Relationships:**
– **Association:** Solid lines connecting actors to the use cases they perform.
– **Include (<<include>>):** Represents a step that is mandatory to complete the primary
use case. For example, ”Checkout Cart” must always <<include>> the ”Authenticate Session” use case first.

– **Extend (<<extend>>):** Represents conditional behavior that only executes under cer-
tain rules. For example,  ”Process Payment” might <<extend>> to ”Show Insufficient
 Funds Error” if the transaction is declined.

- 4.1.2 **Writing a Comprehensive Use Case Narrative Specification**

A UML use case diagram must be supported by a detailed use case narrative to guide develop-
ers.

# <h3 align="center"> Table 1: Use Case Narrative Specification for ”Book Appointment” </h3>
| Use Case Element | Detail Specifications |
| :----: | :---: |
| **Use Case Name** |  Book Doctor Appointment |
| **Primary Actor** |  Registered Patient |
| **Preconditions** |  User is logged in and authenticated on the system dashboard. |
| **Postconditions** |  Appointment status updated in DB; booking notification dispatched to user. |
| **Main Flow** |  
1. User navigates to search scheduling page.
2. User selects Doctor specialty and clicks ”Find Availability”.
3. System returns list of available open time slots.
4. User selects a preferred slot and submits application.
5. System locks slot, updates status to ”Booked”, and alerts the actor.
| **Alternative Flows** |  **Alt-1 (Slot Already Taken):** If the slot is claimed by another thread,display warning message and refresh the booking matrix dashboard. |


- 4.2 **Domain Class Diagrams**

Class diagrams model the static data structure of your system, which eventually translates into
backend code objects and relational database schemas.
Each class is represented by a box with three stacked compartments:

- 1. **Class Name:** The unique name of the entity, capitalized (e.g., Account).

- 2. **Attributes (Properties):** The internal variables of the object. Visibility must be specified:

• Private (-): Access restricted to the class itself.
• Public (+): Access open from any other class.
• Protected (#): Access restricted to class and descendants.

Format: visibility name : type (e.g., - email : string).

- 3. **Operations (Methods):** The callable actions. Format: visibility name(parameter : type)
: return_type (e.g., + checkPassword(pwd: string) : boolean).

- 4.2.1 **Multiplicity & Association Rules**

Lines connecting classes represent database relationships. Multiplicities are indicated at the end of these associations:

• 1: Exactly one.
• 0..*: Zero or more (many).
• 1..*: One or more.

For example, one Customer can have many orders (0..∗), but each Order must belong to exactly 1 Customer.

5 **Step-by-Step Practical Lab Manual (Lab Task 1)**

The goal of this week’s laboratory class is to establish your local development platform, organize your design group, choose your Term Project topic, and draft your initial system blueprints.
- 5.1 **Step 1 : Installing the Development Suite**

You must download and configure the following developer utilities:

-1. **Integrated Development Environment (VS Code):**

• Download and execute the installer from code.visualstudio.com.
• Open VS Code. Navigate to the extensions tab on the left-hand navigation panel (or
press Ctrl+Shift+X).
• Locate and install the following packages:

– Live Server (by Ritwick Dey): Launches a local development server with live-reloading
capabilities for static files.

– Prettier - Code Formatter: Enforces consistent tab widths, semi-colons, and syn-
tax formatting rules across your team.

- 2. **Browser Developer Tools:**

• Ensure you have Google Chrome or Mozilla Firefox Developer Edition installed.

• Launch your browser, navigate to any web portal, and press F12 (or right-click → Inspect).

• Familiarize yourself with the **Elements** tab (for styling), **Console** (for system logs),
and the **Network** panel (to watch HTTP/S requests in real time).

- 3. **Local Web Server (XAMPP):**

• Download and install XAMPP from Apache Friends. Ensure that you select **Apache** and **MySQL** during the installation steps.
• Run the XAMPP Control Panel application. Click the Start buttons next to Apache and
MySQL.
• Open your web browser and navigate to http://localhost/.If the installation was successful, the XAMPP dashboard page will load.

- 5.2 **Step 2: Drafting the Software Requirement Specification (SRS)**

Form teams of 3 to 4 members and assign a Team Lead. Together, choose a web application concept (e.g., E-Commerce platform, Clinic Scheduler, Freelance Hub, or Learning Management System)..

Create a markdown document named Project_SRS.md inside a newly formatted project di-
rectory, following this structure:

# <h3 align="center"> Listing 6: Markdown Template for the Project SRS </h3>
```markdown
# Software Requirements Specification (SRS)
## Project Title: [Your Web Application Name]
### 1. Introduction
* **1.1 Purpose:** A clear paragraph describing what the system is and the core
problem it solves.
* **1.2 Scope:** Outlining which boundaries are included in this project , and
what is excluded.
### 2. User Roles
Describe the actors who interact with the system (e.g., Guest , Client ,
Administrator).
### 3. Functional Requirements
* **FR-1:** [Requirement description]
* **FR-2:** [Requirement description]
...
* **FR-8:** [Requirement description]
### 4. Non-Functional Requirements
* **NFR -1 (Security):** [Password encryption , SQL Injection protection targets]
* **NFR -2 (Performance):** [Target page response times]
* **NFR -3 (Responsiveness):** [Cross -device capabilities]
* **NFR -4 (Compatibility):** [Supported browsers]
```

6 **Lab 1 Assessment & Submission Guidelines**

Your group must submit the final Project_SRS.md containing the markdown text and the em-
bedded diagrams by the start of the Week 2 laboratory class.

- 6.1 **Week 1 Assessment Rubric**

The work will be graded out of 100 points based on the academic standards in Table 2.

# <h3 align="center"> Table 2: Grading Rubric for Week 1 Lab Deliverables </h3>

| Criteria | Excellent(90–100%) | Satisfactory (70–89%)     |Needs Work(0–69%) |
| :----: | :---: | :--------: |:--------:|
| **Environment Setup (20%)** | VS Code extensions and XAMPP local servers running on all machines.  | VS Code setup complete, minor issues running XAMPP servers. | Environment not setup or configured. |
| **Requirements Engineering (30%)** | At least 8 FRs and 4 NFRs drafted using specific, trackable,professional language. | Requirements present but contain ambiguous phrasing or incomplete scope. |Requirements are missing, unformatted, or incomplete. |
| **UML Diagramming (30%)** | Diagrams are syntactically and logically correct, matching all modeled software rules.  | Diagrams are complete but contain minor multiplicity or logical errors. |Diagrams lack structural integrity or contain semantic errors. |
| **Professional Delivery (20%)** | Clean markdown formatting with all files versioned. Roles allocated clearly.  | Document complete but missing markdown formatting structure. |No team formed or document not delivered on time. |



# <h1 align="center"> --- [ The End ] --- </h1>