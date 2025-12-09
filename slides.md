---
# try also 'default' to start simple
theme: ./theme
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: ./theme/assets/slidev-background.svg
# some information about your slides (markdown enabled)
title: ICTPRG553 Create and Develop REST APIs | Session 05 CORS
info: ...
author: Adrian Gould <adrian.gould@nmtafe.wa.edu.au>
description: TBA
keywords: [Adrian Gould, Laravel, CORS, SaaS, Back-End, REST]
# apply UnoCSS classes to the current slide
class: text-left
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# duration of the presentation
duration: 35min
# aspect ratio for the slides
aspectRatio: 16/9
# real width of the canvas, unit in px
canvasWidth: 1080
lineNumbers: true
---

# ICTPRG553 REST APIs

## Session 05 - CORS

A guide to understanding and implementing Cross-Origin Resource Sharing<br> in REST APIs using Laravel.

<div @click="$slidev.nav.next" class="mt-12 ml-12 p-1 px-4 bg-white/10 rounded w-96" hover:bg="black op-30">
  Press <kbd>Space</kbd> or <kbd>Right-Arrow</kbd> for next page <fa7-solid-arrow-right></fa7-solid-arrow-right>
</div>

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="slidev-icon-btn">
    <fa7-solid-edit class="op-50 hover:op-75"/>
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <fa7-brands-github class="op-50 hover:op-75"></fa7-brands-github>
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->


---
layout: about-me
transition: fade-out
hideInToc: true

helloMsg: Hello there!
name: Adrian Gould
imageSrc: ./theme/assets/ady-202401-2.jpg
position: left
job: "Lecturer, ASL | Coach | Mentor"
line1: "@ North Metropolitan TAFE"
line2: "Web App Dev, IoT Dev, and more"
line3: "Manager: Screencraft, SQuASH"
email: "adrian.gould@nmtafe.wa.edu.au"
social2: "Helpdesk: https://help.screencraft.net.au"
social3: https://github.com/AdyGCode
---


---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
hideInToc: true
---

# Navigation

Hover over the bottom-left corner to see the navigation's control panel.

## Keyboard Shortcuts

|                                                     |                             |
| --------------------------------------------------- | --------------------------- |
| <kbd>right</kbd> / <kbd>space</kbd>                 | next animation or slide     |
| <kbd>left</kbd>  / <kbd>shift</kbd><kbd>space</kbd> | previous animation or slide |
| <kbd>up</kbd>                                       | previous slide              |
| <kbd>down</kbd>                                     | next slide                  |


---
layout: two-cols
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
hideInToc: true
---

# Table of contents

<Toc text-sm minDepth="1" maxDepth="1" />


---
layout: image-right
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
image: ./theme/assets/CORS.png
---

# APIs, CORS and Laravel

This session introduces <br>**Cross-Origin Resource Sharing** (CORS):

- CORS theory
- Real-world scenarios
- Handling CORS in Laravel
- Testing CORS policies
- HTTP verb: OPTIONS
- Pre-flight requests
- Practical exercises & research


---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# Preparation

Make sure you have:

- Reviewed the previous session content
- Downloaded a copy of the `session-xx-journal.md` file
- Renamed the file to `session-05-jounal.md`

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
---

# CORS 

## What is it?

- a security feature
- grants permission to a resource made by a request
- restricts access to prescribed 'domains'

<small class="text-xs bg-red-900 px-2 py-1 rounded">Many DEVs will say that CORS is a headache...</small>

<!--
CORS allows web applications to request resources from a 
different domain than the one that served the web page.
-->

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
---

# CORS in the Real World

## Discussion:

- Have you encountered APIs being blocked due to CORS?
    - **If not**, investigate real-world reported issues.<br>&nbsp;
- Where did you, or they, encounter the issue?
- How were the CORS issues resolved?

<br>

<span class="bg-sky-800 py-1 px-2 rounded">Duration: ~10 minutes</span>

<!--
Emphasise that NO AI should be used in researching this or other questions.
Explain it is to encourage and develop good search skills
Explain it will help to distinguish between real and fake information
-->

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in the Real World - 2

### Example Scenarios<br>&nbsp;

- Accessing third-party APIs from a frontend app.<br>&nbsp;
- Loading fonts, scripts, or images from a CDN.<br>&nbsp;
- Enabling secure communication between microservices or subdomains.

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in the Real World - 3

### Third-Party API Integration in Web Applications<br>&nbsp;

- A frontend web app <br> `https://example-app.com` <br> fetches data from a 3rd party API, `https://api.openweathermap.org`

<!--

**Third-Party API Integration in Web Applications**

Since the frontend and the API are on different domains, the browser enforces the same-origin policy and blocks the request unless the API server includes appropriate CORS headers (e.g., `Access-Control-Allow-Origin: https://example-app.com`).
This allows the weather app to securely access external data without compromising user security.
-->

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in the Real World - 4

### Content Delivery Networks (CDNs) Serving Fonts or Scripts<br>&nbsp;

- A website `https://example.com` <br> uses fonts hosted on a CDN like<br> `https://fonts.googleapis.com`

<!--
**Content Delivery Networks (CDNs) Serving Fonts or Scripts**

When the browser tries to load these fonts, it checks if the CDN allows cross-origin access. The CDN must respond with CORS headers like `Access-Control-Allow-Origin: *` or specify the requesting domain.
This ensures that resources like fonts or scripts can be shared across websites while maintaining control over who can access them.

CoPilot Query (2025-10-30) "give me two examples of the use of CORS in the real world"
-->

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
---

# CORS: A Breakdown

## The Parts of CORS<br>&nbsp;

| <span class="font-bold text-gray-400">Component</span> | <span class="font-bold text-gray-400">Short Explanation</span> |
|--------------------------------------------------------|----------------------------------------------------------------|
| **HTTP Headers**                                       | Servers determine access via headers.                          |
| **Same-Origin Policy (SOP)**                           | Requests must come from same domain.                           |
| **CORS Mechanism**                                     | Restrict access from different origins.                        |

<!--
**Same-Origin Policy (SOP):**

Browsers restrict web pages from making requests to a different domain than the one that served the web page, for security reasons.

**CORS Mechanism:**

CORS is a protocol that allows servers to specify who can access their resources from a different origin.

**HTTP Headers:**

Servers use headers like `Access-Control-Allow-Origin` to indicate which domains are permitted to access resources.
-->

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS: A Breakdown - 2

## The Parts of CORS<br>&nbsp;

| <span class="font-bold text-gray-400">Component</span> | <span class="font-bold text-gray-400">Short Explanation</span> |
| ----------------------------- | ---------------------------------------------------- |
| **Pre-flight Requests**       | Browser able to "pre-flight check" HTTP Verb access. |
| **Security Control**          | Helps prevent malicious access to data.              |
| **Server-Side Configuration** | Enabled and configured on the hosting server.        |

<!--
**Pre-flight Requests:**

For certain types of requests (e.g. with custom headers or methods like PUT/DELETE), browsers send an `OPTIONS` request first to check permissions.

**Security Control:**

CORS helps prevent malicious websites from accessing sensitive data on another domain without permission.

**Server-Side Configuration:**

CORS must be enabled and configured on the server hosting the resource.
-->

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
---

# Exercise 1

## Research the following

- Standard HTTP API request sequence.<br>&nbsp;
- The CORS pre-flight request sequence.<br>&nbsp;
- Diagrams or Flowcharts for both sequences.<br>&nbsp;
- The headers involved in CORS.


<span class="bg-red-900 p-1 rounded">**Note:** Ensure you have at least two references for each item.</span>

<span class="bg-sky-800 py-1 px-2 rounded">Duration: ~20 minutes.</span>

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# Exercise 1: Step 2

## Outcomes

- Summarise each of the researched items in the markdown journal.<br>&nbsp;
- Add at least 2 (bibliographic) references for all the above points.<br>&nbsp;
- Use [MyBib.com](https://mybib.com) to make the APA entries.<br>&nbsp;

<span class="bg-sky-800 py-1 px-2 rounded">Duration: ~20 minutes.</span>

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
---

# CORS in Laravel

## Package

Previously you may have required:

- `fruitcake/laravel-cors`

Laravel now has CORS support built in.

## Documentation

Details from the documentation:

- https://laravel.com/docs/12.x/sanctum#cors-and-cookies

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in Laravel - 2

## Publish Configuration

- Publish the CORS settings

```shell
php artisan config:publish cors
```
<br>&nbsp;

## Configuration file

- Configuration is located in `config/cors.php`

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in Laravel - 4

## Configuration options

|                        |                                                         |
| ---------------------- |---------------------------------------------------------|
| `paths`                | Routes where CORS applies (e.g., `api/*`)               |
| `allowed_methods`      | HTTP methods allowed (e.g., `GET`, `POST`)              |
| `allowed_origins`      | Domains allowed to access (e.g., `https://example.com`) |
| `allowed_headers`      | Headers allowed in requests                             |
| `supports_credentials` | Whether cookies or auth headers are allowed         |

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in Laravel - 5

## Example File

```php [php] {none|1,9|2|3|4|5|6|7|8|all}
return [
    'paths' => ['api/*'],
    'allowed_methods' => ['*'],
    'allowed_origins' => ['https://example.com'],
    'allowed_headers' => ['*'],
    'exposed_headers' => [],
    'max_age' => 0,
    'supports_credentials' => false,
];
```
<br>&nbsp;

### To provide access control credentials

- Update `supports_credentials` to `true`.

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in Laravel - 6

- Laravel automatically applies CORS. <br>&nbsp;
- Uses middleware to achieve this functionality. <br>&nbsp;
- Applied to Routes in the `api.php` file. <br>&nbsp;
- Manually add as needed.<br>&nbsp;

```php [php] {none|all}
Route::middleware(['cors'])->group(function () {
    Route::get('/data', 'ApiController@getData');
});
```

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in Laravel - 7

## Testing

Testing is completed via:

- 3rd-party tools:
    - e.g. Postman or Browser Extension/Developer tools.
    - Inspect the response headers.
    - Find `Access-Control-Allow-Origin` in the response.<br>&nbsp;
- Unit Testing
  - e.g. PhpUnit, PEST



---
layout: two-cols
background: ./theme/assets/slidev-background-gray.svg
transition: fade-out
---

# Exercise 2

::left::
## Research time

- Using **Postman** (or equivalent), and
- Testing CORS requests with **Postman**

<br>

You will be broken into small teams...
- Lecturer will give more detail

::right::

## Instructions

- Identify tutorials and resources to assist <br> you in learning **Postman**.
- Share what you have found
- Add details to your `session-05-journal.md`
- Rememeber to use [MyBib.org](https://mybib.org) to create <br>APA v6 or v7 bibliographic references

<span class="bg-sky-800 py-1 px-2 rounded">Duration: ~20 minutes</span>



---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in Laravel - 8

## Testing via PEST

We present some basic examples to assist.

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in Laravel - 9

## Testing via PEST

```php [php] {none|1-2,|4-8|10,16|11-15|all}
use Illuminate\Support\Facades\Route;
use Illuminate\Http\Request;

beforeEach(function () {
    Route::middleware('api')->get('/test-cors', function (Request $request) {
        return response()->json(['message' => 'CORS test']);
    });
});

it('includes CORS headers in response', function () {
    $response = $this->withHeaders([
        'Origin' => 'https://example.com',
    ])->get('/test-cors');

    $response->assertHeader('Access-Control-Allow-Origin', 'https://example.com');
});
```

<!--
### What This Does:

- Defines a test route `/test-cors` using the `api` middleware group.
- Sends a request with an `Origin` header.
- Asserts that the response includes the correct `Access-Control-Allow-Origin` header.
-->

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in Laravel - 10

## Pre-flight Testing

```php [php] {none|1-5|7,16|8-11|13-15|all}
beforeEach(function () {
    Route::middleware('api')->options('/test-cors', function () {
        return response()->json(['message' => 'Preflight OK']);
    });
});

it('responds to preflight OPTIONS request with CORS headers', function () {
    $response = $this->withHeaders([
        'Origin' => 'https://example.com',
        'Access-Control-Request-Method' => 'POST',
    ])->options('/test-cors');

    $response->assertStatus(200);
    $response->assertHeader('Access-Control-Allow-Origin', 'https://example.com');
    $response->assertHeader('Access-Control-Allow-Methods');
});
```

<!--
### 🔍 What This Test Does:

- Sets up a route to respond to `OPTIONS` requests.
- Sends a simulated preflight request with `Origin` and `Access-Control-Request-Method` headers.
- Asserts that the response includes allow control allow origin & URI
- Asserts an OK 200 response
- Asserts the correct Access Control Allow Methods
-->

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in Laravel - 11

## Pre-flight Testing

With credentials:

- **Must not** use `'*'` for `allowed_origins` <br> when `supports_credentials` is `true`.<br> &nbsp;
- **Must** specify exact domains.<br> &nbsp;
- Browser then allows cookies, authorization headers, and <br> other credentials to be sent with cross-origin requests. 

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# CORS in Laravel - 12

### Testing with Credentials

```php [php] {none|1-7|9,18|10-13|15-17|all}
use Illuminate\Support\Facades\Route;

beforeEach(function () {
    Route::middleware('api')->get('/cors-credentials', function () {
        return response()->json(['message' => 'CORS with credentials']);
    });
});

it('responds with CORS headers allowing credentials', function () {
    $response = $this->withHeaders([
        'Origin' => 'https://example.com',
        'Cookie' => 'session=abc123',
    ])->get('/cors-credentials');

    $response->assertStatus(200);
    $response->assertHeader('Access-Control-Allow-Origin', 'https://example.com');
    $response->assertHeader('Access-Control-Allow-Credentials', 'true');
});
```

<!--
TODO: Add details of what the code does

-->

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
---

# CORS in Laravel - Demo

Small demonstration of CORS within Laravel.

- Code: https://github.com/AdyGCode/tba<br>&nbsp;

### Setting up

We follow the standard steps for our development environment at TAFE.

- Clone repository or create new Laravel app
- Install Packages
- Set up environment
- Run development server


---
layout: one-two-cols
background: ./theme/assets/slidev-background-gray.svg
transition: fade-out
level: 2
---

# CORS in Laravel - Demo 2

Setting up a cloned repository &hellip;

::left::
### Steps 

1. clone the repo            
2. `cd` into folder            
3. install `composer` packages
4. install `npm` packages      
5. create database           
6. copy `.env.dev` to `.env`
7. update APP Key            
8. migrate & seed            
9. run dev server            

::right::
### Commands

```shell [shell] {none|1-2|3-4|5-6|7-8|9|all}
git clone https://github.com/adygcode/TBA
cd TBA                                   
composer install                         
npm install                              
touch database/database.sql              
cp .env.dev .env                         
php artisan key:generate                 
php artisan migrate:fresh --seed         
composer run dev                         
```
<br>&nbsp;
<small class="bg-sky-950 p-1 rounded ">
`dev` may be replaced with `dev-win` or `dev-linux` for operating system specific options.
</small>


---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
---

# Practice

- Create a small API (CRUD) for a To-Do API
- Implement CORS for the API
- Create & Execute Tests for API & CORS:
    - <small>PEST tests</small>
    - <small>Postman or equivalent</small>

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# Exercise 3

## Extend the To-Do application

Add Status to to-do entries

- Statuses are: `idea`, `on hold`, `in-progress`, `complete`, `waiting`
- Create Model, Migration, API Controller, API Routes, et al
- Add relationships:
    - <small>ToDo has one Status / Status has many ToDos</small>
    - <small>ToDo belongs to a User / User has many ToDos</small>
- Create & Execute Tests for API & CORS:
    - <small>PEST tests</small>
    - <small>Postman or equivalent</small>

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# Recap & Summary

## Recap Discussion

You will be partnered up with another person in the class.

You and your partner are to:

- discuss what was learned
- add details to your individual `session-05-journal.md`

<span class="bg-sky-800 py-1 px-2 rounded">Duration: ~10 minutes</span>

---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# Closing

## Before finishing this session...

Make sure you have done the following:

- Added commenting to any code to illustrate **your** understanding.
- Added references relevant to this topic for two or more of the following:
  - articles,
  - written tutorials,
  - video tutorials, and
  - framework documentation.
- Pushed your code to GitHub for future reference.



---
layout: center
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
---

# Laissez les bons temps rouler

<div class="text-2xl mt-18">
Remember to look at the content that is set as pre-reading before next session.
</div>
<div class="text-2xl mt-18">
A review quiz will be available just before the next session. 
</div>

<fa7-solid-smile class="text-sky-600 text-3xl mt-6" />


---
layout: default
transition: fade-out
background: ./theme/assets/slidev-background-gray.svg
level: 2
---

# Acknowledgements

| **Item**   | **Reference / Disclosure**                                                                       |
| ---------- | ------------------------------------------------------------------------------------------------ |
| **Images** | Photo of author by himself; Other images by Adrian Gould                                         |
| **Icons**  | [Stencil Icons by Icons8 (https://icons8.com)](https://icons8.com)                               |
| **AI Use** | Some parts of this presentation have been supplemented with use of Microsoft's Copilot AI system |
